# Database Internals
> **Author:** Alex Petrov

# Database Internals — สรุปหนังสือโดย Alex Petrov

## ส่วนที่ 1: Storage Engines (พื้นฐานของ Database ที่เก็บข้อมูลอย่างไร)

### บทที่ 1: DBMS Architecture (สถาปัตยกรรมของ DBMS)

#### ทั่วไป (General)
- Database Management System (DBMS) คือระบบที่จัดการข้อมูลและให้บริการเข้าถึงข้อมูล
- Client ส่ง query ผ่าน Parser ไปยัง Query Planner แล้วส่งไปยัง Execution Engine
- Execution Engine ดึงข้อมูลจาก Storage Engine แล้วส่งกลับไปที่ Client
- ทุกชั้น (layer) ทำงานอิสระและสื่อสารผ่าน interface (API)

#### Table-Oriented vs Column-Oriented (Table vs Column store)
- Table-Oriented (row store) เก็บข้อมูลทีละ row — เหมาะกับ OLTP (Online Transaction Processing) ที่ต้องการอ่าน/เขียน record ทีละตัว
- Column-Oriented (column store) เก็บข้อมูลทีละคอลัมน์ — เหมาะกับ OLAP (Online Analytical Processing) ที่ต้องคำนวณ aggregation บนหลาย record

#### Mutable vs Immutable Data Structures
- Mutable: แก้ไขข้อมูลเดิมโดยตรง (เช่น B-Tree)
- Immutable: สร้างเวอร์ชันใหม่แทนการแก้ไขเดิม (เช่น LSM Tree)
- Mutable เหมาะกับ workload ที่อ่านมาก寫入น้อย; Immutable เหมาะกับ workload ที่เขียนมาก

#### Ordered vs Unordered
- Ordered: เก็บข้อมูลเรียงตาม key (B-Tree) — ค้นหา range ได้เร็ว
- Unordered: เก็บข้อมูลแบบ random (Hash Index) — ค้นหาแบบ equality ได้เร็ว

---

### บทที่ 2: Storage & Index Structures (โครงสร้างการเก็บข้อมูลและ index)

#### B-Trees
- B-Tree คือ balanced tree ที่เก็บข้อมูลเรียงตาม key
- Leaf node เก็บ key-value pairs, internal node เก็บ key สำหรับนำทาง
- B+ Tree คือ B-Tree ที่ leaf node เชื่อมกันเป็น linked list — เหมาะกับ range query
- Write amplification ใน B-Tree: เขียนทีเดียวแต่เกิดหลาย I/O (page split, metadata)

#### LSM Trees (Log-Structured Merge Trees)
- LSM Tree เก็บข้อมูลแบบ append-only (immutable)
- Memtable ในหน่วยความจำ (memory) → Flush ไป Disk เป็น SSTable (Sorted String Table)
- SSTable เก็บข้อมูลเรียงตาม key, immutable — ต้อง compaction เป็นประจำ
- ข้อดี: เขียนเร็ว (sequential write); ข้อเสีย: read amplification (ต้องค้นหลายชั้น)

#### Bloom Filters
- Bloom filter คือ probabilistic data structure สำหรับทดสอบว่า key มีอยู่หรือไม่
- False positive ได้ (บอกว่ามีทั้งที่ไม่มี) แต่ false negative ไม่ได้ (บอกว่าไม่มีต้องไม่มีจริง)
- ใช้ใน LSM Tree เพื่อลด disk I/O เมื่อค้นหา key ที่ไม่มีอยู่จริง

---

### บทที่ 3: Page Layout (ผังหน้าของข้อมูล)

#### Records & Pages
- Record คือข้อมูลหนึ่งตัว; Page คือหน่วยพื้นฐานที่อ่าน/เขียนจาก disk
- Page มีขนาดคงที่ (fixed-size) เช่น 4KB หรือ 8KB

#### Slotted Pages
- Slotted page คือ page ที่มี slot array (header) เรียงตาม key — แต่ละ slot ชี้ไปยัง cell (record) ใน page
- Cell จัดเรียงแบบ compact (มี fragmentation น้อย)
- Page header จัดเก็บ metadata เช่น number of cells, free space, pointers

#### Cell Pointer Pages
- Cell pointer page คือ page ที่เก็บ pointer ไปยัง cell ในหลาย page — ช่วยให้ค้นหาข้อมูลได้เร็วขึ้น

---

### บทที่ 4: Data Recovery & Catalogs (การกู้คืนข้อมูลและ catalog)

#### Write-Ahead Logging (WAL)
- WAL คือ log ที่บันทึกทุกการเปลี่ยนแปลงก่อนเขียนลง data page
- ถ้า system ล่ม → สามารถกู้คืนจาก WAL ได้ (recovery from log)

#### ARIES (Algorithm for Recovery and Isolation Exploiting Semantics)
- ARIES คือ recovery algorithm ที่ใช้ใน database ส่วนใหญ่
- ขั้นตอน: Analysis → Redo → Undo
- Analysis: อ่าน WAL เพื่อกำหนดLSN (Log Sequence Number) เริ่มต้น
- Redo: เล่น WAL จากจุดเริ่มต้นเพื่อกู้คืนข้อมูล
- Undo: ย้อนกลับ transaction ที่ไม่ complete

#### Metadata Storage (Catalogs)
- Database เก็บ metadata (schema, table definitions) ใน catalog
- Catalog มักเก็บใน internal table (เช่น system catalog) — อ่านเหมือน table ปกติ

