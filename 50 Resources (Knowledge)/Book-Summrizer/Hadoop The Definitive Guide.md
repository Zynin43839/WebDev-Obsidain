# Hadoop: The Definitive Guide

> **Author:** Tom White

---

## บทที่ 1: รู้จัก Hadoop

Hadoop เกิดจากปัญหา Big Data — ข้อมูลดิจิทัลทั่วโลกใหญ่ถึง 0.18 zettabytes ในปี 2006 NYSE สร้างข้อมูล trades 1 TB ต่อวัน Facebook มีรูปภาพ 10 พันล้านรูป Large Hadron Collider สร้างข้อมูล 15 PB ต่อปี

ปัญหาหลักคือ storage capacity ของฮาร์ดดิสก์เพิ่มขึ้นมาก แต่ reading speed ไม่ตามทัน ฮาร์ดดิสก์ 1 TB ใช้เวลาอ่านเต็ม 2.5 ชั่วโมง วิธีแก้คืออ่านจากหลาย disk แบบ parallel แต่hardware มักจะ fail จึงต้องมี replication

### MapReduce เปรียบเทียบกับระบบอื่น

- **RDBMS**: ดีสำหรับ transactional queries ข้อมูล GB แต่ไม่เหมาะ batch analysis ข้อมูล PB
- **Grid Computing (HPC/MPI)**: ใช้ shared filesystem บน SAN เหมาะ compute-intensive แต่ network bandwidth เป็น bottleneck MapReduce ใช้ data locality ส่ง computation ไปหาข้อมูล
- **Volunteer Computing (SETI@home)**: ต่างตรงที่ MapReduce รันบน dedicated hardware ใน data center เดียว

### ประวัติ Hadoop

Doug Cutting สร้าง Hadoop จาก Apache Nutch search engine ตั้งชื่อตามช้างยัดนุ่นของลูกสาว หลัง Google เผยแพร่ paper GFS (2003) และ MapReduce (2004) Nutch ported มาใช้ framework นี้ แยกออกมาเป็น Hadoop subproject ในปี 2006 Yahoo! จ้าง Doug Cutting และให้ทีมพัฒนา Hadoop จน scale ถึง 10,000-node cluster ในปี 2008

### Hadoop Ecosystem

- **Common**: components สำหรับ distributed filesystems และ I/O
- **Avro**: serialization system สำหรับ RPC cross-language
- **MapReduce**: distributed data processing model
- **HDFS**: distributed filesystem
- **Pig**: data flow language สำหรับ exploring ข้อมูลขนาดใหญ่
- **Hive**: distributed data warehouse ใช้ SQL-like queries
- **HBase**: distributed column-oriented database
- **ZooKeeper**: distributed coordination service
- **Sqoop**: ย้ายข้อมูลระหว่าง relational DB กับ HDFS

### ความเข้ากันได้

- **API compatibility**: Major release อาจ breaking change minor/point release ไม่
- **Data compatibility**: Format metadata มีการ migrate อัตโนมัติ
- **Wire compatibility**: Client-server communication ต้อง compatible

---

## บทที่ 2: MapReduce

MapReduce เป็น programming model สำหรับ data processing ที่มี 2 phases หลัก คือ map และ reduce ทุก phase รับ input key-value pairs และ output key-value pairs

### ตัวอย่างจริง: วิเคราะห์ข้อมูลอากาศ (NCDC)

ข้อมูลจาก National Climatic Data Center มี format line-oriented ASCII เป้าหมายคือหาอุณหภูมิสูงสุดของแต่ละปี

- **Map function**: ดึง year กับ temperature จากแต่ละ record filter ค่าที่ไม่ valid (9999 = missing)
- **Reduce function**: iterate หา max temperature สำหรับแต่ละ year
- Framework sort และ group output จาก map โดย key ก่อนส่งเข้า reduce

### Java MapReduce

- **Mapper class**: มี type parameters 4 ตัว ใช้ Hadoop types เช่น LongWritable Text IntWritable
- **Reducer class**: รับ Iterable ของ values สำหรับแต่ละ key iterate เพื่อหา max
- **Job class**: ตั้ง input/output paths mapper/reducer classes แล้ว waitForCompletion()

