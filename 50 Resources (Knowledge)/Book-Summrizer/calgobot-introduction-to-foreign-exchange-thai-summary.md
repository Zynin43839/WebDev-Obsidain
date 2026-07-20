# สรุปไทย: Introduction to Foreign Exchange

> **Source:** [[Introduction To Foreign Exchange]] | 17 หน้า | calgobot-introduction-to-foreign-exchange

---

## 1. ภาพรวมตลาด Forex

- **Forex คือตลาดการเงินที่ใหญ่ที่สุดในโลก** โดยมีมูลค่าการซื้อขายต่อวันประมาณ 3.2 ล้านล้านดอลลาร์
- เป็นตลาด Spot หรือ Cash market — ซื้อขายเงินตราต่างประเทศแบบทันที (ไม่รวม derivatives เช่น futures, options, forwards)
- เนื้อหาในเล่มนี้จำกัดอยู่ที่ Spot Forex เท่านั้น

---

## 2. คุณสมบัติเด่นของตลาด Forex

| คุณสมบัติ | รายละเอียด |
|-----------|------------|
| **สภาพคล่องสูงมาก** | ซื้อขายได้ทุกสกุลเงินตลอดเวลา เพราะจะมีคนพร้อมซื้อขายเสมอ |
| **เปิดทำการ 24 ชั่วโมง** | ปิดเฉพาะวันเสาร์-อาทิตย์ ต่างจากตลาดหุ้นที่ต้องรอกริ่ง |
| **เข้าถึงง่ายผ่าน Internet** | โบรกเกอร์ให้บริการ online trading กดซื้อขายได้ทันที ไม่ต้องโทรศัพท์แบบเดิม |

---

## 3. ตลาด Interbank — ต้นกำเนิดของ Quote

- **Interbank** เดิมหมายถึง ธนาคารใหญ่ exchanging rates กันเอง ปัจจุบันหมายถึง **ใครก็ได้ที่พร้อมซื้อขายเงินตรา**
- Quote (ราคา) ที่เราเห็นมาจาก centralized feed จาก **สถาบันการเงิน 300 แห่งแรกของโลก** — มั่นใจได้ว่าคำสั่งซื้อขายจะถูก fulfill
- **70%-90% ของ FX market เป็นการเก็งกำไร (speculative)** — ผู้ซื้อขายส่วนใหญ่ไม่ได้ต้องการรับมอบเงินจริง

---

## 4. Currency Pairs และ Major Currencies

### สกุลเงินหลัก (Major Currencies)
- **USD** (US Dollar) — คิดเป็น 90%+ ของการซื้อขายทั้งหมด
- **EUR** (Euro)
- **JPY** (Japanese Yen)
- **GBP** (Pound Sterling)
- **CHF** (Swiss Franc)

### ตัวอย่าง Currency Pairs
| Pair | ความหมาย |
|------|----------|
| EUR/USD | Euro vs US Dollar |
| GBP/USD | Pound Sterling vs US Dollar |
| USD/JPY | US Dollar vs Japanese Yen |
| USD/CHF | US Dollar vs Swiss Franc |
| USD/CAD | US Dollar vs Canadian Dollar |
| AUD/USD | Australian Dollar vs US Dollar |
| NZD/USD | New Zealand Dollar vs US Dollar |

> สกุลเงินที่ USD อยู่หน้า (เช่น USD/JPY) เรียกว่า **base currency** คือ 1 USD = กี่หน่วยของสกุลอื่น

---

## 5. Bid, Ask, Spread และ PIP

### Bid / Ask (Offer)
- **Bid** = ราคาที่ผู้ซื้อพร้อมซื้อ (ราคาซื้อ)
- **Ask (Offer)** = ราคาที่ผู้ขายพร้อมขาย (ราคาขาย)
- ตัวอย่าง: EUR/USD แสดง `0.9950/0.9955`
  - Bid = 0.9950 (ซื้อ Euro ได้ที่ราคานี้)
  - Ask = 0.9955 (ขาย Euro ได้ที่ราคานี้)