#### Checkpoints
- Checkpoint คือจุดที่ database flush ข้อมูลทั้งหมดลง disk
- ช่วยลดเวลา recovery (ไม่ต้องเล่น WAL ทั้งหมด)

---

### บทที่ 5: Buffer Management (การจัดการ buffer)

#### Page Cache
- Page cache คือ buffer ในหน่วยความจำสำหรับเก็บ page ที่อ่านบ่อย
- ลด disk I/O โดยให้ข้อมูลอยู่ในหน่วยความจำ

#### Eviction Policies (นโยบายการลบ page เมื่อ cache เต็ม)
- **Clock**: หมุนเข็มไปเรื่อยๆ แล้วลบ page ที่ไม่ได้ใช้ (คล้าย Second-Chance)
- **LRU (Least Recently Used)**: ลบ page ที่ไม่ได้ใช้นานที่สุด
- **LIRS (Low Interreference Recency Set)**: ลบ page ที่ interreference recency ต่ำ — ดีกว่า LRU ในบาง workload

#### Multi-Database Caching
- Database หลายตัวใช้ page cache เดียวกัน → ต้องจัดการ eviction อย่างระมัดระวัง (เช่น shared buffer pool)

---

### บทที่ 6: Transaction Processing & ACID (การประมวลผล transaction และ ACID)

#### Transactions
- Transaction คือ unit of work ที่ทำหลายขั้นตอน — ต้อง complete ทั้งหมดหรือไม่เลย (atomicity)

#### ACID Properties
- **Atomicity**: transaction ทำสำเร็จทั้งหมดหรือล้มเหลวทั้งหมด
- **Consistency**: ข้อมูลต้องอยู่ใน valid state หลังจาก transaction (遵守 constraints)
- **Isolation**: transaction ทำงาน parallel โดยไม่รบกวนกัน (visibility rules)
- **Durability**: ข้อมูลที่เขียนสำเร็จต้องอยู่รอดแม้ system ล่ม

#### Write Skew
- Write skew คือ serialization anomaly ที่ transaction A อ่านค่า X แล้วเขียน Y; transaction B อ่าน Y แล้วเขียน X — ผลลัพธ์สุดท้ายอาจผิดปกติ

---

### บทที่ 7: Concurrency Control (การควบคุม concurrency)

#### MVCC (Multi-Version Concurrency Control)
- MVCC เก็บหลาย version ของ record — แต่ละ transaction เห็น snapshot ของตัวเอง
- ข้อดี: อ่านไม่บล็อกเขียน, เขียนไม่บล็อกอ่าน

#### Locks vs Latches
- **Lock**: ใช้กับ logical objects (table, row) — ป้องกัน transaction อื่นเข้าถึง
- **Latch**: ใช้กับ physical structures (page, memory) — ป้องกัน thread อื่นเข้าถึง cùng lúc

#### SSI (Serializable Snapshot Isolation)
- SSI คือ isolation level ที่ป้องกัน serialization anomaly โดยใช้ dependency tracking
- ตรวจจับ write skew และ abortion transaction ที่ conflict

#### Serialization Anomalies
- Write skew (กล่าวแล้วในบทที่ 6)
- Phantoms: transaction อ่าน row บางตัว, transaction อื่น insert row ใหม่ในช่วงเดียวกัน → ผลลัพธ์อาจต่างกัน

---

### บทที่ 8: Log-Structured Storage (การเก็บข้อมูลแบบ log-structured)

#### LSM Trees (เพิ่มเติมจากบทที่ 2)
- LSM Tree ใช้ Memtable (memory) → Flush เป็น SSTable (disk)
- SSTable เรียงตาม key, immutable — ต้อง compaction เป็นประจำ

#### Compaction Strategies
- **Size-Tiered Compaction**: compaction SSTable ขนาดใกล้กัน — เหมาะกับ workload ที่เขียนมาก
- **Leveled Compaction**: compaction SSTable ทีละ level — ลด read amplification แต่เพิ่ม write amplification

#### Bitcask
- Bitcask คือ storage engine ที่เก็บ key-value pairs แบบ append-only
- Key → file offset mapping เก็บใน hash index (memory)
- ข้อดี: เขียนเร็ว; ข้อเสีย: ต้อง compaction เพื่อลบ stale data

#### WiscKey
- WiscKey คือ LSM Tree ที่แยก key (memory) ออกจาก value (disk)
- ข้อดี: ลด write amplification; ข้อเสีย: ต้อง garbage collection สำหรับ value

---

## ส่วนที่ 2: Distributed Database Clusters (ระบบฐานข้อมูลแบบกระจาย)

### บทที่ 9: Introduction to Distributed Systems (แนะนำ Distributed Systems)

#### Network Model
- **Synchronous network**: message ส่งถึงทันที (ไม่มี delay)
- **Asynchronous network**: message อาจ delay, หาย, หรือ duplicate

#### Failure Models
- **Crash-stop**: node หยุดทำงานถาวร (ไม่ recover)
- **Crash-recovery**: node หยุดทำงานชั่วคราวแล้ว recover
- **Byzantine**: node ทำงานผิดปกติ (ส่งข้อมูลเท็จ)

#### CAP Theorem
- **Consistency**: ทุก node เห็นข้อมูลเดียวกัน
- **Availability**: ทุก request ได้ response
- **Partition tolerance**: ระบบทำงานได้แม้ network partition
- CAP theorem บอกว่าเลือกได้แค่ 2 ใน 3 (C, A, P) — แต่ในความจริง network partition เกิดขึ้นได้ → ต้องเลือก CP หรือ AP

