# Designing Data-Intensive Applications
> **Author:** Martin Kleppmann

## ภาพรวมหนังสือ

หนังสือเล่มนี้ครอบคลุมสถาปัตยกรรม (architecture) ของระบบข้อมูล (data systems) ที่ต้องรับมือกับข้อมูลปริมาณมหาศาล ความซับซ้อน และความเร็วในการเปลี่ยนแปลง แบ่งออกเป็น 3 ส่วนหลัก: พื้นฐานระบบข้อมูล, ข้อมูลแบบกระจาย (distributed data), และข้อมูลที่ได้มา (derived data)

---

## ส่วนที่ 1: พื้นฐานระบบข้อมูล (Foundations of Data Systems)

### บทที่ 1 — ระบบที่เชื่อถือได้ ปรับขนาดได้ และดูแลง่าย

**ความน่าเชื่อถือ (Reliability)** — ระบบต้องทำงานได้ถูกต้องแม้เกิดความล้มเหลว (faults) ความล้มเหลวมี 3 ประเภท:
- **Hardware Faults**: ฮาร์ดแวร์เสีย — แก้ด้วย redundancy (RAID, hot-swap, failover)
- **Software Errors**: บั๊ก systematic ที่ส่งผลทุก instance พร้อมกัน — แก้ด้วย testing, process isolation, monitoring
- **Human Errors**: คนตั้งค่าผิด — แก้ด้วย sandbox environment, automated testing, rollback ง่าย, monitoring ละเอียด

**ความแตกต่างระหว่าง Fault กับ Failure**: Fault =  component ตัวใดตัวหนึ่งผิดปกติ, Failure = ระบบหยุดให้บริการ entirely

**การปรับขนาด (Scalability)** — ความสามารถในการรับมือกับโหลดที่เพิ่มขึ้น:
- **Load parameters**: ตัวเลขที่บรรยายโหลด เช่น requests/sec, ratio read:write, จำนวน active users
- **Percentiles**: p50 (median), p95, p99, p999 — สำคัญกว่าค่าเฉลี่ย เพราะ user รู้สึกจาก tail latency
- **Vertical scaling (scale up)** vs **Horizontal scaling (scale out)** — ในระบบจริงใช้ผสมกัน

**การดูแลรักษา (Maintainability)** — 3 หลักการ:
- **Operability**: ทำให้ทีม ops ทำงานง่าย — monitoring, automation, documentation
- **Simplicity**: ลด complexity ด้วย abstraction ที่ดี — abstract ซ่อนรายละเอียด implementation
- **Evolvability**: ระบบต้องปรับเปลี่ยนง่ายตาม requirements ที่เปลี่ยน — refactoring, agile, TDD

---

### บทที่ 2 — โมเดลข้อมูลและภาษี查询 (Data Models & Query Languages)

**Relational Model vs Document Model**:
- **Relational (SQL)**: ข้อมูลอยู่ในตาราง (tables) ประกอบด้วย rows/columns, เหมาะกับ many-to-many relationships, JOIN ง่าย, มี schema-on-write (schema ชัดเจน, บังคับตอนเขียน)
- **Document (JSON/MongoDB)**: ข้อมูลอยู่ใน nested documents, เหมาะกับ one-to-many, locality ดี (ข้อมูลที่เกี่ยวข้องอยู่ที่เดียว), schema-on-read (schema ยืดหยุ่น, ตีความตอนอ่าน)

**Object-Relational Mismatch (Impedance Mismatch)**: โมเดล object ของภาษาโปรแกรมไม่ตรงกับตาราง SQL — ORM (เช่น Hibernate) ช่วยแต่ไม่หมด

**Normalization vs Denormalization**: Normalization = ลดข้อมูลซ้ำซ้อน (ใช้ foreign key/ID), Denormalization = ซ้ำข้อมูลเพื่อ performance แต่ต้อง sync เอง

**Query Languages**:
- **Declarative (SQL)**: บอก "ต้องการอะไร" ไม่ต้องบอก "ทำยังไง" — query optimizer จัดการให้, ย่อ code, parallelize ง่าย
- **Imperative**: บอก step-by-step — ยาว, แก้ยาก, optimize ยาก
- **MapReduce**: โมเดลกึ่ง declarative ใช้ map() + reduce() functions — MongoDB ใช้, แต่ aggregation pipeline ดีกว่า

