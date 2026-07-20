# The DevOps Handbook: How to Create World-Class Agility, Reliability, and Security in Technology Organizations
> **Author:** Gene Kim, Jez Humble, Patrick Debois, John Willis

---

## ภาพรวม
หนังสือเล่มนี้คือคู่มือหลักสำหรับการtransform องค์กรด้วย DevOps ครอบคลุมทั้งทฤษฎี หลักการ และ practices จริง แบ่งเป็น 6 ส่วน สรุปจากประสบการณ์จริงขององค์กรกว่า 100 แห่ง เช่น Google, Amazon, Facebook, Etsy, Netflix, Target ฯลฯ

---

## ความเชื่อผิดๆ เกี่ยวกับ DevOps

**Myth 1: DevOps ขัดกับ ITIL** — จริงๆ แล้วเข้ากันได้ เพียงแต่หลายกระบวนการ ITIL ต้อง automates เพื่อรองรับ deployment frequency ที่สูงขึ้น

**Myth 2: DevOps คือ NoOps** — ผิด entirely DevOps ไม่ได้eliminate IT Ops แต่เปลี่ยน role ให้Ops สร้าง self-service platforms ให้ dev ทำงานเองได้

**Myth 3: DevOps คือแค่ Infrastructure as Code** — ผิด DevOps ต้องมี culture และ architecture ด้วย ไม่ใช่แค่ automation

**Myth 4: DevOps เฉพาะ Open Source** — ผิด สำเร็จได้กับ .NET, COBOL, mainframe, SAP ทุก technology

---

## Core Problem: The Core, Chronic Conflict

IT organization มี conflict 2 ฝั่ง:
- **Dev** ต้องการ deploy เร็ว เปลี่ยนเร็ว (respond to market)
- **Ops** ต้องการ stability, reliability, security (ไม่อยากให้เปลี่ยน)

Dr. Goldratt เรียก configuration นี้ว่า "core, chronic conflict" — เมื่อ incentive ของ silo ต่างกัน ทำให้ global goal สำเร็จไม่ได้

---

## Downward Spiral 3 ขั้นตอน

**ขั้นที่ 1**: IT Ops ต้อง manage ระบบที่fragile, technical debt สูง, daily workarounds

**ขั้นที่ 2**: business ให้ promise ใหม่ที่ใหญ่กว่า dev ต้อง cut corners เพิ่ม technical debt มากขึ้น

**ขั้นที่ 3**: ทุกอย่างช้าลง ต้อง coordination มากขึ้น quality แย่ลง feedback ช้าลง สุดท้าย organization ล้มเหลว

**ค่าเสียหาย**: ผู้เขียนประเมินว่า IT waste ทำให้สูญเสีย ~$2.6 ล้านล้านดอลลาร์/ปี เท่ากับ GDP ของฝรั่งเศส

---

## The Three Ways: หลักการพื้นฐานของ DevOps

### The First Way — Flow (ไหลจากซ้ายไปขวา)
ทำให้งานไหลจาก Dev → Ops → ลูกค้า เร็วขึ้น

**หลักการ:**
- **ทำให้งาน visible** — ใช้ kanban board เพื่อเห็นว่างานติดตรงไหน
- **Limit WIP (Work in Process)** — ไม่ทำหลายอย่างพร้อมกัน ลด context switching
- **Reduce batch sizes** — deploy เปลี่ยนเล็กๆ บ่อยๆ ดีกว่า big bang
- **Reduce handoffs** — ลดการส่งต่องานระหว่างทีม ให้ทีม cross-functional
- **Identify constraints** — หา bottleneck แล้วแก้ (ตาม Theory of Constraints ของ Goldratt)
- **Eliminate waste** — ลด partially done work, extra processes, waiting, rework

**Constraint Progression:**
1. Environment creation → ต้อง on-demand self-service
2. Code deployment → ต้อง automate
3. Test setup → ต้อง automate
4. Architecture → ต้อง loosely-coupled

**Waste Categories:** partially done work, extra processes, extra features, task switching, waiting, motion, defects, nonstandard/manual work, heroics

### The Second Way — Feedback (ไหลจากขวาไปซ้าย)
สร้าง feedback loops ที่เร็วและต่อเนื่องทุกขั้นตอน

**หลักการ:**
- **See problems as they occur** — telemetry, automated tests, feedback loops ทำให้เห็นปัญหาเร็ว
- **Swarm and solve** — แก้ปัญหาทันที ไม่ใช่รอ fix ทีหลัง (like Toyota Andon cord)
- **Push quality closer to source** — ให้ dev test และ fix เอง ไม่ต้องรอ QA ตรวจ
- **Optimize for downstream** — ออกแบบให้ downstream ทำงานง่าย (Design for Manufacturability)

