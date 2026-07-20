# สรุปภาษาไทย: Hey guys, Mark Larsen here — คู่มือเทรด Forex สำหรับคนเริ่มต้น

> **ที่มา:** หนังสือของ Mark Larsen (2010)
> **เนื้อหา:** การเลือกโบรกเกอร์, การติดตั้ง MT4, ความลับวงใน Forex, วิธีสังเกต scam systems, VPS สำหรับเทรด, กลยุทธ์ Bollinger Bands + Moving Average

---

## 1. โบรกเกอร์ Non-Dealing Desk (NDD) vs Dealing Desk

### ข้อดีของ NDD (ECN/STP)
- **ไม่มี Conflict of Interest**: โบรกเกอร์ NDD ไม่เทรดสวนลูกค้า — ไม่มีแรงจูงใจให้ราคาเสียเปรียบ
- **Market Access เท่าเทียม**: เทรดเดอร์เล็ก-ใหญ่ได้เรทจาก Interbank จริง ไม่ใช่ราคาที่โบรกเกอร์ตั้งขึ้นเอง
- **สภาพแวดล้อมการเทรดที่ดี**: ราคา Bid/Ask มาจากระบบ Interbank ไม่ถูกกรองหรือปั่นโดยโบรกเกอร์
- **โปร่งใส**: What you see is what you get

### ข้อเสียของ NDD
- **Spread ผันผวน**: ไม่คงที่ — ช่วง Peak Trading อาจแคบถึง 0 pip, ช่วง Off-Peak กว้างมาก
- **ค่าคอมมิชชั่น**: NDD คิด Commission แยก ซึ่งอาจสูงกว่า Dealing Desk

### โบรกเกอร์ที่ Mark แนะนำ: Iamfx.com
- เป็น ECN/STP Broker (White Label ของ GallantFX)
- **Spread ดิบจริง** (Interbank spread ไม่บวกเพิ่ม 2-3 pip)
- **Commission 1 pip** (เคย 2.4 pip — ปรับลดแล้ว)
- **VPS ฟรี** ทุกบัญชีจริง ไม่จำกัดขนาดเงินฝาก
- รองรับ EA สกัลปิ้ง (FAPTurbo, MegaDroid, RoboMinner ฯลฯ)
- **ข้อเสีย**: ไม่มี NFA member, ไม่รับ Credit Card, ฝากถอนแค่ Wire Transfer/Check
- Prefix คู่เงิน = `ifx`/`iam` — ต้องโหลด History Data แยกสำหรับ Backtest

---

## 2. การติดตั้ง MetaTrader 4 (MT4)
- โหลดฟรีจากโบรกเกอร์ที่สมัคร Demo
- **ใช้ได้เฉพาะ Windows** — Mac/Linux ต้องใช้ VPS
- Iamfx ให้ VPS แบบ Web-based (เข้าผ่าน Browser, OS อะไรก็ได้)
- ตั้ง Leverage **1:100** ใน Demo Account

---

## 3. ทำไม 90% ของคนเทรด Forex ล้มเหลว?

### สาเหตุหลัก — Under-capitalized
- **Leverage คือตัวฆ่า**: คุม $100,000 ด้วย $1,000 = 100:1 leverage — ความเสี่ยงมหาศาล
- อย่าเปิด Standard Account ด้วย $1,000 หรือ Mini Account ด้วย $150
- กฎข้อ 1: **เล่นเท่าที่เสียได้**

### ปัญหาอื่นๆ
- **ขาดความอดทน**: Forex ต้องใช้ patience, skill, strategy — ใจร้อน = เจ๊ง
- **ไม่เรียนรู้จากความผิดพลาด**: ทุกขาดทุนคือบทเรียน
- **Money Management ไม่เป็น**: สำคัญพอๆ กับกลยุทธ์

---

## 4. ความจริงที่วงการ Forex ไม่บอก (Ugly Truth)