**Graph-Like Data Models** — สำหรับข้อมูลที่เชื่อมต่อกันซับซ้อน:
- **Property Graph (Neo4j)**: vertices + edges + properties, Cypher query language — ย่อและอ่านง่าย
- **Triple Store (RDF)**: (subject, predicate, object), SPARQL query — เหมาะกับ semantic web
- **Datalog**: logic-based query language สำหรับ graphs

---

### บทที่ 3 — การเก็บข้อมูลและการเรียกค้น (Storage & Retrieval)

**โครงสร้างข้อมูลที่อยู่เบื้องหลัง database**:

- **Hash Indexes**: ดีสำหรับ key-value lookup เร็วมาก O(1), แต่ไม่ range query ได้
- **SSTables + LSM-Trees**: เขียนเร็ว (append), merge ตอน idle — นิยมใน write-heavy workload (Cassandra, LevelDB, RocksDB)
- **B-Trees**: structure ดั้งเดิมที่ใช้ใน PostgreSQL, MySQL InnoDB — เหมาะกับ read-heavy, มี O(log n) lookup, data อยู่บน disk pages

**B-Tree vs LSM-Tree**:
- LSM: เขียนเร็ว,  compact ตอน idle, แต่ read ช้ากว่า (ต้อง check multiple levels)
- B-Tree: อ่านเร็ว, เขียนช้ากว่า (overwrite), fragmentation

**Column-Oriented Storage (OLAP)**: เก็บข้อมูลแบบ columns แทน rows — เหมาะกับ analytics/aggregation, compression ดี (ข้อมูลชนิดเดียวกันอยู่ติดกัน), ตัวอย่าง: Cassandra, Bigtable

**Data Warehousing (OLAP vs OLTP)**:
- **OLTP (Online Transaction Processing)**: เขียน/อ่านทีละ record, สำหรับ app จริง
- **OLAP (Online Analytical Processing)**: query ข้อมูลจำนวนมากพร้อมกัน, สำหรับ reporting/analytics
- **Star/Snowflake Schemas**: schema สำหรับ data warehouse — fact table ตรงกลาง, dimension tables ล้อมรอบ

---

### บทที่ 4 — การเข้ารหัสและการเปลี่ยนแปลง (Encoding & Evolution)

**Data Encoding Formats**:
- **JSON/XML**: text-based, human-readable, แต่ verbose
- **Binary formats (Thrift, Protocol Buffers, Avro)**: compact, fast, มี schema — ใช้ใน serialization ระหว่าง services
- **Schema evolution**: เมื่อ schema เปลี่ยน (เพิ่ม/ลบ field) — backward/forward compatibility สำคัญมาก

**Modes of Dataflow**:
- **Through databases**: เขียนข้อมูลผ่าน DB, อ่านผ่าน DB — schema evolution ผ่าน DB
- **Through services (REST/RPC)**: client ส่ง request, server ตอบ — protobuf/gRPC สำหรับ internal, JSON/HTTP สำหรับ external
- **Message-passing (async)**: producer ส่ง message ผ่าน message broker (Kafka, RabbitMQ) — decouple producer/consumer

---

## ส่วนที่ 2: ข้อมูลแบบกระจาย (Distributed Data)

### บทที่ 5 — การทำซ้ำ (Replication)

**จุดประสงค์**: เพิ่ม availability, ลด latency (read จาก replica ที่ใกล้), เพิ่ม throughput

**Leader-Follower Replication**:
- Leader รับ write, replicate ไป follower
- **Synchronous replication**: รับประกันว่า write อยู่ทุก node, แต่ช้า (ถ้า node หนึ่ง down ทั้ง system หยุด)
- **Asynchronous replication**: เร็ว แต่ข้อมูลอาจหายถ้า leader crash ก่อน sync
- **Semi-synchronous**: sync กับ follower หนึ่งตัว, async กับตัวอื่น

**Replication Lag Problems**:
- **Read your own writes**: user เขียนข้อมูล แล้วอ่านทันที — อาจเห็นข้อมูลเก่าถ้าอ่านจาก follower ที่ lag
- **Monotonic reads**: อ่านทีละนัดได้ข้อมูลเก่ากว่าเดิม — แก้ด้วย consistent hashing
- **Consistent prefix reads**: เห็นข้อมูลในลำดับที่ถูกต้อง — แก้ด้วย causal ordering

**Multi-Leader Replication**: เขียนได้หลายที่พร้อมกัน (offline-first apps, cross-datacenter) — มี conflict resolution (last-write-wins, merge, CRDT)

**Leaderless Replication (Dynamo-style)**: ทุก node รับ write — ใช้ quorum (W + R > N) เพื่อ consistency, Sloppy quorum + hinted handoff สำหรับ fault tolerance

