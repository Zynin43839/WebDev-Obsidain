# Site Reliability Engineering

> **Author:** Betsy Beyer, Chris Jones, Jennifer Petoff, and Niall Richard Murphy
> **Source:** `meow-webdev` | **Slug:** `meow-webdev-site-reliability-engineering` | **Pages:** 550

---

## Part I: Introduction

### บทที่ 1: ความเป็นมาของ Site Reliability Engineering (SRE)

SRE คือแนวคิดที่เกิดขึ้นเมื่อ Ben Treynor Sloss วิศวกรซอฟต์แวร์ของ Google ถูกมอบหมายให้ดูแล "Production Team" ที่มีสมาชิก 7 คนในปี 2003 เขาออกแบบทีมในแบบที่ตัวเองอยากให้มันทำงาน — โดยใช้ซอฟต์แวร์-engineering มาแก้ปัญหา operations

**แบบเดิม (Sysadmin Approach):**
- ใช้ sysadmin รันบริการที่ซับซ้อน โดยมี dev team กับ ops team แยกกันชัดเจน
- Dev ต้องการปล่อย features เร็ว ส่วน Ops ต้องการระบบไม่พัง
- ทีมสองฝั่งมี vocabulary ต่างกัน, ต่างมี assumptions ต่างกันเรื่อง risk
- ผลลัพธ์คือ trench warfare — ops ตั้ง launch gates, dev ก็หาทาง绕 ypass

**แบบ SRE ของ Google:**
- จ้าง **software engineer** มาทำงาน operations (ไม่ใช่ sysadmin ทั่วไป)
- ทีม SRE ประกอบด้วย 50-60% software engineer ตามมาตรฐาน Google + 40-50% คนที่เก่งด้าน UNIX internals, networking
- กำหนด **operational work cap 50%** — SRE ต้องใช้เวลาอย่างน้อย 50% ไปกับ engineering projects
- เป้าหมาย: ทำให้ระบบ run และ repair ตัวเองได้ — "systems that are automatic, not just automated"

**Error Budget (งบประมาณข้อผิดพลาด):**
- 100% ไม่ใช่เป้าหมายความน่าเชื่อถือที่ถูกต้อง — ไม่มี user คนไหนแยกออกระหว่าง 99.99% กับ 100%
- Error Budget = 1 - availability target เช่น SLO 99.99% = error budget 0.01%
- ตราบใดที่ error budget ยังเหลือ สามารถปล่อย release ได้ตามปกติ
- เมื่อ budget หมด → หยุด release, หันมา investment ใน reliability
- แก้ปัญหา conflict ระหว่าง dev (ต้องการ velocity) กับ SRE (ต้องการ stability)

**DevOps vs SRE:**
- DevOps เป็น generalization ของหลักการ SRE ไปใช้กับองค์กรที่หลากหลาย
- SRE เป็น implementation ที่ specific ของ DevOps โดยเน้น reliability เป็นหลัก
- ทั้งสองใช้ automation, engineering practices, และ collaboration เป็นแกนหลัก

**Tenets ของ SRE:**
1. ** Ensuring Durable Focus on Engineering** — cap ops ที่ 50%, on-call max 2 events ต่อ shift
2. **Maximum Change Velocity** — ไม่ละเมิด SLO โดยใช้ error budget
3. **Monitoring** — software ควรตีความ alert, human ควรรับ alert เฉพาะตอนที่ต้องทำอะไรจริงๆ
4. **Emergency Response** — MTTR (mean time to repair) สำคัญกว่า MTTF, playbook ช่วยได้ 3x
5. **Change Management** — ~70% ของ outage เกิดจากการเปลี่ยนแปลง live system
6. **Capacity Planning** — ต้องมี sufficient capacity ก่อนที่จะต้องการ
7. **Efficiency and Performance** — utilization เป็น function ของ demand, capacity, software efficiency

---

### บทที่ 2: Production Environment ของ Google

**Hardware:**
- Google ใช้ datacenter ที่ออกแบบเอง โดย hardware ทั้งหมดเป็น proprietary — power distribution, cooling, networking, compute
- ใช้คำว่า **Machine** สำหรับ hardware/vm และ **Server** สำหรับ software ที่ run บริการ
- Topology: tens of machines → rack → row → cluster → datacenter building → campus

**Borg (Cluster Operating System):**
- จัดการ jobs ที่มีทั้ง long-running servers และ batch processes เช่น MapReduce
- แต่ละ task ได้รับ name/index จาก Borg Naming Service (BNS) แทน IP address ตรงๆ
- BNS path เช่น `/bns/<cluster>/<user>/<job>/<task>` แปลเป็น IP:port
- Borg ทำ binpacking ของ tasks บน machines โดยพิจารณา failure domains

