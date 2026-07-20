# The Data Warehouse Toolkit, 3rd Edition
> **Author:** Ralph Kimball, Margy Ross

## สรุปหนังสือ

### ภาพรวม
หนังสือเล่มนี้เป็น "คัมภีร์" ของวงการ Data Warehouse และ Business Intelligence เขียนโดย Ralph Kimball (ผู้บุกเบิกแนวทาง Dimensional Modeling) และ Margy Ross (ประธาน Kimball Group) ตีพิมพ์ครั้งแรกปี 1996 เล่มที่ 3 นี้อัปเดตเทคนิคใหม่ๆ ครอบคลุม 21 บท พร้อม case studies จาก 14 อุตสาหกรรม

---

### ส่วนที่ 1: พื้นฐาน DW/BI และ Dimensional Modeling (บท 1-2)

#### โลกของ Data Capture vs Data Analysis
- **Operational Systems** = ระบบบันทึกธุรกรรม เน้นความเร็ว  처리ทีละรายการ ไม่เก็บประวัติ
- **DW/BI Systems** = ระบบวิเคราะห์ เน้น Query ประสิทธิภาพสูง ต้องดึงข้อมูลหลายแสนรายการมารวมกัน ต้องเก็บประวัติ

#### เป้าหมาย 6 ข้อของ DW/BI
1. **เข้าถึงง่าย (Simple & Fast)** — ข้อมูลต้องเข้าใจง่าย Query เร็ว
2. **สม่ำเสมอ (Consistent)** — ข้อมูลจากหลายแหล่งต้องมีความหมายเดียวกัน
3. **ปรับตัวได้ (Adaptable)** — รองรับการเปลี่ยนแปลงโดยไม่ทำลายข้อมูลเดิม
4. **ทันเวลา (Timely)** — ส่งข้อมูลได้ภายในชั่วโมง/นาที/วินาที ตามต้องการ
5. **ปลอดภัย (Secure)** — ปกป้องข้อมูล crown jewels ขององค์กร
6. **เป็นฐานการตัดสินใจ (Authoritative Foundation)** — ข้อมูลต้องถูกต้องเชื่อถือได้
7. **ผู้ใช้ยอมรับ (Business Acceptance)** — ถ้าผู้ใช้ไม่ใช้ = ล้มเหลว

#### Publishing Metaphor — ผู้จัดการ DW/BI = บรรณาธิการนิตยสาร
- ต้องเข้าใจ "ผู้อ่าน" (business users) — ว่าต้องการอะไร
- ต้องเสิร์ฟเนื้อหาคุณภาพสูง น่าสนใจ ตรงประเด็น
- ต้องรักษาความเชื่อมั่น อัปเดตสม่ำเสมอ
- **Key insight**: เทคโนโลยีเป็นแค่เครื่องมือ ไม่ใช่เป้าหมาย

---

### Dimensional Modeling คืออะไร?

#### Star Schema vs OLAP Cube
- **Star Schema** = ใช้ relational database มี fact table ตรงกลาง ล้อมรอบด้วย dimension tables รูปดาว
- **OLAP Cube** = multidimensional database ใช้ pre-aggregation index เร็วกว่าแต่ flexible น้อยกว่า
- **คำแนะนำ**: เก็บ atomic data ใน star schema แล้ว optionally สร้าง OLAP cube จากนั้น

#### Fact Table (ตารางข้อเท็จจริง)
- เก็บ **ตัวเลขวัดได้ (measurements)** จากเหตุการณ์ธุรกิจ
- แต่ละ row = 1 measurement event ที่ grain เดียวกัน (ห้าม mixing grain!)
- **Additive Facts** = บวกได้ทุกมิติ เช่น ยอดขาย (ดีที่สุด)
- **Semi-Additive Facts** = บวกได้บางมิติ เช่น ยอดเงินในบัญชี (ห้ามบวกข้ามเวลา)
- **Non-Additive Facts** = บวกไม่ได้ เช่น ราคาเฉลี่ย (ต้องใช้ count/avg)
- Fact tables ใหญ่มาก (90%+ ของ DB size) แต่แถวแคบ (narrow)