---

### บทที่ 10: Failure Detection (การตรวจจับความล้มเหลว)

#### Gossip Protocol
- Gossip คือ protocol ที่ node แลกเปลี่ยนข้อมูลกับ node อื่นแบบ random — ช่วยให้ข้อมูลแพร่กระจายเร็ว

#### Phi-Accrual Failure Detection
- Phi-accrual failure detector คำนวณ probability ที่ node จะ crash — ไม่ใช่ binary (alive/dead) แต่เป็น score
- ใช้สำหรับตัดสินใจว่าจะส่ง request ไปที่ node หรือไม่

---

### บทที่ 11: Consensus (การบรรลุฉันทามติ)

#### Paxos
- Paxos คือ protocol สำหรับบรรลุฉันทามติใน distributed system
- มี 3 -role: Proposer, Acceptor, Learner
- Phase 1: Proposer ส่ง Prepare → Acceptor ตอบ promise
- Phase 2: Proposer ส่ง Accept → Acceptor ตอบ accepted

#### Basic Paxos vs Multi-Paxos
- Basic Paxos: ตกลงค่าเดียว (single value)
- Multi-Paxos: ตกลงค่าหลายตัว (sequence of values) — ใช้ใน log replication

#### Generalized Paxos
- Generalized Paxos อนุญาตให้ commutative operations ทำงาน parallel (เช่น increment A, increment B)

---

### บทที่ 12: Replication (การทำ replication)

#### Leader-Follower Replication
- Leader รับ write, Follower รับ read
- Follower sync จาก leader ผ่าน log replication
- ถ้า leader crash → เลือก follower ใหม่เป็น leader (leader election)

#### Multi-Leader Replication
- หลาย node เป็น leader พร้อมกัน — เขียนได้จากหลายที่
- ต้อง resolve conflict (เช่น last-write-wins, vector clocks)

#### Leaderless Replication
- ไม่มี leader — client ส่ง request ไปหลาย node พร้อมกัน
- ใช้ quorum (W + R > N) เพื่อรับประกัน consistency

#### Quorum
- Quorum คือจำนวน node ที่ต้องตอบสนองเพื่อให้ operation สำเร็จ
- ถ้า W + R > N → อ่านได้ค่าล่าสุดเสมอ (strong consistency)

---

### บทที่ 13: Consistent Hashing (การทำ consistent hashing)

#### Virtual Nodes
- Virtual node คือ node เสมือนหลายตัวที่แทน physical node เดียว — ช่วยกระจายข้อมูลสม่ำเสมอ

#### Key Range
- Key range คือช่วงของ key ที่ assign ให้แต่ละ node — เปลี่ยนแปลงได้ (rebalancing)

#### Token Ring
- Token ring คือ circular hash space ที่ assigns key ranges ให้แต่ละ node

---

### บทที่ 14: Partitioning (การ partition ข้อมูล)

#### Sharding
- Sharding คือการแบ่งข้อมูลเป็นหลาย partition (shard) บนหลาย node
- แต่ละ shard เก็บ subset ของข้อมูล

#### Consistent Hashing (เพิ่มเติมจากบทที่ 13)
- Consistent hashing ช่วยลดข้อมูลที่ต้องย้ายเมื่อเพิ่ม/ลบ node

#### Range-Based Partitioning
- Range-based: แบ่งข้อมูลตามช่วงของ key (เช่น A-M → node 1, N-Z → node 2)
- ข้อดี: range query เร็ว; ข้อเสีย: hotspot (node ที่เก็บ key ที่เขียนบ่อย)

#### Hash-Based Partitioning
- Hash-based: ใช้ hash function เพื่อกำหนด shard — กระจายข้อมูลสม่ำเสมอ
- ข้อเสีย: range query ต้องค้นหลาย shard

---

### บทที่ 15: Transactions (การจัดการ transaction แบบกระจาย)

#### Two-Phase Commit (2PC)
- 2PC มี 2 phase: Prepare → Commit
- Phase 1 (Prepare): Coordinator ส่ง prepare → participants ตอบ yes/no
- Phase 2 (Commit): ถ้าทุก参与者ตอบ yes → coordinator ส่ง commit
- ปัญหา: blocking protocol (ถ้า coordinator crash → participants รอ)

#### Three-Phase Commit (3PC)
- 3PC เพิ่ม phase pre-commit เพื่อลด blocking
- แต่ใน practice ไม่ได้ใช้มาก (network partition ยังทำให้ inconsistent)

#### Paxos Commit
- Paxos Commit ใช้ Paxos protocol สำหรับ commit — ไม่ blocking

---

### บทที่ 16: Consistency & Replication (Consistency และ Replication)

#### CAP Theorem (เพิ่มเติมจากบทที่ 9)
- CP system: เลือก consistency มากกว่า availability (เช่น ZooKeeper)
- AP system: เลือก availability มากกว่า consistency (เช่น Cassandra)

#### ACID vs BASE
- ACID: Atomicity, Consistency, Isolation, Durability (สำหรับ relational DB)
- BASE: Basically Available, Soft state, Eventually consistent (สำหรับ NoSQL)

#### Tunable Consistency
- Tunable consistency อนุญาตให้เลือกระดับ consistency ตามต้องการ
- ตัวอย่าง: Cassandra ให้เลือกระดับ consistency (ONE, QUORUM, ALL)

