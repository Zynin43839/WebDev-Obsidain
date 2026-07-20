# 🐰 BunnyGirl's FAQ — สรุปภาษาไทย

> **ที่มา:** BunnyGirl's FAQ (Rev 1.0)
> **เนื้อหา:** ระบบเทรด Forex แบบ WMA Cross + Filter, กฎการเข้า-ออก, เทคนิคจัดการ Position

---

## 1. เงื่อนไขการเข้า-ออกทั่วไป

### คู่เงินที่เทรด
- คู่หลัก: **EUR/USD, GBP/USD, USD/CHF**
- คู่เสริม: EUR/JPY (บางครั้ง)
- คู่อื่นทดสอบแล้วไม่เหมาะกับระบบนี้

### สัญญาณเข้า (Entry Signal)
- **5 WMA ตัด 20 WMA** บนกราฟ 30 นาที
- **Filter**: หลัง crossover ต้องรอ price เคลื่อนไปอีก **20 pips** (EUR) หรือ **25 pips** (คู่อื่น) เพื่อกรอง whipsaw
- สำหรับ Long: ต้องบวก spread เข้าไปใน filter ด้วย; Short ไม่ต้องบวก

### ตั้งค่ากราฟ
- **4 timeframes**: 5 นาที, 30 นาที, 4 ชั่วโมง, Daily
- **Indicators**:
  - WMA: 5, 20, 100
  - Stochastic 2 ชุด: (5,3,3) สำหรับ fast + (50,3,3) สำหรับ slow
  - RSI 14
  - Bollinger Bands (20, 2)

### เงื่อนไขเพิ่มเติม
- **Daily Open** ใช้ 00:00 GMT เป็นจุดเปิดวัน
  - Long: เข้าเฉพาะเมื่อ crossover เหนือ daily open
  - Short: เข้าเฉพาะเมื่อ crossover ต่ำกว่า daily open
  - ไม่เข้าถ้า price อยู่ห่าง daily open 50+ pips แล้วกลับมา — มีความเสี่ยงสูง

---

## 2. Stop Loss & Take Profit

### Stop Loss
- SL ที่จุด crossover (ใช้วิจารณญาณถ้า crossover ใกล้ daily open หรือ WMA100)

### Take Profit (3 ระดับ)
1. **TP1**: Bollinger Band
2. **TP2**: 100 pips จาก high/low ของวัน
3. **TP3**: WMA20 บนกราฟ daily

### การเลื่อน SL
- หลัง profit +20 pips → เลื่อน SL ไป break even (+10 ถ้าเทรดชะลอ)

---

## 3. การจัดการ Position (4 Lots)

| Lot | ออกที่ | SL หลังมี Pips | วิธี Trail |
|:---:|:------:|:-------------:|:---------:|
| 1 | +30 pips | SL = 30 pips | — |
| 2 | +50 pips | SL = 50 pips | — |
| 3 | Trail จาก high/low ก่อนหน้า | min +30 | ตาม low ของ bar ก่อนหน้า |
| 4 | 100+ pips | SL = 100 pips | Trail ทุก 10 pips |

---

## 4. กฎสำคัญในการเข้า-ออก

### ห้ามเข้าเทรด
- ❌ ในช่วง **5 นาทีสุดท้าย** ของแท่ง 30 นาที — รอแท่งถัดไป break high/low ของแท่งก่อน
- ❌ เมื่อ **WMA100 อยู่ใกล้ crossover** และ price มุ่งหน้าไปหา (ต้องห่าง > 20 pips)
- ❌ เมื่อมี **Strong Resistance** อยู่ใกล้
- ❌ เมื่อ price **สวน Daily Trend**
- ❌ เมื่อ price ค้างอยู่ที่ระดับ entry นาน 2-3 ชั่วโมง (ไม่ขยับ)

### เข้าเทรดได้
- ✅ **Bounce** treated เหมือน crossover ( bounce off WMA20/WMA100 = สัญญาณเข้า)
- ✅ ถ้าถูก SL ออก → รอ crossover/bounce ใหม่ แล้วเข้าเมื่อ break high/low ของแท่ง 30 นาที
- ✅ **3 คู่เงินถึง entry พร้อมกัน** = สัญญาณที่ดีที่สุด (high-confidence)
- ✅ ถ้าเทรดค้างนาน → ลดลงมาดูกราฟ 5 นาที เพื่อ exit เร็ว

---

## 5. ศัพท์เฉพาะของ BunnyGirl (Bunnyisms)

| ศัพท์ | ความหมาย |
|:------|:---------|
| **Three in a bed** | WMA5, WMA20, WMA100 อยู่ใกล้กันมาก (confluence zone) |
| **In the dark** | ช่วงกลางแท่ง 30 นาที (ไม่ควรเข้า/ออก) |
| **Shadow zone** | Daily chart — เกิดเมื่อ price ขึ้น/ลง แล้วย้อนกลับมาที่ราคาเปิด |
| **The 5:10** | 17:10 (เวลา UK) — เวลาเลิกงาน London, stop ถูกดึง tight แล้วมักถูก hit |
| **Mr Sheen** | การ retrace หลังจาก move ใหญ่ —  ideal คือ retrace 25-30 pips แล้ว bounce off WMA20/WMA100 → เข้าเทรด |
| **Measuring Bar** | แท่งเทียนใหญ่ ที่ price ถัดมาเปิด/ปิดอยู่ใน range ของแท่งนั้น = ยังอยู่ในช่วง Range |
| **Baruch Bar** | แท่งที่ price ลงแต่ WMA ไม่ตัดลง (bearish candle แต่ WMA ยังไม่ confirm) |