---

### บทที่ 6 — การแบ่งข้อมูล (Partitioning/Sharding)

**จุดประสงค์**: กระจายข้อมูลไปหลายเครื่อง เพื่อ scalability

**Partitioning Strategies**:
- **Key Range**: แบ่งตาม key range — เหมาะกับ range query, แต่ hotspot ได้ถ้า key มี skew
- **Hash of Key**: hash key แล้ว mod number of partitions — กระจายดี แต่ range query ต้อง scatter-gather

**Secondary Indexes บน partitioned data**:
- **Partition by document**: index อยู่บน partition เดียวกับ document (local index) — เขียนเร็ว แต่ query ต้อง scatter-gather
- **Partition by term**: index อยู่บน partition ต่างหาก (global index) — query เร็ว แต่เขียนช้า (write หลาย partitions)

**Rebalancing**: เปลี่ยน mapping ของ partitions — ต้องไม่หยุด service, ต้องรักษา locality, ต้อง avoid unnecessary movement

**Request Routing**: client ต้องรู้ว่า partition อยู่บน node ไหน — ใช้ gossip protocol, coordination service (ZooKeeper), หรือ client-side routing

---

### บทที่ 7 — Transaction (ธุรกรรม)

**ACID Properties**:
- **Atomicity**: ทุก operation สำเร็จทั้งหมด หรือไม่มีเลย
- **Consistency**: ข้อมูลอยู่ใน state ที่ valid หลัง transaction
- **Isolation**: transaction ทำงาน parallel โดยไม่เห็น intermediate state ของกันและกัน
- **Durability**: เขียนสำเร็จแล้วไม่หาย แม้ไฟดับ

**Weak Isolation Levels**:
- **Read Committed**: เห็นเฉพาะข้อมูลที่ committed — ป้องกัน dirty read
- **Snapshot Isolation (Repeatable Read)**: อ่านจาก snapshot ของช่วงเวลาหนึ่ง — ป้องกัน non-repeatable read แต่มี write skew ได้
- **Preventing Lost Updates**: ใช้ atomic compare-and-set หรือ SELECT FOR UPDATE
- **Write Skew**: two transactions เขียนคนละ record แต่ result ผิด — ป้องก้วย serializable isolation

**Serializability (Strongest Isolation)**:
- **Actual Serial Execution**: ทำงานทีละ transaction — เร็วพอถ้า transaction สั้น
- **Two-Phase Locking (2PL)**: lock ก่อนเขียน/อ่าน — ถูกต้องแต่ slow, deadlock ได้
- **Serializable Snapshot Isolation (SSI)**: snapshot isolation + detect write skew — เป็น default ใน PostgreSQL

---

### บทที่ 8 — ปัญหาของระบบกระจาย (Trouble with Distributed Systems)

**Partial Failures**: ระบบ distributed อาจล้มเหลวบางส่วน — บาง node ทำงาน, บาง node ไม่ — ยากกว่า single-node มาก

**Unreliable Networks**: message อาจหาย, delay, duplicate — ไม่มี guarantee ว่า message ถึง

**Unreliable Clocks**: clock แต่ละ machine ไม่ sync กัน 100% — Google TrueTime, NTP, ความคลาดเคลื่อน会影响 ordering

**Process Pauses**: garbage collection, page fault ทำให้ process หยุดชั่วคราว — ไม่รู้ตัวว่าหยุดนานแค่ไหน

**Truth and Lies (Byzantine Faults)**: node อาจโกหก (send wrong data) — ต้องมี checksum, digital signatures

**FLP Impossibility**: ใน asynchronous system ที่มี faulty node, ไม่มี consensus algorithm ที่ guarantee ได้ 100%

---

### บทที่ 9 — Consistency และ Consensus

**Linearizability (Strong Consistency)**: ทุก operation ดูเหมือนเกิด ณ จุดเดียวในเวลาหนึ่ง — expensive ต้อง coordination ทุกครั้ง

**Causal Consistency**: operation ที่ causally related ต้องเรียงลำดับถูกต้อง — อ่อนกว่า linearizability แต่ performant กว่า

**Total Order Broadcast (Linearizable Broadcasting)**: ทุก node เห็น message ในลำดับเดียวกัน — ใช้ใน leader election, replicated state machines