#### Coherence Protocol
- Coherence protocol จัดการ cache หลาย copy ของข้อมูลให้ consistent
- ตัวอย่าง: Snoopy protocol (bus-based), Directory-based protocol

---

### บทที่ 17: Anti-Entropy & Repair (การซ่อมแซมข้อมูล)

#### Gossip Protocol (เพิ่มเติมจากบทที่ 10)
- Gossip ใช้สำหรับ anti-entropy: node แลกเปลี่ยนข้อมูลเพื่อตรวจจับ inconsistency

#### Merkle Tree
- Merkle Tree คือ hash tree ที่ช่วยเปรียบเทียบข้อมูลระหว่าง node อย่างรวดเร็ว
- แต่ละ leaf node เป็น hash ของ data block; internal node เป็น hash ของลูก

#### Read Repair
- Read repair: ตอนอ่านข้อมูล ถ้าเห็น inconsistency → ซ่อมแซมทันที

#### Anti-Entropy Protocol
- Anti-entropy protocol ใช้ Merkle Tree เพื่อ sync ข้อมูลระหว่าง node — ตรวจจับ difference แล้วส่ง patch

---

### บทที่ 18: Cascading Failures (ความล้มเหลวแบบ cascade)

#### Backpressure
- Backpressure คือ mechanism ที่ส่งสัญญาณให้ sender หยุดส่งข้อมูลชั่วคราวเมื่อ receiver ไม่สามารถรับได้ทัน

#### Circuit Breaker
- Circuit breaker คือ pattern ที่หยุดส่ง request ไปที่ service ที่ล้มเหลว — ป้องกัน cascade failure

#### Exponential Backoff & Jitter
- Exponential backoff: เพิ่ม delay ระหว่าง retry แบบ exponential (เช่น 1s, 2s, 4s, 8s)
- Jitter: เพิ่ม randomness ใน delay เพื่อลด thundering herd

#### Checksum
- Checksum คือค่า hash ที่ใช้ตรวจสอบความสมบูรณ์ของข้อมูล — ตรวจจับ corruption ระหว่างส่ง

---

# Database Internals — สรุปเนื้อหาทั้งหมด

## ส่วนที่ 1: Storage Engines

หนังสือเล่มนี้เริ่มจากสถาปัตยกรรมพื้นฐานของ DBMS — Client ส่ง query ผ่าน Parser → Planner → Execution Engine → Storage Engine ทีละชั้นอย่างอิสระ

### Row Store vs Column Store
- **Row store** (table-oriented) เก็บข้อมูลทีละ record — เหมาะกับ OLTP ที่อ่าน/เขียน record เดียว
- **Column store** เก็บข้อมูลทีละคอลัมน์ — เหมาะกับ OLAP ที่คำนวณ aggregate บนหลาย record

### Mutable vs Immutable
- **Mutable** (B-Tree) แก้ไขข้อมูลเดิม — เหมาะกับ workload ที่อ่านมาก
- **Immutable** (LSM Tree) สร้างเวอร์ชันใหม่ — เหมาะกับ workload ที่เขียนมาก

### B-Trees & LSM Trees
- **B-Tree**: balanced tree ที่เก็บ key เรียงลำดับ — range query เร็ว แต่ write amplification สูง
- **LSM Tree**: append-only → Memtable → SSTable — เขียนเร็ว แต่ read amplification สูง (ต้องค้นหลายชั้น)
- **Bloom Filter**: ช่วยลด disk I/O ใน LSM Tree โดยทดสอบ key ก่อนค้น (false positive ได้, false negative ไม่ได้)

### Page Layout & Slotted Pages
- **Page**: หน่วยพื้นฐานที่อ่าน/เขียนจาก disk (ขนาดคงที่ เช่น 4KB)
- **Slotted page**: page ที่มี slot array (header) เรียงตาม key — แต่ละ slot ชี้ไปยัง cell (record)
- **Cell pointer page**: page ที่เก็บ pointer ไปยัง cell ในหลาย page — ช่วยค้นหาข้อมูลเร็วขึ้น

### Data Recovery (WAL, ARIES, Checkpoints)
- **WAL**: บันทึกทุกการเปลี่ยนแปลงก่อนเขียน data page — กู้คืนจาก log เมื่อ system ล่ม
- **ARIES**: recovery algorithm ที่ใช้ใน database ส่วนใหญ่ (Analysis → Redo → Undo)
- **Checkpoint**: จุดที่ flush ข้อมูลลง disk — ลดเวลา recovery

### Buffer Management
- **Page cache**: buffer ในหน่วยความจำสำหรับเก็บ page ที่อ่านบ่อย
- **Eviction policies**: Clock, LRU, LIRS — ลบ page เมื่อ cache เต็ม

### Transaction Processing & ACID
- **Atomicity**: transaction ทำสำเร็จทั้งหมดหรือล้มเหลวทั้งหมด
- **Consistency**: ข้อมูลต้องอยู่ใน valid state
- **Isolation**: transaction ทำงาน parallel โดยไม่รบกวนกัน
- **Durability**: ข้อมูลที่เขียนสำเร็จต้องอยู่รอด

### Concurrency Control (MVCC, Locks vs Latches, SSI)
- **MVCC**: เก็บหลาย version — อ่านไม่บล็อกเขียน, เขียนไม่บล็อกอ่าน
- **Lock**: ใช้กับ logical objects (table, row); **Latch**: ใช้กับ physical structures (page)
- **SSI**: ป้องกัน serialization anomaly โดยใช้ dependency tracking