### Spread
- **Spread** = ความต่างระหว่าง Bid กับ Ask
- สำหรับ major currencies spread ปกติประมาณ **5 pips** (±1 pip)

### PIP (Point in Percentage)
- **PIP** คือหน่วยเล็กสุดที่ใช้วัดการเคลื่อนไหวของราคา
- สำหรับคู่ที่มี 4 Decimal Places (เช่น EUR/USD): 1 PIP = 0.0001
- สำหรับ JPY (มี 2  Decimal Places): 1 PIP = 0.01

### การคำนวณ Pip Value

**เมื่อ USD อยู่หน้า (เช่น USD/JPY, USD/CHF):**
```
Pip Value = (0.01 หรือ 0.0001) / Exchange Rate × Lot Size
```
- USD/JPY @ 116.73: `(0.01 / 116.73) × $100,000 = $8.56 per pip`
- USD/CHF @ 1.4840: `(0.0001 / 1.4840) × $100,000 = $6.73 per pip`

**เมื่อ USD ไม่ได้อยู่หน้า (เช่น EUR/USD, GBP/USD):**
```
Pip Value = (0.0001 / Exchange Rate) × สกุลเงิน 100,000 × Exchange Rate
```
- EUR/USD @ 0.9887: ได้ ≈ **$10 per pip**
- GBP/USD @ 1.5506: ได้ ≈ **$10 per pip**

> โบรกเกอร์จะคำนวณให้อัตโนมัติ — ไม่ต้องคำนวณเอง แต่ควรเข้าใจวิธีคิด

---

## 6. Lot Size และ Leverage

### Lot Size
| ประเภท | มูลค่า |
|--------|--------|
| **Standard Lot** | $100,000 |
| **Mini Lot** | $10,000 |

### Leverage (ทดน้ำ)
- ตลาด Forex ใช้ **margin account** — วางเงินมัดจำ (margin) เล็กน้อย就能 控制เงินจำนวนมาก
- ตัวอย่าง: Margin 1% → วาง $1,000就能 ควบคุม $100,000
- **Leverage = เงินที่ควบคุม / เงินจริง**

### Margin Call
- ถ้าขาดทุนเข้าใกล้ margin (เงินมัดจำ) → โบรกเกอร์จะ **margin call** — ให้เติมเงินหรือปิด position
- **Variation Margin** = กำไร/ขาดทุนของ position ที่เปิดอยู่
- โบรกเกอร์บางรายจะเพิ่ม margin requirement ในวันหยุด (เช่น 1% → 2% หรือมากกว่า)
- **Margin requirement แตกต่างกันในแต่ละโบรกเกอร์** (ตั้งแต่ 0.5% ถึง 5%)

---

## 7. Rollovers (การrollover ข้ามวัน)

- **Spot Forex deals** จะต้อง settle (ส่งมอบเงิน) ใน 2 วันทำการ (T+2) เรียกว่า **Value Date**
- ส่วนใหญ่ไม่ต้องการรับเงินจริง → broker จะ **rollover position** อัตโนมัติเวลา 21:59 ตามเวลาลอนดอน
- การ rollover เรียกว่า **tom.next** (tomorrow and next day)
- **ดอกเบี้ย (Interest Differential)**:
  - ถ้า long สกุลที่มีอัตราดอกเบี้ยสูงกว่า → **ได้รับ** ส่วนต่างดอกเบี้ย
  - ถ้า long สกุลที่มีอัตราดอกเบี้ยต่ำกว่า → **จ่าย** ส่วนต่างดอกเบี้ย

---

## 8. บัญชี (Accounts)