**Distributed Transactions & Consensus**:
- **Two-Phase Commit (2PC)**: coordinator ถามทุก node ว่า "commit ได้ไหม?" → ถ้าทุก node ตอบ yes → commit, ไม่เช่นนั้น abort — blocking protocol, coordinator down = ทั้ง system ค้าง
- **Fault-Tolerant Consensus (Raft/Paxos)**: elect leader ผ่าน voting —容忍 faulty nodes ได้ถ้า < 50% ของ nodes
- **ZooKeeper/etcd**: consensus service สำหรับ distributed coordination — leader election, configuration management, distributed locks

---

## ส่วนที่ 3: ข้อมูลที่ได้มา (Derived Data)

### บทที่ 10 — Batch Processing

**Unix Tools Philosophy**: ทำทีละอย่าง ต่อท่อ (pipe) กัน — ง่าย ยืดหยุ่น, แต่จำกัดที่ single machine

**MapReduce**: โมเดล batch processing สำหรับ cluster ขนาดใหญ่:
- **Map phase**: อ่าน input, split เป็น key-value pairs
- **Shuffle**: group by key
- **Reduce**: ประมวลผลแต่ละกลุ่ม

**Hadoop (HDFS + MapReduce)**: filesystem แบบ distributed + engine สำหรับ batch processing

**Beyond MapReduce**:
- **Spark, Flink**: ใน-memory processing เร็วกว่า MapReduce 10-100x
- **Materialization**: intermediate state เขียนลง disk/memory — tradeoff ระหว่าง computation กับ I/O
- **Graph Processing (Pregel)**: iterative computation บน graphs — PageRank, shortest path

---

### บทที่ 11 — Stream Processing

**Event Streams**: ข้อมูลเกิดขึ้นเรื่อยๆ (ไม่จบ) — ต่างจาก batch ที่มีจุดสิ้นสุด

**Messaging Systems**:
- **Message brokers (RabbitMQ, ActiveMQ)**: push messages ไป consumers — traditional, มี acknowledgment
- **Log-based (Kafka, Amazon Kinesis)**: append-only log — consumer อ่านเอง, replay ได้, retain ข้อมูลได้นาน

**Change Data Capture (CDC)**: จับการเปลี่ยนแปลงจาก database (transaction log) → stream → ส่งไป downstream systems

**Event Sourcing**: เก็บทุก event ที่เกิดขึ้น (ไม่ใช่แค่ state ปัจจุบัน) — audit trail, ย้อนหลังได้, rebuild state ได้

**Stream Processing Operations**:
- **Windowing**: แบ่ง stream เป็น time windows (tumbling, sliding, session) สำหรับ aggregation
- **Stream Joins**: join ระหว่าง streams — event-time vs processing-time
- **Fault Tolerance**: exactly-once semantics ผ่าน transaction log + checkpointing (Flink, Kafka Streams)

---

### บทที่ 12 — อนาคตของระบบข้อมูล (Future of Data Systems)

**Data Integration**: ในโลกจริง ไม่มี database เดียวที่ทำได้ทุกอย่าง — ต้อง combine tools หลายตัว (polyglot persistence)

**Unbundling Databases**: แยกฟังก์ชันของ database ออกเป็น microservices:
- Storage engine ตัวหนึ่ง
- Search index ตัวหนึ่ง (Elasticsearch)
- Caching ตัวหนึ่ง (Redis)
- Message queue ตัวหนึ่ง (Kafka)

**Batch + Stream Processing (Lambda/Kappa Architecture)**:
- **Lambda**: batch layer + stream layer + serving layer — ซับซ้อน
- **Kappa**: ใช้ stream processing ทั้งหมด — replay log เมื่อต้อง reprocess

**Correctness & End-to-End Argument**: constraint ควร enforce ที่ database layer ไม่ใช่ application layer — ใช้ unique constraints, foreign keys

**Trust but Verify**: monitoring, auditing, data validation pipeline — อย่าเชื่อ 100% ว่า system ถูกต้อง ต้อง verify เสมอ

**Privacy & Predictive Analytics**: ethical considerations, data protection (GDPR), ไม่應該ใช้ data ในทางที่ผิด

---

## สรุปรวม

หนังสือเล่มนี้ให้ framework สำหรับคิดเรื่อง data systems อย่างมีระบบ — เริ่มจาก single-node fundamentals (reliability, data models, storage engines, encoding) → distributed challenges (replication, partitioning, transactions, consistency) → derived data patterns (batch/stream processing) → future directions. หัวใจสำคัญคือ **trade-off ทุกอย่างมี cost** ไม่มี silver bullet — ต้องเลือก tool ให้เหมาะกับ use case

---