### Log-Structured Storage (Bitcask, WiscKey, Compaction)
- **Bitcask**: append-only key-value store — เขียนเร็ว แต่ต้อง compaction
- **WiscKey**: แยก key (memory) ออกจาก value (disk) — ลด write amplification
- **Compaction strategies**: Size-Tiered (เขียนมาก), Leveled (read amplification ต่ำ)

---

## ส่วนที่ 2: Distributed Systems

### CAP Theorem & Network Model
- **CAP theorem**: เลือกได้แค่ 2 ใน 3 (Consistency, Availability, Partition tolerance)
- **Synchronous vs Asynchronous network**: synchronous ส่งทันที, asynchronous อาจ delay
- **Failure models**: Crash-stop, Crash-recovery, Byzantine

### Failure Detection (Gossip, Phi-Accrual)
- **Gossip protocol**: node แลกเปลี่ยนข้อมูลแบบ random — ข้อมูลแพร่กระจายเร็ว
- **Phi-accrual failure detector**: คำนวณ probability ที่ node จะ crash — ไม่ใช่ binary

### Consensus (Paxos)
- **Paxos**: protocol สำหรับบรรลุฉันทามติ — มี 3 role: Proposer, Acceptor, Learner
- **Basic Paxos**: ตกลงค่าเดียว; **Multi-Paxos**: ตกลงค่าหลายตัว

### Replication (Leader-Follower, Multi-Leader, Leaderless, Quorum)
- **Leader-Follower**: leader รับ write, follower รับ read — sync ผ่าน log replication
- **Multi-Leader**: หลาย leader พร้อมกัน — ต้อง resolve conflict
- **Leaderless**: client ส่ง request ไปหลาย node พร้อมกัน — ใช้ quorum
- **Quorum**: W + R > N → อ่านได้ค่าล่าสุดเสมอ (strong consistency)

### Consistent Hashing & Partitioning
- **Consistent hashing**: ลดข้อมูลที่ต้องย้ายเมื่อเพิ่ม/ลบ node
- **Virtual node**: node เสมือนหลายตัวแทน physical node เดียว — กระจายข้อมูลสม่ำเสมอ
- **Range-based vs Hash-based**: range query เร็วแต่ hotspot; hash กระจายสม่ำเสมอแต่ range query ช้า

### Distributed Transactions (2PC, 3PC, Paxos Commit)
- **2PC**: Prepare → Commit — blocking protocol (ถ้า coordinator crash → participants รอ)
- **3PC**: เพิ่ม pre-commit phase — แต่ network partition ยังทำให้ inconsistent
- **Paxos Commit**: ใช้ Paxos — ไม่ blocking

### Consistency & Replication (CAP, ACID vs BASE, Tunable Consistency)
- **CP system**: เลือก consistency มากกว่า availability
- **AP system**: เลือก availability มากกว่า consistency
- **ACID**: สำหรับ relational DB; **BASE**: สำหรับ NoSQL
- **Tunable consistency**: เลือกระดับ consistency ตามต้องการ

### Anti-Entropy & Repair (Merkle Tree, Read Repair)
- **Merkle Tree**: hash tree ที่ช่วยเปรียบเทียบข้อมูลระหว่าง node อย่างรวดเร็ว
- **Read Repair**: ตอนอ่านข้อมูล ถ้าเห็น inconsistency → ซ่อมแซมทันที
- **Anti-entropy protocol**: ใช้ Merkle Tree เพื่อ sync ข้อมูลระหว่าง node

### Cascading Failures (Backpressure, Circuit Breaker, Backoff/Jitter)
- **Backpressure**: ส่งสัญญาณให้ sender หยุดส่งชั่วคราว
- **Circuit breaker**: หยุดส่ง request ไปที่ service ที่ล้มเหลว — ป้องกัน cascade failure
- **Exponential backoff & jitter**: เพิ่ม delay แบบ exponential + randomness — ลด thundering herd
- **Checksum**: hash สำหรับตรวจสอบความสมบูรณ์ของข้อมูล

---

# Database Internals — สรุปเชิงลึก

## ส่วนที่ 1: Storage Engines

### สถาปัตยกรรม DBMS
หนังสือเล่มนี้เริ่มจากสถาปัตยกรรมพื้นฐานของ Database Management System (DBMS) — Client ส่ง query ผ่าน Parser (แยกส่วน), Query Planner (วางแผน), Execution Engine (ดำเนินการ), แล้วส่งไปยัง Storage Engine (จัดเก็บ) ทีละชั้นอย่างอิสระ ทำให้แต่ละชั้นแก้ไขได้โดยไม่กระทบชั้นอื่น

### Row Store vs Column Store
Table-Oriented (row store) เก็บข้อมูลทีละ record — เหมาะกับ OLTP ที่ต้องอ่าน/เขียน record ทีละตัว Column-Oriented (column store) เก็บข้อมูลทีละคอลัมน์ — เหมาะกับ OLAP ที่ต้องคำนวณ aggregation บนหลาย record

### Mutable vs Immutable
Mutable (เช่น B-Tree) แก้ไขข้อมูลเดิมโดยตรง — เหมาะกับ workload ที่อ่านมาก Immutable (เช่น LSM Tree) สร้างเวอร์ชันใหม่แทนการแก้ไขเดิม — เหมาะกับ workload ที่เขียนมาก

