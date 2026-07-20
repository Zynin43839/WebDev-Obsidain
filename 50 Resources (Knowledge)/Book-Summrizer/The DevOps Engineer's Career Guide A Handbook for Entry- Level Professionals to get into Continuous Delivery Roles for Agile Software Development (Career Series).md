# The DevOps Engineer's Career Guide: A Handbook for Entry-Level Professionals to Get into Continuous Delivery Roles for Agile Software Development

> **Author:** Stephen Fleming
> **Pages:** 124

---

## ภาพรวมหนังสือ

หนังสือเล่มนี้เป็นคู่มือสำหรับมือใหม่ที่ต้องการเข้าสู่สายงาน DevOps (เดฟออปส์) โดยเฉพาะคนที่มีประสบการณ์ 0-5 ปี ต้องการเข้าใจว่า DevOps คืออะไร ต้องมีทักษะอะไรบ้าง ใช้เครื่องมืออะไร และจะเตรียมตัวสัมภาษณ์งานได้อย่างไร

---

## บทที่ 1: DevOps คืออะไร?

### คำจำกัดความ
DevOps (เดฟออปส์) เกิดจากการรวมกันของสองแนวโน้ม: Agile Infrastructure (โครงสร้างพื้นฐานแบบคล่องตัว) และความร่วมมือระหว่างทีม Development (พัฒนา) กับทีม Operations (ปฏิบัติการ) Jez Humble นิยามว่า DevOps คือการศึกษา和发展ระบบแบบเปลี่ยนแปลงเร็ว โดยใช้ข้อมูลจากหลายสาขาวิชา

### คำว่า "Dev" และ "Ops"
- "Dev" ไม่ได้หมายถึงแค่ Programmers (โปรแกรมเมอร์) แต่รวมถึงทุกคนที่เกี่ยวข้องกับการพัฒนาผลิตภัณฑ์ เช่น QA (คิวเอ) Analysts, Product Analysts
- "Ops" ครอบคลุม System Admin (แอดมินระบบ), Network Engineers (วิศวกรเครือข่าย), DBA (ผู้ดูแลฐานข้อมูล), Security (รักษาความปลอดภัย), Release Engineers

### ความสัมพันธ์กับ Agile (ไจล์)
DevOps เป็นแขนงหนึ่งของ Agile Software Development (การพัฒนาซอฟต์แวร์แบบคล่องตัว) โดย Agile ให้ความสำคัญกับความร่วมมือระหว่างทีม Product, Developer และลูกค้า ส่วน DevOps ขยายขอบเขตไปถึงการส่งมอบบริการทั้งหมดให้ลูกค้า

### 5 ระดับของ DevOps
1. **Agile Values** (ค่านิยมแบบไจล์) — ค่านิยมหลักที่ต้องเห็นตรงกัน
2. **Agile Principles** (หลักการไจล์) — กลยุทธ์ที่สนับสนุนค่านิยม
3. **Agile Methods** (วิธีการไจล์) — เช่น Scrum, XP
4. **Agile Practices** (แนวทางปฏิบัติ) — เช่น Planning Poker, Stand-ups
5. **Agile Tools** (เครื่องมือ) — เช่น planningpoker.com, Greenhopper

### 3 พื้นที่ปฏิบัติหลักของ DevOps
1. **Infrastructure Automation** (โครงสร้างพื้นฐานอัตโนมัติ) — สร้างระบบ, ตั้งค่า OS, Deploy (部署) แอปเป็นโค้ด
2. **Continuous Delivery (CD)** (การส่งมอบต่อเนื่อง) — สร้าง, ทดสอบ, Deploy แอปแบบอัตโนมัติและรวดเร็ว
3. **Site Reliability Engineer (SRE)** (วิศวกรความน่าเชื่อถือ) — ดูแลระบบ, ตรวจสอบการทำงาน, ออกแบบให้ทีมทำงานร่วมกัน

### ประวัติ DevOps
- 2000s: Enterprise Systems Management (ESM) เริ่มต้น
- 2008: O'Reilly จัด Velocity Conference ครั้งแรก
- 2009: Patrick Debois และ Andrew Clay Shafer บัญญัติคำว่า "DevOps"
- Patrick จัดงาน DevOps Days ที่ Ghent ทำให้แนวคิดแพร่หลาย