**Storage Stack:**
- ชั้นล่างสุด: **D (disk)** — ใช้ทั้ง spinning disks และ flash storage
- ชั้นบน D: **Colossus** — filesystem ระดับ cluster ที่มี replication + encryption (successor ของ GFS)
- ชั้นฐานข้อมูล:
  - **Bigtable** — NoSQL ระดับ petabytes, eventually consistent, รองรับ cross-datacenter replication
  - **Spanner** — SQL-like interface ที่มี real consistency ทั่วโลก
  - **Blobstore** — สำหรับ binary data

**Networking:**
- ใช้ **OpenFlow-based software-defined network** แทน "smart" routing hardware
- ใช้ switching hardware ราคาถูก + central controller ที่ precompute best paths
- **Bandwidth Enforcer (BwE)** จัดการ bandwidth allocation เพื่อ maximize average bandwidth
- **Global Software Load Balancer (GSLB)** ทำ load balancing 3 ระดับ:
  1. Geographic load balancing สำหรับ DNS
  2. Load balancing ระดับ service (เช่น YouTube, Google Maps)
  3. Load balancing ระดับ RPC

**Infrastructure Components อื่นๆ:**
- **Chubby** — lock service ใช้ Paxos protocol สำหรับ distributed consensus, ใช้สำหรับ master election
- **Borgmon** — monitoring program ที่ "scrape" metrics จาก servers อย่างสม่ำเสมอ
- **Stubby/gRPC** — RPC infrastructure ที่ทุก service ใช้สื่อสารกัน
- **Protocol Buffers** — ใช้ serialization ข้อมูล, เล็กกว่า XML 3-10x, เร็วกว่า 20-100x

**Shakespeare Example Service:**
- ตัวอย่าง service ที่ค้นหาคำในบทละครของ Shakespeare
- Batch component ใช้ MapReduce สร้าง index แล้วเก็บใน Bigtable
- Frontend component รับ request จากผู้ใช้, ติดต่อ backend ผ่าน GSLB, แล้ว query Bigtable
- Request chain: DNS → GSLB → Google Frontend (GFE) → Shakespeare Frontend → Shakespeare Backend → Bigtable
- ทั้งหมดนี้ทำงานในไม่กี่ร้อยมิลลิวินาที

---

## Part II: Principles

### บทที่ 3: Embracing Risk (การยอมรับความเสี่ยง)

**การจัดการความเสี่ยง:**
- การเพิ่ม reliability ไม่ได้ราคาถูก — incremental improvement อาจแพงขึ้น 100x
- ต้นทุน 2 ด้าน: (1) redundant resources (2) opportunity cost ของวิศวกรที่ไม่ได้ทำ features
- เป้าหมาย: **make a service reliable enough, but no more reliable than it needs to be**
- มอง availability target เป็นทั้ง minimum และ maximum

**วัดความเสี่ยงของ service:**
- ใช้ **request success rate** แทน time-based availability (เพราะ Google serve traffic ทั่วโลกตลอดเวลา)
- สูตร: `availability = successful requests / total requests`
- ตั้ง quarterly targets แล้ว track performance เป็นรายสัปดาห์หรือรายวัน

**Risk Tolerance ของ Service:**
- **Consumer services:** พิจารณาจาก level of service ที่ users คาดหวัง, revenue impact, competitors, paid vs free
  - Google Apps for Work (enterprise) → target 99.9% + SLA with penalties
  - YouTube (consumer, growing fast) → target ต่ำกว่า เพราะต้องการ feature velocity สูง
- **Types of failures:** full outage vs partial — เช่น expose private contacts ร้ายแรงกว่า profile picture fail
- **Cost:** ถ้า cost of improving one nine > revenue increase → ไม่คุ้ม
- **Infrastructure services:** ต้องมี explicit levels of service เพื่อให้ clients เลือก cost/risk trade-off ที่เหมาะสม

**Error Budget (งบประมาณข้อผิดพลาด):**
- Product Management กำหนด SLO → monitoring วัด actual uptime → ความต่างคือ error budget
- เมื่อ budget ยังเหลือ → ปล่อย release ได้ → เมื่อหมด → หยุด release, ลงทุนใน reliability
- **Benefits:** ทำให้ dev team self-police — รู้ budget, จัดการ risk ด้วยตัวเอง
- ทุกคนรับผิดชอบ uptime ร่วมกัน — network outage หรือ datacenter failure ก็กิน budget เหมือนกัน

**Key Insights:**
- 100% ไม่ใช่เป้าหมายที่ถูกต้อง — user ไม่เห็นความต่างระหว่าง 99.99% กับ 100%
- Error budget ทำให้ dev กับ SRE มี incentive เดียวกัน — joint ownership

---

### บทที่ 4: Service Level Objectives (เป้าหมายระดับบริการ)