### B-Trees & LSM Trees
B-Tree คือ balanced tree ที่เก็บ key เรียงลำดับ — range query เร็ว แต่ write amplification สูง LSM Tree คือ append-only structure ที่เก็บข้อมูลใน Memtable (memory) แล้ว Flush ไป Disk เป็น SSTable (Sorted String Table) — เขียนเร็ว แต่ read amplification สูง Bloom Filter ช่วยลด disk I/O ใน LSM Tree โดยทดสอบ key ก่อนค้น (false positive ได้, false negative ไม่ได้)

### Page Layout & Slotted Pages
Page คือหน่วยพื้นฐานที่อ่าน/เขียนจาก disk (ขนาดคงที่ เช่น 4KB) Slotted page คือ page ที่มี slot array (header) เรียงตาม key — แต่ละ slot ชี้ไปยัง cell (record) Cell pointer page เก็บ pointer ไปยัง cell ในหลาย page — ช่วยค้นหาข้อมูลเร็วขึ้น

### Data Recovery (WAL, ARIES, Checkpoints)
WAL (Write-Ahead Logging) บันทึกทุกการเปลี่ยนแปลงก่อนเขียน data page — กู้คืนจาก log เมื่อ system ล่ม ARIES (Algorithm for Recovery and Isolation Exploiting Semantics) คือ recovery algorithm ที่ใช้ใน database ส่วนใหญ่ — มี 3 ขั้นตอน: Analysis → Redo → Undo Checkpoint คือจุดที่ flush ข้อมูลลง disk — ลดเวลา recovery

### Buffer Management
Page cache คือ buffer ในหน่วยความจำสำหรับเก็บ page ที่อ่านบ่อย Eviction policies: Clock (Second-Chance), LRU (Least Recently Used), LIRS (Low Interreference Recency Set) — ลบ page เมื่อ cache เต็ม

### Transaction Processing & ACID
Atomicity: transaction ทำสำเร็จทั้งหมดหรือล้มเหลวทั้งหมด Consistency: ข้อมูลต้องอยู่ใน valid state หลังจาก transaction Isolation: transaction ทำงาน parallel โดยไม่รบกวนกัน (visibility rules) Durability: ข้อมูลที่เขียนสำเร็จต้องอยู่รอดแม้ system ล่ม

### Concurrency Control
MVCC (Multi-Version Concurrency Control) เก็บหลาย version ของ record — อ่านไม่บล็อกเขียน, เขียนไม่บล็อกอ่าน Lock ใช้กับ logical objects (table, row); Latch ใช้กับ physical structures (page, memory) SSI (Serializable Snapshot Isolation) ป้องกัน serialization anomaly โดยใช้ dependency tracking

### Log-Structured Storage
Bitcask คือ append-only key-value store — เขียนเร็ว แต่ต้อง compaction WiscKey คือ LSM Tree ที่แยก key (memory) ออกจาก value (disk) — ลด write amplification Compaction strategies: Size-Tiered (เขียนมาก), Leveled (read amplification ต่ำ)

---

## ส่วนที่ 2: Distributed Systems

### CAP Theorem & Network Model
CAP theorem บอกว่าเลือกได้แค่ 2 ใน 3: Consistency (ทุก node เห็นข้อมูลเดียวกัน), Availability (ทุก request ได้ response), Partition tolerance (ทำงานได้แม้ network partition) Synchronous network ส่ง message ทันที; Asynchronous network อาจ delay, หาย, หรือ duplicate Failure models: Crash-stop (หยุดถาวร), Crash-recovery (หยุดชั่วคราว), Byzantine (ส่งข้อมูลเท็จ)

### Failure Detection
Gossip protocol: node แลกเปลี่ยนข้อมูลแบบ random — ข้อมูลแพร่กระจายเร็ว Phi-accrual failure detector: คำนวณ probability ที่ node จะ crash — ไม่ใช่ binary (alive/dead)

### Consensus (Paxos)
Paxos คือ protocol สำหรับบรรลุฉันทามติ — มี 3 role: Proposer (เสนอค่า), Acceptor (รับ/ปฏิเสธ), Learner (เรียนรู้ค่าที่ตกลง) Basic Paxos ตกลงค่าเดียว; Multi-Paxos ตกลงค่าหลายตัว (sequence of values)

### Replication
Leader-Follower: leader รับ write, follower รับ read — sync ผ่าน log replication Multi-Leader: หลาย leader พร้อมกัน — ต้อง resolve conflict (เช่น last-write-wins) Leaderless: client ส่ง request ไปหลาย node พร้อมกัน — ใช้ quorum Quorum: W + R > N → อ่านได้ค่าล่าสุดเสมอ (strong consistency)

### Consistent Hashing & Partitioning
Consistent hashing: ลดข้อมูลที่ต้องย้ายเมื่อเพิ่ม/ลบ node Virtual node: node เสมือนหลายตัวแทน physical node เดียว — กระจายข้อมูลสม่ำเสมอ Range-based partitioning: range query เร็ว แต่ hotspot; Hash-based: กระจายสม่ำเสมอ แต่ range query ช้า

### Distributed Transactions
2PC (Two-Phase Commit): Prepare → Commit — blocking protocol (ถ้า coordinator crash → participants รอ) 3PC: เพิ่ม pre-commit phase — แต่ network partition ยังทำให้ inconsistent Paxos Commit: ใช้ Paxos — ไม่ blocking

### Consistency & Replication
CP system: เลือก consistency มากกว่า availability (เช่น ZooKeeper) AP system: เลือก availability มากกว่า consistency (เช่น Cassandra) ACID: สำหรับ relational DB; BASE: สำหรับ NoSQL Tunable consistency: เลือกระดับ consistency ตามต้องการ (ONE, QUORUM, ALL)

