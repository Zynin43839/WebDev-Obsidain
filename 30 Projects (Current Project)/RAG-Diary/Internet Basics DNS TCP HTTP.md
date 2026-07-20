---
type: concept
topic: networking
status: growing
created: 2026-06-30
---

# Internet Basics: DNS → TCP → HTTP → Client-Server

## What is this?

เวลาเราพิมพ์ `https://rag-diary.vercel.app` ใน browser แล้วกด Enter — เกิดอะไรขึ้นบ้างกว่าจะเห็นหน้าเว็บ?

เบื้องหลังมี stack protocol หลายชั้นทำงานร่วมกัน:
```
┌──────────────────────────────────────────────────┐
│  Layer 7: Application  │  HTTP/HTTPS             │
│  Layer 6: Presentation │  TLS/SSL (encryption)   │
│  Layer 5: Session      │  TLS Handshake          │
│  Layer 4: Transport    │  TCP (connection)       │
│  Layer 3: Network      │  IP (routing)           │
│  Layer 2: Data Link    │  Ethernet/Wi-Fi (MAC)   │
│  Layer 1: Physical     │  ไฟฟ้า/แสง/คลื่นวิทยุ   │
└──────────────────────────────────────────────────┘
```

4 ขั้นตอนหลักกว่าจะเห็นหน้าเว็บ:
1. **DNS Resolution** (Layer 7) — แปลง domain name → IP address
2. **TCP Connection** (Layer 4) — สร้าง reliable connection
3. **TLS Handshake** (ถ้า HTTPS) — เข้ารหัส
4. **HTTP Request/Response** (Layer 7) — ส่งคำขอ-รับคำตอบ

> **หมายเหตุ:** แต่ละ layer ไม่รู้จัก layer อื่น — HTTP ไม่รู้ว่าใช้ TCP หรือ UDP, TCP ไม่รู้ว่าส่ง HTTP หรือ FTP —这叫 **abstraction**

## Why This Matters

Internet ทำงานบน Stack นี้ทั้งหมด — web app ทุกตัว (RAG-Diary, Facebook, Netflix) ใช้ protocol เดียวกัน ถ้าไม่เข้าใจ จะงงเวลาเจอปัญหา:

- DNS ไม่ resolve (เข้าเว็บไม่ได้) — error `DNS_PROBE_FINISHED_NXDOMAIN`
- Connection timeout (server ไม่ตอบ) — error `ERR_CONNECTION_TIMED_OUT`
- HTTP status codes (404, 500) — backend error vs client error
- REST API design (GET/POST/PUT/DELETE) — ทำไมต้องแยก method
- CORS error — ทำไม browser บล็อก request
- WebSocket — ทำไม chat app ถึง connect ตลอดเวลา
- CDN — ทำไมเว็บถึงโหลดเร็วเวลาใช้ Cloudflare

---

## Part 1: OSI Model (7 Layers)

### Layer 1: Physical

ส่ง bits ผ่านสื่อทางกายภาพ — สายทองแดง (Ethernet), ใยแก้วนำแสง (Fiber), คลื่นวิทยุ (Wi-Fi, 5G)

### Layer 2: Data Link

จัดกลุ่ม bits เป็น **frames** + แก้ไข error — ใช้ **MAC address** (48-bit) ระบุ device ใน network เดียวกัน

```
Frame: [MAC src] [MAC dst] [Payload] [Checksum]
```

Protocol: Ethernet, Wi-Fi (802.11), PPP

### Layer 3: Network

Routing — ส่ง packet จาก network หนึ่งไปยังอีก network หนึ่ง — ใช้ **IP address** (32-bit IPv4 / 128-bit IPv6)

Protocol: IP (IPv4, IPv6), ICMP (ping), ARP (IP → MAC)

```
IP Packet: [IP src] [IP dst] [TTL] [Protocol] [Payload]
```

### Layer 4: Transport

เชื่อมต่อระหว่าง **application** (port-to-port) — 2 protocol หลัก:

| Feature | TCP | UDP |
|---------|-----|-----|
| Connection | มี handshake | ไม่มี |
| Reliability | ส่งครบแน่ | อาจหาย |
| Ordering | เรียงลำดับให้ | ไม่เรียง |
| Speed | ช้ากว่า | เร็วกว่า |
| ใช้กับ | HTTP, SSH, Email | DNS, Video, Gaming |

### Layer 5: Session

จัดการ session — เปิด/ปิด/resume connection

### Layer 6: Presentation

แปลงข้อมูล — encryption (TLS), compression, encoding (UTF-8)

### Layer 7: Application

Protocol ที่ application ใช้สื่อสาร — HTTP, FTP, SMTP, DNS, WebSocket

---

## Part 2: DNS — Domain Name System

### 2.1 What is DNS?