**ศัพท์สำคัญ:**
- **SLI (Service Level Indicator)** — ตัวชี้วัดเชิงปริมาณของ service level เช่น request latency, error rate, throughput
- **SLO (Service Level Objective)** — เป้าหมายสำหรับ SLI เช่น "99% of requests < 100ms"
- **SLA (Service Level Agreement)** — สัญญาที่มี consequences (มักเป็น financial) หากไม่ทำตาม SLO

**SLI ในทางปฏิบัติ:**
- **User-facing serving:** ให้ความสำคัญกับ availability, latency, throughput
- **Storage systems:** ให้ความสำคัญกับ latency, availability, durability
- **Big data systems:** ให้ความสำคัญกับ throughput, end-to-end latency
- **ทุก system ควร care เรื่อง correctness**
- วัดทั้ง server-side และ client-side (ไม่งั้นจะพลาดปัญหาที่ user เจอจริงๆ)

**Aggregate อย่างระมัดระวัง:**
- อย่าใช้ average เฉยๆ — ต้องดู percentiles (50th, 95th, 99th, 99.9th)
- User study แสดงว่าคนชอบ system ที่ช้าเล็กน้อยแต่ variance ต่ำ มากกว่า system ที่เร็วแต่ variance สูง
- ต้องระวัง statistical fallacies — data อาจไม่ normally distributed

**SLO ในทางปฏิบัติ:**
- คิดจาก "users ต้องการอะไร" ก่อน แล้วค่อยหาวิธีวัด (ไม่ใช่วัดสิ่งที่ง่ายก่อน)
- อย่าตั้ง target จาก performance ปัจจุบัน — อาจล็อคตัวเองเข้ากับระบบที่ต้อง "heroic effort"
- ใช้ SLO น้อยที่สุดเท่าที่จำเป็น — ถ้า defend SLO ไม่ได้ ก็ไม่คุ้มที่จะมี
- สามารถ loosen SLO ได้ถ้า strict เกินไป — เริ่มจาก loose แล้วค่อย tighten ดีกว่า

**Control Measures:**
1. Monitor and measure SLIs
2. เปรียบเทียบ SLIs กับ SLOs, ตัดสินใจว่าต้องทำอะไร
3. หาว่าต้องทำอะไรเพื่อให้ถึง target
4. ลงมือทำ

**Chubby Planned Outage:**
- Chubby มี reliability สูงมากจน service owners วางใจ dependencies มากเกินไป
- แก้ไขโดยจงใจ shutdown Chubby บ่อยๆ เพื่อ force service owners รับมือกับ failure
- ไม่ overachieve SLO — ถ้า performance ดีกว่าที่บอก จะทำให้ users over-depend

---

### บทที่ 5: Eliminating Toil (กำจัดงานจำเจ)

**Toil คืออะไร:**
- งานที่เกี่ยวข้องกับ running production service ที่เป็น manual, repetitive, automatable, tactical, ไม่มี enduring value, และ scale linearly กับ service growth
- **ไม่ใช่** "งานที่ไม่อยากทำ" — toil เป็นคำจำกัดความที่ชัดเจน, ไม่ใช่เรื่องของ taste
- **Overhead** แยกจาก toil — overhead คืองาน admin ที่ไม่ได้เกี่ยวกับ production service โดยตรง (เช่น meetings, HR)

**คุณสมบัติของ Toil:**
1. **Manual** — แม้จะรัน script ที่ automate แล้ว เวลาที่ human ใช้ run script ก็ยังเป็น toil
2. **Repetitive** — ถ้าทำครั้งแรกหรือสองครั้ง ไม่ใช่ toil; ต้องทำซ้ำแล้วซ้ำอีก
3. **Automatable** — ถ้าเครื่องทำได้เท่ากับคน ก็เป็น toil; ถ้า human judgment จำเป็น อาจไม่ใช่
4. **Tactical** — interrupt-driven, reactive, ไม่ใช่ strategy-driven
5. **No enduring value** — ทำแล้วระบบไม่ได้เปลี่ยนไปในทางที่ดีขึ้น
6. **O(n) with service growth** — งานเพิ่มขึ้นlinearly กับขนาดระบบ

**ทำไมต้องกำจัด Toil:**
- Google กำหนดให้ SRE ทำงาน engineering อย่างน้อย 50% ของเวลา
- ค่าเฉลี่ย in practice อยู่ที่ ~33% toil (ดีกว่า target 50%)
- แต่มี outliers — บางคน 0% toil (pure dev), บางคน 80% toil

**后果 ของ Toil มากเกินไป:**
- Career stagnation — ไม่มีเวลาทำ projects ที่มี impact
- Low morale — burnout, boredom
- Creates confusion — คนอื่นสับสนว่า SRE ไม่ใช่ ops team
- Sets precedent — dev team จะ loading SRE ด้วย toil มากขึ้น
- Promotes attrition — คนเก่งจะจากไป
- Causes breach of faith — คนที่เข้ามาเพราะ promise เรื่อง projects จะรู้สึกถูกหลอก