#### Dimension Table (ตารางมิติ)
- เก็บ **บริบทเชิงพรรณนา** — ตอบคำถาม "who, what, where, when, how, why"
- มักมี 50-100 attributes เช่น Product Dimension มี brand, category, package type ฯลฯ
- **ห้าม normalize (snowflake)** เก็บข้อมูลซ้ำเพื่อความง่าย — dimension table เล็กอยู่แล้ว
- Dimension attributes = source ของ query filters, groupings, report labels
- **ยิ่งมี dimension attributes มาก = ยิ่งวิเคราะห์ได้มาก**

#### Star Schema = Fact + Dimension รวมกัน
- รูปดาว: fact table ตรงกลาง dimension tables ล้อมรอบ
- ข้อดี: เข้าใจง่าย, query เร็ว (optimizer ทำ n-way join ได้ใน single pass), ขยายง่าย (เพิ่ม dimension/fact ได้โดยไม่ต้อง reload)

---

### Kimball DW/BI Architecture (4 ชั้น)

1. **Operational Source Systems** — ระบบบันทึกธุรกรรมต้นทาง (ERP, CRM ฯลฯ)
2. **ETL System (Back Room)** — แปลง ทำความสะอาด ปรับรูปแบบ แล้วโหลด
3. **Presentation Area** — star schema / OLAP cube ที่ผู้ใช้ query ได้
4. **BI Applications** — ad hoc queries, reports, dashboards, data mining

#### Restaurant Metaphor
- **Kitchen (ETL)** = รับวัตถุดิบดิบ → แปลงเป็นอาหารคุณภาพ — ห้ามลูกค้าเข้า
- **Dining Room (Presentation)** = เสิร์ฟอาหารให้ลูกค้า — ต้องอร่อย สะอาด ตรงเวลา
- **Kitchen goals**: throughput สูง, คุณภาพสม่ำเสมอ, integrity ดี

---

### สถาปัตยกรรมทางเลือก

1. **Independent Data Mart** ❌ — แต่ละแผนกสร้าง data mart เอง → ข้อมูลไม่เชื่อมกัน (stovepipe)
2. **Hub-and-Spoke (Inmon)** — สร้าง normalized enterprise DB แล้วแยก data marts → ซับซ้อน แพง
3. **Hybrid** — Inmon + Kimball ผสมกัน
4. **Kimball Bus Architecture** ✅ — ใช้ conformed dimensions เชื่อม dimensional models ทั้งองค์กร → สร้าง incrementally แบบ decentralized

#### 5 Myths ของ Dimensional Modeling
1. ❌ "Dimensional model เก็บแค่ summary data" → เก็บ atomic data ได้
2. ❌ "เป็นของแผนก ไม่ใช่ enterprise" → ใช้ conformed dimensions เชื่อมทั้งองค์กรได้
3. ❌ "Scale ไม่ได้" → scale ได้ดี
4. ❌ "ใช้ได้แค่กับ usage pattern ที่คาดเดาได้" → รองรับ ad hoc queries ได้
5. ❌ "รวม data จากหลายแหล่งไม่ได้" → ใช้ conformed dimensions เชื่อมได้

---

### ส่วนที่ 2: เทคนิค Dimensional Modeling แบบครบถ้วน (บท 2)

บทนี้สรุปเทคนิค 75+ ของ Kimball Group:

#### 4-Step Dimensional Design Process
1. **เลือก Business Process** — เช่น Sales, Inventory, Procurement
2. **กำหนด Grain** — แต่ละ row ใน fact table แทนอะไร (เช่น 1 row = 1 product ต่อ 1 transaction)
3. **ระบุ Dimensions** — ใคร, อะไร, ที่ไหน, เมื่อไหร่
4. **ระบุ Facts** — ตัวเลขวัดอะไร