---

## 6. เวลาเทรดที่ดีที่สุด

| ช่วงเวลา (GMT) | ระดับความน่าเชื่อถือ |
|:----------------:|:-------------------:|
| **06:00 - 09:00** | ⭐⭐⭐ ดีมาก |
| **12:30 - 15:00** | ⭐⭐⭐ ดีมาก (US Open) |
| 10:00 - 12:30 | ⚠️ ระวัง |
| หลัง 16:00 | ⚠️ ระวัง |

- **วันที่ดีที่สุด**: อังคาร & พุธ (เทรดทุกวัน 5 วัน)
- **เป้าหมาย**: 50 pips/วัน, 250 pips/สัปดาห์ขั้นต่ำ
- **เป้า**: 1 trade ต่อคู่ต่อวัน (บางที 2)

---

## 7. เทคนิคเพิ่มเติม

### RSI 14
- ชื่นชอบจุด **ตัดเส้น 50** มากที่สุด

### Stochastic 2 ชุด
- ใช้ Stoch (5,3,3) + Stoch (50,3,3) คู่กัน
- เมื่อทั้งคู่อยู่ที่ extreme เดียวกัน → สังเกต price action ใน 2-4 แท่งถัดไป
- ดู **divergence** ของ Stoch ด้วย

### Fibonacci
- ใช้ Fib ตอนเปิดวัน + เมื่อเห็น top/bottom ที่ชัดเจน
- ระวัง Fib retrace ตอน market เปิด — price มักจะล่อไปทางหนึ่งก่อน แล้วกลับทิศตอน 07:30

### WMA5 Touch Pattern (Daily Chart)
- Price มักจะ **แตะ WMA5 อย่างน้อย 1 ครั้งต่อวัน**
- ถ้าวันไหน **ไม่แตะ WMA5** → มักเป็น **สัญญาณกลับตัว** หรือ **คำเตือนว่าจะกลับตัว**
- ตัวอย่าง GBP: แตะ WMA5 ทุกวัน 15 มิ.ย. - 16 ก.ค. → 19 ก.ค. ไม่แตะ → ลง 600 pips!
- **เทรดวันถัดจากวันที่ไม่แตะ WMA5** = โอกาสดี

---

## 8. การออก (Exit Strategy)

- **แบ่ง exit ออกเป็น 4 ส่วน** — ใช้ strategy ต่างกันสำหรับแต่ละส่วน
- ไม่มี "วิธี exit ที่ดีที่สุด" เดียว — ต้องปรับตามเทรด

### Exit Methods ที่ใช้
1. **5 นาที chart** — ดู reversal bar หรือ break WMA20
2. **เข้าใกล้ WMA100**
3. **Bounce off Bollinger Band** (Gimme bar)
4. **100 pips จาก high/low**
5. **เข้าใกล้ Daily Open**
6. **เข้าใกล้ Local Resistance / Pivot / Round Numbers**
7. **High/Low ของแท่งก่อนหน้า**

---

## 9. ตัวอย่างกราฟ GBP (01/09/04)

| จุด | เหตุการณ์ |
|:---:|:---------|
| A | Cross down ที่ 1.8026, รอ entry ที่ 1.8001 |
| B | 1.8001 ถึงใน 5 นาทีสุดท้ายของแท่ง → ไม่เข้า (ผิดกฎ) |
| C | Entry 1.8001 อีกครั้ง, เข้าได้, ปิดต่ำกว่า WMA100 (สัญญาณดี) |
| D | ลงมาทำ low ที่ 1.7917 (+85 pips) |
| E | Bounce ลงจาก WMA100 → สัญญาณเข้าใหม่ |
| F | WMA5/20 bounce down ที่ 1.7972, รอ entry 1.7947 |
| G | Entry 1.7947 |
| H | High 1.7964 (ต่ำกว่า bounce) + ปิดต่ำกว่า WMA20 |
| I | Low 1.7892 (+55 pips) |

### Early Entry Setup (Case พิเศษ)
- แท่งที่ crossover ปิด **สวนทาง** → แท่งถัดไปกลับมาตาม crossover → เข้าเร็วได้
- **ใช้เฉพาะกับ setup นี้เท่านั้น** — ทุก setup อื่นต้องรอ filter ตามปกติ

---

## 10. สรุปหลักการสำคัญ

1. **WMA Cross + Filter = ระบบหลัก** — ใช้ crossover 5/20 WMA + 20-25 pips filter
2. **Daily Open เป็นตัวกรองที่สำคัญที่สุด** — ไม่เข้าข้าม daily open
3. **จัดการ 4 lots แบบแบ่ง_exit** — ออกทีละส่วน trailing ตาม progression
4. **"In the dark" คือ zone อันตราย** — ช่วงกลางแท่ง 30 นาที ไม่ควรเข้า/ออก
5. **WMA5 Touch Pattern** — ถ้า price ไม่แตะ WMA5 บน daily = สัญญาณกลับตัว
6. **Mr Sheen = retrace opportunity** — หลัง move ใหญ่ retrace 25-30 pips แล้ว bounce = จุดเข้าดี
7. **3 คู่เงินพร้อมกัน = สัญญาณแรงสุด** — ทั้ง 3 คู่ถึง entry พร้อมกัน = High confidence
8. **退出 ต้องมี 4 strategies** — ไม่มีวิธี exit เดียวที่ work ทุกครั้ง

---

*สรุปจาก: BunnyGirl's FAQ — ระบบเทรด Forex แบบ WMA Cross สำหรับนักเทรดที่ต้องการกฎที่ชัดเจนและมี disciple ในการจัดการ Position*