### Anti-Entropy & Repair
Merkle Tree: hash tree ที่ช่วยเปรียบเทียบข้อมูลระหว่าง node อย่างรวดเร็ว Read Repair: ตอนอ่านข้อมูล ถ้าเห็น inconsistency → ซ่อมแซมทันที Anti-entropy protocol: ใช้ Merkle Tree เพื่อ sync ข้อมูลระหว่าง node

### Cascading Failures
Backpressure: ส่งสัญญาณให้ sender หยุดส่งชั่วคราว Circuit breaker: หยุดส่ง request ไปที่ service ที่ล้มเหลว — ป้องกัน cascade failure Exponential backoff & jitter: เพิ่ม delay แบบ exponential + randomness — ลด thundering herd Checksum: hash สำหรับตรวจสอบความสมบูรณ์ของข้อมูล

---

# Database Internals — สรุป完整 (Complete Summary)

## ส่วนที่ 1: Storage Engines

### บทที่ 1: DBMS Architecture
หนังสือเล่มนี้เริ่มจากสถาปัตยกรรมพื้นฐานของ Database Management System (DBMS) — Client ส่ง query ผ่าน Parser → Query Planner → Execution Engine → Storage Engine ทีละชั้นอย่างอิสระ ทำให้แต่ละชั้นแก้ไขได้โดยไม่กระทบชั้นอื่น Table-Oriented (row store) เก็บข้อมูลทีละ record — เหมาะกับ OLTP Column-Oriented (column store) เก็บข้อมูลทีละคอลัมน์ — เหมาะกับ OLAP Mutable (เช่น B-Tree) แก้ไขข้อมูลเดิมโดยตรง — เหมาะกับ workload ที่อ่านมาก Immutable (เช่น LSM Tree) สร้างเวอร์ชันใหม่ — เหมาะกับ workload ที่เขียนมาก Ordered (B-Tree) เก็บข้อมูลเรียงตาม key — range query เร็ว Unordered (Hash Index) เก็บข้อมูลแบบ random — equality query เร็ว

### บทที่ 2: Storage & Index Structures
B-Tree คือ balanced tree ที่เก็บ key เรียงลำดับ — leaf node เก็บ key-value pairs, internal node เก็บ key สำหรับนำทาง B+ Tree คือ B-Tree ที่ leaf node เชื่อมกันเป็น linked list — เหมาะกับ range query LSM Tree คือ append-only structure ที่เก็บข้อมูลใน Memtable (memory) แล้ว Flush ไป Disk เป็น SSTable (Sorted String Table) — เขียนเร็ว แต่ read amplification สูง Bloom Filter คือ probabilistic data structure สำหรับทดสอบว่า key มีอยู่หรือไม่ — false positive ได้, false negative ไม่ได้

### บทที่ 3: Page Layout
Page คือหน่วยพื้นฐานที่อ่าน/เขียนจาก disk (ขนาดคงที่ เช่น 4KB) Slotted page คือ page ที่มี slot array (header) เรียงตาม key — แต่ละ slot ชี้ไปยัง cell (record) Cell pointer page เก็บ pointer ไปยัง cell ในหลาย page — ช่วยค้นหาข้อมูลเร็วขึ้น

### บทที่ 4: Data Recovery & Catalogs
WAL (Write-Ahead Logging) บันทึกทุกการเปลี่ยนแปลงก่อนเขียน data page — กู้คืนจาก log เมื่อ system ล่ม ARIES (Algorithm for Recovery and Isolation Exploiting Semantics) คือ recovery algorithm ที่ใช้ใน database ส่วนใหญ่ — Analysis → Redo → Undo Catalog เก็บ metadata (schema, table definitions) — อ่านเหมือน table ปกติ Checkpoint คือจุดที่ flush ข้อมูลลง disk — ลดเวลา recovery

### บทที่ 5: Buffer Management
Page cache คือ buffer ในหน่วยความจำสำหรับเก็บ page ที่อ่านบ่อย Clock: หมุนเข็มไปเรื่อยๆ แล้วลบ page ที่ไม่ได้ใช้ (Second-Chance) LRU (Least Recently Used): ลบ page ที่ไม่ได้ใช้นานที่สุด LIRS (Low Interreference Recency Set): ลบ page ที่ interreference recency ต่ำ — ดีกว่า LRU ในบาง workload Multi-Database Caching: database หลายตัวใช้ page cache เดียวกัน → ต้องจัดการ eviction อย่างระมัดระวัง

### บทที่ 6: Transaction Processing & ACID
Transaction คือ unit of work ที่ทำหลายขั้นตอน — ต้อง complete ทั้งหมดหรือไม่เลย (atomicity) Atomicity: transaction ทำสำเร็จทั้งหมดหรือล้มเหลวทั้งหมด Consistency: ข้อมูลต้องอยู่ใน valid state หลังจาก transaction Isolation: transaction ทำงาน parallel โดยไม่รบกวนกัน Durability: ข้อมูลที่เขียนสำเร็จต้องอยู่รอดแม้ system ล่ม Write Skew: serialization anomaly ที่ transaction A อ่านค่า X แล้วเขียน Y; transaction B อ่าน Y แล้วเขียน X — ผลลัพธ์สุดท้ายอาจผิดปกติ