> **NotebookLM Analogy:** DNS is the **phone book of the internet** [1]. When you want to call a friend, you look up their name in your contacts to find their phone number. Similarly, humans prefer to use easy-to-remember domain names (like `rag-diary.vercel.app`), but computer network routers require numerical IP addresses (like `76.76.21.21`) to find and connect to servers [1]. Every time you type a URL and hit Enter, you ask DNS to "look up the number" for that name. [Source: NotebookLM]

DNS = **สมุดโทรศัพท์ของอินเทอร์เน็ต** — มนุษย์จำชื่อ (`rag-diary.vercel.app`) แต่ computer คุยกันด้วย IP address (`76.76.21.21`)

```
Browser  ── "rag-diary.vercel.app อยู่ที่ไหน?" ──→  DNS Resolver
         ←── "76.76.21.21" ──────────────────────────
```

### 2.2 DNS Hierarchy (ลำดับชั้น)

DNS ไม่ได้มี server เดียว — เป็นระบบลำดับชั้น:

```
Root (.) ──── 13 clusters ทั่วโลก
  │
  ├── .com TLD  (Verisign 管理)
  │     │
  │     └── vercel.app  (Authoritative NS)
  │           │
  │           └── rag-diary.vercel.app  → 76.76.21.21
  │
  ├── .org TLD
  ├── .th TLD
  └── .io TLD
```

**ลำดับ query (recursive resolution):**
```
1. Browser → Local DNS Resolver (ISP / 8.8.8.8)
2. Resolver → Root Server: ".com ใคร掌管?"
3. Root → TLD Server: "vercel.app อยู่กับ NS ไหน?"
4. TLD → Authoritative NS: "rag-diary.vercel.app = ?"
5. NS → Resolver: "A record = 76.76.21.21"
6. Resolver → Browser: 76.76.21.21 (cache ไว้ด้วย)
```

### 2.3 DNS Record Types

| Type | ใช้ทำอะไร | ตัวอย่าง |
|------|-----------|---------|
| **A** | domain → IPv4 address | `rag-diary.vercel.app → 76.76.21.21` |
| **AAAA** | domain → IPv6 address | `→ 2001:db8::1` |
| **CNAME** | domain → domain alias | `www.rag-diary.vercel.app → rag-diary.vercel.app` |
| **MX** | domain → mail server | `@ → mail.google.com priority 10` |
| **TXT** | ข้อความ (verify, SPF) | `v=spf1 include:_spf.google.com` |
| **NS** | domain → name server | `vercel.app → ns1.vercel-dns.com` |
| **SOA** | authority info | admin, serial, refresh interval |

### 2.4 DNS Caching (ทำไมครั้งที่ 2 เร็วกว่า)

```
[Caches: Browser → OS → Router → ISP DNS → TLD → Root]
         ms       10ms   50ms    100ms     200ms  500ms
```

**TTL (Time To Live):** แต่ละ record บอกว่า cache ได้กี่วินาที
- `A record TTL=300` → cache 5 นาที
- ถ้าเปลี่ยน IP ก่อน TTL หมด → propagation ยังไม่ทั่วถึง (DNS propagation delay)

### 2.5 DNS ใน RAG-Diary

| Domain | ใช้ทำอะไร | DNS |
|--------|-----------|-----|
| `rag-diary.vercel.app` | Frontend (Vercel) | A → Vercel edge IP |
| `localhost` | Development | /etc/hosts → 127.0.0.1 |
| `supabase.co` | Database | A → Supabase IP |

> **Tip:** เวลา local dev ใช้ `127.0.0.1` (loopback) — ไม่ต้องออก network เลย

**DNS Debug Commands:**
```bash
ping rag-diary.vercel.app          # หา IP
nslookup rag-diary.vercel.app      # DNS query
tracert rag-diary.vercel.app       # ดู route (Windows)
```

---

## Part 3: TCP — Transmission Control Protocol

### 3.1 What is TCP?

TCP = protocol ที่รับประกันว่าข้อมูลไปถึงปลายทาง **ครบถ้วน ถูกต้อง เรียงลำดับถูก**

**TCP vs UDP Analogy:**
- **TCP** = ส่งพัสดุลงทะเบียน (มี tracking number, เซ็นรับ, ถ้าหายส่งใหม่)
- **UDP** = ส่งโปสการ์ด (อาจหาย อาจมาผิดลำดับ, แต่ส่งไว)

### 3.2 Three-Way Handshake (建立连接)

> **NotebookLM Analogy:** Think of the 3-way handshake like establishing communication over a **two-way radio** [Source: NotebookLM]:
> 1. **SYN:** You say, "Hey Bob, this is Alice, do you copy?"
> 2. **SYN-ACK:** Bob replies, "Alice, this is Bob, I copy you. Do you copy?"
> 3. **ACK:** You respond, "Bob, this is Alice. I copy you. Communication established."
>
> After these three exchanges, both sides know the channel is open and ready for data. That's exactly what TCP does — three messages to confirm both sides are ready before sending any actual data.

