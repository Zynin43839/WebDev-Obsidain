# Hadoop Beginner's Guide

> **Author:** Packt Publishing (398 pages)

---

## Chapter 1: What It's All About — Big Data & Hadoop

ยุค big data ทำให้ข้อมูลมหาศาลกลายเป็นทรัพยากรสำคัญ แต่เดิมมีแค่บริษัทใหญ่เท่านั้นที่เข้าถึง data processing ขนาดใหญ่ได้ เพราะhardware แพงและการ scale ทำได้ยาก

ระบบประมวลผลแบบเดิมมี 2 แนวทางหลัก: **scale-up** (ซื้อเครื่องใหญ่ขึ้น) กับ **scale-out** (เพิ่มเครื่องมากขึ้น) ทั้งสองมีข้อจำกัด — scale-up มีเพดาน hardware, scale-out ต้องเขียน distributed logic เอง ซึ่งยากและซับซ้อน

Hadoop เกิดจาก paper ของ Google (GFS + MapReduce) ที่ Doug Cutting นำมาพัฒนาเป็น open source ต่อมากลายเป็น top-level Apache project โดย Yahoo เป็นผู้สนับสนุนหลักคนแรก

## Chapter 2: Getting Hadoop Up and Running

ติดตั้ง Hadoop บน Ubuntu ต้องมี JDK, ตั้งค่า SSH key ให้ password-less, download Hadoop binary, ตั้ง environment variables (HADOOP_HOME, JAVA_HOME)

Hadoop มี 3 โหมด: **local standalone** (default, ทุกอย่างใน JVM เดียว), **pseudo-distributed** (คนละ JVM, เหมือน cluster จำลอง), **fully distributed** (หลายเครื่องจริง)

ตั้งค่า pseudo-distributed ต้องแก้ 3 XML: core-site.xml (NameNode address), hdfs-site.xml (replication factor = 1), mapred-site.xml (JobTracker address) แล้ว format NameNode ก่อนใช้งานครั้งแรก

ทดสอบด้วย `hadoop jar hadoop-examples pi 4 1000` — จะคำนวณค่า Pi ผ่าน MapReduce job แสดงให้เห็นว่า Hadoop แบ่งงานเป็น task หลายตัวแล้วรวมผล

## Chapter 3: Understanding MapReduce

MapReduce ทำงานกับ **key/value pairs** — input เป็น key/value, mapper แปลงเป็น key/value ใหม่, reducer รวมผล

Java API มี 3 คลาสหลัก: **Mapper** (ประมวลผลแต่ละ record), **Reducer** (aggregate ข้อมูล), **Driver** (ตั้งค่าและรัน job)

**Combiner** ช่วย reduce network traffic โดย aggregate บางส่วนฝั่ง mapper ก่อนส่ง — ใช้ได้เมื่อ logic เป็น commutative/associative

Hadoop มี data type เฉพาะ: **Writable** (serialization) และ **WritableComparable** (排序ได้) เช่น IntWritable, Text, รวมถึง array/map wrappers

Input/Output ใช้ **InputFormat** (split data) + **RecordReader** (อ่าน record) → mapper → **OutputFormat** + **RecordWriter** (เขียนผลลัพธ์)

## Chapter 4: Developing MapReduce Programs

**Hadoop Streaming** ใช้ภาษาใดก็ได้ (Ruby, Python) — อ่าน stdin เป็น key/value, เขียน stdout กลับ

กรณีศึกษา **UFO sighting data** — วิเคราะห์รูปแบบ UFO, ความสัมพันธ์ระหว่าง duration กับ shape, ใช้ Streaming scripts นอก Hadoop ได้

**ChainMapper** ใช้หลาย mapper ต่อกันใน job เดียว — เช่น validate fields แล้ว analyze ในขั้นตอนเดียว

**Distributed Cache** ให้ส่ง reference data (ไฟล์ lookup) ไปทุก node — ใช้เชื่อม zip code กับ location ได้

**Counter** สำหรับ track metrics, **status update** สำหรับ reporting progress, **log output** สำหรับ debug — ทุกอย่าง monitor ผ่าน JobTracker UI

## Chapter 5: Advanced MapReduce Techniques

**Joins**: Map-side join เร็วกว่า (Distributed Cache) แต่ต้อง fit ใน memory, Reduce-side join ใช้ MultipleInputs ได้แต่ช้ากว่า

**Graph algorithms** (เช่น PageRank): Represent graph เป็น adjacency list, ใช้ iterative MapReduce jobs — ส่ง message ผ่าน edges, aggregate ที่ reducer จน convergence

**Avro** — data serialization ที่ cross-language: define schema (JSON), produce/consume data ด้วย Ruby หรือ Java, ใช้ใน MapReduce ได้