#### Basic Fact Table Techniques
- **Transaction Fact Table** — บันทึกเหตุการณ์ทีละรายการ (common ที่สุด)
- **Periodic Snapshot Fact Table** — ถ่ายภาพเป็นช่วงๆ (เช่น ยอดคงเหลือรายวัน)
- **Accumulating Snapshot Fact Table** — ติดตาม lifecycle/pipeline (เช่น order lifecycle)
- **Factless Fact Table** — มีแค่ keys ไม่มีตัวเลข (เช่น นักเรียนลงทะเบียนเรียน)
- **Aggregate Fact Table** — summary เพื่อ performance

#### Basic Dimension Table Techniques
- **Surrogate Key** — ระบบ-generated key (ไม่ใช่ natural key จาก source)
- **Degenerate Dimension** — dimension ที่อยู่ใน fact table เอง (เช่น transaction number)
- **Role-Playing Dimension** — dimension เดียวกันใช้หลาย role (เช่น date_dim: order_date, ship_date)
- **Junk Dimension** — รวม flags/indicators หลายตัวเข้า table เดียว
- **Snowflake / Outrigger** — หลีกเลี่ยงถ้าทำได้ (เก็บ denormalized ดีกว่า)
- **Calendar Date Dimension** — ต้องมี pre-populated rows สำหรับทุกวัน

#### Conformed Dimensions — กุญแจสำคัญ!
- Dimension ที่ใช้ร่วมกันทั้งองค์กร (เช่น date_dim, product_dim, store_dim)
- **Identical Conformed** — เหมือนกันทุกประการ
- **Shrunken Conformed** — เล็กลง (subset ของ attributes หรือ rows)
- **Drill Across** — query ข้าม fact tables โดยใช้ conformed dimensions
- **Bus Matrix** — ตาราง business processes × dimensions บอกว่า process ไหนใช้ dimension ไหน

#### Slowly Changing Dimensions (SCD) — 7 Types
| Type | วิธี | ใช้เมื่อ |
|------|------|----------|
| **0** | Retain Original | ข้อมูลไม่เปลี่ยน (เช่น birth date) |
| **1** | Overwrite | ไม่ต้องการ track history |
| **2** | Add New Row (มี effective/expiration dates) | **ต้องการ track history** (ใช้มากที่สุด) |
| **3** | Add New Column | ต้องการเฉพาะ previous value |
| **4** | Mini-Dimension | ข้อมูลเปลี่ยนบ่อยมาก |
| **5** | Mini-Dimension + Type 1 Outrigger | ผสม 4+1 |
| **6** | Type 1 + Type 2 | ต้องการทั้ง history + current rollup |
| **7** | Dual Type 1 + Type 2 | ดูได้ทั้ง current snapshot + historical |

---

### ส่วนที่ 3: Case Studies 14 อุตสาหกรรม (บท 3-16)

#### 3. Retail Sales — Classic Example
- Fact: ยอดขายต่อ product ต่อ transaction
- Dimensions: Date, Product, Store, Promotion, Customer, Clerk
- **Lesson**: เริ่มจาก simplest example แล้วค่อยๆ ขยาย

#### 4. Inventory — Bus Architecture Introduction
-  introduce **Enterprise Data Warehouse Bus Matrix**
- 3 fact table types เทียบกัน: Transaction vs Periodic Snapshot vs Accumulating Snapshot
- **Conformed Dimensions** เชื่อม Retail Sales + Inventory เข้าด้วยกัน

#### 5. Procurement — SCD Deep Dive
- SCD Type 0-7 อธิบายละเอียดพร้อมตัวอย่าง
- **Value Chain** — มองภาพรวม business processes ทั้ง supply chain

#### 6. Order Management — Role-Playing & Multiple Facts
- **Role-Playing Dimensions**: order_date, ship_date, invoice_date ใช้ date_dim เดียวกัน
- **Header/Line Pattern**: order_header + order_line (หลีกเลี่ยงถ้าทำได้)
- **Multiple Currencies**: เก็บทั้ง transaction currency + reporting currency
- **Accumulating Snapshot**: ติดตาม order fulfillment pipeline