ก่อนส่งข้อมูลต้องสร้าง connection ก่อน — 3 ขั้นตอน:

```
[Browser]                          [Server]
    │                                   │
    │  ── SYN (seq=100) ──────────────→ │  Step 1: Client ส่ง SYN (Synchronize)
    │                                   │         "ขอเชื่อมต่อ"
    │  ←─ SYN-ACK (seq=300, ack=101) ── │  Step 2: Server ตอบ SYN-ACK
    │                                   │         "OK, พร้อม connect"
    │  ── ACK (seq=101, ack=301) ───── → │  Step 3: Client ส่ง ACK
    │                                   │         "收到 开始发送数据"
    │  ====== CONNECTION ESTABLISHED ======
```

**Sequence Number (seq):**
- แต่ละ byte ของข้อมูลมี seq number
- `seq=100` → byte ที่ 100
- `ack=101` → "ได้รับถึง byte 100 แล้ว ขอ byte 101 ถัดไป"

**ทำไมต้อง 3-way?** — ป้องกัน **half-open connection** (connection ที่ server คิดว่าปิด แต่ client ยังเปิด)

### 3.3 Data Transfer (ส่งข้อมูล)

```
[Browser]                         [Server]
    │                                   │
    │  ── seq=101, len=100 ───────────→ │  ส่ง 100 bytes
    │  ←─ ack=201 ──────────────────── │  Server ยืนยันรับแล้ว
    │                                   │
    │  ── seq=201, len=50 ────────────→ │  ส่ง 50 bytes
    │  ←─ ack=251 ──────────────────── │  ยืนยันรับ
```

**Window Size:**
- Server บอก client ว่า "ส่งมาได้ทีละ X bytes"
- **Flow Control** — ป้องกัน client ส่งเร็วเกิน → server รับไม่ทัน
- **Congestion Control** — ป้องกัน network แออัด (TCP Slow Start)

### 3.4 TCP Slow Start (为什么网页刚打开慢)

TCP เริ่มส่งข้อมูลช้าๆ แล้วค่อยๆ เร็วขึ้น:

```
Request 1: ส่ง 1 segment  → รอ ack
Request 2: ส่ง 2 segments → รอ ack
Request 3: ส่ง 4 segments → รอ ack
Request 4: ส่ง 8 segments → รอ ack
...
```
- เริ่มต้น **congestion window (cwnd) = 10 segments** (~14KB)
- ทุกครั้งที่ได้ ack → cwnd double
- จนถึง threshold → slow start ends, congestion avoidance เริ่ม

> **NotebookLM ขยายความ:** TCP Slow Start คือ **กลไกป้องกัน network congestion** — TCP เริ่มส่งข้อมูลช้าๆ เพื่อ "วัดความกว้างของท่อ" ก่อนส่งเต็มที่ ถ้าส่งพรวดพราดตั้งแต่แรก อาจทำให้ router ตามไม่ทันและ packet หาย (congestion collapse) [Source: NotebookLM]
>
> **ผลต่อ web performance:** การเปิดหน้าเว็บที่ต้องโหลดหลายไฟล์ (CSS, JS, images) จะช้าในช่วงแรกเพราะแต่ละ request ต้องเริ่ม slow start ใหม่ HTTP/2 แก้ปัญหานี้ด้วย multiplexing — ใช้ 1 TCP connection ส่งหลายไฟล์พร้อมกัน (อยู่คนละ stream) ทำให้ slow start เกิดขึ้นแค่ครั้งเดียว

**ทำไม HTTP/1.1 ถึงช้า?** — ต้องรอ slow start สำหรับ request แรก, แล้วของในหน้า (CSS, JS, img) ต้องรอ request ใหม่ → HTTP/2 multiplexing ช่วยตรงนี้

### 3.5 Connection Termination (关闭连接)

```
[Browser]                          [Server]
    │                                   │
    │  ── FIN ────────────────────────→ │  Browser: "จะปิด connection"
    │  ←─ ACK ──────────────────────── │  Server: "รับทราบ"
    │  ←─ FIN ──────────────────────── │  Server: "ของฉันหมดแล้ว"
    │  ── ACK ────────────────────────→ │  Browser: "OK ปิดได้"
```

### 3.6 Ports

TCP ใช้ **port** เพื่อแยก application บนเครื่องเดียวกัน:

```
[Your Computer]                 [Server]
  Browser port 54321 ──────→ port 443 (HTTPS)
  Email client port 54322 ──→ port 587 (SMTP)
  Game port 54323 ─────────→ port 27015 (Game)
```

