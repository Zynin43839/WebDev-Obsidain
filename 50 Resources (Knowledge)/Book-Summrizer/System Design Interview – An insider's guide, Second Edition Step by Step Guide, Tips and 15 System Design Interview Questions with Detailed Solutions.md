# System Design Interview — An insider's guide, Second Edition

> **Author:** Alex Xu

หนังสือ "System Design Interview" ของ Alex Xu เล่มนี้เป็นคู่มือเตรียมสัมภาษณ์งานตำแหน่งวิศวกรที่ต้องออกแบบระบบขนาดใหญ่ (large-scale distributed systems) เนื้อหาครอบคลุมตั้งแต่พื้นฐานไปจนถึง 15 ตัวอย่างข้อสอบพร้อมวิธีออกแบบโดยละเอียด

---

## บทที่ 1 — Scale จาก 0 ถึงหลักล้านผู้ใช้ (Ch.1)

การขยายระบบจาก user ไม่กี่คนไปถึงล้านคนต้องค่อยๆ เพิ่ม component ทีละชั้น:
- **Single server** — เริ่มจากทุกอย่างอยู่ในเครื่องเดียว (web, app, DB)
- **Separate DB** — แยก database server เพื่อลด contention
- **Load balancer** — กระจาย traffic ไปยัง web server หลายตัว พร้อม public IP แทน
- **Database replication** — master เขียน, slave อ่าน (read-heavy scale)
- **Cache** — ใช้ Redis/Memcached ลดโหลด DB สำหรับ data ที่อ่านบ่อย
- **CDN (Content Delivery Network)** — เก็บ static assets ใกล้ user ลด latency
- **Stateless web tier** — ย้าย state (session) ไปอยู่ใน shared data store เช่น Redis
- **Multi data center** — active-active หรือ active-standby ป้องกัน disaster
- **Message queue** — decouple component สำหรับ async processing
- **Database sharding** — กระจายข้อมูลแบ่งตาม shard key (เช่น user_id)

หลักสำคัญ: เริ่มจาก simplest แล้วเพิ่ม complexity ตาม scale ที่เติบโต

---

## บทที่ 2 — การประมาณค่าแบบคร่าวๆ (Ch.2)

Back-of-the-envelope estimation คือการคำนวณโดยประมาณเพื่อตรวจว่า design ของเราสมเหตุสมผลหรือไม่:
- **Power of two**: 2^10 ≈ 1K, 2^20 ≈ 1M, 2^30 ≈ 1B — จำเป็นต้องจำ
- **Latency numbers**: L1 cache ~0.5ns, RAM ~100ns, SSD ~10-100μs, disk seek ~10ms, network call ~10-100ms
- **Availability**: 99.9% (3 nines) = downtime ~8.76 ชม./ปี, 99.99% = ~52 นาที/ปี, 99.999% = ~5 นาที/ปี
- **QPS estimation**: DAU 10 ล้าน, แต่ละ user 1 post/วัน → write QPS ~116, peak = ~580
- **Storage estimation**: 500M tweets/วัน, tweet ~140B + metadata ~30B → 85 TB/ปี

---

## บทที่ 3 — Framework สำหรับ Design Interview (Ch.3)

กรอบ 4 ขั้นตอนที่ใช้ได้กับทุกข้อสอบ:
1. **Understand the problem** — ถาม clarifying questions อย่าเดาเอง
2. **Propose high-level design** — วาด diagram อธิบาย main component
3. **Design deep dive** — โฟกัสที่ส่วนสำคัญที่สุด เช่น database model, API
4. **Wrap up** — สรุป trade-off, propose improvement

---

## บทที่ 4 — Design a Rate Limiter (Ch.4)

Rate limiter ป้องกันระบบจาก abuse และ DoS:
- **Client-side VS server-side** — server-side ปลอดภัยกว่า
- **อัลกอริทึม**: Token Bucket (อนุญาต burst), Leaky Bucket (rate ราบรื่น), Fixed Window (มี spike ที่ขอบ), Sliding Window Log (แม่นยำแต่ใช้หน่วยความจำเยอะ), Sliding Window Counter (สมดุลระหว่างแม่นยำกับประสิทธิภาพ)
- **Distributed rate limiter**: ใช้ Redis + Lua script เพื่อ atomic increment
- **Design** — middleware layer แยก rate limiter service, return HTTP 429 (Too Many Requests) พร้อม Retry-After header

---

## บทที่ 5 — Design Consistent Hashing (Ch.5)