---

## บทที่ 2: ข้อเท็จจริงและประโยชน์ของ DevOps

### ประโยชน์หลัก
- **Deployment เร็วขึ้น**: ปล่อยระบบใหม่ได้รวดเร็วและมีประสิทธิภาพ
- **Agility (ความคล่องตัว)**: ทุกองค์กรไม่ว่าขนาดเท่าไหร่ก็เปลี่ยนเป็นระบบ Agile ได้
- **ประหยัดเงิน**: อัตโนมัติลดค่าใช้จ่าย Manual (Manual = ทำมือ)
- **ไม่มี Silos (ไซโล)**: ทีมต่างๆ ทำงานร่วมกันอย่างโปร่งใส
- **Development Cycle เร็วขึ้น**: การสื่อสารและความร่วมมือที่ดีขึ้น
- **Continuous Service Delivery (การส่งมอบบริการต่อเนื่อง)**: โค้ดปล่อยให้ User เร็ว ไม่มีช่องว่างระหว่าง Requirement กับ Production
- **Defects (ข้อบกพร่อง) ลดลง**: การพัฒนาแบบ Iterative (ทำซ้ำ) ช่วยลดข้อผิดพลาด

### DevOps ไม่ใช่อะไร?
- **ไม่ใช่ NoOps**: DevOps ไม่ได้หมายถึง Dev เข้ามายึดงาน Ops ทั้งหมด
- **ไม่ใช่แค่ Tools (เครื่องมือ)**: ต้องเข้าใจหลักการก่อน ค่อยเลือกเครื่องมือ
- **ไม่ใช่แค่ Culture (วัฒนธรรม)**: DevOps มีทั้งค่านิยม หลักการ และวิธีปฏิบัติ
- **ไม่ใช่แค่ Dev กับ Ops**: ทุกฝ่ายที่เกี่ยวข้องกับการส่งมอบซอฟต์แวร์ล้วนเป็นส่วนหนึ่ง
- **ไม่ใช่แค่ Job Title (ตำแหน่งงาน)**: แค่เปลี่ยนชื่อตำแหน่งไม่พอ ต้องเปลี่ยนวิธีทำงานจริง
- **ไม่ใช่ทุกอย่าง**: DevOps เป็นส่วนหนึ่งของวัฒนธรรมองค์กร ไม่ใช่ทั้งหมด

---

## บทที่ 3: Agile Management (การจัดการแบบไจล์)

### 3 เสาหลัก (Pillars)
1. **Transparency (ความโปร่งใส)**: ข้อมูลส่งต่อกันง่าย ทุกคนเห็นภาพเดียวกัน
2. **Inspection (การตรวจสอบ)**: ทดสอบ Feature (ฟีเจอร์) ทันทีเมื่อทำเสร็จ
3. **Adaptation (การปรับตัว)**: แก้ไขทันทีเมื่อพบปัญหา

### Agile Manifesto (宣言)
- บุคคลและการมีปฏิสัมพันธ์ สำคัญกว่า กระบวนการและเครื่องมือ
- ซอฟต์แวร์ที่ทำงานได้ สำคัญกว่า เอกสารประกอบที่สมบูรณ์
- ความร่วมมือกับลูกค้า สำคัญกว่า การเจรจาสัญญา
- ตอบสนองต่อการเปลี่ยนแปลง สำคัญกว่า ทำตามแผนเดิม

### 12 หลักการ Agile
- ให้ลูกค้าได้ของเร็วและสม่ำเสมอ
- รับ Feedback (ฟีดแบ็ก) จากลูกค้าแล้วปรับปรุง
- ส่งมอบซอฟต์แวร์ที่ทำงานได้เป็นประจำ
- ทีม Business และ Development ทำงานร่วมกันตลอด
- ให้ทีมพัฒนาได้รับการสนับสนุนที่เพียงพอ
- สื่อสาร Face-to-face (ต่อหน้า) ได้ผลดีที่สุด
- วัดความคืบหน้าจากซอฟต์แวร์ที่ทำงานได้
- ทีมต้อง Self-organizing (จัดการตนเอง) ได้ดี
- ทีมต้องทบทวนและปรับปรุงตนเองอยู่เสมอ