### โบรกเกอร์ Dealing Desk หลอกยังไง?
- **ไม่ส่งคำสั่งออกไปจริง**: ใช้ซอฟต์แวร์จำลองการเทรดภายใน (Dealing Server) — เงินไม่ไปตลาดจริง
- **ล่า Stop Loss (SL Hunting)**: ซอฟต์แวร์รวบรวมข้อมูล SL ของลูกค้า แล้วส่ง Spike ไปจัดการ SL ทิ้งยับ
- **ปิดบัญชีถ้าเทรดเดอร์ทำกำไรสม่ำเสมอ**: โบรกเกอร์ Deal Desk ไม่สนใจทำธุรกิจกับคนที่เอาตังค์ออก
- **โพสต์ Analytics ปลอม**: จ้างคนเขียนบทความชี้ทางผิด ให้เทรดเดอร์ตัดสินใจพลาด

### Guru ขายของ
- ขายคอร์สแพงๆ สอน Fibonacci, Elliot Waves — แต่ใช้เทรดจริงไม่ได้
- Internet เต็มไปด้วย "Scam Systems" — ระบบเทรดปลอมที่ promise กำไรเกินจริง
- Forex Industry ไม่อยากให้ข้อมูลนี้ถึงคุณ — ไม่งั้นพวกเขาจะเสียเงิน

---

## 5. วิธีสังเกต Forex Scam Systems

### Red Flags ⚠️

| Red Flag | คำอธิบาย |
|:---------|:---------|
| **Claims เกินจริง** | "ทำ 4,000,000 pips ปีที่แล้ว" = 16,000 pip/วัน, 900 pip/คู่/วัน — เป็นไปไม่ได้ |
| **Statement ปลอม** | รูปภาพ/ screenshot ใช้ Photoshop ได้ — ของจริงต้องเป็น HTML report หรือลิงก์ MT4Stats |
| **Black Box Approach** | อ้างว่าระบบซับซ้อนจน Backtest ไม่ได้ = ไม่มี proof |
| **Signal Service** | แจ้ง Signal รายวัน — มนุษย์ไม่สามารถนั่งจอ 24/5; พลาด Signal = โทษคุณ |
| **Indicators แบบไม่ Auto** | ขาย Indicator ที่ต้องมานั่งดู Alert ตลอดเวลา — ต้องเอาไปทำ Expert Advisor (Robot) |
| **EA ที่เชื่อมต่อ Server ภายนอก** | EA ส่ง Signal จาก Server Vendor — ไม่สามารถ Backtest; ถ้า Server ล่ม = เจ๊ง |
| **ไม่มี Backtest Results** | ไม่มีผลการเทรดย้อนหลัง = ไม่มีหลักฐาน |

### วิธีป้องกัน
- เช็คเว็บไซต์ของระบบก่อนซื้อ — ดู Red Flags ข้างต้น
- ใช้ **MT4Stats.com** หรือ Broker Statement แบบ HTML ตรวจสอบผลจริง
- ระบบที่ดีควรสามารถ **Backtest ได้**
- Mark มีบริการรีวิว Forex Systems ฟรีที่ `forex-systems-reviews.com`

---

## 6. VPS (Virtual Private Server) สำหรับเทรด Forex

### ทำไมต้องใช้ VPS?
- Forex Robot ต้องการเทรด 24/5 ถ้าเครื่องพัง/ไฟดับ/อินเทอร์เน็ตขาด = พลาดเทรดหรือเสียเงิน
- ใครใช้เครื่องร่วมกับครอบครัว — ลูกอาจปิด Platform หรือทำ Manual Trade พังพอร์ต
- Maintenance, Restart, Power Shortage — ปัญหาทุกอย่างแก้ได้ด้วย VPS

### VPS มี 3 แบบ

| ประเภท | ตัวอย่าง | เหมาะกับ |
|:-------|:---------|:---------|
| **Traditional VPS** (Windows Server 2003) | swvps.com, forexvps.com | สายเทคนิคที่ Setup ทุกอย่างเองได้ ถูก, ลง MT4 ได้หลายตัว |
| **Pre-installed MT4 Forex VPS** | ezforexhost.com, forexhoster.com | คนมีพื้นฐานคอมฯ แพงกว่าแต่ติดตั้ง MT4 มาให้แล้ว |
| **Web-based VPS** 🏆 | Iamfx.com VPS | **มือใหม่** — Set & Forget, เข้าผ่าน Browser, OS ไหนก็ได้, **ฟรี** |

### คำแนะนำ
- Mark มี **VPS ถึง 21 ตัว** จากหลายบริษัท — Diversification คือกุญแจ
- **อย่าไว้ใจ Home Computer** — Family, Kids, Power Outage = Recipe for Disaster
- สั่ง VPS เลย! อย่าเสียโอกาสกำไรเพราะ Hardware พัง