**Well-known ports:**
| Port | Service |
|------|---------|
| 20, 21 | FTP |
| 22 | SSH |
| 25 | SMTP |
| 53 | DNS |
| 80 | HTTP |
| 443 | HTTPS |
| 5432 | PostgreSQL |
| 3000 | RAG-Diary Backend (dev) |
| 5173 | RAG-Diary Frontend (Vite) |

---

## Part 4: TLS/SSL — HTTPS Security

### 4.1 ทำไมต้อง HTTPS?

HTTP ส่งข้อมูลเป็น **plaintext** — ใครก็อ่าน中间人攻击 (Man-in-the-Middle) ได้

```
❌ HTTP:  Browser ── "password=1234" ──→ [HACKER] ──→ Server
✅ HTTPS: Browser ── "加密乱码" ──────→ [HACKER sees乱码] ──→ Server
```

### 4.2 TLS Handshake (建立加密通道)

> **NotebookLM Analogy:** Think of TLS like **sending a locked box in the mail** [Source: NotebookLM].
> 1. You ask the server for its **public key** (lock) — this is like the server sending you an open padlock.
> 2. You put your message in a box, lock it with the server's padlock, and send it.
> 3. Only the server has the **private key** to unlock that padlock.
>
> Before any actual data is exchanged, the browser and server undergo a **handshake** where they negotiate encrypted keys and exchange certificates to establish a secure connection. This uses **asymmetric cryptography** — a public key to encrypt, a private key to decrypt. [Source: NotebookLM]
>
> The **Certificate Authority (CA)** is like a **passport office** — they verify the server's identity and issue a certificate that browsers trust. The certificate chain is: Root CA → Intermediate CA → Your Server, similar to how a passport office issues passports through authorized agents.
>
> **คำอธิบาย:** TLS = **ส่งของในกล่องที่ล็อคกุญแจ** — client ขอกุญแจล็อค (public key) จาก server → ใส่ข้อความในกล่อง → ล็อคด้วยกุญแจนั้น → ส่งไป — มีแต่ server ที่มีกุญแจไข (private key) เท่านั้นที่เปิดกล่องได้ แม้ของจะหลุดมือระหว่างทาง คนอื่นก็เปิดไม่ได้ CA (Certificate Authority) = **หน่วยงานออกพาสปอร์ต** — รับรองว่า "server นี้คือของจริง ไม่ใช่ของปลอม"

หลังจาก TCP handshake → TLS handshake:

```
[Browser]                           [Server]
    │                                      │
    │  ── ClientHello ───────────────────→ │  1. Browser: "ฉันรองรับ TLS 1.3, Cipher Suite XYZ"
    │  ←─ ServerHello + Cert ──────────── │  2. Server: "ใช้ TLS 1.3 + AES-GCM, นี่ certificate ฉัน"
    │  ←─ ServerHelloDone ─────────────── │  
    │  ── ClientKeyExchange ─────────────→ │  3. Browser: สร้าง pre-master secret
    │  ── ChangeCipherSpec ──────────────→ │     เข้ารหัสด้วย public key ของ server
    │  ── Finished ──────────────────────→ │  
    │  ←─ ChangeCipherSpec ────────────── │  4. Server: "OK ตอนนี้คุย encrypted แล้ว"
    │  ←─ Finished ────────────────────── │  
    │  ====== SECURE CONNECTION ============
```

### 4.3 Certificate Chain (为什么浏览器เชื่อถือ server)

Certificate Authority (CA) = **หน่วยงานรับรองตัวตน**

```
Root CA (built-in browser trust store)
  └── Intermediate CA
        └── Server Certificate (rag-diary.vercel.app)
```

**Process:**
1. Vercel ซื้อ certificate → CA ตรวจสอบว่า Vercel เป็นเจ้าของ domain จริง
2. CA เซ็น (sign) certificate ด้วย private key ของ CA
3. Browser มี public key ของ CA อยู่แล้ว (built-in)
4. Browser verify ลายเซ็น → trust ✅
5. ถ้า self-signed cert → browser ไม่ trust → warning ⚠️

### 4.4 HTTPS ใน RAG-Diary

| Environment | Protocol | Port |
|-------------|----------|------|
| Production (Vercel) | HTTPS | 443 |
| Local Backend | HTTP | 3000 |
| Local Frontend | HTTP | 5173 |
| Supabase | HTTPS | 443 |

---

## Part 5: HTTP — Hypertext Transfer Protocol

### 5.1 HTTP Message Structure

HTTP = **request-response protocol** — client ส่ง request, server ตอบ response

#### HTTP Request

```
GET /api/diary?tag=Learning HTTP/1.1      ← Request Line
Host: localhost:3000                       ← Headers
Authorization: Bearer eyJhbG...
Accept: application/json
User-Agent: Mozilla/5.0
                                           ← Blank line (separator)
(dody for POST/PUT)                        ← Body (optional)
```

**Request Line:** `METHOD path HTTP_VERSION`

#### HTTP Response