**Engineering Work คืออะไร:**
- **Software engineering:** เขียน/แก้ code, automation scripts, tools, frameworks
- **Systems engineering:** ตั้งค่า production systems, monitoring, load balancing — สร้าง lasting improvement จาก one-time effort

---

### บทที่ 6: Monitoring Distributed Systems

**Definitions:**
- **Monitoring** — เก็บ, process, aggregate, display ข้อมูล real-time เกี่ยวกับระบบ
- **White-box monitoring** — ใช้ internal metrics (logs, JVM profiling, HTTP handlers)
- **Black-box monitoring** — ทดสอบ external behavior เหมือนที่ user เห็น
- **Dashboard** — app ที่ให้ summary view ของ core metrics
- **Alert** — notification ที่ต้อง action (page = ด่วน, ticket = ไม่ด่วน, logging = เก็บไว้ดูทีหลัง)
- **Root cause** — ข้อบกพร่องที่ถ้าแก้แล้ว จะมั่นใจได้ว่า event เดิมจะไม่เกิดอีก

**ทำไมต้อง Monitor:**
- วิเคราะห์ long-term trends
- เปรียบเทียบ performance ตามเวลาหรือ experiment groups
- Alerting เมื่อมีปัญหา
- Building dashboards
- Debugging แบบ ad hoc

**Four Golden Signals:**
1. **Latency** — เวลาที่ใช้ในการตอบ request (แยก success กับ failure latency)
2. **Traffic** — demand ที่วางบนระบบ (เช่น HTTP requests/second)
3. **Errors** — อัตรา request ที่ fail (explicit, implicit, หรือ by policy)
4. **Saturation** — ระบบ "เต็ม" แค่ไหน (ใช้ resource ที่ constraint ที่สุดเป็นตัววัด)

**Symptoms vs Causes:**
- Monitoring ควรตอบ 2 คำถาม: "อะไรพัง?" (symptom) และ "ทำไม?" (cause)
- จับ symptom ให้ได้ก่อน, แล้วค่อยหา cause — ไม่ใช่ ngượcกัน

**Black-box vs White-box:**
- Black-box = symptom-oriented, จับปัญหาที่กำลังเกิดอยู่ตอนนี้
- White-box = มอง internal, detect ปัญหาที่จะเกิด, failures ที่ masked by retries
- ใช้ทั้งสองร่วมกัน — black-box สำหรับ paging, white-box สำหรับ debugging

**Monitoring Philosophy:**
- ทุกครั้งที่ pager ดัง ควร reaction ด้วย sense of urgency — แต่ทำได้แค่ไม่กี่ครั้งต่อวันก่อนจะ fatigue
- ทุก page ควร actionable — ถ้า page ต้องการแค่ robotic response ก็ไม่ควรเป็น page
- ทุก page ควรเกี่ยวกับ novel problem หรือ event ที่ไม่เคยเห็น
- จับ symptom ดีกว่า cause; cause เฉพาะที่ definite และ imminent

**Long-term Monitoring:**
- Bigtable SRE case — over-alerting ทำให้ team เสียเวลา triaging; แก้โดย temporarily ลด SLO target + disable email alerts
- Gmail case — scheduler bugs ทำให้ alerts เยอะ; แก้โดย automate response ชั่วคราว
- Pages ที่มี rote, algorithmic responses ควรเป็น red flag — บ่งบอกว่ามี technical debt

---

### บทที่ 7: The Evolution of Automation at Google

**คุณค่าของ Automation:**
- **Consistency** — เครื่องทำงานได้ consistent กว่าคนมาก
- **A Platform** — automation สร้าง platform ที่ extend ได้, centralizes mistakes (bug แก้ที่เดียวแล้วหายหมด)
- **Faster Repairs** — ลด MTTR สำหรับ common faults
- **Faster Action** — เครื่องตอบเร็วกว่าคน
- **Time Saving** — ประหยัดเวลาสำหรับทุกคนที่ใช้ automation

**Google SRE ใช้ Automation อย่างไร:**
- Google มี production environment ที่ uniform — สร้าง API ได้เองถ้า vendor ไม่มี
- เขียน solutions เองแม้จะแพงกว่าในสั้น — เพราะ long-term benefits มากกว่า
- Platform-based approach เป็นสิ่งจำเป็นสำหรับ manageability และ scalability

**Use Cases:**
- Cluster turnups — automate provisioning + configuration
- Borg — cluster operating system ที่จัดการ resource allocation, task scheduling, failure recovery
- ค่อยๆ evolve จาก manual → scripted → automated → autonomous system

---

### บทที่ 8: Release Engineering

**Release Engineer's Role:**
- รับผิดชอบ build, test, package, deploy software
- ไม่ใช่แค่ "release button" — ต้อง ensure consistency, safety, speed