#### 7. Accounting — Hierarchies & Consolidated Facts
- **General Ledger**: periodic snapshot + chart of accounts
- **Year-to-Date Facts**: semi-additive (sum across time ไม่ได้)
- **Dimension Hierarchies**: fixed depth, ragged/variable depth
- **Bridge Tables** สำหรับ ragged hierarchies (เช่น organization chart)
- **Consolidated Fact Tables**: รวมข้อมูลจากหลาย business processes

#### 8. CRM — Customer Dimension Deep Dive
- **Name/Address Parsing**: แยกชื่อ-ที่อยู่เป็นส่วนๆ (สำหรับ international)
- **Aggregated Facts as Dimension Attributes**: เช่น lifetime value ใน dimension
- **Bridge Tables**: multivalued dimensions (เช่น customer หลาย contact)
- **Behavior Study Groups**: cohort analysis
- **Customer Data Integration**: Master Data Management (MDM)

#### 9. HR — Fact Table จาก Dimension
- Dimension ที่ "เกือบ" เป็น fact table (เช่น employee profile tracking)
- **Recursive Hierarchies**: management chain
- **Survey Questionnaire Data**: การทำแบบสำรวจ

#### 10. Financial Services — Heterogeneous Products
- **Supertype/Subtype Schema**: ผลิตภัณฑ์ต่างประเภทกันมี attributes/measurements ต่างกัน
- **Household Dimension**: ความสัมพันธ์ account-customer-household
- **Hot Swappable Dimensions**: เปลี่ยน dimension ได้ตามเวลา

#### 11. Telecommunications — Design Review
- บทนี้ชวนหาข้อผิดพลาดใน model ที่ดูเหมือนถูกต้อง
- **Geographic Location Dimension**: จัดการข้อมูลภูมิศาสตร์

#### 12. Transportation — Multiple Granularity Facts
- Fact tables หลายระดับ granularity เชื่อมกัน (segment → trip)
- **Multiple Time Zones**: จัดการ timezone ต่างกัน

#### 13. Education — Factless Fact Tables
- **Admissions Events, Course Registrations** = factless fact tables
- **Accumulating Snapshot**: ติดตาม applicant pipeline, research grant pipeline

#### 14. Healthcare — Complex Models
- **Bridge Tables**: multiple diagnoses + providers ต่อ patient treatment
- **EMR (Electronic Medical Records)**: ข้อมูลทางการแพทย์ซับซ้อน
- **Retroactive Changes**: แก้ไขข้อมูลย้อนหลัง

#### 15. E-Commerce — Clickstream Data
- **Clickstream**: ข้อมูลการคลิกเว็บ — dimensionality ไม่เหมือนธุรกิจอื่น
- **Step Dimension**: ติดตาม sequential steps (เช่น checkout process)
- **Page/Event/Session Dimensions**

#### 16. Insurance — Pulling It All Together
- รวมเทคนิคทั้งหมด: SCD, bridge tables, accumulating snapshots, heterogeneous products
- **Claim Lifecycle**: ติดตามตั้งแต่ policy → claim → payment
- **10 Common Modeling Mistakes** (นับจากบนลงล่าง):
  1. ไม่ conform facts/dimensions
  2. คาดหวังให้ users query normalized data
  3. ใช้ report ออกแบบ model
  4. ไม่ declare grain
  5. ใช้ operational keys แทน surrogate keys
  6. แก้ performance ด้วย hardware อย่างเดียว
  7. ไม่ track dimension changes
  8. แบ่ง hierarchies เป็นหลาย dimension
  9. จำกัด verbose descriptors เพื่อประหยัดพื้นที่
  10. ใส่ text attributes ใน fact table

---

### ส่วนที่ 4: DW/BI Lifecycle (บท 17-20)