```
HTTP/1.1 200 OK                            ← Status Line
Content-Type: application/json             ← Headers
Content-Length: 245
Cache-Control: max-age=3600
Set-Cookie: session=abc123; HttpOnly
                                           ← Blank line
{"success": true, "data": [...]}           ← Body
```

**Status Line:** `HTTP_VERSION status_code status_text`

### 5.2 Headers Deep Dive

#### Request Headers

| Header | ใช้ทำอะไร | ตัวอย่าง |
|--------|-----------|---------|
| `Host` | target server (required ใน HTTP/1.1) | `Host: localhost:3000` |
| `Authorization` | authentication | `Bearer eyJhbGci...` |
| `Content-Type` | format ของ body | `application/json` |
| `Accept` | format ที่ client ต้องการ | `text/html, application/json` |
| `User-Agent` | browser identity | `Mozilla/5.0 (Windows NT 10.0)` |
| `Cookie` | session cookie | `session=abc123` |
| `Cache-Control` | caching policy | `no-cache` |
| `Origin` | source origin (ใช้ใน CORS) | `http://localhost:5173` |
| `Referer` | มาจากหน้าไหน | `http://localhost:5173/diary` |

#### Response Headers

| Header | ใช้ทำอะไร | ตัวอย่าง |
|--------|-----------|---------|
| `Content-Type` | format ของ response | `application/json; charset=utf-8` |
| `Content-Length` | ขนาด body (bytes) | `245` |
| `Cache-Control` | caching directive | `public, max-age=3600` |
| `Set-Cookie` | client เก็บ cookie นี้ | `session=abc123; HttpOnly; Secure` |
| `Access-Control-Allow-Origin` | CORS อนุญาต origin ไหน | `*` หรือ `http://localhost:5173` |
| `ETag` | version identifier | `"abc123"` |
| `Location` | redirect ไปที่ไหน | `/login` |

### 5.3 Status Codes — Full Taxonomy

> **NotebookLM Stories for Each Category** [Source: NotebookLM]:
>
> **1xx:** "I hear you, hold on a second." — Server acknowledges receipt but isn't done yet.
>
> **2xx:** "Here is exactly what you asked for! All done." — Everything worked perfectly.
>
> **3xx:** "The princess is in another castle. Go look over there." — The resource you want isn't here anymore; here's where it moved.
>
> **4xx:** "You messed up. You asked for something impossible, or you aren't allowed to have it." — Client-side error; retrying the same request won't help.
>
> **5xx:** "I messed up. I know what you want, but my system is broken right now." — Server-side error; retrying later might work.
>
> **คำอธิบาย:** Status codes 5 กลุ่มบอกผลลัพธ์ของ HTTP request:
> - **1xx** (Information) = รับทราบ — "ได้ยินแล้ว เดี๋ยวจัดให้" (rare)
> - **2xx** (Success) = ทำงานได้ — "ได้ตามที่ขอเลย!" เช่น 200 OK
> - **3xx** (Redirect) = ย้ายที่ — "ของไม่ได้อยู่ที่นี่แล้ว ไปดูที่นี่" เช่น 301 Moved
> - **4xx** (Client Error) = client พัง — "คุณผิดเอง" เช่น 404 Not Found
> - **5xx** (Server Error) = server พัง — "ผมผิดเอง" เช่น 500 Internal Error

#### 1xx Informational (信息性)

| Code | ความหมาย | ใช้เมื่อ |
|------|----------|---------|
| 100 | Continue | client ส่ง header ก่อน, server ว่า OK ส่ง body ต่อ |
| 101 | Switching Protocols | upgrade connection (HTTP → WebSocket) |

#### 2xx Success (成功)

| Code | ความหมาย | ใช้เมื่อ |
|------|----------|---------|
| 200 | OK | GET, PUT, PATCH สำเร็จ |
| 201 | Created | POST สำเร็จ — สร้าง resource ใหม่ |
| 202 | Accepted | request รับแล้ว แต่ยังไม่เสร็จ (async job) |
| 204 | No Content | DELETE สำเร็จ — ไม่มี body |

#### 3xx Redirection (重定向)

| Code | ความหมาย | ใช้เมื่อ |
|------|----------|---------|
| 301 | Moved Permanently | URL เปลี่ยนถาวร (browser cache ใหม่) |
| 302 | Found (Temporary Redirect) | URL เปลี่ยนชั่วคราว |
| 304 | Not Modified | ใช้กับ ETag — content ไม่เปลี่ยน ใช้ cache ได้ |
| 307 | Temporary Redirect | เหมือน 302 แต่ method ไม่เปลี่ยน |
| 308 | Permanent Redirect | เหมือน 301 แต่ method ไม่เปลี่ยน |

#### 4xx Client Error (客户端错误)

