# Accelerate

> **Author:** Nicole Forsgren, Jez Humble, Gene Kim

---

## ภาพรวมหนังสือ

หนังสือเล่มนี้สรุปผลวิจัย 4 ปีจาก State of DevOps Reports เกี่ยวกับว่า practices และ capabilities อะไรบ้างที่ทำให้ทีม software delivery มี performance ดีขึ้น ข้อมูลมาจากแบบสำรวจ (survey) มากกว่า 23,000 ชุด จากองค์กรกว่า 2,000 แห่งทั่วโลก ทั้ง startup ขนาดเล็กจนถึง enterprise ขนาดใหญ่

---

## ทำไม Software Delivery Performance ถึงสำคัญ

- องค์กรที่ delivery performance ดี มีโอกาสเกินเป้าหมาย profitability, productivity, market share มากกว่าองค์กรที่ performance ต่ำถึง **2 เท่า**
- **High performers** deploy ได้บ่อยกว่า low performers ถึง 46 เท่า, lead time เร็วกว่า 440 เท่า, MTTR (Mean Time To Recover) เร็วกว่า 170 เท่า, change failure rate ต่ำกว่า 5 เท่า
- ความเร็ว (tempo) และความเสถียร (stability) **ไม่ใช่ trade-off** — ทีมที่ดีได้ทั้งสองอย่างพร้อมกัน
- ช่องว่างระหว่าง high performers กับ low performers กำลัง **ขยายตัวขึ้นเรื่อยๆ** ทุกปี

---

## การวัด Performance

ใช้ 4 metrics หลัก:

1. **Delivery Lead Time** — เวลาจาก code commit ถึง production
2. **Deployment Frequency** — ความถี่ในการ deploy (proxy สำหรับ batch size)
3. **Time To Restore Service (MTTR)** — เวลาที่ใช้กู้คืนระบบเมื่อมี outage
4. **Change Failure Rate** — เปอร์เซ็นต์ของ change ที่ทำให้ระบบล่ม

การวัดแบบเก่าเช่น lines of code, velocity, utilization มีปัญหา because มันวัด outputs แทน outcomes และวัด individual แทน team-level

---

## วัฒนธรรมองค์กร (Organizational Culture)

ใช้ **Westrum's Typology** จำแนก 3 ประเภท:

- **Pathological (Power-Oriented)** — กลัวและข่มขู่, เก็บข้อมูล, กล่าวโทษ
- **Bureaucratic (Rule-Oriented)** — ยึดกฎระเบียบ, ปกป้อง turf ของแต่ละแผนก
- **Generative (Performance-Oriented)** — มุ่งเน้นภารกิจ, ให้ความร่วมมือสูง, failure leads to inquiry

**Key finding:** วัฒนธรรม Generative ทำนาย performance ได้ดีกว่า และ **เปลี่ยนได้** โดยการ implement practices ที่ถูกต้อง — "act your way to a better culture"

---

## Technical Practices — Continuous Delivery

**5 หลักการสำคัญ:**
1. **Build quality in** — สร้างคุณภาพตั้งแต่ต้น ไม่ใช่ inspection ตอนท้าย
2. **Work in small batches** — แบ่งงานเล็กๆ เพื่อ feedback เร็ว
3. **Automate everything** — เครื่องทำงานซ้ำๆ คนแก้ปัญหา
4. **Continuously improve** — ไม่เคยพอใจ ปรับปรุงตลอดเวลา
5. **Everyone is responsible** — ทุกคนรับผิดชอบ system-level outcomes

**Capabilities ที่สำคัญ:**
- **Version Control** — ใช้กับทุก production artifact (code, config, scripts) — config สำคัญกว่า code ที่คิด
- **Continuous Integration (CI)** — merge code เข้า trunk ทุกวัน, branch สั้น (ไม่เกิน 1 วัน)
- **Trunk-Based Development** — ไม่มี long-lived branches, ไม่มี code freeze
- **Test Automation** — ทดสอบอัตโนมัติที่ reliable, developer เป็นคนสร้างและดูแล
- **Test Data Management** — มี test data เพียงพอ, ไม่เป็น bottleneck
- **Deployment Automation** — deploy อัตโนมัติ, ไม่ต้อง manual intervention