#### 17. Kimball Lifecycle Overview
- **4 Tracks**: Planning → Technology → Data → BI Applications
- **Phase 1**: Project Planning + Business Requirements Definition
- **Phase 2**: Technical Architecture + Product Selection
- **Phase 3**: Dimensional Modeling + Physical Design + ETL Design
- **Phase 4**: BI Application Development + Deployment
- **Phase 5**: Maintenance + Growth

#### 18. Dimensional Modeling Process
- **Workshop** กับ business users เป็นกุญแจสำคัญ
- ใช้ **Bubble Chart** สำหรับ high-level design
- **Data Profiling** ก่อนออกแบบ
- **Naming Conventions** ต้องชัดเจนตั้งแต่แรก

#### 19. ETL Subsystems — 34 ระบบย่อย!

**Extracting (3 subsystems)**:
1. Data Profiling
2. Change Data Capture (CDC)
3. Extract System

**Cleaning & Conforming (5 subsystems)**:
4. Data Cleansing
5. Error Event Schema
6. Audit Dimension Assembler
7. Deduplication
8. Conforming System

**Delivering (13 subsystems)**:
9. SCD Manager (จัดการ Type 0-7)
10. Surrogate Key Generator
11. Hierarchy Manager
12. Special Dimensions Manager
13. Fact Table Builders
14. Surrogate Key Pipeline
15. Multivalued Dimension Bridge Table Builder
16. Late Arriving Data Handler
17. Dimension Manager System
18. Fact Provider System
19. Aggregate Builder
20. OLAP Cube Builder
21. Data Propagation Manager

**Managing (13 subsystems)**:
22. Job Scheduler
23. Backup System
24. Recovery & Restart
25. Version Control
26. Version Migration
27. Workflow Monitor
28. Sorting System
29. Lineage & Dependency Analyzer
30. Problem Escalation
31. Parallelizing/Pipelining
32. Security System
33. Compliance Manager
34. Metadata Repository Manager

#### 20. ETL Design & Development
- **10 Steps**: High-level plan → Choose ETL tool → Default strategies → Drill down → Spec doc → Historical load (dimensions then facts) → Incremental load → Aggregate/OLAP loads → Automation
- **Real-Time ETL**: Trade-offs ระหว่าง latency vs complexity

---

### ส่วนที่ 5: Big Data Analytics (บท 21)

#### Big Data Architecture Options
- **Extended RDBMS** — ขยาย relational DB ด้วย appliance/in-memory
- **MapReduce/Hadoop** — distributed processing สำหรับ data ขนาดใหญ่
- **Comparison**: ไม่ต้องเลือกอย่างใดอย่างหนึ่ง — ใช้ร่วมกันได้

#### Best Practices for Big Data
- **Management**: ต้องมี strategy ชัดเจน ไม่ใช่แค่ adopts technology
- **Architecture**: DW/BI + Big Data ทำงานร่วมกัน — Big Data เป็น extension ไม่ใช่ replacement
- **Data Modeling**: Dimensional modeling ยังใช้ได้กับ Big Data
- **Data Governance**: คุณภาพข้อมูล important เท่าเดิม

---

### Key Takeaways ทั้งเล่ม

1. **Simple = Powerful** — Dimensional modeling ชนะเพราะเข้าใจง่าย
2. **Conformed Dimensions คือกุญแจ** — เชื่อมทั้งองค์กรเข้าด้วยกัน
3. **Grain ต้องชัด** — กำหนด grain ก่อนเสมอ แล้วทำตาม
4. **Business-driven** — ออกแบบจาก business needs ไม่ใช่ technology
5. **Atomic data ก่อน** — เก็บข้อมูลระดับละเอียดที่สุด แล้ว aggregate ทีหลัง
6. **Surrogate keys เสมอ** — ห้ามใช้ operational keys ใน dimensional model
7. **ETL คือ 60-80% ของงาน** — อย่ามองข้าม
8. **Bus architecture** — ใช้ conformed dimensions สร้าง incrementally
9. **SCD Type 2** เป็น default สำหรับ tracking history
10. **User adoption = ความสำเร็จ** — ถ้าผู้ใช้ไม่ใช้ = ล้มเหลว

---