---

## 7. กลยุทธ์เทรดของ Mark Larsen (Bollinger Bands + Moving Average)

### ของที่ต้องใช้
- **Bollinger Bands (Period 10)** — บน M15 GBPUSD
- **Moving Average (Period 14)** — แสดงเป็นเส้นสีแดง
- **Broker แบบ Raw Spread** เช่น Iamfx.com (กลยุทธ์นี้ sensitive กับ spread)

### จังหวะเข้า Long (BUY)
1. รอให้ราคาแตะ **Lower Band** แล้วแท่งเทียนปิด **เหนือ** Lower Band (ไม่ปิดใต้)
2. รอให้แท่งเทียนปิดสนิท
3. เมื่อแท่งเทียนถัดไปเปิด **ใต้ Lower Band** — Enter Long

### จังหวะออก (Close)
- **อย่ารอจนถึง Upper Band** — คนส่วนใหญ่รอไม่ถึงแล้วพลาด
- **ให้รอจนราคามาแตะ Moving Average 14** แล้ว Close — นี่คือ trick

### Settings
- **TP = 30**, **SL = 130**
- ใช้ได้ดีกับ: EURUSD, USDCAD, EURGBP, USDJPY (ปรับ parameter ตามคู่)
- **ห้ามเทรดช่วง Economic News** ที่มีผลกระทบสูงต่อตลาด

### ตัวอย่างผลลัพธ์ของ Mark
- Micro Account ที่ Iamfx — เทรด GBPUSD, EURGBP, USDCAD, USDJPY
- ได้กำไรสม่ำเสมอ, Loss จำกัดมากเพราะ SL โดนยาก
- Mark ย้ำว่าไม่ได้ใช้แค่กลยุทธ์นี้กลยุทธ์เดียว แต่เป็นหนึ่งในกลยุทธ์ที่ดีที่สุดเท่าที่มี

---

## 8. Mark's Trading Law of 3/4 — วิธี Optimize ที่ไม่หลอกตัวเอง

### ปัญหา
- คนส่วนใหญ่ optimize settings บนข้อมูลในอดีตทั้งหมด → ได้ settings ที่ "เรียนรู้อดีต" แต่ใช้ในอนาคตไม่ได้
- Optimization คือ **การทำให้ระบบเรียนรู้ประวัติศาสตร์** ไม่ใช่การทำให้มันเทรดเก่งขึ้น

### วิธีของ Mark (Walk-Forward Optimization)
1. เลือกช่วงเวลาที่จะ Optimize (เช่น 2004–2005)
2. **แบ่งเป็น 4 ส่วน** — 3 ส่วนแรก = **Learning Zone**, 1 ส่วนสุดท้าย = **Testing Zone** (มองไม่เห็นตอน Optimize)
3. ใช้ Learning Zone Optimize หา Best Settings
4. **เอา Settings ไปรันบน Testing Zone** — ถ้าผลยังดี = ใช้ได้จริง
5. ถ้า Testing Zone ดี → Live ได้เลย ไม่ต้องเสียเวลา Demo ต่อ
6. ใช้ได้ยาวนานแค่ **2-3 สัปดาห์/เดือน** — แล้ว Optimize ใหม่ (ตลาดเปลี่ยนตลอด)

### สูตรย่อ
- สามารถใช้ย่อลงมาเป็น 4 สัปดาห์ (Learn 3 wk + Test 1 wk) หรือ 4 วันก็ได้
- ถ้า Settings ไหนรอด Testing Zone = Holy Grail ชั่วคราว

---

## 9. การติดตาม Mark Larsen
- Twitter: @forexmark
- Facebook: Mark Larsen
- Forex Systems Reviews: http://forex-systems-reviews.com/
- Forex Tester: http://forex-tester.com/
- ชมรมนักเทรด: http://secrets.bz/

---

> **ข้อควรระวัง**: เนื้อหานี้เป็นคำแนะนำจากปี 2010 — โบรกเกอร์, เทคโนโลยี, และตลาดเปลี่ยนแปลงไปมาก ควรตรวจสอบความทันสมัยก่อนนำไปใช้
