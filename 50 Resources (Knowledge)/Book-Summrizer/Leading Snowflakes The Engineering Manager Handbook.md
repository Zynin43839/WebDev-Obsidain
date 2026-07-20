# Leading Snowflakes: The Engineering Manager Handbook

> **Author:** Oren Ellenbogen

## ภาพรวมของหนังสือ

หนังสือเล่มนี้พูดถึงการเป็น **Engineering Manager** ที่ดี ครอบคลุมทุกแง่มุมตั้งแต่การจัดการเวลา การตัดสินใจ การสื่อสาร การ delegation การสร้างความไว้วางใจ ไปจนถึงการสร้างทีมที่เติบโตได้ ผู้เขียนเน้นว่า เราต้อง "ละทิ้ง" ความเป็น maker บางส่วน แต่ไม่ใช่ทั้งหมด เพราะถ้าตัดขาดจาก technical details ทีมจะไม่เชื่อใจเรา

---

## 1. สร้างสมดุลระหว่าง Maker กับ Manager

ปัญหาหลักของคนที่เลื่อนตำแหน่งจาก engineer มา manager คือ ชอบกลับไปทำ maker work เพราะสนุกกว่าและเห็นผลทันที แต่จริงๆ แล้วหน้าที่ของเราคือ **放大 (amplify) ทีม** ไม่ใช่ทำเอง

### ความแตกต่างระหว่าง Makers กับ Managers

- **Makers** ต้องการ time block ยาวๆ ไม่มี interruption เพื่อ "get into the zone" เพราะต้องแบกรับ context จำนวนมาก
- **Markers** วัดผลจาก ability ในการ scale ทีม ชอบ "small wins" จากการช่วยคนในทีม

### วิธีสร้าง "Maker Focus"

- ใช้ **role-based calendar** — แยกสีน้ำเงิน (Manager) กับสีเหลือง (Maker) แล้ว block เวลา maker ล่วงหน้า
- วางแผนวันที่บ้านตอนเช้ามืด เช่น เสร็จ 08:15 แล้วพร้อมลุย
- ใช้ **gesture เล็กๆ** เช่น ใส่หูฟัง, ไปนั่งห้องเงียบ, work from home ช่วงเช้า เพื่อสื่อสารว่ากำลัง focused

### เลือก task ที่เหมาะกับ Maker Mode

- **เล็ก (<4 ชม.)** — ไม่เป็น bottleneck ของทีม
- **ไม่ critical path** — ให้ทีม owned implementation ไปเลย
- **เพิ่ม velocity ทีม** — เช่น ทำ tool ลดงาน manual, ลด Technical Debt
- **ลอง technology ใหม่** — ทำ demo ให้ทีมดู
- **สร้าง brand บริษัท** — เขียน blog, ทำ tech talk, ทำ open-source

### สัดส่วนเวลาที่แนะนำ

- ทีมเล็ก 2-3 คน: maker time 50-60%
- ทีมใหญ่: อย่างน้อย 4-5 ชม./สัปดาห์ ห้ามตัดทิ้งหมด
- ทุกเดือนมา review ว่าสัดส่วนยังใช่ไหม

---

## 2. "Code Review" การตัดสินใจของ Manager

ผู้เขียนแนะนำให้สร้าง **Decision & Feedback Loop Tracking System** — ก็คือ spreadsheet บันทึก dilemma ที่เจอ

### โครงสร้างที่บันทึก

- **When**: วันที่เกิด
- **Who**: เกี่ยวกับใคร
- **The dilemma**: ปัญหาคืออะไร
- **The decision**: ตัดสินใจอย่างไร
- **Retrospection**: ทบทวนทีหลังว่าจะทำต่างออกไปไหม
- **分享แล้วหรือยัง**: ได้ขอ feedback จากใครไหม

### วิธีฝึกฝน

- **1:1 กับ boss**: เลือก 2 dilemma แล้วให้ boss แสดงวิธีจัดการของเขาก่อน ค่อย share ว่าเราทำอย่างไร
- **คู่หู EM**: นัด bi-weekly 30-60 นาที สลับกัน review
- **1:1 กับทีม**: บางครั้ง asked ทีมก็ช่วยได้มากกว่าคิด
- **10 นาที/วัน**: เขียน self-retrospection ตอนเช้า
- **1 ชม./เดือน**: ทบทวน decision ที่ทำไป 1-3 เดือนก่อน