**Philosophy:**
- **Push on green** — release ผ่าน tests แล้ว deploy อัตโนมัติ (ไม่ต้องมี human gate)
- **Continuous Build and Deployment** — build ทุกครั้งที่มี code change, run tests อัตโนมัติ
- **Configuration Management** — จัดการ configs ให้ reproducible และ auditable

---

### บทที่ 9: Simplicity (ความเรียบง่าย)

- ระบบที่ซับซ้อนมักวิวัฒน์มาจากระบบที่เรียบง่าย
- **The Virtue of Boring** — ใช้สิ่งที่ proven ดีกว่า innovate ทุกครั้ง
- **Negative Lines of Code** — ลบ code ได้ก็มีค่า
- **Minimal APIs** — API ที่เรียบง่าย, ไม่ over-engineer
- **Modularity** — แยกส่วนให้ชัดเจน
- **Release Simplicity** — release ที่เรียบง่ายมีความเสี่ยงน้อยกว่า

---

## Part III: Practices

### บทที่ 10: Practical Alerting from Time-Series Data

**Borgmon — Monitoring System ของ Google:**
- เก็บ time-series data จาก exported variables ของ tasks
- ใช้ rule evaluation สำหรับ alerting — rule สร้าง time-series ใหม่จาก time-series ที่มีอยู่

**Rule Evaluation:**
- ตัวอย่าง: คำนวณ HTTP error rate ของ cluster โดย aggregate rates ของ response codes ทุก task
- ใช้ labels (เช่น instance, code) สำหรับ aggregation
- Rate function คำนวณ delta/time สำหรับ discrete data points
- สร้าง alert เมื่อ error ratio > threshold หรือ error count > limit

**Alertmanager:**
- รับ Alert RPC, routes ไปที่ destination ที่ถูกต้อง
- ทำ deduplication, inhibition, fan-in/fan-out ของ alerts

**Sharding:**
- ใช้ hierarchy ของ Borgmon — datacenter-level → global-level
- Global Borgmon ดึงข้อมูลจาก datacenter Borgmon, กรองเฉพาะ data ที่ต้องการ
- ให้ scaling independent กับ alerting rules

**Black-Box Monitoring:**
- ใช้ **Prober** — ทดสอบ protocol เหมือน user จะเห็น
- ตรวจสอบ response payload ว่าถูกต้อง
- สามารถ monitor ทั้ง load-balanced endpoint และ individual datacenter

---

### บทที่ 11: Being On-Call

**Life of an On-Call Engineer:**
- ต้อง respond ภายใน 5 นาที (user-facing) หรือ 30 นาที (less time-sensitive)
- จัดการ outage, review production changes
- ทีมส่วนใหญ่มี primary + secondary on-call rotation

**Balanced On-Call:**
- **Quantity:** SRE ใช้เวลา on-call ไม่เกิน 25% (50% engineering + 25% on-call + 25% other ops)
- ทีม single-site ขนาดเล็กสุด 8 คน (หนึ่งสัปดาห์ on-call ต่อเดือน)
- **Quality:** on-call shift ควรมีไม่เกิน 2 incidents (แต่ละ incident ~6 ชั่วโมง)
- Multi-site team ดีกว่า single-site เพราะ avoid night shifts แต่ต้องแลก communication overhead

**Feeling Safe:**
- ลด stress ที่เกี่ยวข้องกับ on-call — stress hormones (cortisol, CRH) ทำให้ decision-making แย่
- ต้องมี: clear escalation paths, well-defined incident management, blameless postmortem culture
- อย่าใช้ intuition ตอนจับ incident — ใช้ rational, deliberate approach ดีกว่า

**Operational Overload:**
- ถ้า operational work > 50% → ต้อง采取 action: spread toil, add staff, หรือ "give back the pager"
- บางครั้งต้อง renegotiate on-call responsibilities กับ dev team
- **Operational Underload** ก็ไม่ดี — ไม่ได้ on-call นานๆ จะทำให้ lose touch กับ production

---

### บทที่ 12: Effective Troubleshooting

**Theory — Hypothetico-Deductive Method:**
1. รับ problem report → ดู telemetry + logs → เข้าใจ current state
2. ใช้ knowledge ของระบบ → identify possible causes
3. Test hypotheses โดยเปรียบเทียบ observed state กับ theory หรือ active "treatment"
4. ทำซ้ำจนหา root cause ได้ → fix + write postmortem

**Common Pitfalls:**
- ดู symptoms ที่ไม่เกี่ยวข้อง → wild goose chase
- ไม่เข้าใจวิธีเปลี่ยน system อย่าง safe
- ยึดกับ past problems มากเกินไป (confirmation bias)
- ตาม spurious correlations ที่เป็น coincidence

**In Practice:**
- **Problem Report:** ควรมี expected behavior, actual behavior, reproduction steps
- **Triage:** อย่ารีบ root-cause — ทำให้ระบบทำงานได้ดีที่สุด under circumstances ก่อน
- นักบินฝึกหัดสอนว่า: "first responsibility คือ fly the airplane" — ไม่ใช่ troubleshoot