### Data Flow ขนาดใหญ่

- Hadoop แบ่ง input เป็น fixed-size input splits (default = HDFS block 64 MB)
- สร้าง map task หนึ่งต่อ split รันบน node ที่มีข้อมูล (data locality optimization)
- Map output เขียนลง local disk เพราะเป็น intermediate data
- Reduce task รวม output จากทุก map ผ่าน network (shuffle)

### Combiner Functions

- Combiner รันบน map output ก่อนส่งเข้า reduce ลด data transfer
- ใช้ได้เฉพาะ commutative/associative functions เช่น max sum แต่ใช้กับ mean ไม่ได้

### Hadoop Streaming

允许เขียน map/reduce ด้วยภาษาใดก็ได้ที่อ่าน stdin เขียน stdout

- **Ruby**: ใช้ STDIN.each_line อ่าน input puts เขียน tab-separated output
- **Python**: ใช้ sys.stdin อ่าน print เขียน
- ลด key groups ด้วยการ track last_key เอง

### Hadoop Pipes (C++)

ใช้ sockets แทน stdin/stdout key/value เป็น STL strings extend HadoopPipes Mapper และ Reducer classes

---

## บทที่ 3: HDFS (Hadoop Distributed Filesystem)

HDFS ออกแบบสำหรับไฟล์ขนาดใหญ่ streaming data access (write-once read-many) รันบน commodity hardware ที่มีโอกาส fail สูง ไม่เหมาะกับ low-latency access small files จำนวนมาก multiple writers random modifications

### Blocks

- HDFS block = 64 MB ใหญ่กว่า disk block 512 bytes มาก ลด seek cost
- ไฟล์เล็กกว่า 1 block ไม่กินพื้นที่เต็ม block
- Block abstraction ทำให้ไฟล์ใหญ่กว่า disk เดียวได้ ง่ายต่อ storage management และรองรับ replication

### Namenodes และ Datanodes

- **Namenode** (master): จัดการ filesystem namespace metadata namespace image + edit log
- **Datanodes** (workers): เก็บ blocks รายงานกลับไป namenode เป็นระยะ
- Namenode คือ single point of failure ต้อง backup metadata ไป NFS ใช้ secondary namenode merge namespace image กับ edit log

### HDFS Federation

แยก namespace เป็นหลาย namenodes แต่ละตัวจัดการส่วนของ filesystem namenodes ไม่ communicate กัน failure ของตัวหนึ่งไม่กระทบอีกตัว

### HDFS High-Availability

- ใช้ active-standby namenode configuration
- Standby อ่าน edit log จาก shared storage (NFS) เพื่อ sync state
- Failover controller ใช้ ZooKeeper จัดการ failover ใช้เวลาประมาณ 1 นาที
- Fencing: ป้องกัน active namenode เก่า cause corruption ได้แก่ kill process revoke storage access หรือ STONITH

### Command-Line Interface

- hadoop fs -copyFromLocal -copyToLocal -mkdir -ls -cat
- File permissions คล้าย POSIX: read (r) write (w) execute (x)
- Hadoop filesystems: Local (file) HDFS (hdfs) HFTP WebHDFS HAR S3 (s3n) KFS FTP ViewFS

### Java Interface

- อ่านไฟล์ผ่าน FileSystem.get() -> fs.open(Path) -> FSDataInputStream
- FSDataInputStream รองรับ seek() readFully() positional reads
- เขียนไฟล์ผ่าน fs.create(Path) -> FSDataOutputStream
- ค้นหา metadata ด้วย getFileStatus() listStatus() globStatus()

### Data Flow การอ่านไฟล์

1. Client ขอ block locations จาก namenode (RPC)
2. Namode return datanode addresses เรียงตาม proximity
3. Client อ่าน data จาก datanode ใกล้ที่สุดโดยตรง
4. DFSInputStream จัดการ failover ถ้า datanode error