### คำถามตอน retrospect

- มี technical dilemma ซ้ำๆ ไหม? → อาจเป็น Technical Debt
- มี conflict ระหว่างคนในทีมไหม?
- เราตัดสินใจเองหมดเลยไหม? ควรปล่อยให้ทีมตัดสินใจบ้าง
- share ได้ 50%+ ของ dilemma ไหม?

---

## 3. Confront และ Challenge ทีม

ส่วนที่ยากที่สุดของการเป็น manager คือการ "พูดตรงๆ" กับทีม โดยเฉพาะเมื่อเราเพิ่งเลื่อนมาจากเพื่อนร่วมทีม

### ความผิดพลาดของผู้เขียน

- กลัว hurt ความรู้สึก → ไม่บอก feedback → แก้ code ให้ทีมตอนไม่มีใครเห็น
- ผลคือ ทีมยิ่ง disconnected จากงาน ทำขั้นต่ำ ผู้เขียนทำงานหนักขึ้น

### Empathy vs Sympathy

- **Empathy**: เข้าใจมุมมองของเขา โดยไม่จำเป็นต้องเห็นด้วย
- **Sympathy**: รู้สึกแบบเดียวกับเขา → มักทำให้เรา subjective เกินไป เพราะใช้ความรู้สึกตัวเองตัดสิน

### Anti-Asshole Checklist

ถามตัวเองหลังให้ feedback ยากๆ:

1. **แสดง empathy ไหม?** — สะท้อนสถานการณ์จากมุมมองเขา
2. **ชัดเจนกับ expectation ไหม?** — บอกให้ชัดว่าคาดหวังอะไร
3. **practice what you preach ไหม?** — ทำตามที่พูดจริงไหม

ถ้าตอบ YES ทั้ง 3 ข้อ = เรา lead จากจุดที่ถูกต้อง

### 4 ความผิดพลาดที่ต้องหลีกเลี่ยง

1. **ไม่ share lesson learned ของตัวเอง** → ทีมไม่เห็นว่าเราก็กำลังเรียนรู้
2. **ไม่ follow up** → ส่ง email summary ทุกครั้งหลัง feedback เพื่อ track progress
3. **ใช้ medium ผิด** — ทีม introvert = 1:1, ทีม mature = team retrospective ได้
4. **เลื่อน feedback** — "เธอไม่ว่าง เดี๋ยวพรุ่งนี้ค่อยคุย" = เราตัดสินใจว่ามันไม่สำคัญ

---

## 4. สอนวิธี "Get Things Done" ด้วย Example

### Extreme Transparency

- ทำทุกอย่างให้ visible — ให้ทีมเห็นว่าเราคิดอย่างไร ทำไมถึงเลือก approach นี้
- แสดง "why" ชัดเจนเท่ากับ "how"

### ลดความเสี่ยง (Reducing Risks)

- ปล่อย feature เป็น phases → เรียนรู้จาก usage จริงเร็วขึ้น
- ปล่อยให้ subset เล็กๆ ก่อน → ไม่ต้อง solve scalability ตั้งแต่วันแรก
- มี backup plan เรื่อง performance เสมอ

### การวางแผน

- Break feature เป็น task เล็กๆ 1-3 ชม. → estimation แม่นยำขึ้น
- ยิ่ง task เล็ก ยิ่ง estimate ได้ดี → deadline น่าเชื่อถือ

### สร้าง Peer Pressure

- สร้าง **explicit dependencies** ระหว่าง task — ถ้าทำคู่กัน จะรู้สึกต้องทำให้ทันกัน
- ใช้ dummy data ก่อน แล้ว meet บ่อยๆ เพื่อ replace ด้วย code จริง
- สื่อสาร progress ทั้ง internal และ external สม่ำเสมอ

### Support, Bug, Documentation = First-Class Citizens

- หลัง feature live ต้อง split งาน bug fix, support, documentation ด้วย
- นี่คือวิธีสร้าง trust กับทีมอื่น

---

## 5. Delegation ที่ไม่สูญเสียคุณภาพ

### Responsibility List: Must / Delegate / External

- **Must**: สิ่งที่เราต้องรับผิดชอบเอง
- **Delegate**: สิ่งที่มอบหมายให้ทีม
- **External**: สิ่งที่คาดหวังจาก boss/ทีมอื่น