Key lesson: หลายปัญหาดูเหมือนไม่ fit MapReduce แต่ break ออกเป็น map/reduce steps ได้เสมอ

## Chapter 6: When Things Break

Hadoop ออกแบบมาให้ **embrace failure** — hardware ราคาถูกจำนวนมาก ทำใจว่าตัวใดตัวหนึ่งต้องพัง

**DataNode failure**: NameNode ตรวจ replica แล้วschedule replication ใหม่ — HDFS ทน DataNode หายหลายตัวได้

**TaskTracker failure**: JobTracker reschedule task ไป node อื่น — **speculative execution** รัน task ซ้ำบน node เร็วถ้าตัวเดิมช้า

**NameNode failure** ร้ายแรงกว่า — ต้อง restore จาก **fsimage** + **edit log** หรือใช้ **SecondaryNameNode** / **BackupNode**

**Dirty data** จัดการได้ 2 ทาง: เขียน skip logic ใน code หรือใช้ **Hadoop skip mode** ที่ข้าม records ที่ fail ซ้ำๆ

## Chapter 7: Keeping Things Running

**Configuration properties** มี default values ทุกตัว — แก้ใน XML ไฟล์, site-specific overrides อยู่ใน `-site.xml`

ตั้ง cluster: คำนวณ usable space (replication factor), วาง master nodes แยก, เลือก hardware ratio (CPU:RAM:Storage)

**Networking**: Hadoop วาง blocks ตาม **rack awareness** — replica กระจาย across racks เพื่อ fault tolerance + ลด network bottleneck

**Security model**: default ไม่มี authentication — ต้อง disable for production, ใช้ **Kerberos** หรือ physical access control

**NameNode management**: config fsimage backup locations, เตรียม swap NameNode host ไว้ก่อน disaster, fsimage คือ single most important data

**HDFS management**: ใช้ **balancer** เมื่อ data skew, write data บน nodes ที่มีพื้นที่

**MapReduce management**: จัด job priority, ใช้ **Fair Scheduler** หรือ **Capacity Scheduler** แทน default FIFO

## Chapter 8: A Relational View on Data with Hive

**Hive** ให้ query Hadoop ด้วย SQL-like syntax (HQL) — เหมาะสำหรับคนคุ้น relational database

สร้าง table, load data, query, join, ใช้ **views** — ทุกอย่างคล้าย SQL แต่แปลเป็น MapReduce jobs ใต้ hood

**Partitioning** แบ่ง table ตามค่า column (เช่น year/month) — query เร็วขึ้นเพราะ skip partitions ที่ไม่เกี่ยว

**UDF (User Defined Function)** สร้างฟังก์ชันใหม่ใน Hive — extend ด้วย Java

**Hive บน AWS (EMR)**: run HQL queries บน Elastic MapReduce, ใช้ interactive job flows สำหรับ dev/test

## Chapter 9: Working with Relational Databases

**Data paths** ระหว่าง Hadoop กับ RDBMS: Hadoop เป็น archive store, preprocessing step, หรือ data input tool

**Sqoop** — tool สำหรับ import/export ระหว่าง relational DB ↔ HDFS/Hive: `sqoop import`, `sqoop export`

Import แบบ selective (where clause), type mapping, raw query import — จัดการ datatype issues ได้

Export จาก Hadoop กลับ MySQL — ต่างจาก import ตรงที่ destination table ต้องมีอยู่แล้ว

**AWS RDS** เป็น alternative สำหรับ managed relational DB คู่กับ Hadoop

## Chapter 10: Data Collection with Flume

**Apache Flume** — framework สำหรับ collect streaming data (logs, network traffic) เข้า Hadoop

Architecture: **Source** (รับ data) → **Channel** (buffer) → **Sink** (เขียน destination เช่น HDFS)

ตั้งค่า Flume ผ่าน config files — event-based, กำหนด source type, sink type, channel type

**Multi-level Flume networks**: chain agents เข้าด้วยกัน — fan-in/fan-out, replicating/multiplexing selectors

จัดการ sink failure ด้วย failover channels — data ไม่หายแม้ sink หนึ่งล่ม

## Chapter 11: Where to Go Next

**Hadoop ecosystem** มีหลาย tools เพิ่มเติม: **HBase** (NoSQL database on Hadoop), **Oozie** (workflow scheduler), **Mahout** (machine learning), **Pig** ( scripting layer)

**Alternative distributions**: Cloudera, Hortonworks, MapR — รวม packaging, support, และ extensions

**Other programming abstractions**: Pig (data flow), Cascading (Java-based workflow)

**AWS services**: HBase on EMR, SimpleDB, DynamoDB — เพิ่ม flexibility สำหรับ different use cases

**Community resources**: source code, mailing lists, LinkedIn groups, HUGs (Hadoop User Groups), conferences

---