เมื่อเราต้อง scale DB ผ่าน sharding, การเพิ่ม/ลด node ทำได้ยาก:
- **Naive hashing**: hash(key) % N — เมื่อ N เปลี่ยน เกือบทุก key ต้องย้าย
- **Consistent hashing**: จัด node และ key บนวงแหวน (hash ring) — เฉพาะ neighbor node เท่านั้นที่ได้รับผลกระทบ
- **Virtual nodes**: แต่ละ physical node มีหลาย virtual node บน ring — กระจายโหลดสม่ำเสมอขึ้น
- **ใช้กับ**: database sharding, CDN, load balancer

---

## บทที่ 6 — Design a Key-Value Store (Ch.6)

KV store (คล้าย Dynamo/Riak) ต้อง balance ระหว่าง CAP theorem — เลือก AP (availability + partition tolerance) ตามความเหมาะสม:
- **Data partitioning**: consistent hashing + virtual nodes
- **Data replication**: async replication ไปยัง N nodes ถัดไป
- **Consistency**: Quorum (W + R > N), Versioning (vector clock), Hinted handoff
- **Failure detection**: Gossip protocol + Phi Accrual failure detection
- **Persistence**: Write-ahead log (WAL) + SSTables + MemTable + Bloom filter
- **Anti-entropy**: Merkle tree ตรวจสอบ sync ระหว่าง replica

---

## บทที่ 7 — Design a Unique ID Generator (Ch.7)

ระบบ distributed ต้องสร้าง unique ID ที่ scalable:
- **UUID v4**: 128-bit, unique 100% (collision probability ต่ำมาก) แต่ไม่ sortable และใช้พื้นที่เยอะ
- **Snowflake approach (Twitter)**: 64-bit ID = timestamp (41 bits) + datacenter ID (5) + machine ID (5) + sequence (12) → sortable, เก็บใน int64 ได้
- **Ticket server**: ใช้ DB atomic auto-increment — bottleneck สูง
- **Multi-master replication**: auto-increment ด้วย step = N — scale ได้จำกัด

---

## บทที่ 8 — Design a URL Shortener (Ch.8)

URL shortener (เช่น TinyURL) ต้องแปลง long URL → short key แล้ว redirect ไว:
- **API**: POST /shorten (รับ longUrl) → GET /{shortKey} → HTTP 301 redirect
- **Key generation**: base64(unique_id) หรือ hash(longUrl) + base62 encode
- **Storage**: relational DB — id (PK), shortKey (unique), longUrl
- **Caching**: ใช้ Redis เก็บ recent/frequent key → ลดโหลด DB
- **Redirection**: 301 (permanent, browser cache) vs 302 (temporary, analytics ได้)

---

## บทที่ 9 — Design a Web Crawler (Ch.9)

Web crawler (คล้าย Googlebot) ต้องรวบรวมข้อมูลเว็บอย่างมีประสิทธิภาพ:
- **ลักษณะที่ดี**: scalability, politeness (ไม่โหลดเว็บเดียวหนักเกินไป), extensibility, robustness
- **Architecture**: URL frontier → DNS resolver → fetcher → content parser → content seen? → duplicate detection → URL extractor → URL filter → URL frontier
- **Politeness**: FIFO queue per host + delay ระหว่าง request
- **Priority**: PageRank, traffic, update frequency
- **Freshness**: Recrawl ตาม update history, จัดลำดับความสำคัญ
- **Duplicate detection**: Bloom filter + checksum สำหรับหน้าเว็บซ้ำ (~30% ของหน้าเว็บ)
- **Spider trap**: ตั้ง max URL length, manual blacklist

---

## บทที่ 10 — Design a Notification System (Ch.10)

Notification system รองรับ push (iOS/Android), SMS, และ Email:
- **iOS**: Provider → APNS → iOS device
- **Android**: Provider → FCM → Android device
- **SMS**: Twilio, Nexmo
- **Email**: SendGrid, Mailchimp
- **High-level design**: API server → notification server → message queues (แยกตามประเภท) → workers → third-party services
- **Deep dive**: template reuse, rate limiting, retry mechanism, dedup (event ID), notification setting per user, monitoring queued notifications

---

## บทที่ 11 — Design a News Feed (Ch.11)

News feed (Facebook/Instagram/Twitter timeline):
- **Fanout on write (push)**: คำนวณล่วงหน้าเมื่อมี post ใหม่ — โหลดสูงถ้า user มี friends เยอะ (hotkey)
- **Fanout on read (pull)**: คำนวณตอน user เปิด feed — latency สูง, ประหยัดทรัพยากรสำหรับ inactive user
- **Hybrid**: push สำหรับ user ทั่วไป, pull สำหรับ celebrity (หลีกเลี่ยง hotkey)
- **Cache architecture**: 5 ชั้น — News Feed IDs, Content (post), Social Graph, Actions (like/reply), Counters
- **CDN**: สำหรับ media content (images, videos)