---

### บทที่ 13: Emergency Response

**_types of emergencies:**
1. **Test-Induced** — test ที่ไม่ดีทำให้ production พัง
2. **Change-Induced** — deployment หรือ config change ทำให้ระบบ fail
3. **Process-Induced** — process ที่ไม่ดีทำให้เกิดปัญหา

**หลักการ:**
- ทุกปัญหามีทางแก้ — ต้อง resolve ให้ได้
- เรียนรู้จากอดีต, อย่าทำซ้ำ
- ควรมี playbook สำหรับ emergency types ที่พบบ่อย

---

### บทที่ 14: Managing Incidents

**Unmanaged Incident:**
- ไม่มี owner, ไม่มี communication, ไม่มี process ชัดเจน
- ผลลัพธ์: chaos, แก้ปัญหาช้า, ทำซ้ำ

**Managed Incident:**
- มี **Incident Manager** ที่ชัดเจน
- มี communication channel ที่จัดตั้ง
- มี timeline บันทึก actions
- มี escalation process

**When to Declare Incident:**
- ถ้ามี user-visible impact หรือมี potential ที่จะ impact
- ดีกว่า declare แล้ว cancel  מאשרไม่ declare เลย

---

### บทที่ 15: Postmortem Culture — Learning from Failure

**Google's Postmortem Philosophy:**
- **Blameless** — ไม่โทษคน, โทษระบบ/process
- มุ่งเน้นที่ **what** เกิดขึ้น ไม่ใช่ **who** ผิด
- Postmortem สำหรับทุก significant incident (แม้ไม่ได้ page)

**Collaborate and Share Knowledge:**
- Postmortem ควร share กับทุกคนที่เกี่ยวข้อง
- สร้าง knowledge base ที่ searchable
- เรียนรู้จาก incident ของทีมอื่นด้วย

**Introducing Postmortem Culture:**
- ต้องมี management support
- ต้องมี process ที่ชัดเจน
- ต้อง celebrate learning จาก failure (ไม่ใช่แค่ celebrate success)

---

### บทที่ 16: Tracking Outages

**Escalator:** ระบบ internal สำหรับ tracking outages ที่ Google
**Outalator:** อีก tool หนึ่งสำหรับ tracking — เก็บ data เกี่ยวกับ outages สำหรับ analysis

- ทั้งสองช่วยให้ team ติดตาม patterns, measure MTTR, identify recurring issues

---

### บทที่ 17: Testing for Reliability

**Types of Software Testing:**
- Unit testing
- Integration testing
- System testing
- Regression testing
- Production testing (canary, A/B testing)

**Creating Test and Build Environment:**
- Test environment ควร replicate production ให้มากที่สุด
- ใช้ feature flags สำหรับ testing ใน production

**Testing at Scale:**
- Google ใช้ continuous testing — ทุก code change ถูก test โดยอัตโนมัติ
- Large-scale testing ต้อง考虑 resource ที่ใช้
- ใช้ staging environments ก่อน production

---

### บทที่ 18: Software Engineering in SRE

**Auxon Case Study:**
- โครงการที่ SRE สร้าง software tool สำหรับ capacity planning
- **Intent-Based Capacity Planning:** แทนที่จะ specify raw capacity, ให้ specify intent (เช่น "need 99.99% availability")
- Tool คำนวณ capacity ที่ต้องการจาก intent

**Fostering Software Engineering in SRE:**
- SRE ควรสร้าง tools ที่ replace manual work
- Engineering projects ควร reduce toil หรือ improve service
- ต้องมี balance ระหว่าง operational work กับ engineering projects

---

### บทที่ 19: Load Balancing at the Frontend

- **DNS-based load balancing** — ใช้ DNS สำหรับ geographic distribution
- **Virtual IP Address** — ใช้ VIP สำหรับ load balancing ระดับ network
- Google ใช้ **GSLB** สำหรับ global load balancing ที่สามระดับ (DNS, service, RPC)
- ไม่ได้ใช้ "power" (hardware-based) — ใช้ software-based approaches ที่มี flexibility มากกว่า

---

### บทที่ 20: Load Balancing in the Datacenter

- **Ideal case:** กระจายน้ำหนักเท่าๆ กันทุก task
- **Bad task detection:** Flow control และ lame ducks (tasks ที่ slow/broken)
- **Subsetting:** จำกัด connection pool — ไม่ให้ทุก backend เชื่อมต่อกับทุก frontend
- **Load balancing policies:** Round robin, least connections, weighted, ฯลฯ

---

### บทที่ 21: Handling Overload