### Platinum Principles
- **หลีกเลี่ยง Formality (ความเป็นทางการ)**: อย่าเสียเวลากับ Presentation (พรีเซนเทชัน) ที่ไม่จำเป็น ติดต่อกันตรงๆ ดีกว่า
- **คิดและทำเป็นทีม**: ไม่ต้องมี Title (ตำแหน่ง) ให้ยุ่งยาก ให้ผลงานเป็นตัวบอก
- **Visualize (แสดงภาพ) แทนการเขียน**: ใช้ Diagram (แผนภาพ) และ Graph (กราฟ) สื่อสารได้เร็วกว่าเขียนรายงานยาวๆ

---

## บทที่ 4: หลักการและแนวคิด DevOps

### หลักการ DevOps
1. **Customer-Centric Action (เน้นลูกค้าเป็นหลัก)**: ต้องมี Feedback Loop (วงจรฟีดแบ็ก) สั้นๆ ติดต่อลูกค้าโดยตรง
2. **Always Create With The End In Mind (คิดถึงผลลัพธ์สุดท้าย)**: ต้องมองเห็นภาพรวมของผลิตภัณฑ์ ไม่ใช่แค่ทำตามหน้าที่
3. **End-To-End Responsibility (รับผิดชอบจนจบ)**: ทีมดูแลผลิตภัณฑ์ตั้งแต่ต้นจนจบ
4. **Cross-Functional Autonomous Teams (ทีมข้ามสายงาน)**: ทีมต้องมีทักษะครบและเป็นอิสระ
5. **Continuous Improvement (ปรับปรุงต่อเนื่อง)**: ต้องปรับตัวตามการเปลี่ยนแปลง ทดลองและเรียนรู้จากความล้มเหลว
6. **Automate Everything (ทำทุกอย่างเป็นอัตโนมัติ)**: ลดของเสีย สร้าง Cloud Platform (แพลตฟอร์มคลาวด์) มอง Infrastructure เป็นโค้ด

### Key Metrics (ตัววัดผล)
- **Deployment Lead Time**: เวลาตั้งแต่วิศวกรแก้ไขโค้ดจนถึงลูกค้าได้ใช้
- **Lead Time vs Process Time**: Lead Time = เวลาลูกค้ารอทั้งหมด, Process Time = เวลาทีมทำงานจริง (ไม่รวมคิว)

---

## บทที่ 5: ทักษะสำคัญสำหรับ DevOps Engineer

### หน้าที่หลักของ DevOps Engineer
- สร้าง CI/CD Pipeline (สายส่งโค้ดต่อเนื่อง) เพื่อเร่งการทดสอบและ Deploy
- ติดตาม Feedback Loop ให้ลูกค้าได้รับ Feedback เร็ว
- สร้าง Release (รีลีส) ที่ถี่และน่าเชื่อถือ

### วันทำงานปกติ
- เข้าประชุมหลายครั้งเพื่อให้ทีมทำงานร่วมกัน
- ลด Manual Effort (งานทำมือ) ให้มากที่สุด

### ทักษะทางเทคนิค (Technical Proficiency)
- **Languages**: Python, Ruby, Perl, Bash
- **OS**: Linux, Ubuntu
- **Networking & Storage**: ต้องเข้าใจเพราะไม่ใช่ Silo อีกต่อไป
- **Automation Tools**: Puppet, Vagrant, Chef, Ansible
- **Build Tools**: Jenkins, Maven
- **Source Control**: Git, GitHub, Perforce
- **Monitoring**: Nagios, Munin, Sensu