**ผลลัพธ์:** CD ทำให้ deployment pain ลดลง, team burnout ลดลง, คุณภาพดีขึ้น (high performers ใช้เวลา 49% กับ new work vs 21% rework, low performers 38% vs 27%)

---

## Architecture

- **Loosely coupled architecture** เป็น key — ทีม deploy/test/change ได้โดยไม่ต้องรอทีมอื่น
- **System type ไม่สำคัญ** — ไม่ว่าจะเป็น mainframe, greenfield, embedded, packaged software ก็ทำ performance ดีได้ ถ้า architecture ถูกต้อง
- **Deployability + Testability** — สองคุณสมบัติ architectural ที่สำคัญที่สุด
- **Inverse Conway Maneuver** — ออกแบบทีม organization ให้ mirror architecture ที่ต้องการ
- **Loosely coupled = Scalable** — จำนวน developer เพิ่มขึ้น, high performers deploy ได้บ่อยขึ้น linearly
- ให้ทีม **เลือก tools เอง** — มี correlation กับ delivery performance สูง

---

## Security — Shift Left

- รวม security เข้าใน software delivery lifecycle ตั้งแต่ต้น ไม่ใช่ทำตอนท้าย
- Infosec รีวิวทุก feature, ช่วยสร้าง pre-approved libraries, ทดสอบ security features ใน automated test suite
- High performers ใช้เวลา remediate security issues น้อยกว่า low performers **50%**
- ตัวอย่าง: cloud.gov — ใช้ platform-level compliance ลดงาน implement RMF จากเดิมหลายเดือนเหลือไม่กี่สัปดาห์

---

## Lean Management Practices

3 components:
1. **WIP Limits** — จำกัดงานค้าง, ใช้ทำ process improvement
2. **Visual Displays** — dashboard แสดง metrics คุณภาพและ productivity
3. **Monitoring** — ใช้ข้อมูลจาก monitoring tools ตัดสินใจ business decisions ทุกวัน

**Lightweight Change Approval:**
- **CAB (Change Advisory Board) = แย่** — negative correlation กับ performance
- **Peer review = ดี** — เร็วกว่า, มีคุณภาพเท่ากันหรือดีกว่า
- External approval แย่กว่าไม่มี process เลย

---

## Lean Product Management

4 capabilities:
1. **Small batches** — ตัด features เล็กๆ, release บ่อย, ใช้ MVP
2. **Value stream visibility** — เข้าใจ flow จาก business ถึง customer
3. **Customer feedback** — รับ feedback สม่ำเสมอ, ใช้ปรับ product design
4. **Team experimentation** — ทีมสร้าง/update specs ได้เองโดยไม่ต้องขออนุมัติจากนอกทีม

**Virtuous Cycle:** Delivery performance → Lean product practices → Delivery performance (วนลูป)

---

## Work Sustainability

### Deployment Pain
- ความกลัวและความเจ็บปวดเวลา push code ขึ้น production
- CD practices ลด deployment pain — ทำให้ deployment เป็นกิจวัตร ไม่ใช่ big-bang event
- Microsoft Bing: work/life balance satisfaction จาก 38% → 75% หลัง implement CD

### Burnout (อาการหมดไฟ)
- สาเหตุหลักจาก Maslach: work overload, lack of control, insufficient rewards, breakdown of community, absence of fairness, value conflicts
- **วัฒนธรรม pathological = burnout สูง**, deployment pain = burnout สูง
- แก้ได้ด้วย: CD practices, Lean management, ให้ control กับทีม, align ค่านิยม

---

## Employee Satisfaction & Identity

- **eNPS (Employee Net Promoter Score):** High performers มีค่าสูงกว่า 2.2 เท่า — พนักงานอยากแนะนำที่ทำงานให้เพื่อน
- **Identity** — พนักงานรู้สึกเชื่อมโยงกับองค์กร ทำนาย generative culture และ organizational performance
- วัฒนธรรมที่ดี = people want to stay, people want to do their best work
- **Diversity** — ข้อมูล: 91% male, 6% female, 3% non-binary; 33% ทำงานในทีมที่ไม่มีผู้หญิงเลย ต้อง improve