- **QPS (Queries Per Second) ไม่ใช่ metric ที่ดีเสมอไป** — ไม่ได้ reflect actual load ที่ system ต้องรับมือ
- **Per-customer limits** — จำกัด request จาก customer แต่ละราย
- **Client-side throttling** — ให้ client ลด request rate เมื่อ system overload
- **Criticality** — ให้ priority สูงกับ requests ที่สำคัญกว่า
- **Utilization signals** — ใช้ utilization metrics เป็น signal สำหรับ throttling
- **Handling overload errors:** graceful degradation, retry with backoff, shed load

---

### บทที่ 22: Addressing Cascading Failures

** Causes of Cascading Failures:**
- Single point of failure → cascade ไปทั่วระบบ
- Resource exhaustion (CPU, memory, network)
- Retry storms — retries ทำให้ load มากขึ้น

** Prevention:**
- Circuit breakers — ตัด connection เมื่อ backend fail
- Load shedding — ทิ้ง requests เมื่อ overload
- Timeouts ที่ appropriate
- Retry budgets — จำกัดจำนวน retries

** Testing:**
- Chaos engineering — จงใจ fail components เพื่อดูว่า system รับมืออย่างไร

---

### บทที่ 23: Managing Critical State — Distributed Consensus

**Distributed Consensus (Paxos/Raft):**
- ปัญหา: ทำให้ multiple nodes เห็นพ้องต้องกันในdistributed system
- **Paxos** — protocol ที่ Google ใช้ใน Chubby
- **Performance:** ต้อง考虑 latency, throughput, fault tolerance

**Architecture Patterns:**
- Single leader, multi-paxos, chain replication
- Deploying consensus-based systems ต้อง consideration เรื่อง failure domains

---

### บทที่ 24: Distributed Periodic Scheduling with Cron

**Cron at Scale:**
- ต้อง handle: job scheduling, resource allocation, failure recovery
- **Idempotency** — cron jobs ควรรันซ้ำได้โดยไม่มีผลข้างเคียง
- Google สร้าง distributed cron ที่ scale กับ cluster ขนาดใหญ่

---

### บทที่ 25: Data Processing Pipelines

- **Pipeline design pattern** — data ผ่าน processing stages หลายจุด
- Big data ทำให้ simple pipeline ซับซ้อนขึ้น
- **Periodic pipelines** มีปัญหา: uneven work distribution, resource contention
- Google ใช้ **Workflow** สำหรับ distributed pipeline orchestration

---

### บทที่ 26: Data Integrity

- **Data Integrity's Requirements** — สูงมากสำหรับ systems ที่เก็บข้อมูลสำคัญ
- Google SRE ใช้ techniques หลายอย่าง:
  - Backup และ restore
  - Replication (cross-datacenter)
  - Checksums และ verification
  - Gradual rollout สำหรับ schema changes
- **Case studies:** Bigtable, Spanner, Colossus
- หลักการ: **What You Read Is What You Wrote** — ถ้าอ่านแล้วได้ค่าเดียวกับที่เขียน ถือว่า integrity ดี

---

### บทที่ 27: Reliable Product Launches at Scale

**Launch Coordination Engineering (LCE):**
- ทีมที่รับผิดชอบ coordination สำหรับ product launches ขนาดใหญ่
- **Launch Checklist** — รายการที่ต้องทำก่อน, ระหว่าง, และหลัง launch
- **Techniques:**
  - Gradual rollout (1% → 10% → 50% → 100%)
  - Canary testing
  - Feature flags
  - Rollback plans

---

## Part IV: Management

### บทที่ 28: Accelerating SREs to On-Call and Beyond

**New SRE Onboarding:**
- ให้ structured learning experiences แทน "sink or swim"
- Reverse engineering exercises — เข้าใจ system โดยดู code
- Improvisational thinking — ฝึกแก้ปัญหาที่ไม่รู้ล่วงหน้า

**Five Practices for Aspiring On-Callers:**
1. Learn the system deeply
2. Shadow on-call
3. Wheel of Misfortune exercises
4. Practice incident response
5. Write playbooks

**On-Call as Rite of Passage:**
- ผ่าน on-call shift แรก = milestone สำคัญ
- Continuing education สำคัญตลอดอาชีพ

---

### บทที่ 29: Dealing with Interrupts

**Managing Operational Load:**
- Interrupts (non-urgent messages, emails) เป็น source ของ toil ที่ใหญ่ที่สุด
- ต้องจัดการอย่างสมดุล — ไม่ให้ interrupt รบกวน project work มากเกินไป

**Factors:**
- Priority ของ interrupt
- ความพร้อมของ engineer
- ผลกระทบต่อ service

**Imperfect Machines:**
- ระบบที่ต้องการ human intervention บ่อย = poorly designed
- เป้าหมาย: automate ให้มากที่สุด

---

### บทที่ 30: Embedding an SRE to Recover from Operational Overload

**3 Phases:**
1. **Learn the Service and Get Context** — เข้าใจ system, team dynamics, pain points
2. **Sharing Context** — share knowledge กับ team
3. **Driving Change** — implement improvements