### วิธีคัดว่าจะ delegate อะไร

ถาม 2 คำถามกับ task ทุกอย่าง:

1. มันใช้ **unique strength** ของเราไหม?
2. มัน push เราไปสู่ **leader ที่อยากเป็น**ไหม?

ถ้าไม่ใช่ทั้งคู่ = delegate ไป

### ทบทวนด้วย "Virtual Vacation"

ถามตัวเอง: ถ้าหายไป 1 เดือน ทีมจะพังไหม? แล้วใครจะรับช่วง? คำตอบคือทีมจะไม่พัง และนั่นคือสิ่งที่ควร delegate

### 1-Pager สำหรับ Delegation

เขียนเอกสาร 1 หน้า ประกอบด้วย:

- **Goal**: มอบหมายอะไร
- **Behaviors**: คาดหวังให้ทำอย่างไร (communicative, active, aware)
- **Output**: ผลงานที่คาดหวัง
- **Success metric**: วัดว่าทำได้ดีไหม
- **Next action items**: สิ่งที่จะทำใน handoff meeting

### เป้าหมายสูงสุดของการ delegation

ค่อยๆ รู้สึกสบายขึ้นเมื่อถูก "CC" ใน email แทนที่จะเป็นคนตัดสินใจ → ทีมตัดสินใจเองได้โดยไม่ต้องรอเรา

---

## 6. สร้างความไว้วางใจกับทีมอื่น

### ทำไม teams มักทำงานแบบ silo?

- โฟกัสแค่ productivity → มองไม่เห็นภาพรวม
- ไม่มี vision alignment → ลืมว่าทำไมทีมถึงถูกตั้งขึ้น
- ไม่ share context กัน → ต่างคนต่าง optimize local throughput
- "Need vs Want" ปนกัน → ทุกอย่างดู urgent

### โครงสร้าง Bi-Weekly Sync สำหรับ Managers

1. **My top 3 priorities** พร้อมเหตุผล
2. **Would you change anything?** — ให้คนอื่นท้าทาย priorities
3. **Holdbacks & pains** — ใครจะ vacation, ปัญหา technical
4. **Things others depend on** — สิ่งที่รู้ว่าคนอื่นรอจากเรา
5. **Things I need help with** — ความต้องการของเรา
6. **Reconsider** — หลัง meeting ทบทวนว่า requirement ไหนยังไม่ได้ addressed

### 5 วิธีสร้าง trust ระหว่างทีม

1. **"Thank You!" email** — ขอบคุณคนจากทีมอื่นต่อหน้าทีม
2. **Internal tech talks** — ทีมละ 1 topic/เดือน share pain ที่เจอ
3. **Cross-team exchange** — แลกคนไปทำงานทีมอื่น 2-4 สัปดาห์
4. **Pizzability** — กิน pizza ดู usability video ของลูกค้าด้วยกัน
5. **Rotation ไป Design Review** — ส่งคน不同ไปเข้า meeting ทีมอื่น

---

## 7. Optimize สำหรับ Business Learning

### AARRR Framework

Dave McClure's 5 phases:

1. **Acquisition**: หา user ใหม่ยังไง
2. **Activation**: user ทำ task สำเร็จครั้งแรกไหม
3. **Retention**: user กลับมาใช้ซ้ำไหม
4. **Referrer**: user แนะนำคนอื่นไหม
5. **Revenue**: ทำเงินได้ไหม

### หลักการสำคัญ

- ถ้า **Retention ต่ำ** อย่าเพิ่ง spend เงินหา user ใหม่
- ถ้า **Activation แย่** อย่า optimize Retention
- **Technical Debt ไม่น่ากลัวเท่า BUSINESS FAIL** — ถ้า company ใกล้เจ๊ง ปล่อยของก่อนค่อย refactor

### ก่อน Product/Market Fit

- วัด **cycle time** ของ feature แต่ละตัว (hypothesis → release → learn)
- ตัด scope เหลือ MVP/POC → validate impact ก่อน invest หนัก
- ต้องมี analytics ทุก feature → ตัดสินใจด้วย data ไม่ใช่ gut feeling

### หลัง Product/Market Fit

- วัด **throughput** — feature count ต่อ sprint
- Track baseline ด้วย simple spreadsheet (T-shirt sizes)
- Optimize bottleneck: resolve dependencies, infrastructure, automation