### บทที่ 7: Concurrency Control
MVCC (Multi-Version Concurrency Control) เก็บหลาย version ของ record — อ่านไม่บล็อกเขียน, เขียนไม่บล็อกอ่าน Lock ใช้กับ logical objects (table, row) — ป้องกัน transaction อื่นเข้าถึง Latch ใช้กับ physical structures (page, memory) — ป้องกัน thread อื่นเข้าถึง cùng lúc SSI (Serializable Snapshot Isolation) ป้องกัน serialization anomaly โดยใช้ dependency tracking — ตรวจจับ write skew

### บทที่ 8: Log-Structured Storage
LSM Tree (เพิ่มเติม): Memtable → Flush เป็น SSTable → Compaction Size-Tiered Compaction: compaction SSTable ขนาดใกล้กัน — เหมาะกับ workload ที่เขียนมาก Leveled Compaction: compaction SSTable ทีละ level — ลด read amplification แต่เพิ่ม write amplification Bitcask: append-only key-value store — key → file offset mapping เก็บใน hash index (memory) WiscKey: แยก key (memory) ออกจาก value (disk) — ลด write amplification

---

## ส่วนที่ 2: Distributed Systems

### บทที่ 9: Introduction to Distributed Systems
Synchronous network: message ส่งถึงทันที (ไม่มี delay) Asynchronous network: message อาจ delay, หาย, หรือ duplicate Crash-stop: node หยุดทำงานถาวร Crash-recovery: node หยุดทำงานชั่วคราวแล้ว recover Byzantine: node ทำงานผิดปกติ (ส่งข้อมูลเท็จ) CAP Theorem: เลือกได้แค่ 2 ใน 3 — Consistency, Availability, Partition tolerance

### บทที่ 10: Failure Detection
Gossip Protocol: node แลกเปลี่ยนข้อมูลแบบ random — ข้อมูลแพร่กระจายเร็ว Phi-Accrual Failure Detection: คำนวณ probability ที่ node จะ crash — ไม่ใช่ binary

### บทที่ 11: Consensus
Paxos: protocol สำหรับบรรลุฉันทามติ — Proposer (เสนอค่า), Acceptor (รับ/ปฏิเสธ), Learner (เรียนรู้ค่าที่ตกลง) Basic Paxos: ตกลงค่าเดียว Multi-Paxos: ตกลงค่าหลายตัว (sequence of values) — ใช้ใน log replication Generalized Paxos: อนุญาตให้ commutative operations ทำงาน parallel

### บทที่ 12: Replication
Leader-Follower: leader รับ write, follower รับ read — sync ผ่าน log replication Multi-Leader: หลาย leader พร้อมกัน — ต้อง resolve conflict (last-write-wins, vector clocks) Leaderless: client ส่ง request ไปหลาย node พร้อมกัน — ใช้ quorum Quorum: W + R > N → อ่านได้ค่าล่าสุดเสมอ (strong consistency)

### บทที่ 13: Consistent Hashing
Virtual Nodes: node เสมือนหลายตัวแทน physical node เดียว — กระจายข้อมูลสม่ำเสมอ Key Range: ช่วงของ key ที่ assign ให้แต่ละ node — เปลี่ยนแปลงได้ (rebalancing) Token Ring: circular hash space ที่ assigns key ranges ให้แต่ละ node

### บทที่ 14: Partitioning
Sharding: แบ่งข้อมูลเป็นหลาย partition (shard) บนหลาย node Consistent Hashing: ลดข้อมูลที่ต้องย้ายเมื่อเพิ่ม/ลบ node Range-Based: range query เร็ว แต่ hotspot Hash-Based: กระจายสม่ำเสมอ แต่ range query ช้า

### บทที่ 15: Transactions
2PC (Two-Phase Commit): Prepare → Commit — blocking protocol (ถ้า coordinator crash → participants รอ) 3PC: เพิ่ม pre-commit phase — แต่ network partition ยังทำให้ inconsistent Paxos Commit: ใช้ Paxos — ไม่ blocking

### บทที่ 16: Consistency & Replication
CAP Theorem (เพิ่มเติม): CP system เลือก consistency (ZooKeeper); AP system เลือก availability (Cassandra) ACID vs BASE: ACID สำหรับ relational DB; BASE สำหรับ NoSQL Tunable Consistency: เลือกระดับ consistency (ONE, QUORUM, ALL) Coherence Protocol: จัดการ cache หลาย copy ให้ consistent (Snoopy, Directory-based)

### บทที่ 17: Anti-Entropy & Repair
Gossip Protocol (เพิ่มเติม): ใช้สำหรับ anti-entropy — node แลกเปลี่ยนข้อมูลเพื่อตรวจจับ inconsistency Merkle Tree: hash tree ที่ช่วยเปรียบเทียบข้อมูลระหว่าง node อย่างรวดเร็ว Read Repair: ตอนอ่านข้อมูล ถ้าเห็น inconsistency → ซ่อมแซมทันที Anti-Entropy Protocol: ใช้ Merkle Tree เพื่อ sync ข้อมูลระหว่าง node

### บทที่ 18: Cascading Failures
Backpressure: ส่งสัญญาณให้ sender หยุดส่งชั่วคราว Circuit Breaker: หยุดส่ง request ไปที่ service ที่ล้มเหลว — ป้องกัน cascade failure Exponential Backoff & Jitter: เพิ่ม delay แบบ exponential + randomness — ลด thundering herd Checksum: hash สำหรับตรวจสอบความสมบูรณ์ของข้อมูล