### Data Flow การเขียนไฟล์

1. Client สั่ง create namenode สร้าง file record
2. DFSOutputStream แบ่ง data เป็น packets ส่งผ่าน pipeline ของ 3 datanodes
3. แต่ละ datanode store packet แล้ว forward ไป datanode ถัดไป
4. Ack queue ติดตาม acknowledgements จากทุก datanode

### Replica Placement

- Replica 1: บน client node (ถ้าอยู่ใน cluster)
- Replica 2: ต่าง rack
- Replica 3: เดียวกับ rack 2 แต่ต่าง node
- สมดุลระหว่าง reliability write bandwidth read performance

### Network Topology

ระยะห่าง = ระยะถึง common ancestor ใกล้สุด same node=0 same rack=2 same DC diff rack=4 diff DC=6

### Coherency Model

ไฟล์ที่สร้างใหม่ visible ใน namespace แต่ data ที่เขียนยังไม่ visible จนกว่า flush/sync ป้องกัน data loss ได้ถึง 1 block

### distcp

เครื่องมือ copy ข้อมูล parallel ระหว่าง HDFS clusters implemented เป็น MapReduce job (map-only)

### Hadoop Archives (HAR)

แก้ปัญหา small files กิน memory namenode pack ไฟล์เล็กหลายไฟล์เข้า block เดียว

---

## บทที่ 4: Hadoop I/O

### Data Integrity

- HDFS ใช้ checksums ป้องกัน data corruption datanode ตรวจ checksum ตอน read
- LocalFileSystem: client-side checksums เขียนไฟล์ checksum คู่กับ data
- ChecksumFileSystem: wrap any FileSystem ด้วย checksums

### Compression

- **Codecs**: GZip (เร็ว good ratio) BZip2 (ช้า Splittable) LZO (fast not splittable) Snappy (fast moderate ratio) LZ4
- Splittable compression: BZip2 และ LZO (with indexing)
- ใช้ compression ใน MapReduce ผ่าน configuration properties

### Serialization

- **Writable interface**: Hadoop serialization compact fast แต่ไม่ flexible
- Writable classes: IntWritable Text BytesWritable NullWritable
- Custom Writable: implement write() และ readFields() methods
- **Avro**: schema-based serialization cross-language schema evolution
- **SequenceFile**: binary key-value format สำหรับ MapReduce รองรับ block/record compression
- **MapFile**: sorted SequenceFile with index สำหรับ random lookups

---

## บทที่ 5: พัฒนา MapReduce Application

- **Configuration API**: รวม resources จาก XML files variable expansion
- **GenericOptionsParser/Tool/ToolRunner**: standard ways ตั้ง input/output
- **Unit testing**: ใช้ LocalJobRunner รันทดสอบ MiniDFSCluster สำหรับ integration test
- **Running on cluster**: package JAR hadoop jar command monitor ผ่าน Web UI
- **Debugging**: Hadoop logs remote debugging counters
- **Workflow**: แบ่งปัญหาเป็นหลาย MapReduce jobs ใช้ JobControl หรือ Apache Oozie

---

## บทที่ 6: MapReduce Working Internals

### Classic MapReduce (MR1)

- JobTracker จัดการ job lifecycle TaskTracker รัน tasks
- TaskTracker ส่ง heartbeats ทุก 3 วินาที

### YARN (MR2)

- ResourceManager จัดการ resources ทั้ง cluster
- ApplicationMaster จัดการ application lifecycle
- NodeManager แทน TaskTracker

### Failures

ทั้ง MR1 และ YARN รองรับ task failures job failures node failures framework reschedule tasks ให้

### Job Scheduling

- Fair Scheduler: แชร์ resources ยุติธรรม
- Capacity Scheduler: รับประกัน capacity

### Shuffle and Sort

Map side: sort + partition + spill to disk Reduce side: fetch map outputs merge sort

### Speculative Execution

รัน duplicate task บน fast node เร็วขึ้นถ้า task ช้า

---

## บทที่ 7: Types และ Formats