### ทักษะอื่นๆ
- **Big Picture Thinking (คิดภาพรวม)**: มองให้เห็นว่าลูกค้าต้องการอะไร
- **Network Awareness (ตระหนักเรื่องเครือข่าย)**: ต้องรู้เรื่อง Network
- **Testing (การทดสอบ)**: ทักษะทดสอบที่แข็งแกร่งเป็นพื้นฐานของ Automation
- **Soft Skills (ทักษะนุ่ม)**: สื่อสารได้ดี, อยากรู้อยากเห็น, ฟังเก่ง
- **Security Training (การรักษาความปลอดภัย)**: ต้องเขียนโค้ดที่ปลอดภัย
- **Customer-First Mindset (คิดถึงลูกค้าก่อน)**: ให้คุณค่ากับผลิตภัณฑ์ที่มีคุณภาพสูง
- **Collaboration (ความร่วมมือ)**: ช่วยเพื่อนร่วมงาน ทำหลายงานพร้อมกันได้

---

## บทที่ 6: ประเภทของ Deployment

### 4 ประเภทหลัก
1. **Minimum In-Service Deployment**: ระบุจำนวน Instance (ตัวอย่าง) ที่ต้องทำงานตลอด ทยอยอัปเดตทีละส่วน
2. **Rolling Application Updates**: กำหนดจำนวน Container (คอนเทนเนอร์) ที่อัปเดตพร้อมกัน แล้วทยอยอัปเดตไปเรื่อยๆ
3. **Blue/Green Deployment (บลูกรีน)**: สร้าง Infrastructure จำลองขึ้นมาใหม่ ตัวใหม่ทำงานคู่กับตัวเก่า ทดสอบเสร็จแล้วสลับไปใช้ตัวใหม่
4. **A/B Testing (เอบีเทสต์)**: เหมือน Blue/Green แต่ส่ง Traffic (ทราฟฟิก) ไปที่ตัวใหม่บางส่วนก่อน ทำให้สลับได้แม่นยำกว่า

### ตัวอย่าง Cycle
- Amazon ปล่อยโค้ดใหม่ทุก 30 นาที
- Southwest Airlines ปล่อย Patch 2 ครั้งต่อสัปดาห์เพราะต้องปฏิบัติตามกฎการบิน

---

## บทที่ 7: DevOps Engineer กับวิศวกรประเภทอื่น

### DevOps vs Build and Release Engineer
- **DevOps Engineer**: สร้าง Pipeline สำหรับส่ง Stack และ Tool ให้ทีมพัฒนา ใช้ Puppet, Chef, Bamboo
- **Build and Release Engineer**: ดูแล Source Control System (ระบบควบคุมซอร์สโค้ด) และ Build System (ระบบสร้างโค้ด) ใช้ Ansible, Jenkins

### DevOps vs Site Reliability Engineer (SRE)
- **DevOps**: เน้น Automation (อัตโนมัติ) ในฝั่ง Development, ใช้ Lean/Agile
- **SRE**: เน้นรักษา Infrastructure ใน Production Environment (สภาพแวดล้อมการผลิต)
- ทั้งสองมีจุดร่วม: ทำงานบนยอดปิรามิด สร้างวัฒนธรรมและระบบให้องค์กร Automation ได้

### DevOps vs Systems Engineer
- **Systems Engineer**: ออกแบบคอมพิวเตอร์ระบบ, ใช้ Data Modeling (แบบจำลองข้อมูล), สร้าง Prototype (ต้นแบบ)
- **DevOps Engineer**: เขียนโค้ด (Python, Java), สร้างระบบอัปเดตตัวเอง, ทำ Modular (แยกส่วน) ให้ง่ายต่อการ Automation

### DevOps vs Cloud Engineer
- **Cloud Engineer**: ออกแบบและดูแล Cloud Infrastructure (โครงสร้างพื้นฐานคลาวด์)
- **DevOps Engineer**: ใช้ Cloud Platform (เช่น AWS) เพื่อ Deploy และจัดการแอป

### DevOps vs Tools Engineer
- **Tools Engineer**: พัฒนา Tool สำหรับ Game Studio (สตูดิโอเกม) เช่น Maya, Unreal Engine
- **DevOps Engineer**: สร้าง Pipeline อัตโนมัติสำหรับการ Deploy ซอฟต์แวร์

---

## บทที่ 8: Microservice (ไมโครเซอร์วิส) และ DevOps Engineer