- กำไร/ขาดทุนอาจไม่ได้อยู่ในรูป USD เสมอ → อาจอยู่ใน JPY, EUR, GBP หรือ CHF แล้วแต่คู่ที่เทรด
- วิธีจัดการ: แจ้ง broker ให้แปลงกำไร/ขาดทุนเป็นสกุลเงินที่ต้องการเมื่อปิด position
- **Segregation of Funds**: เงินของลูกค้าต้องแยกออกจากเงินของ broker เพื่อป้องกันความเสี่ยง
- โบรกเกอร์ที่มีชื่อเสียงจะ deposit เงินลูกค้าไว้กับธนาคารที่ได้รับการ regulation
- **ดอกเบี้ยจากเงินฝาก**: โบรกเกอร์จ่ายดอกเบี้ยจากเงินที่ไม่ได้ใช้ margin

---

## 9. ผู้เล่นหลักในตลาด Forex

| ผู้เล่น | บทบาท |
|---------|-------|
| **Central Banks & Governments** | ควบคุมปริมาณเงิน (money supply) เพื่อรักษาเสถียรภาพทางการเงิน |
| **Banks ขนาดใหญ่** | ซื้อขายหลายพันล้านดอลลาร์ต่อวัน — ทั้งให้บริการลูกค้าและเก็งกำไร |
| **Hedge Funds** | จัดสรรเงินส่วนหนึ่งมาเก็งกำไรใน FX — leverage ได้สูงกว่าตลาดหุ้น |
| **Corporate Businesses** | ค้าขายระหว่างประเทศ → ต้องแปลงสกุลเงินเป็นประจำ (หลายพันล้านดอลลาร์ต่อวัน) |
| **Individual Traders (Man in the Street)** | แม้ไม่รู้ตัวก็เทรดอยู่แล้ว — เช่น ซื้อเงินต่างประเทศตอนไปเที่ยว, รูดบัตรเครดิตต่างประเทศ |
| **Speculators & Investors** | ซื้อขายเพื่อเก็งกำไรจากความเคลื่อนไหวของคู่เงิน (long/short) |

---

## 10. วิเคราะห์ Forex: Fundamental vs Technical

### Fundamental Analysis
- วิเคราะห์ **อุปสงค์และอุปทาน** ของสกุลเงิน
- หา **intrinsic value** (มูลค่าที่แท้จริง) เปรียบเทียบกับราคาตลาด
  - Intrinsic value < ราคาตลาด → มีโอกาส **ซื้อ**
  - Intrinsic value > ราคาตลาด → มีโอกาส **ขาย**

### Technical Analysis
- ศึกษา **Price Action** ผ่าน Chart และ Indicators
- หลักการสำคัญ 3 ข้อ:
  1. **Market action discounts everything** — ราคาที่เห็นรวมข้อมูลทุกอย่างไว้แล้ว
  2. **Price moves in trends** — ราคามีแนวโน้ม (trend)
  3. **History tends to repeat** — รูปแบบราคามักเกิดซ้ำ
- เครื่องมือ: Moving Averages, Support/Resistance, Bollinger Bands, Momentum, ฯลฯ

---

## 11. บทสรุปและคำแนะนำ

> **ความระมัดระวังคือหนทางที่ดีที่สุดในการเทรด**

- **อย่าเสี่ยงเงินที่ขาดทุนไม่ได้**
- **อย่าเทรดด้วยเงินจริงจนกว่าจะ paper trade ได้อย่างน้อย 3 เดือน**
- **ควบคุมอารมณ์** — อย่าให้ความกลัวหรือความโลภมาตัดสินใจแทน
- เลือกโบรกเกอร์ที่ **ได้รับการ regulation, มี insurance/bonding, มี track record ชัดเจน**
- ตรวจสอบว่า funds ของลูกค้า **segregated** หรือไม่
- อ่านเอกสาร policies ของโบรกเกอร์ให้ละเอียดก่อนเปิดบัญชี

---

*สรุปจากหนังสือ "Introduction to Foreign Exchange" — เนื้อหาครอบคลุมภาพรวมตลาด, กลไกการซื้อขาย, คำศัพท์สำคัญ, วิธีคำนวณ pip/lot, leverage, rollover, ผู้เล่นในตลาด, และแนวทางการวิเคราะห์*