- การ embedding SRE ช่วย recovery จาก operational overload โดยให้ fresh perspective + expertise

---

### บทที่ 31: Communication and Collaboration in SRE

**Production Meetings:**
- Regular meetings สำหรับ discuss production health, incidents, improvements
- Agenda ชัดเจน, action items documented

**Collaboration within SRE:**
- Cross-team collaboration สำหรับ shared infrastructure
- Case study: **Viceroy** — tool ที่สร้างโดย SRE สำหรับ monitoring

**Collaboration Outside SRE:**
- กับ product development teams
- Case study: **DFP to F1 migration** — collaboration สำหรับ infrastructure migration

---

### บทที่ 32: The Evolving SRE Engagement Model

**PRR Model (Production Readiness Review):**
- SRE ตรวจสอบ service ก่อน production deployment
- ตรวจสอบ: monitoring, alerting, capacity, dependencies, runbooks

**Early Engagement:**
- SRE เข้ามามีส่วนร่วมตั้งแต่ design phase (ไม่ใช่แค่ตอน deploy)
- ช่วยให้ reliability เป็น consideration ตั้งแต่แรก

**SRE Platform:**
- สร้าง shared platform ที่ teams สามารถใช้ได้
- ลด burden ของ SRE โดยให้ self-service capabilities

---

## Part V: Conclusions

### บทที่ 33: Lessons Learned from Other Industries

**Preparedness and Disaster Testing:**
- อุตสาหกรรมอื่น (aviation, nuclear) มี disaster testing ที่เข้มข้นกว่า IT
- Google ใช้ **DiRT (Disaster Recovery Training)** — annual event ทดสอบ disaster recovery

**Postmortem Culture:**
- อุตสาหกรรมอื่นเรียนรู้จาก failure ได้ดีกว่า
- IT ยังมี blame culture อยู่มาก

**Automating Away Repetitive Work:**
- Manufacturing, aviation automate ได้ดีกว่า IT
- IT ควรเรียนรู้จาก automation practices ของอุตสาหกรรมอื่น

**Structured Decision Making:**
- ใช้ data-driven decisions แทน intuition
- Pre-mortems — คิดว่า "ถ้า project ล้มเหลว จะเป็นเพราะอะไร?" ก่อนเริ่ม

---

### บทที่ 34: Conclusion

**Key Takeaways:**
1. **Reliability เป็น feature ที่สำคัญที่สุด** — system ไม่มีค่าถ้าไม่มีใครใช้ได้
2. **Automation เป็น force multiplier** — แต่ต้องใช้อย่างชาญฉลาด
3. **Postmortem culture** — เรียนรู้จาก failure โดยไม่ blame
4. **Error budgets** — จัดการ trade-off ระหว่าง velocity กับ reliability
5. **Simplicity** — ระบบที่เรียบง่าย maintain ได้ง่ายกว่า
6. **Monitoring** — ต้องมี signal มากกว่า noise
7. **Engineering mindset** — ใช้ software engineering มาแก้ปัญหา operations

**The SRE Way:**
- Thoroughness และ dedication
- Belief ใน value ของ preparation และ documentation
- Awareness ของสิ่งที่อาจผิดพลาด + แรงจูงใจที่จะป้องกัน
- ไม่ใช่แค่ "hope" — แต่ใช้ engineering มาทำให้ reliable

---

## Appendix

### Appendix A: Availability Table
ตารางแสดง downtime ที่ยอมรับได้สำหรับ availability targets ต่างๆ (99% ถึง 99.999%) — เช่น 99.99% = ~52.56 นาที/ปี, 99.999% = ~5.26 นาที/ปี

### Appendix B: Best Practices for Production Services
รายการ best practices สำหรับ production services ที่-cover monitoring, alerting, capacity planning, change management

### Appendix C: Example Incident State Document
ตัวอย่างเอกสารที่ใช้ durante incident management — รวมข้อมูล status, timeline, actions

### Appendix D: Example Postmortem
ตัวอย่าง postmortem document — root cause analysis, timeline, action items

### Appendix E: Launch Coordination Checklist
รายการตรวจสอบสำหรับ product launches — ครอบคลุม testing, monitoring, rollback plans

### Appendix F: Example Production Meeting Minutes
ตัวอย่าง meeting minutes สำหรับ production meetings

---

> **สรุป:** หนังสือเล่มนี้เป็นคู่มือครบวงจรสำหรับ SRE — จากหลักการ (risk management, SLO, toil elimination) ผ่าน practices (monitoring, troubleshooting, incident management) ไปจนถึง management (on-call, communication, engagement model) ทั้งหมดนี้มุ่งเน้นที่การใช้ software engineering มาแก้ปัญหา operations เพื่อให้ระบบมี reliability ที่เหมาะสมโดยไม่สูญเสีย innovation velocity