- **Input Splits**: TextInputFormat (line-based) SequenceFileInputFormat (binary) DBInputFormat (JDBC)
- **Multiple Inputs**: ใช้ different Mapper classes สำหรับ different input formats
- **Output Formats**: TextOutputFormat SequenceFileOutputFormat MultipleOutputs LazyOutputFormat

---

## บทที่ 8: Features ขั้นสูง

- **Counters**: นับ event ใน jobs built-in (input/output records bytes) และ user-defined
- **Sorting**: Partial sort Total sort Secondary sort (sort values ใน mapper)
- **Joins**: Map-side join (efficient สำหรับ sorted data) Reduce-side join (flexible แต่ shuffle-heavy)
- **Side Data**: ใช้ Distributed Cache สำหรับ read-only data ที่ tasks ต้องการ

---

## บทที่ 9: ตั้งค่า Hadoop Cluster

- **Cluster Specification**: namenode + secondary namenode + resource manager + multiple node managers
- **Network Topology**: กำหนด rack awareness ด้วย script
- **Installation**: ติดตั้ง Java สร้าง hadoop user config SSH passwordless config XML files
- **Configuration**: core-site.xml hdfs-site.xml yarn-site.xml mapred-site.xml
- **YARN configuration**: memory limits container sizes scheduler settings
- **Security**: Kerberos authentication delegation tokens encryption
- **Benchmarking**: TestDFSIO NNBench MRBench
- **Cloud**: Amazon EC2 run Hadoop clusters on demand

---

## บทที่ 10: ดูแล Hadoop

- **HDFS Administration**: Persistent data structures (fsimage edit log) Safe mode audit logging balancer tool
- **Monitoring**: Logging Metrics (JMX) Ganglia integration
- **Maintenance**: Commissioning/decommissioning nodes rolling upgrades data migration

---

## บทที่ 11: Pig

Pig เป็น data flow language ที่รันบน HDFS + MapReduce ง่ายกว่าเขียน Java MapReduce ตรงๆ

### Pig Latin Basics

- **Statements**: LOAD STORE FILTER FOREACH GENERATE GROUP JOIN ORDER BY
- **Types**: int long float double chararray bytearray tuples bags maps
- **Schemas**: กำหนด column names และ types ตอน load data
- **UDFs**: Filter UDF (คืน boolean) Eval UDF (คืน value) Load UDF (คืน tuples)
- **Macros**: กำหนด logic ที่ reuse ได้

### Pig Execution

- **Mode**: Local (JVM เดียว) หรือ MapReduce cluster
- **Parallelism**: ตั้ง number of reducers ด้วย PARALLEL keyword
- **Parameter Substitution**: ใช้ -param ตอนรัน script

---

## บทที่ 12: Hive

Hive เป็น distributed data warehouse ที่เก็บ data ใน HDFS ใช้ SQL-like queries (HiveQL) ที่ compile เป็น MapReduce jobs

### Hive Architecture

- **Hive Services**: CLI JDBC/ODBC driver Web UI
- **Metastore**: metadata repository table definitions schemas partitions embedded Derby หรือ external database

### HiveQL

- **Schema on Read**: data อยู่ใน HDFS แล้ว schema กำหนดตอน query ต่างจาก RDBMS ที่ schema on write
- **Managed Tables vs External Tables**: Managed = Hive จัดการ lifecycle External = แค่ reference data
- **Partitions**: แบ่ง data โดย column values เช่น date query เร็วขึ้น
- **Buckets**: hash partitioning ใน partition สำหรับ efficient joins
- **Storage Formats**: Text SequenceFile RCFile ORC Parquet Avro

### Querying

- **Joins**: Map-side join (sorted) Reduce-side join bucket join (bucketed tables)
- **Subqueries**: รองรับ (nested SELECT)
- **Views**: virtual tables จาก complex queries
- **UDFs/UDAFs**: custom functions UDF คืน single value UDAF aggregate

---

## บทที่ 13: HBase

HBase เป็น distributed column-oriented database ที่รันบน HDFS สำหรับ random read/write ที่ low latency