### Microservice Engineer
- Microservice คือ Service (บริการ) ที่ Deploy แยกกันได้ ทำหน้าที่เฉพาะเจาะจง
- สื่อสารกันผ่าน Web Interface (อินเทอร์เฟซเว็บ) เท่านั้น
- ข้อดี: ไม่มี SPOF (Single Point of Failure — จุดล้มเหลวเดียว), Scale (ขยาย) ได้ตามต้องการ, เพิ่มฟีเจอร์ใหม่ได้โดยไม่กระทบส่วนอื่น

### DevOps Architect (สถาปนิกเดฟออปส์)
- วาดภาพรวมของแอป สร้าง Blueprint (พิมพ์เขียว) ให้ทีม
- วาง Framework (กรอบงาน) DevOps ทั้งองค์กร

### DevOps ในวงการ Banking (ธนาคาร)
- ธนาคารย้ายจาก On-premise (ติดตั้งในบริษัท) ไป Cloud (เช่น AWS)
- ต้องการ DevOps Engineer ที่ดูแลความปลอดภัยและ Deploy ได้รวดเร็ว

### AWS Certified DevOps Engineer
- ได้รับการรับรองจาก AWS ว่ามีทักษะด้าน Provisioning (จัดสรรทรัพยากร), Operating, Managing ระบบ
- ต้องเชี่ยวชาญ: Continuous Delivery บน AWS, Security Automation, Monitoring, High Availability (ความพร้อมใช้งานสูง)

---

## บทที่ 9: เครื่องมือ DevOps

### Nagios และ Icinga
- **Nagios**: เครื่องมือ Monitoring (ตรวจสอบ) Infrastructure, มี Plugin (ปลั๊กอิน) มากมายจาก Community
- **Icinga**: Fork (แยกออกมา) ของ Nagios, มีฟีเจอร์ใหม่กว่า แต่ Nagios ยังเป็นที่นิยมกว่า

### Monit
- เครื่องมือ "Watchdog" (สุนัขเฝ้าบ้าน) ตรวจสอบว่า Process (กระบวนการ) ทำงานปกติ
- ถ้า Apache หรือ Adobe ล่ม Monit จะ Restart (รีสตาร์ท) ให้อัตโนมัติ
- เหมาะกับ Microservice Architecture (สถาปัตยกรรมไมโครเซอร์วิส)

### ELK Stack (Elasticsearch, Logstash, Kibana)
- เครื่องมือ Log Analytics (วิเคราะห์บันทึก) ที่นิยมที่สุด
- เก็บ Log จากทุก Service, Server, Network ไว้ที่เดียว
- ใช้แก้ปัญหา, ตรวจสอบ, Audit (ตรวจทาน), Security

### Consul.io
- เครื่องมือ Service Discovery (ค้นหาบริการ) สำหรับ Microservice
- ให้ Internal DNS Name สำหรับแต่ละ Service
- ใช้ Register (ลงทะเบียน) Server เป็น Cluster (คลัสเตอร์)

### Jenkins
- CI/CD Tool (เครื่องมือส่งต่อเนื่อง) ที่นิยมมาก มี Plugin มากมาย
- ใช้สร้าง Docker Container (คอนเทนเนอร์), รัน Unit Test (ทดสอบหน่วย)
- มีปัญหา Performance (ประสิทธิภาพ) และ Scaling (การขยาย) บ้าง
- ทางเลือก: CircleCI, Travis

### Docker
- เครื่องมือ Containerization (ทำเป็นคอนเทนเนอร์) เปลี่ยนวงการ IT
- ใช้จัดการ Configuration Management (การจัดการการตั้งค่า) และ Scale
- ข้อเสีย: สร้าง Container เล็กๆ ก็ใช้เวลานาน, จัดการ Storage/Networking/Security ยาก

### Ansible
- Configuration Management Tool (เครื่องมือจัดการการตั้งค่า) เหมือน Puppet และ Chef
- ง่ายกว่า Puppet/Chef มาก แนะนำให้เริ่มเรียนตัวนี้
- ใช้ Push และ Re-configure เครื่องที่ Deploy ใหม่

### Collectd / Collectl
- เครื่องมือเก็บ System Metrics (ตัววัดระบบ)
- วัดหลาย Parameters (พารามิเตอร์) พร้อมกัน
- ส่งข้อมูลไป ELK ให้ Business สร้าง Alert (แจ้งเตือน) และ Report (รายงาน)