---

## 8. Inbound Recruiting — ดึงดูด talent ด้วย brand

### Outbound vs Inbound

- **Outbound**: ติดต่อ candidate โดยตรง (phone, LinkedIn) — หยุดทำก็หยุดได้คน
- **Inbound**: สร้าง content/projects ที่ candidate อยากเห็น — long-lasting brand

### 9 วิธี Inbound Recruiting

1. **ตอบคำถามออนไลน์** 1 ชม./สัปดาห์ (StackOverflow, Quora)
2. **ซ่อน Easter egg** ใน product/website เช่น console.log, custom header
3. **เขียน guest post** — share lesson learned จากประสบการณ์จริง
4. **ฉลองวันเกิดแบบพิเศษ** — ทำ website/video ให้เพื่อนร่วมทีม
5. **จัด Hackathon** — ลงทุน $2,000 แทน referral program
6. **พูด 30 นาที** ที่ meetup — เลือก topic ที่ candidate สนใจ
7. **Side-projects** — สร้างเครื่องมือเล็กๆ ที่คนอยากใช้
8. **Open-source internal tools** — เช่น Netflix Chaos Monkey
9. **สร้าง public challenge** — coding puzzle ให้คนมาแก้

### การกระจายงาน

- Assign 1 ชม./สัปดาห์ให้คนในทีมสลับกัน
- ให้ ownership กับแต่ละคน เช่น "เดือนนี้ James เขียน 3 คำตอบที่ StackOverflow"
- ทำเป็น competition เล็กๆ ให้สนุก

---

## 9. สร้างทีมที่ Scalable

### 6 Traits ของทีมที่ Scalable

1. **Alignment of Vision** — ทีมเข้าใจ context และเชื่อมต่อกับ vision ของบริษัท
2. **Alignment of Core Values** — ค่านิยมร่วมที่ไม่ compromise
3. **Self-balanced** — distribute ทั้ง functional + growth responsibilities
4. **Core values over individuals** — ถ้าคนไม่ fit core values ก็ต้องยอมปล่อย
5. **Sense of accomplishment** — ฉลองชัยชนะเล็กๆ ไม่ให้ burnout

### วิธีทำให้เป็นจริง

- **Defining Vision**: เขียน vision ของทีม แล้วถามว่า "มัน move บริษัทไปสู่ winning position ไหม?" และ "มัน big enough เป็น engineering challenge ไหม?"
- **Defining Core Values**: ถามตัวเองว่า "สำคัญพอที่จะ fire คนที่ไม่เชื่อไหม?" ถ้าไม่ นั่นไม่ใช่ core value
- **Writing Expectations**: เขียนชัดว่าทีมคาดหวังจากเราอะไร และเราคาดหวังจากทีมอะไร

### Monitoring Bottlenecks

ถามตัวเอง 4 คำถาม:

1. **ใคร "all over the place"?** — คนนี้คือ bottleneck, ต้อง distribute knowledge
2. **ใครเป็น expertise bottleneck?** — มีคนเดียวที่รู้เรื่องนี้ ถ้าเขาลาออกทีมพัง
3. **ใครไม่ build trust?** — ต้อง expect ให้ชัดเจนเรื่อง communication
4. **"What's the worst thing about working here?"** — ถามใน 1:1 เพื่อหา blockers จริงๆ

---

## บทสรุป

เป้าหมายสูงสุดของ leader คือ **ทำตัวเองให้ redundant** — สร้างคนที่ manage ตัวเองได้ สิ่งที่เราต้องทำ:

- ตั้ง expectation ให้ชัด
- สอน พร้อม hold accountable
- ท้าทายให้คนในทีม grow

ท้ายที่สุดแล้ว ผลลัพธ์คือ **ทีมที่ scalable** — เปลี่ยนทิศทางได้โดยไม่สูญเสียวัฒนธรรม ขยายทีมได้โดยไม่สูญเสียคุณภาพ

---

**引用语ที่น่าจดจำ:**

> "Managers should exist for only one reason. To help those who work underneath them be successful and succeed." — Marc Schiller

> "A team is not a group of people that work together. A team is a group of people that trust each other." — Simon Sinek

> "You can't empower people by approving their actions. You empower by designing the need for your approval out of the system." — Kris Gale

> "If you can't make engineering decisions based on data, then make engineering decisions that result in data." — Kent Beck