### Data Model

- **Tables**: แบ่งเป็น column families (ตั้งตอน create table เปลี่ยนไม่ได้)
- **Rows**: key (BytesComparable) column families column qualifiers timestamps
- **Versioning**: เก็บหลาย versions ของ cell โดย timestamp
- **Coordinates**: row key + column family + column qualifier + timestamp

### Implementation

- **Region Servers**: จัดการ regions (horizontal partitions ของ table)
- **HBase Master**: จัดการ regions assignment DDL operations
- **Storage**: HDFS สำหรับ persistence ใช้ MemStore (write cache) + StoreFiles (HFiles read cache)

### Clients

- **Java API**: HTable Put Get Scan Delete Increment
- **REST (Avro, Thrift)**: HTTP interface สำหรับ non-Java clients

### HBase vs RDBMS

- HBase: Schema-flexible Automatic sharding Linear scaling High write throughput Eventually consistent
- RDBMS: Fixed schema Manual sharding Vertical scaling High read throughput Strongly consistent

---

## บทที่ 14: ZooKeeper

ZooKeeper เป็น distributed coordination service สำหรับ building distributed applications

### Data Model

- Tree ของ znodes แต่ละ node เก็บ data bytes + ACLs + state information
- 3 types: persistent ephemeral (delete เมื่อ session end) sequential

### Operations

- create delete exists get/set data get children sync
- Watches: one-time triggers บน data changes ทำให้ clients รู้การเปลี่ยนแปลงแบบ async

### Sessions

- Client เชื่อมต่อ ZooKeeper server ด้วย session (TCP connection)
- Session มี timeout ถ้าไม่ได้ heartbeat ทันเวลา session จะ expire

### Consistency

- Linearizable writes: คำสั่ง write execute ตามลำดับเดียว
- FIFO client order: คำสั่งจาก client เดียว execute ตามลำดับที่ส่ง

### Applications

- **Configuration Service**: store config data clients watch สำหรับ updates
- **Group Membership**: track members ของ distributed group (join leave list)
- **Lock Service**: distributed locks ด้วย ephemeral sequential nodes
- **Leader Election**: ใช้ sequential znodes หา leader

### Production

- รัน ZooKeeper ensemble 3-5 nodes (odd numbers)
- ใช้ dedicated hardware (fast disks plenty RAM)
- Monitor latency throughput client connections

---

## บทที่ 15: Sqoop

Sqoop ย้ายข้อมูลระหว่าง relational databases กับ HDFS/Hive/HBase

### Import

- sqoop import --connect jdbc:mysql://host/db --table employees
- สร้าง MapReduce job อ่านจาก database เขียนลง HDFS
- Generated code: Java classes สำหรับ records
- Direct mode: ใช้ native database tools เช่น mysqldump เร็วกว่า JDBC

### Export

- sqoop export --connect jdbc:mysql://host/db --table employees --export-dir /user/hadoop/employees
- อ่าน data จาก HDFS เขียนลง relational database
- ใช้ transactions 保证 consistency

### ใช้กับ Hive

- sqoop import --hive-import import ตรงเข้า Hive table
- สร้าง Hive table อัตโนมัติจาก database schema

---

## บทที่ 16: Case Studies

### Last.fm

ใช้ Hadoop สำหรับ generate charts สร้าง personalized chart สำหรับ user แต่ละคนจาก listening history

### Facebook

ใช้ Hive เป็น data warehouse query ข้อมูลจาก user activity logs หลาย PB ต่อวัน

### Nutch (Search Engine)

ใช้ Hadoop สำหรับ crawler + indexer จัดการ billions of web pages

### Rackspace (Log Processing)

ใช้ Hadoop + MapReduce สำหรับ email log analysis หา geographic distribution ของ users

### Cascading (ShareThis)

ใช้ Cascading abstraction layer บน MapReduce สร้าง data processing pipelines ที่ maintain ง่าย

### TeraByte Sort

ใช้ Hadoop sort 1 TB ใน 209 วินาทีบน 910 nodes ทำลาย world record