### Git / GitHub
- SCM (Source Control Management — ระบบควบคุมซอร์สโค้ด) ที่ใช้กันมากที่สุด
- มีฟีเจอร์ Pull Request และ Forking
- เชื่อมกับ Jenkins สำหรับ CI/CD

---

## บทที่ 10: เส้นทางอาชีพ

### ความต้องการในตลาด
- DevOps Engineer เป็นหนึ่งในอาชีพ IT ที่เงินเดือนสูงที่สุด
- องค์กรที่ใช้ DevOps Deploy โค้ดถี่กว่า 30 เท่า
- ตำแหน่งงาน DevOps บน Indeed/LinkedIn เพิ่มขึ้น 50% ใน 2 ปี

### เงินเดือน
- เริ่มต้นที่ $94,000/ปี (ขึ้นอยู่กับประสบการณ์)
- Median (ค่ากลาง) อยู่ที่ $104,000 - $129,230

### ตำแหน่งงานที่เกี่ยวข้อง
- DevOps Architect (สถาปนิก)
- Automation Engineer (วิศวกรอัตโนมัติ)
- Software Tester (ผู้ทดสอบซอฟต์แวร์)
- Security Engineer (วิศวกรรักษาความปลอดภัย)
- Integration Specialist (ผู้เชี่ยวชาญการเชื่อมต่อ)
- Release Manager (ผู้จัดการรีลีส)

### เริ่มต้นอย่างไร?
- **College Courses**: จบ Computer Science (วิทยาการคอมพิวเตอร์) หรือ Electrical Engineering (วิศวกรรมไฟฟ้า) เป็นพื้นฐาน
- **MOOCs (คอร์สออนไลน์ฟรี)**: EdX, Udacity, Coursera มีคอร์ส DevOps
- **Public Demonstration**: เขียน Tool หรือ Blog แล้วแชร์บน GitHub
- **Follow DevOps Conversation**: ติดตามกระแสในวงการ, เข้า Community

---

## บทที่ 11: ทำการตลาดตัวเองเป็น DevOps Engineer

### Cross-Training (ฝึกข้ามสาย)
- Developer มีข้อได้เปรียบเพราะเข้าใจ Coding
- Sys Admin ต้องฝึก scripting (Python, Perl, Ruby, Bash) และ Automation Frameworks

### Build Skills In The Job You Have
- ทำ DevOps-minded tasks ในงานปัจจุบัน แม้ยังไม่เปลี่ยนตำแหน่ง
- Automate (ทำอัตโนมัติ) งานที่น่าเบื่อ จะเข้าใจเทคโนโลยีมากขึ้น

### Involve With Community (มีส่วนร่วมกับชุมชน)
- เข้า Meetup (พบปะ), เขียน Blog, ทำงาน Open Source (โอเพนซอร์ส)
- เข้า Slack Channel สาธารณะ ตอบคำถาม ช่วยเหลือคนอื่น
- ไม่ได้ทำแค่สร้างโปรไฟล์ แต่เป็นการเชื่อมต่อกับคนในวงการ

### Demonstrate Empathy and Curiosity (แสดงความเห็นอกเห็นใจและความอยากรู้)
- **Curiosity**: ถามคำถามที่ถูกต้อง, สร้างทักษะกว้างๆ
- **Empathy**: ฟังเก่ง, วางตัวในสถานการณ์ที่ต้องสื่อสารกับคนอื่น
- อธิบายสิ่งที่ทำให้คนอื่นเข้าใจ ไม่ใช่ทำแค่คนเดียวเข้าใจ

---

## บทที่ 12: คำถามสัมภาษณ์งาน

### DevOps คืออะไร?
- ปรับปรุงความร่วมมือระหว่าง Operations และ Development
- ไม่ใช่แค่เครื่องมือ แต่เน้นที่ People (คน) และ Process (กระบวนการ)
- ได้รับแรงบันดาลใจจาก Lean (ลีน) และ Agile (ไจล์)
- อัตโนมัติ Development, Operation, Release