---

## บทที่ 12 — Design a Chat System (Ch.12)

Chat system (WhatsApp/Messenger) รองรับ 50M DAU:
- **Protocol**: ใช้ WebSocket สำหรับ bidirectional real-time messaging (ไม่ใช้ polling/long polling)
- **Architecture**: Chat server (stateful, persistent connection), Presence server (online status), Notification server (push), API server (login/profile)
- **Storage**: Chat history ใช้ key-value store (เช่น Cassandra) เพื่อ low-latency read/write
- **Group chat (≤100 คน)**: sender → message queue → fanout ไป receivers
- **Online presence**: Heartbeat + last_seen_at อัปเดตทุก 5-30 วินาที
- **Multi-device**: เก็บ sequence number แยกตาม device เพื่อ sync

---

## บทที่ 13 — Design a Search Autocomplete (Ch.13)

Autocomplete system (Google Suggest):
- **Trie (prefix tree)**: เก็บ top N queries ในแต่ละ node
- **ต้องเร็ว**: ใช้ level order traversal แทน DFS, serialize cache สำหรับ restart
- **Scale**: Trie ใหญ่ → แบ่งตาม prefix (sharding) + top N cached ใน Redis
- **Update**: offline analytics → rebuild trie ทุก 1-2 สัปดาห์
- **Personalization**: เพิ่ม weight สำหรับ user-specific history

---

## บทที่ 14 — Design YouTube (Ch.14)

Video streaming platform:
- **Upload flow**: Load balancer → upload service → metadata DB + object store (BLOB) → transcoding queue → transcoder → CDN
- **Streaming flow**: CDN → client (adaptive bitrate — เลือก quality ตาม bandwidth)
- **Transcoding**: แปลงวิดีโอเป็นหลาย format/resolution, parallel processing
- **Metadata**: relational DB (views, likes, comments)
- **Processing queue**: แยก video queue (non-real-time, ~200ms/frame) กับ metadata queue (real-time)
- **Key metric**: CDN cost ~$0.02/GB, ยิ่ง popular ยิ่งต้องใช้ CDN

---

## บทที่ 15 — Design Google Drive (Ch.15)

Cloud file storage:
- **Upload**: File → Block server (แบ่งไฟล์เป็น blocks ~4MB) → object store (S3) → metadata DB
- **Download**: Client → API → metadata → CDN (ถ้า popular) หรือ direct download
- **Sync**: Delta sync (ส่งเฉพาะส่วนที่เปลี่ยน) + compression — ลด bandwidth 95%
- **Conflict resolution**: last-writer-wins หรือสร้าง conflicted copy
- **Notification**: WebSocket push เมื่อมีไฟล์เปลี่ยนแปลง (แจ้ง device อื่น)
- **Metadata DB**: ช้ากว่า object store แต่ใช้ query ที่ซับซ้อนได้ (search, permissions)

---

## บทที่ 16 — The Learning Continues (Ch.16)

คำแนะนำเพิ่มเติม:
- ฝึกออกแบบระบบบ่อยๆ — เริ่มจาก whiteboard, ไม่ต้องลงโค้ด
- ศึกษาจาก engineering blogs ของบริษัทใหญ่ (Netflix, Uber, Meta, Google, Amazon, Twitter, LinkedIn, Slack, Discord)
- เรียนรู้จากระบบ open-source เช่น Kafka, ZooKeeper, Cassandra, Redis, Nginx
- ไม่มีคำตอบถูกผิดตายตัว — trade-off สำคัญกว่า perfect design

---

## ตัวอย่างข้อสอบเพิ่มเติม (Supplementary — 15 Questions)

ข้อสอบเพิ่มเติมอีก 15 ข้อที่ใช้กรอบเดียวกัน:
1. Design a CDN
2. Design a parking garage
3. Design a video conference platform
4. Design a ride-sharing service (Uber/Lyft)
5. Design a social media analytics dashboard
6. Design a webhook system
7. Design a voting/kudos system
8. Design a food delivery app
9. Design a live comment system
10. Design a distributed email service
11. Design a digital wallet
12. Design a booking system
13. Design a search engine
14. Design a recommendation system
15. Design a stock exchange matching engine

ข้อสอบเหล่านี้ฝึกให้คิดถึง trade-off ระหว่าง latency, throughput, consistency, availability, และ cost ในทุกการตัดสินใจ

---
