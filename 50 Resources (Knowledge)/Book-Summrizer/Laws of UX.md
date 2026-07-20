# Laws of UX

> **Author:** Jon Yablonski (2nd Edition, O'Reilly, 2023)
> **Pages:** 186 | **Focus:** กฎจิตวิทยา 10 ข้อที่นักออกแบบ UX ต้องรู้

---

## ภาพรวม: จิตวิทยากับการออกแบบ

หนังสือเล่มนี้รวมกฎหมายจิตวิทยาที่มีผลโดยตรงต่อการออกแบบ UX (User Experience) ผู้แต่งรวบรวมจากงานวิจัยทางจิตวิทยาหลายยุค ตั้งแต่ Gestalt Psychology ต้นศตวรรษที่ 20 ผ่าน Human Factors Engineering ยุคสงครามโลกครั้งที่ 2 จนถึง Human-Computer Interaction (HCI) และ UX Design ยุคปัจจุบัน

แก่นสำคัญคือ: **ออกแบบให้เข้ากับคน ไม่ใช่ให้คนปรับตัวเข้ากับเทคโนโลยี**

---

## 1. Jakob's Law — กฎของจาคอบ

> "ผู้ใช้ใช้เวลาส่วนใหญ่บนเว็บไซต์อื่น และต้องการให้เว็บของคุณทำงานเหมือนที่พวกเขาคุ้นเคย"

ผู้ใช้จะ **โอนย้ายความคาดหวัง (mental models)** จากเว็บหรือแอปที่ใช้บ่อยมาสู่เว็บใหม่ ถ้าดีไซน์ตรงกับสิ่งที่พวกเขาเคยเห็น พวกเขาจะทำงานได้ทันทีโดยไม่ต้องเรียนรู้ใหม่

**ตัวอย่าง:** ปุ่มฟอร์ม, toggle, radio input ทุกวันนี้ออกแบบเลียนแบบแผงควบคุมในโลกจริง เพราะผู้ใช้มี mental model อยู่แล้ว

**ข้อควรระวัง:** การเปลี่ยน UI แบบกระชาก (เช่น Snapchat 2018) ทำให้ผู้ใช้สับสนและย้ายไปคู่แข่ง (Instagram) แต่ถ้าให้ผู้ใช้เลือกเปลี่ยนเองแบบ Google (YouTube 2017) จะลดความขัดแย้งได้

**เทคนิค:** User Personas — สร้างตัวแทนผู้ใช้จากข้อมูลจริง เพื่อให้ทีมเข้าใจว่า "ผู้ใช้" คือใคร

---

## 2. Fitts's Law — กฎของฟิตส์

> "เวลาที่ต้องใช้ในการเลือกเป้าหมาย แปรผันตามระยะทางและขนาดของเป้าหมาย"

**เป้าหมายที่ใหญ่ขึ้น = เลือกง่ายขึ้น** | **เป้าหมายที่อยู่ใกล้ขึ้น = เลือกเร็วขึ้น**

สูตร: ID = log₂(2D/W) — ค่า D คือระยะทาง, W คือความกว้างของเป้าหมาย

**หลักการออกแบบ:**
- Touch targets ต้องใหญ่พอ (Apple 44×44 pt, Google 48×48 dp, WCAG 44×44 CSS px)
- ควรมี spacing ระหว่าง touch targets อย่างน้อย 8dp
- วางปุ่มสำคัญในจุดที่เข้าถึงง่าย เช่น ปุ่ม submit ควรอยู่ชิด input ตัวสุดท้าย

**ตัวอย่าง:** macOS วาง Dock ที่ขอบล่าง = infinite target (ไม่ต้องกังวล overshoot), iPhone มี Reachability สำหรับมือเดียว, Tesla Model 3 ใช้หน้าจอ 15" ต้องระวัง spacing ให้มาก

**เทคนิค:** Contextual Inquiry — สังเกตและสัมภาษณ์ผู้ใช้ในสภาพแวดล้อมจริง เพื่อเข้าใจพฤติกรรมที่แท้จริง

---

## 3. Miller's Law — กฎของมิลเลอร์

> "คนทั่วไปจดจำรายการได้ประมาณ 7 ± 2 ชิ้นใน working memory"

**สิ่งสำคัญ:** มิลเลอร์ไม่ได้บอกว่า UI ต้องมีไม่เกิน 7 ชิ้น เขาพูดถึง **chunking** — การจัดกลุ่มข้อมูลให้สมองประมวลผลง่ายขึ้น

**Cognitive Load (โหลดทางปัญญา)** คือปริมาณทรัพยากรทางจิตที่ต้องใช้เพื่อเข้าใจและใช้งาน interface ถ้าเกิน capacity ผู้ใช้จะสับสน หงุดหงิด หรือเลิกใช้

**เทคนิค Chunking:**
- จัดกลุ่มเนื้อหาที่เกี่ยวข้องด้วย สี, ขนาด, เส้นคั่น, ระยะห่าง
- เบอร์โทรแบบ chunked (081-234-5678) จำง่ายกว่า (0812345678)
- Nike.com จัดกลุ่มสินค้า + ตัวกรองอย่างชัดเจน แม้เมนูมีมากกว่า 7 ชิ้นก็ scan ง่าย

**ข้อควรระวัง:** งานวิจัยใหม่พบว่า capacity จริงอาจใกล้ 4 ชิ้นมากกว่า 7 ชิ้น

---

## 4. Hick's Law — กฎของฮิก

> "เวลาตัดสินใจเพิ่มขึ้นตามจำนวนและความซับซ้อนของตัวเลือกที่มี"

สูตร: RT = a + b log₂(n) — ยิ่งตัวเลือกมาก ยิ่งตัดสินใจช้า

**Paradox of Choice (ความขัดแย้งของทางเลือก):** การมีทางเลือกมากไม่ได้ทำให้มีความสุขขึ้น — ทดลองกับแยม 24 ชนิด vs 6 ชนิด กลุ่มหลังซื้อมากกว่า 10 เท่า

**วิธีออกแบบ:**
- ลดทางเลือกเมื่อเวลาสำคัญ เช่น Google Search แสดงตัวกรองเฉพาะหลังค้นหาแล้ว
- Onboarding แบบ Progressive — Notion ให้ทำทีละ step แทนที่จะโชว์ทุกฟีเจอร์
- Netflix ใช้ "Trending Now" เพื่อแนะนำและลด decision paralysis

**ข้อควรระวัง:** อย่า simplify จนเกินไป — icon ที่ไม่มี label จะทำให้ผู้ใช้สับสนมากขึ้น

**เทคนิค:** Card Sorting — ให้ผู้ใช้จัดกลุ่มหัวข้อตามที่พวกเขาเข้าใจ เพื่อออกแบบ Information Architecture ที่ตรง mental model

---

## 5. Postel's Law — กฎของโพสเทล

> "จงอนุรักษ์ในสิ่งที่คุณทำ จงเสรีในสิ่งที่คุณรับจากผู้อื่น"

**ส่วนที่ 1 — Output ต้องน่าเชื่อถือ:** Interface ต้องทำงานได้กับทุกอุปกรณ์, ทุกขนาดจอ, ทุก connection speed

**ส่วนที่ 2 — Input ต้องยืดหยุ่น:** ยอมรับ input ได้หลายรูปแบบ — keyboard, touch, voice, assistive technology

**ตัวอย่าง:**
- **Responsive Design:** CSS ทำให้เว็บปรับขนาดตามจอโดยอัตโนมัติ
- **Progressive Enhancement:** ให้ content พื้นฐานกับทุกคน เพิ่มฟีเจอร์เฉพาะอุปกรณ์ที่รองรับ
- **Face ID:** แปลง input ที่หลากหลาย (ใบหน้า) เป็น authentication ที่เข้าใจง่าย
- **Design Systems (IBM Carbon, Salesforce Lightning):** รับ input หลากหลายจากทีม 产出 output ที่ชัดเจน

**ข้อควรระวัง:** ออกแบบให้รองรับ internationalization — ข้อความภาษาอังกฤษขยายได้ 300% เมื่อแปลเป็นอิตาลี

**เทคนิค:** User Interviews — สัมภาษณ์ผู้ใช้ 1:1 เพื่อเข้าใจว่าพวกเขาคิดและใช้งานอย่างไร

---

## 6. Peak–End Rule — กฎจุดสูงสุดและจุดจบ

> "คนตัดสินประสบการณ์จากจุดที่รู้สึกมากที่สุด (peak) และตอนจบ (end) ไม่ใช่จากภาพรวมทั้งหมด"

**Memory Bias:** สมองจำเหตุการณ์เป็น "snapshot" ไม่ใช่ timeline ครบถ้วน

**การทดลอง:** จุ่มน้ำ 14°C 60 วินาที vs จุ่ม 60 วินาที + ต่ออีก 30 วินาที (น้ำอุ่นขึ้น 15°C) — คนเลือกทำซ้ำแบบที่สองแม้เจ็บนานกว่า เพราะจำได้ว่าตอนจบดีกว่า

**วิธีออกแบบ:**
- **Spotify Wrapped:** สร้าง peak moment บวกท้ายปี ให้ผู้ใช้รู้สึกดีกับประสบการณ์ทั้งปี
- **Uber:** ลด waiting pain ด้วย animation (idleness aversion), แสดง ETA (operational transparency), บอกขั้นตอน (goal gradient effect)
- **Duolingo:** ใช้ gamification (levels, streaks, Legendary Level) สร้างช่วงเวลาแห่งความสำเร็จ

**Negativity Bias (อคติด้านลบ):** ประสบการณ์ลบมีผลกระทบมากกว่าบวก — ต้องลด negative peaks เช่น Mailchimp ใช้ real-time password validation ป้องกันความหงุดหงิด

**เทคนิค:** Journey Mapping — วาดแผนที่ประสบการณ์ผู้ใช้ตาม timeline เพื่อหาจุด peak ทั้งบวกและลบ

---

## 7. Aesthetic–Usability Effect — ผลของความงามต่อการใช้งาน

> "ผู้ใช้มักมองว่า design ที่สวยนั้นใช้งานง่ายกว่า"

**Automatic Cognitive Processing:** สมองตัดสินว่า "สวยหรือไม่" ใน 50 มิลลิวินาทีแรก (System 1 thinking — เร็ว, อัตโนมัติ) และความรู้สึกนี้ rarely เปลี่ยนเมื่อใช้เวลานานขึ้น

**ผลวิจัย:** Kurosu & Kashimura (1995) ทดสอบ ATM 26 แบบ กับผู้เข้าร่วม 252 คน — พบว่า perceived usability แปรผันตาม aesthetic attractiveness มากกว่า inherent usability

**ตัวอย่าง:** Apple ออกแบบทั้ง hardware (iPod, iPhone, iMac) และ software ให้สวย + ใช้งานง่าย — ผู้ใช้ยอมอภัยปัญหาเล็กน้อยเพราะภาพรวมสวยงาม

**ข้อควรระวัง:** ความงามสามารถ mask ปัญหา usability ได้ — ในการทดสอบ ผู้ใช้จะให้คะแนน interface ที่สวยสูงกว่าแม้ใช้งานยากกว่า (Sonderegger & Sauer, 2010)

**เทคนิค:** Usability Testing — สังเกตผู้ใช้จริงทำ task วัดทั้ง speed, ease, และสิ่งที่พวกเขาพูด (เพราะสิ่งที่พูดกับทำมักไม่ตรงกัน)

---

## 8. Von Restorff Effect — ผลกระทบของความแตกต่าง

> "เมื่อมีวัตถุคล้ายกันหลายชิ้น ชิ้นที่ต่างจากคนอื่นจะถูกจำได้มากที่สุด"

**Selective Attention:** สมองกรองข้อมูลที่ไม่เกี่ยวออกเพื่อรักษา focus — ทำให้เกิด phenomena เช่น **Banner Blindness** (เมินโฆษณา) และ **Change Blindness** (ไม่เห็นการเปลี่ยนแปลง)

**วิธีสร้าง visual contrast:**
- **สี:** ปุ่มที่ต้องการให้กดควรโดดเด่นกว่าปุ่มอื่น (เช่น confirmation modal ที่มีปุ่ม Delete สีแดง)
- **ขนาด:** ข่าวเด่นใช้ขนาดตัวอักษรใหญ่กว่า headlines อื่น
- **Shape/Position/Motion:** Floating Action Button (FAB) ของ Material Design

**ข้อควรระวัง:**
- อย่าให้หลาย elements แข่งกันโดดเด่น — จะทำให้ทุกอย่างดูธรรมดาหมด
- ระวังผู้มี color vision deficiency — ต้องมี visual cues เพิ่มเติม (stroke, pattern, shape)
- ระวัง motion sensitivity — ผู้มี BPPV หรือ migraine อาจได้รับผลกระทบ

**เทคนิค:** Eye Tracking — วัดว่าผู้ใช้มองตรงไหน ช่วยเปิดเผย banner blindness, attention patterns

---

## 9. Tesler's Law — กฎของเทสเลอร์

> "ทุกระบบมีความซับซ้อนที่ลดไม่ได้อีกแล้ว ต้องถูกแบกรับโดย system หรือ user"

**แก่น:** ความซับซ้อนไม่หายไป แค่ย้ายที่ — นักออกแบบต้องรับไว้เอง อย่าผลักให้ผู้ใช้

**ตัวอย่าง:**
- **Email:** ระบบ fill "From" ให้อัตโนมัติ + แนะนำ "From" ตาม contact = ย้ายความซับซ้อนจากผู้ใช้ไปที่ system
- **Gmail Smart Compose:** AI ช่วยเขียนอีเมล = complexity abstraction ขั้นสูง
- **Amazon Go:** ไม่ต้อง cashier, ไม่ต้อง queue — ย้าย complexity ทั้งหมดไปที่ ML + computer vision
- **Intent-based interaction (AI):** ผู้ใช้บอกว่าต้องการอะไร ระบบจัดการเอง

**Complexity Bias:** คนมักเลือก solution ที่ซับซ้อนกว่า เพราะเข้าใจผิดว่า "ซับซ้อน = ดี"

**Paradox of the Active User:** คนไม่อ่านคู่มือ แต่เริ่มใช้ทันที — ต้องออกแบบ guidance ที่อยู่ใน context ของงาน (เช่น tooltips)

**เทคนิค:** Progressive Disclosure — แสดงเฉพาะสิ่งสำคัญก่อน เปิดเผยฟีเจอร์เพิ่มเติมเมื่อจำเป็น (dropdown, accordion, toggle)

---

## 10. Doherty Threshold — ขีดจำกัดโดเฮอร์ตี้

> "Productivity พุ่งขึ้นเมื่อ computer และ user interact ในจังหวะที่ไม่ต้องรอคอยกัน (<400 ms)"

**เวลาตอบสนองมีผลต่อ UX:**
- 100 ms = รู้สึกทันที
- 100-300 ms = เริ่มรู้สึกช้า
- >1 วินาที = สมาธิหลุด, คิดเรื่องอื่น
- >10 วินาที = สมาธิขาด

**Flow State (สภาวะลื่นไหล):** สมดุลระหว่างความยากของงานกับทักษะ — เมื่อถึงจุดนี้ ผู้ใช้จะจดจ่อและ productive ถึง 5 เท่า

**เทคนิคเพิ่ม perceived performance:**
- **Skeleton Screen:** Instagram แสดง placeholder แทน blank page — รู้สึกเร็วขึ้น
- **Blur Up:** Unsplash โหลด image ต่ำก่อน แล้ว blur รอ image จริง
- **Progress Bar:** แม้ไม่แม่น 100% แต่ทำให้รอได้ทนขึ้น (ให้ feedback ว่ากำลังทำงาน)
- **Optimistic UI:** Instagram แสดง comment ทันทีก่อน server confirm — รู้สึกเร็ว
- **ตั้งใจใส่ delay:** Google Privacy Checkup ใส่เวลาเกินจริงเพื่อสร้างความเชื่อมั่นว่าสแกนละเอียด

**ข้อควรระวัง:** ตอบเร็วเกินไปก็มีปัญหา — ผู้ใช้จะไม่ทันสังเกตการเปลี่ยนแปลง หรือไม่เชื่อถือ

---

## 11. การนำหลักจิตวิทยาไปใช้ในการออกแบบ

**3 วิธีฝังจิตวิทยาเข้าในกระบวนการออกแบบ:**

### 11.1 สร้างความตระหนักรู้ (Building Awareness)
- **Visibility:** ติดโปสเตอร์ Laws of UX บนผนังสำนักงาน
- **Show-and-Tell:** จัด session แบ่งปันความรู้เป็นประจำ

### 11.2 สร้าง Design Principles
Design Principles คือเข็มทิศของทีม — ช่วยให้ตัดสินใจได้เร็วและสอดคล้องกัน

**วิธีสร้าง:**
1. ระบุทีมที่เกี่ยวข้อง
2. Align บน success criteria
3. Diverge —  brainstorm design principles แต่ละคน
4. Converge — จัดกลุ่ม + dot voting
5. Refine + ระบุวิธีประยุกต์ใช้
6. Circulate — เผยแพร่ทั่วทีม

**Design Principles ที่ดี:** ไม่ใช่ truism (ไม่ใช่ "design ควรใช้งานง่าย" ที่ไม่ช่วยอะไร), แก้ปัญหาจริง, มีจุดยืนชัด, จำง่าย

### 11.3 เชื่อม Principles กับ Laws
ตัวอย่าง:
- **Principle:** "Clarity over abundance of choice" → **Law:** Hick's Law → **Rule:** จำกัดตัวเลือกไม่เกิน 3 ชิ้น
- **Principle:** "Familiarity over novelty" → **Law:** Jakob's Law → **Rule:** ใช้ common design patterns, หลีกเลี่ยง UI แปลกใหม่ที่ไม่จำเป็น

---

## 12. เมื่อมีอำนาจ มาพร้อมความรับผิดชอบ

**หัวใจ:** การใช้จิตวิทยาออกแบบให้ "ติด" (hook) ผู้ใช้ เป็นเรื่องอันตราย

### วิธีที่เทคโนโลยี_shapes พฤติกรรม (มักไม่ตั้งใจ):
- **Intermittent Variable Rewards:** Pull-to-refresh เหมือนสล็อตแมชชีน — random reinforcement ทำให้ติด
- **Infinite Loops:** Autoplay, infinite scroll = ไม่ต้องตัดสินใจ ดูต่อเรื่อยๆ
- **Social Affirmation:** Like/Comment = dopamine hit ชั่วคราว
- **Personalization:** TikTok ใช้ ML ดึงคนเข้า rabbit hole เนื้อหาสุดโต่ง (filter bubble)
- **Default Settings:** คนส่วนใหญ่ไม่เปลี่ยน default → default คือตัวตัดสินใจจริงๆ
- **(Lack of) Friction:** Amazon Dash button — ลบ friction จนกดซื้อได้ในคลิกเดียว
- **Reciprocity:** LinkedIn ใช้ความรู้สึก "ต้องตอบ" connection request
- **Dark Patterns:** Princeton study พบ dark patterns 1,818 จุดในเว็บ shopping 11,000 แห่ง

### ทำไมจริยธรรมสำคัญ:
- **ผลที่ไม่ตั้งใจ:** Facebook like button → วัดคุณค่าตัวเองจากยอด like, Snapchat filter → บางคนศัลยกรรมตาม
- **Smartphone ลด cognitive capacity** แม้ปิดอยู่ (Ward et al., 2017)
- **Social media ↔ ภาวะซึมเศร้าในวัยรุ่น** (Hunt et al., 2018)

### วิธีออกแบบอย่างรับผิดชอบ:
1. **Think beyond the happy path:** ออกแบบสำหรับ edge cases ก่อน ไม่ใช่ user ในอุดมคติ
2. **Diversify teams:** ทีมที่หลากหลาย = มองเห็น blind spots ได้มากกว่า
3. **Look beyond data:** Data บอก "what" แต่ไม่บอก "why" — ต้องคุยกับผู้ใช้จริง
4. **Embrace friction:** Friction ไม่ใช่ศัตรูเสมอไป — ใช้ป้องกัน error, ปกป้อง privacy, สร้างความเชื่อมั่น

---

## สรุปรวม: 10 กฎหมาย UX

| # | กฎหมาย | แก่น |
|---|--------|------|
| 1 | Jakob's Law | ใช้ mental model ที่ผู้ใช้มีอยู่ |
| 2 | Fitts's Law | เป้าหมายใหญ่ + อยู่ใกล้ = เลือกง่าย |
| 3 | Miller's Law | Chunk ข้อมูลเป็นกลุ่มเล็กๆ |
| 4 | Hick's Law | ตัวเลือกน้อย = ตัดสินใจเร็ว |
| 5 | Postel's Law | Output น่าเชื่อถือ, Input ยืดหยุ่น |
| 6 | Peak–End Rule | จดจำจากจุด peak + ตอนจบ |
| 7 | Aesthetic–Usability | สวย = ดูง่าย (แต่ระวัง mask ปัญหา) |
| 8 | Von Restorff | สิ่งที่ต่าง = ถูกจำ |
| 9 | Tesler's Law | ย้าย complexity ไปที่ system |
| 10 | Doherty Threshold | ตอบสนอง <400ms = productive |

---

**สรุปรวม:** หนังสือเล่มนี้คือ field guide สำหรับนักออกแบบ UX — ผสมผสานหลักจิตวิทยาเข้ากับตัวอย่างจริงจาก Apple, Google, Netflix, Uber, Spotify และแบรนด์อื่นๆ ทุกกฎหมายมีรากฐานจากงานวิจัย และทุกตัวอย่างมีทั้งข้อดีและข้อควรระวัง ผู้อ่านจะได้ทั้งความรู้และวิธีประยุกต์ใช้จริง

---