### องค์ประกอบของ DevOps
- Continuous Monitoring (ตรวจสอบต่อเนื่อง)
- Continuous Integration (รวมโค้ดต่อเนื่อง)
- Continuous Delivery (ส่งมอบต่อเนื่อง)
- Continuous Testing (ทดสอบต่อเนื่อง)

### Continuous Integration (CI) คืออะไร?
- นักพัฒนาทำงานบน User Story (เรื่องผู้ใช้) แล้ว Commit (ส่งโค้ด) เข้าระบบ
- โค้ดทุกชิ้นรวมกันเป็นผลิตภัณฑ์สุดท้าย ทดสอบสม่ำเสมอ

### Continuous Delivery (CD) คืออะไร?
- ฟีเจอร์ที่พัฒนาแล้วถึงลูกค้าเร็วที่สุด
- เป็น Extension (ส่วนขยาย) ของ CI
- ผ่าน Testing และ QA (คุณภาพ) แล้ว Deploy สู่ Production

### Continuous Testing คืออะไร?
- ใช้ Automation และ Unit Test ตรวจสอบโค้ดให้ตรงกับวิสัยทัศน์

### Continuous Monitoring คืออะไร?
- ตรวจสอบประสิทธิผลหลัง Deploy แล้ว หา Defect (ข้อบกพร่อง) ตั้งแต่เนิ่นๆ

### ขั้นตอน Implement DevOps
1. **Stage 1**: ประเมินกระบวนการปัจจุบันอย่างน้อย 3 สัปดาห์ ใช้ 5 แอปทดสอบ
2. **Stage 2**: ทำ Pilot POC (การทดสอบนำร่อง) แสดงให้ลูกค้าเห็น
3. **Stage 3**: Implement Model DevOps แล้วรันทุกกระบวนการ

### Waterfall (น้ำตก) vs Agile
- Waterfall มีช่องว่างระหว่าง Build กับ Deploy มาก ทำให้ Feedback ช้า
- Agile ลดช่องว่างนี้ DevOps เป็น Offshoot (แขนงย่อย) ของ Agile

### Continuous Delivery vs Continuous Deployment
- **CD Delivery**: โค้ดพร้อม Deploy เสมอ แต่ไม่ได้ Deploy ทุกฟีเจอร์
- **CD Deployment**: ทุก Change ผ่าน Testing แล้ว Deploy สู่ Production ทั้งหมด

### เครื่องมือ DevOps ที่สำคัญ
| เครื่องมือ | ใช้ทำอะไร |
|:---------:|:--------:|
| Jira | Issue Management (จัดการปัญหา) |
| GitHub | CI/CD + Version Control |
| Jenkins | CI/CD (โอเพนซอร์ส) |
| SonarQube | Code Analysis (วิเคราะห์โค้ด) |
| JFrog Artifactory | Binary Repository (เก็บไฟล์ไบนารี) |
| Chef/Ansible/Puppet | Configuration Management |
| Nagios/Splunk | Continuous Monitoring |

### Workflow ตัวอย่าง
1. Task/Defect/User Story เก็บใน Jira
2. Developer เลือก Task แล้วเขียนโค้ดเก็บใน Git
3. Jenkins ดึงโค้ดมา Build ทุกครั้งที่ Check-in
4. WAR File เก็บใน Nexus/Artifactory
5. Unit Test ใช้ SonarQube
6. Continuous Delivery Deploy ไป Environment ต่างๆ
7. Continuous Testing รันใน Test Environment
8. Continuous Monitoring ตรวจสอบใน Production

---

## สรุป

DevOps ไม่ใช่แค่เครื่องมือหรือตำแหน่งงาน แต่เป็นวิธีคิดและวิธีทำงานที่ให้ความสำคัญกับความร่วมมือระหว่างทีม Development และ Operations เพื่อส่งมอบซอฟต์แวร์ที่มีคุณภาพให้ลูกค้าได้เร็วที่สุด ผู้ที่ต้องการเข้าสู่สายงานนี้ควรมีทักษะทั้งทางเทคนิค (Coding, Automation, Cloud) และ Soft Skills (สื่อสาร, ร่วมมือ, อยากรู้) พร้อมทั้งมีส่วนร่วมกับ Community ของวงการ DevOps

---