| Code | ความหมาย | ใช้เมื่อ |
|------|----------|---------|
| 400 | Bad Request | JSON malformed, validation fail |
| 401 | Unauthorized | ต้อง login ก่อน |
| 403 | Forbidden | login แล้ว แต่ไม่มีสิทธิ์ |
| 404 | Not Found | resource ไม่มี |
| 405 | Method Not Allowed | ส่ง POST ไป endpoint ที่รับแค่ GET |
| 408 | Request Timeout | client ส่ง request ไม่ทัน |
| 409 | Conflict | สร้าง resource ซ้ำ (duplicate) |
| 413 | Payload Too Large | body ใหญ่เกิน limit |
| 429 | Too Many Requests | rate limit exceeded |

#### 5xx Server Error (服务器错误)

| Code | ความหมาย | ใช้เมื่อ |
|------|----------|---------|
| 500 | Internal Server Error | error ทั่วไปที่ server |
| 502 | Bad Gateway | upstream server (เช่น database) ตอบ error |
| 503 | Service Unavailable | server overload / maintenance |
| 504 | Gateway Timeout | upstream server ไม่ตอบทัน |

### 5.4 HTTP Methods — Beyond CRUD

| Method | ความหมาย | Idempotent? | Safe? | มี Body? |
|--------|----------|-------------|-------|----------|
| **GET** | อ่านข้อมูล | ✅ | ✅ | ❌ |
| **HEAD** | เหมือน GET แต่ไม่ส่ง body (check headers อย่างเดียว) | ✅ | ✅ | ❌ |
| **POST** | สร้าง / submit | ❌ | ❌ | ✅ |
| **PUT** | แทนที่ resource ทั้งหมด | ✅ | ❌ | ✅ |
| **PATCH** | อัปเดตบางส่วน | ❌ | ❌ | ✅ |
| **DELETE** | ลบ resource | ✅ | ❌ | ❌ |
| **OPTIONS** | ถามว่า server รองรับ method อะไรบ้าง | ✅ | ✅ | ❌ |

> **Safe** = ไม่เปลี่ยน state บน server (read-only)
> **Idempotent** = request N ครั้ง → result เดียวกัน

### 5.5 HTTP Versions Evolution

| Version | ปี | จุดเด่น | ปัญหา |
|---------|-----|---------|-------|
| **HTTP/0.9** | 1991 | แค่ GET, HTML เท่านั้น | เรียบง่ายเกิน |
| **HTTP/1.0** | 1996 | GET/POST/HEAD, headers, status codes | 1 request = 1 TCP connection |
| **HTTP/1.1** | 1999 | Keep-Alive, chunked transfer, cache headers | Head-of-line blocking |
| **HTTP/2** | 2015 | Multiplexing, server push, header compression | TCP-level HoL blocking |
| **HTTP/3** | 2022 | Use QUIC (UDP-based), zero RTT | ต้องใช้ QUIC |

#### Head-of-Line Blocking (ทำไม HTTP/1.1 ช้า)

```
HTTP/1.1 (1 connection = 1 request at a time):
[Request 1  ████████████████..........................]
[Request 2  .................████████████████.........]
[Request 3  ................................██████████]
Total time = sum ของทุก request

HTTP/2 (1 connection = multiple streams):
[Request 1  ████████████████]
[Request 2  ████████████████] ← parallel!
[Request 3  ████████████████] ← parallel!
Total time = longest request เท่านั้น
```

**ข้อเสีย HTTP/2:** ถ้า 1 packet หายทั้ง TCP stream รอ — HTTP/3 ใช้ UDP (QUIC) แก้ปัญหานี้

> **NotebookLM เรียบเรียงให้เข้าใจง่ายขึ้น** [Source: NotebookLM]:
> - **HTTP/1.1** — สั่งของทีละชิ้น รอชิ้นแรกมาก่อนถึงสั่งชิ้นต่อไปได้ (1 connection = 1 request at a time)
> - **HTTP/2** — สั่งของพร้อมกันหลายชิ้นใน 1 สายพาน (multiplexing via streams) แต่ถ้าชิ้นนึงตก สายพานทั้งเส้นหยุด (TCP head-of-line blocking)
> - **HTTP/3** — สั่งของพร้อมกันหลายชิ้น แต่ละชิ้นใช้สายพานแยกกัน (QUIC over UDP) — ชิ้นนึงตก เฉพาะชิ้นนั้นที่รอ

### 5.6 Content Negotiation (การเจรจารูปแบบ)

Client บอก server ว่าต้องการ format อะไรผ่าน `Accept` header:

```
Client sends:
  Accept: application/json, text/html; q=0.9

Server responds with:
  Content-Type: application/json
```

`q` = quality value (0-1) — priority

### 5.7 Caching Strategies