---

## Transformational Leadership

5 คุณสมบัติของ Transformational Leader:
1. **Vision** — เข้าใจว่าองค์กรกำลังจะไปไหน
2. **Inspirational Communication** — สื่อสารสร้างแรงบันดาลใจ
3. **Intellectual Stimulation** — ท้าทายให้คิดปัญหาแบบใหม่
4. **Supportive Leadership** — ใส่ใจความต้องการของลูกน้อง
5. **Personal Recognition** — ชมเชยเมื่อทำได้ดี

**Key findings:**
- Leadership มีผล **indirect** — ผ่าน practices ที่ leader สนับสนุน ไม่ใช่ direct impact
- Leader ที่เก่งที่สุด 10% ไม่ได้ guarantee performance ถ้าไม่มี team execution
- Leadership ทำนาย eNPS, CD practices, Lean practices

---

## 24 Key Capabilities (5 Categories)

**Continuous Delivery (9):**
1. Version control สำหรับทุก artifact
2. Deployment automation
3. Continuous integration
4. Trunk-based development
5. Test automation
6. Test data management
7. Shift left on security
8. Continuous delivery (CD)
9. Loosely coupled architecture

**Architecture (2):**
10. Empowered teams (เลือก tools เอง)
11. Loosely coupled architecture (ทำซ้ำกับ CD)

**Product & Process (4):**
12. Customer feedback
13. Value stream visibility
14. Small batches
15. Team experimentation

**Lean Management (5):**
16. Lightweight change approval
17. Monitoring (app + infrastructure)
18. Proactive health checks
19. WIP limits
20. Visualize work

**Cultural (4):**
21. Generative culture (Westrum)
22. Supporting learning
23. Collaboration among teams
24. Transformational leadership

---

## วิธีวิจัย (Methodology)

- **Primary research** ใช้ survey แบบ Likert-type scale (1-7)
- **Latent constructs** — วัดสิ่งที่วัดตรงไม่ได้ (เช่น culture) ผ่าน manifest variables หลายตัว แล้วใช้ statistical tests ยืนยัน validity และ reliability
- **Snowball sampling** — เหมาะกับ population ที่ identify ยาก เช่น DevOps practitioners
- **Inferential predictive analysis** — hypothesis-driven, ไม่ใช่ data mining
- ใช้ PLS regression, cluster analysis, ANOVA, Pearson correlation
- Cross-sectional design ซ้ำ 4 ปีเพื่อ observe industry patterns

---

## บทเรียนจาก ING Netherlands (Case Study)

- ING ใช้ **Tribes + Squads + Chapters** โครงสร้าง Agile ที่ scale ได้
- **Obeya room** — visualizes strategic objectives แบบ 360 องศา
- **Catchball rhythm** — vertical + horizontal communication ทำให้ learning flow ทั้งองค์กร
- Leader ต้อง **เปลี่ยน behavior ก่อน** ค่อยคาดหวังให้ทีมเปลี่ยน — "lead by example"
- ไม่มี checklist/playbook — ต้อง experiment และ learn from own context
- "You can't implement culture change — you can only create conditions for it to emerge"

---

## สรุป核心 Message

1. **Software delivery performance สำคัญกับทุกองค์กร** — ไม่ใช่แค่ tech company
2. **ไม่มี trade-off ระหว่าง speed กับ stability** — ทำทั้งคู่ได้
3. **Focus on capabilities, ไม่ใช่ maturity model** — ปรับปรุงตลอดเวลา ไม่ใช่ "ถึง level แล้วหยุด"
4. **Practices เปลี่ยน culture ได้** — "act your way to a better culture"
5. **Leadership สำคัญ but indirect** — leader ต้องสร้าง environment ให้ทีม execute
6. **High performers ชนะอย่างต่อเนื่อง** — ใครไม่ improve จะถูกทิ้งไว้ข้างหลัง

---