**Complex Systems:** ทำงานใน complex systems ต้อง accepts ว่า failure เกิดได้เสมอ ต้อง create safe systems of work ไม่ใช่ blame people

### The Third Way — Continual Learning & Experimentation
สร้าง culture แห่งการเรียนรู้และทดลอง

**หลักการ:**
- **Just Culture** — ไม่ blame people แต่ redesign systems (blameless post-mortems)
- **Institutionalize improvement** — จัดเวลาfix technical debt 20% ของ cycles
- **Transform local to global** — เปลี่ยนความรู้ที่ค้นพบในท้องถิ่นให้เป็น organization-wide
- **Inject resilience** — ลองทำสิ่งที่ยากขึ้นเรื่อยๆ เช่น Netflix Chaos Monkey
- **Leaders coach** — ผู้นำไม่ใช่คนตัดสินใจทุกอย่าง แต่ช่วยให้ทีมค้นพบ greatness

**Culture Types (Westrum):**
- **Pathological**: กลัว, ซ่อนปัญหา
- **Bureaucratic**: มี rules มากมาย รักษา turf
- **Generative**: หาข้อมูล, sharing, learning จาก failure

---

## Part II: Where to Start — เริ่มอย่างไร

### เลือก Value Stream ที่จะ transform
- เลือก team ที่ receptive กับ new ideas (innovators, early adopters)
- เริ่มกับ greenfield (ใหม่) หรือ brownfield (เก่า) ก็ได้ — DevOps ใช้ได้ทั้งคู่
- **Nordstrom Example**: เริ่มจาก mobile app → restaurant systems → ขยาย organization-wide

### ทำ Value Stream Map
- รวมทุก stakeholder (Product, Dev, QA, Ops, Infosec)
- วัด lead time, process time, %C/A (% complete and accurate)
- สร้าง future state map เป็นเป้าหมาย

### สร้าง Dedicated Transformation Team
- แยกทีมออกจาก daily operations
- กำหนด measurable goal (เช่น reduce lead time 50%)
- ให้ 20% ของ cycles สำหรับ technical debt
- **LinkedIn Example (Operation InVersion)**: หยุด feature dev 2 เดือนเพื่อ fix infrastructure ทำให้ deployment เร็วขึ้นมาก

### Conway's Law — ออกแบบทีมตาม architecture
- "Organizations which design systems are constrained to produce designs which are copies of their communication structures"
- **Etsy Sprouter Example**: 2 teams (Dev + DBA) → tight coupling → fix ด้วย ORM ให้ dev เข้า DB ตรงๆ

### Organizational Archetypes
1. **Functional**: optimize for expertise/cost (traditional Ops silos)
2. **Matrix**: combine both (มักซับซ้อน)
3. **Market**: optimize for speed (cross-functional teams like Amazon, Netflix)

### สร้าง Market-Oriented Teams
- ฝัง Ops, QA, Infosec เข้าไปใน product team
- ให้ self-service platforms สำหรับ environments, testing, deployment
- **Amazon Two-Pizza Team**: ทีมเล็ก 5-10 คน, autonomous, fitness function

### Integrate Ops into Dev Daily Work
- Ops ไป standup, retrospective ของ dev
- ใช้ shared kanban board
- **Big Fish Games**: Ops liaison model ฝัง Ops เข้าไปใน dev team

---

## Part III: The First Way — Technical Practices of Flow

### Create Deployment Pipeline Foundations
- ใช้ production-like environments ตั้งแต่ต้น
- ทุกอย่างอยู่ใน version control (code, environments, tests, scripts)
- Infrastructure as Code (Puppet, Chef, Ansible, Docker, Cloud)
- **Immutable Infrastructure**: rebuild ไม่ repair, เหมือน cattle ไม่ใช่ pets
- **Definition of Done**: code ต้อง run ใน production-like environment

### Enable Automated Testing
- **Testing Pyramid**: unit tests เยอะสุด, acceptance tests ตรงกลาง, integration tests น้อยสุด
- **TDD (Test-Driven Development)**: เขียน test ก่อนเขียน code
- ทดสอบ performance, security, NFRs (Non-Functional Requirements) เป็น automated tests
- **Andon Cord**: เมื่อ build break ต้อง fix ทันที ห้าม commit ใหม่
- **Google Example**: 75 million test cases run daily, 40,000 commits/day