> **NotebookLM Analogy:** Imagine you have a niece who constantly asks you, "How far is the Moon?" [Source: NotebookLM]
> - The first time she asks, you have to Google it — takes time and effort
> - After she asks the same question repeatedly, you get tired of searching. So you write the answer on a sticky note and put it on the fridge.
> - Next time she asks, you just point to the sticky note — instant!
>
> **That's HTTP caching.** The first request goes to the server (slow), the response gets cached (sticky note), and subsequent requests are served from the cache (fast). The `Cache-Control` header is like telling the sticky note "this answer is good for 1 hour" (max-age=3600). After that, you check the server again to see if the answer changed.
>
> **คำอธิบาย:** Caching = **การจดคำตอบไว้บน sticky note** — ครั้งแรกถาม server (ช้า) → จำคำตอบไว้ (cache) → ครั้งต่อไปตอบจาก sticky note ได้เลย (เร็ว) โดยไม่ต้องถาม server ซ้ำ ถ้าคำตอบเก่าเกิน (`max-age` หมด) ก็ค่อยถาม server อีกครั้ง HTTP caching ช่วยลด request ไป server และทำให้หน้าเว็บโหลดเร็วขึ้นมาก

### 5.7 Caching Strategies

```
Browser Cache:
  First visit:  GET /diary → status 200 + Cache-Control: max-age=3600
  Second visit: ใช้ cache (ไม่ต้อง request) → instant!
  After 1 hour: GET /diary → server อาจตอบ 304 Not Modified (ใช้ ETag)

ETag = version hash:
  Request:  GET /diary, If-None-Match: "abc123"
  Response: 304 Not Modified (ถ้า content ไม่เปลี่ยน)
            200 OK + new ETag (ถ้า content เปลี่ยน)
```

### 5.8 Cookies: Session Management

Cookies = **ข้อมูลที่ browser เก็บไว้** และส่งไปกับทุก request ที่ domain นั้น

```
Server → Set-Cookie: session=eyJsb2dpbmVkIjp0cnVlfQ; HttpOnly; Secure; SameSite=Lax
Browser → Cookie: session=eyJsb2dpbmVkIjp0cnVufQ

Cookie Flags:
  HttpOnly  → JavaScript อ่าน cookie ไม่ได้ (ป้องกัน XSS)
  Secure    → ส่งผ่าน HTTPS เท่านั้น
  SameSite  → ป้องกัน CSRF (Lax = default, Strict = เข้ม, None = 不จำกัด)
```

**Cookie vs localStorage vs sessionStorage:**

| | Cookie | localStorage | sessionStorage |
|--|--------|-------------|----------------|
| Size | 4KB | 5-10MB | 5-10MB |
| ส่งไป server ทุก request? | ✅ | ❌ | ❌ |
| Expire | กำหนดได้ | Manual delete | ปิด tab = ตาย |
| Access | Server + Client | Client only | Client only |

---

## Part 6: Client-Server Model

### 6.1 2-Tier Architecture

```
[Client (Browser)] ←────HTTP────→ [Server (Backend)]
                                      │
                                      └─── [Database]
```

RAG-Diary เป็น 2-tier:
```
[React @ localhost:5173] ←──→ [Elysia @ localhost:3000] ←──→ [Supabase]
```

### 6.2 Stateless vs Stateful

**REST = Stateless** — server ไม่จำ state ของ client:
```
✅ Stateless:
  Request 1: POST /api/login {user, pass} → token
  Request 2: GET /api/diary (Authorization: Bearer token)
  Server: "request 2 = request 2 ไม่รู้ว่าก่อนหน้านี้ login มา"

❌ Stateful:
  Server จำว่า "user A login แล้ว" → request 2 ไม่ต้องส่ง token
  ปัญหา: scale ไม่ได้ (server B จำไม่ได้ว่า A login)
```

### 6.3 CORS — Cross-Origin Resource Sharing

> **NotebookLM Analogy:** CORS is like a **VIP list at a club** [Source: NotebookLM].
> - **Same-Origin Policy** = the bouncer's default rule: "Only people from this club can enter." Scripts from other origins (other clubs) can't read responses from your server.
> - **CORS** = the club manager giving the bouncer a list of exceptions: "People from `http://localhost:5173` are allowed in too."
> - The browser enforces this — not the server. The server just returns the guest list (CORS headers), and the browser bouncer checks it.
>
> That's why **curl doesn't have CORS issues** — curl doesn't have a bouncer. It just makes the request and shows you the response. The Same-Origin Policy is a browser security feature, not a server or network rule.

**Why CORS?** — Browser block cross-origin request โดย default (Same-Origin Policy)

```
Same-Origin:
  http://localhost:5173  ──→  http://localhost:5173/api  ✅

Cross-Origin:
  http://localhost:5173  ──→  http://localhost:3000/api  ❌
  (origin ต่างกัน — port ต่าง → block)
```

**Simple Request (GET/POST with standard headers):**
```
[Client]                          [Server]
  POST /api/diary
  Origin: http://localhost:5173 ──→
  Content-Type: application/json
  
  ←── Access-Control-Allow-Origin: http://localhost:5173
  ←── 200 OK
```

**Preflight Request (PUT/DELETE/custom headers):**
```
[Client]                          [Server]
  OPTIONS /api/diary              ← Preflight
  Origin: http://localhost:5173 ──→
  Access-Control-Request-Method: PUT
  
  ←── Access-Control-Allow-Origin: http://localhost:5173
  ←── Access-Control-Allow-Methods: GET, POST, PUT, DELETE
  ←── 204 No Content
  
  PUT /api/diary/abc              ← Actual request
  Origin: http://localhost:5173 ──→
  
  ←── 200 OK
```

### 6.4 Reverse Proxy & CDN

```
[Client] ──→ [CDN / Reverse Proxy (Cloudflare, Nginx)]
                   │
              ┌────┴────┐
           [Static Files]  ──→ [Dynamic Server]
```

- **CDN** — cache static files (HTML, CSS, JS, images) ใกล้ client
- **Reverse Proxy** — load balancing, SSL termination, rate limiting
- RAG-Diary on Vercel = built-in CDN + edge network

### 6.5 WebSocket — Real-time Connection

HTTP = request-response (client ต้อง request ก่อน server ถึงตอบได้)

WebSocket = **bidirectional** — server ส่งหาลูกค้าได้เอง (push)

```
Upgrade: HTTP → WebSocket:
  GET /ws HTTP/1.1
  Upgrade: websocket
  Connection: Upgrade
  ├── 101 Switching Protocols ──→
  
  [Now bidirectional]
  Client → Server: "new message"
  Server → Client: "new notification" (push!)
```

**ใช้กับ:** Chat, real-time notifications, live updates

---

## Part 7: Real-world Debugging

### 7.1 Packet Flow Analysis

เวลา `POST /api/diary` ใน RAG-Diary:

```
1. [DNS]      localhost:5173 → DNS: localhost → 127.0.0.1 (จาก /etc/hosts)
2. [TCP]      127.0.0.1:5173 → SYN → 127.0.0.1:3000
3. [TCP]       ← SYN-ACK →
4. [TCP]      → ACK (connected)
5. [TLS]      ข้าม (HTTP, ไม่ใช่ HTTPS)
6. [HTTP]     POST /api/diary HTTP/1.1
              Host: localhost:3000
              Content-Type: application/json
              {"content":"Hello","tag":"Learning"}
7. [Server]   Elysia รับ → Supabase INSERT → embedding
8. [HTTP]     HTTP/1.1 201 Created
              Content-Type: application/json
              {"success":true,"data":{...}}
```

### 7.2 Common Issues

| Symptom | Likely Cause | Check |
|---------|-------------|-------|
| `ERR_NAME_NOT_RESOLVED` | DNS ไม่ resolve | `nslookup domain.com` |
| `ERR_CONNECTION_TIMED_OUT` | Firewall / Server down | `ping server` |
| `ERR_CONNECTION_REFUSED` | Port not open | `telnet localhost 3000` |
| `CORS error` | Missing CORS headers | check server CORS config |
| `502 Bad Gateway` | Backend → DB dead | check Supabase status |
| `504 Gateway Timeout` | Backend ตอบช้า | check query performance |
| `413 Payload Too Large` | Body ใหญ่เกิน | check server body limit |
| `429 Too Many Requests` | Rate limit | wait + retry |

---

## Question to Think About

1. **TCP Slow Start:** ถ้า cwnd = 10 segments (14KB) และ RTT = 100ms — ต้องใช้กี่ round trip กว่าจะส่ง 1MB ได้?
2. **DNS Propagation:** ถ้าคุณเปลี่ยน IP server แล้วตั้ง TTL = 86400 (24 ชม.) — user จะเห็นเว็บเก่านานแค่ไหน?
3. **HTTP/2 Multiplexing:** ทำไมถึงไม่ทำให้ request เร็วขึ้น 10x ทั้งๆ ที่ส่ง parallel ได้?
4. **Statelessness:** ถ้า server stateless จริง — แล้วระบบ login ทำงานยังไง? (คำใบ้: JWT vs Session)
5. **CORS:** ทำไม `curl` ถึงไม่โดน CORS block แต่ browser โดน?
6. **TLS:** ถ้า Root CA certificate ของ browser หมดอายุ — เกิดอะไรขึ้นกับทุก HTTPS website?

## Links

- Related concept: [[REST API JSON]], [[HTTP Methods]], [[RAG Flow]]
- Code ref: [[server/src/index.ts]], [[server/src/lib/db.ts]]
- Source: Alex Xu — System Design Interview (NotebookLM), Traversy Media
- Q&A from: Socratic session 2026-06-30