### Continuous Integration
- Developer commit code to trunk อย่างน้อยวันละครั้ง
- ไม่ long-lived feature branches — merge เรื่อยๆ ลด merge conflicts
- **HP LaserJet Example**: เปลี่ยนจาก 2 firmware releases/yr → dev time สำหรับ features เพิ่มจาก 5% → 40%
- **Bazaarvoice Example**: customer incidents ลดจาก 44 → 0 ภายใน 3 เดือน

### Automate Low-Risk Releases
- **Blue-Green Deployment**: มี 2 production envs, สลับ traffic ไปมา
- **Canary Releases**: deploy ให้ user กลุ่มเล็กๆ ก่อน
- **Feature Flags/Toggles**: เปิด-ปิด feature ได้โดยไม่ต้อง deploy ใหม่
- **Dark Launching**: test feature ใน production กับ user จริงก่อนannounce
- Decouple deployment จาก release — deploy บ่อย release เมื่อพร้อม
- **Facebook**: deploy 1-3 ครั้ง/วัน
- **Etsy**: developer  deploy เองตั้งแต่วันแรกที่ทำงาน

---

## Part IV: The Second Way — Feedback Practices

### Create Production Telemetry
- ต้อง monitor ทุก component ใน production
- วัด latency, traffic, errors, saturation (the Four Golden Signals)
- **Etsy Example**: สร้าง dashboards ที่visible ทุกคน
- ใช้ telemetry เพื่อ validate business hypothesis (A/B testing)

### ทำ A/B Testing
- ใช้production data ตัดสินใจ
- Decouple deployment จาก release ด้วย feature flags
- วัดผลจริงๆ ไม่ใช่ guess

### สร้าง Review & Coordination Processes
- **Peer reviews**: code reviews ก่อน merge
- **Deployment approvals**: automated gates มากกว่า manual approvals
- ใช้ tools เพื่อ enforce standards (pre-commit hooks, CI pipelines)

---

## Part V: The Third Way — Learning Practices

### Blameless Post-Mortems
- หลัง incident ทุกครั้ง ต้อง blameless review
- **etsy Morgue tool**: document incidents อย่างเป็นระบบ
- ถามว่า "systems ออกแบบอย่างไรที่ทำให้เกิดปัญหา" ไม่ใช่ "ใครผิด"

### Institutionalize Improvement of Daily Work
- จัดเวลา fix technical debt อย่างชัดเจน
- **Kaizen Blitzes**: ให้ engineer จัดทีมfix ปัญหาที่อยากfix
- ทำ Game Day exercises — inject failures ใน production (Netflix Chaos Monkey)
- **Antifragility**: ใช้ stress เพื่อ build resilience

### Leaders Create Learning Culture
- ไม่ใช่ leader ตัดสินใจทุกอย่าง แต่สร้าง conditions ให้ทีม discover greatness
- **Coaching Kata** (Mike Rother): ถาม-ตอบแบบ scientific method
- คำถาม coaching: What was your last step? What did you learn? What's your next target?

### Transform Local to Global
- ทำblameless post-mortems searchable
- ใช้ shared source code repositories
- เหมือน US Navy Nuclear Program: 5,700 reactor-years without accident

---

## Part VI: Security & Compliance (สรุปย่อ)
- Integrate security into deployment pipeline (automated scanning)
- Security as automated tests, ไม่ใช่ manual review ตอนท้าย
- Protect deployment pipeline itself
- ใช้ version control เป็น audit trail สำหรับ compliance

---

## Key Statistics
- High performers: deploy 30x more frequent, lead time 200x faster
- High performers: 60x higher change success rate, 168x faster MTTR
- High performers: 2x more likely to exceed profitability goals, 50% higher market cap growth
- High performers: 50% less time remediating security issues
- Amazon: grew from 7,000 deploys/day (2011) → 130,000 deploys/day (2015)
- Google: 50,000 builds/day, 75 million test cases/day

---

## บทเรียนสำคัญ
1. **DevOps ไม่ใช่ tool** — เป็น culture + architecture + practices
2. **Start small, expand** — ไม่ big bang, เริ่มกับ receptive teams แล้วขยาย
3. **Automate everything possible** — environments, testing, deployment, security
4. **Small batches win** — deploy เปลี่ยนเล็กๆ บ่อยๆ ดีกว่า big releases
5. **Everyone owns quality** — Dev, Ops, QA, Infosec ร่วมรับผิดชอบ
6. **Measurement matters** — วัด lead time, MTTR, deployment frequency, change failure rate
7. **Leadership support is essential** — ต้องมี executive sponsor จริงจัง
8. **Just Culture over blame culture** — เรียนรู้จาก failure ไม่ใช่ลงโทษ

---
