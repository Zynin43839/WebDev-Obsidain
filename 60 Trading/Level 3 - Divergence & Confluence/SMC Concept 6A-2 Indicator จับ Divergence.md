---
created: 2026-05-29
type: note
topic: SMC
status: learning
---

# Divergence Indicators — Concept 6A-2: Indicator จับ Divergence (5 ตัวนอกเหนือ Relative Strength Index (RSI))

---

## 🔹 ความหมาย

RSI ไม่ใช่ Indicator เดียวที่ใช้หา Divergence ได้ — ยังมีอีก 5 ตัวที่นิยมใช้ แต่ละตัวมีจุดเด่น จุดอ่อน และ Timeframe ที่เหมาะสมต่างกัน

---

## 🔹 จุดประสงค์

1. **ยืนยัน Divergence** — ใช้ 2 Indicators พร้อมกัน = Confluence สูงขึ้น
2. **เลือก Tool ให้เหมาะกับ Timeframe** — M15 ใช้ Stochastic, H1 ใช้ MACD
3. **เห็นมุมมองต่างกัน** — แต่ละ Indicator มอง Market ไม่เหมือนกัน

---

## 🔹 วิธีใช้

### 1. MACD (Moving Average Convergence Divergence)

#### ความหมาย
MACD วัด **ความสัมพันธ์ระหว่าง Exponential Moving Average (EMA) 2 เส้น** — เส้นเร็ว (EMA 12) กับเส้นช้า (EMA 26)

```
MACD Line = EMA 12 - EMA 26
Signal Line = EMA 9 ของ MACD Line
Histogram = MACD Line - Signal Line
```

#### ใช้หา Divergence ยังไง
ใช้ **Histogram** เป็นหลัก — ดู Histogram สูงขึ้น/ต่ำลง เทียบกับราคา:

```
Bearish Divergence:
  ราคา: Higher High (HH)
  Histogram: Lower High (LH) — แท่งสั้นลง
  → โมเมนตัมซื้อลดลง → เตรียมกลับตัวลง

Bullish Divergence:
  ราคา: Lower Low (LL)
  Histogram: Higher Low (HL) — แท่งสั้นลง (ติดลบน้อยลง)
  → โมเมนตัมขายลดลง → เตรียมกลับตัวขึ้น
```

#### จุดเด่น
- ✅ นิยมมากที่สุดในหมู่นักเทรดสถาบัน
- ✅ แยก Divergence ได้ชัดกว่า RSI (เห็นเป็นแท่ง Histogram)
- ✅ ใช้กับ H1+ ได้ดีมาก

#### จุดอ่อน
- ❌ **ช้า** — ใช้กับ M15 อาจ Lag
- ❌ สัญญาณเกิดหลังจากราคาไปแล้ว 1-2 candle
- ❌ Histogram ดูยากตอน Sideway

#### ใช้กับ SMC ยังไง
```
MACD Divergence → หา Market Structure Shift → หา Order Block → Entry

เช่น: MACD Histogram Bearish Divergence → รอ External Change of Character ลง
→ หา Order Block Sell → เข้า
```

---

### 2. Stochastic Oscillator

#### ความหมาย
Stochastic เปรียบเทียบ **ราคาปิดล่าสุด** กับ **ช่วงราคาสูง-ต่ำ** ใน 14 แท่ง

```
%K = ((ราคาปิด - ต่ำสุด 14 แท่ง) / (สูงสุด 14 แท่ง - ต่ำสุด 14 แท่ง)) × 100
%D = SMA 3 ของ %K
```

#### ใช้หา Divergence ยังไง
ใช้เส้น **%K** หรือ **%D** — ดูเหมือน RSI แต่ **ไวกว่า**:

```
Bearish Divergence:
  ราคา: Higher High (HH)
  Stochastic: Lower High (LH)
  → โมเมนตัมเริ่มอ่อน — แม้ราคาจะขึ้นต่อ

Bullish Divergence:
  ราคา: Lower Low (LL)
  Stochastic: Higher Low (HL)
  → แรงขายเริ่มหมด — เตรียมกลับตัวขึ้น
```

#### Overbought / Oversold
```
Stochastic > 80 = Overbought  (ซื้อมากไป → เตรียมลง)
Stochastic < 20 = Oversold    (ขายมากไป → เตรียมขึ้น)
```

#### จุดเด่น
- ✅ **เร็วกว่า RSI** — Divergence โผล่ก่อน 1-2 candle
- ✅ เหมาะกับ M15 — ทันต่อการเคลื่อนไหวเร็ว
- ✅ Overbought/Oversold ชัด (80/20)

#### จุดอ่อน
- ❌ สัญญาณปลอมเยอะ — ต้องใช้ Confluence เสมอ
- ❌ ไวไป — อาจ Divergence แล้วราคาไปต่อ
- ❌ ต้องตั้งค่าถูก (14,3,3 เป็นมาตรฐาน)

#### ใช้กับ SMC ยังไง
```
Stochastic Divergence → รอ Market Structure Shift → Order Block → Entry

ข้อควรระวัง: Stochastic Divergence อย่างเดียว = 40% reliability
ต้องรอ Market Structure Shift ยืนยันก่อนเข้า
```

---

### 3. CCI (Commodity Channel Index)

#### ความหมาย
CCI วัด **ความเบี่ยงเบนของราคาจากค่าเฉลี่ยสถิติ** — ถ้าราคาเบี่ยงเบนมาก = ตลาด "ร้อนผิดปกติ"

```
CCI = (Typical Price - SMA of Typical Price) / (0.015 × Mean Deviation)
Typical Price = (High + Low + Close) / 3
```

#### ใช้หา Divergence ยังไง
```
Bearish Divergence:
  ราคา: Higher High (HH)
  CCI: Lower High (LH)
  → ถึงแม้ราคาจะขึ้นสูงขึ้น แต่ความเบี่ยงเบนลดลง = แรงซื้อแท้จริงอ่อน

Bullish Divergence:
  ราคา: Lower Low (LL)
  CCI: Higher Low (HL)
  → แรงขายที่แท้จริงเริ่มลดลง
```

#### Overbought / Oversold
```
CCI > +100 = Overbought  (ขึ้นแรงเกิน → เตรียมกลับ)
CCI < -100 = Oversold    (ลงแรงเกิน → เตรียมกลับ)
```

#### จุดเด่น
- ✅ จับ Turning Point ได้ดีกว่า RSI — เห็นก่อน
- ✅ ไม่มีขอบเขตตายตัว (RSI ตันที่ 0-100)
- ✅ ทนต่อ Sideway ได้ดีกว่า Stochastic

#### จุดอ่อน
- ❌ **แปรปรวนสูง** — CCI อาจพุ่ง 200+ แล้วกลับได้บ่อย
- ❌ ตีความยาก — ไม่ intuitive เท่า RSI
- ❌ Overbought/Oversold ที่ +100/-100 ไม่ได้แปลว่ากลับทันที

#### ใช้กับ SMC ยังไง
```
CCI Divergence เหมาะกับ:
- หาจุดกลับตัวของ Trend ใหญ่ (H1+)
- Entrance หลัง Market Structure Shift ชัดเจนแล้ว

ข้อควรระวัง: CCI Divergence อาจเกิด 2-3 ครั้งก่อนกลับจริง
→ อย่าเข้าทันทีที่เห็น CCI Divergence — รอ Market Structure Shift ก่อน
```

---

### 4. OBV (On-Balance Volume)

#### ความหมาย
OBV วัด **ปริมาณการซื้อขายสะสม** — ราคาปิดสูงขึ้น = บวก Volume ราคาปิดต่ำลง = ลบ Volume

```
ถ้าราคาปิด > ราคาปิดก่อนหน้า: OBV += Volume ปัจจุบัน
ถ้าราคาปิด < ราคาปิดก่อนหน้า: OBV -= Volume ปัจจุบัน
```

#### ใช้หา Divergence ยังไง
OBV บอก **"ปริมาณหนุนราคาจริงหรือเปล่า"**:

```
Bearish Divergence:
  ราคา: Higher High (HH)
  OBV: Lower High (LH)
  → ราคาขึ้นแต่ Volume ลดลง = แนวโน้มอ่อน
  → "ขาขึ้นไม่มีแรงซื้อจริง" → เตรียมกลับตัว

Bullish Divergence:
  ราคา: Lower Low (LL)
  OBV: Higher Low (HL)
  → ราคาลงแต่ Volume ซื้อเริ่มสะสม = กำลังมีแรงซื้อแอบสะสม
  → เตรียมกลับตัวขึ้น

Convergence (สัญญาณแข็ง):
  ราคา: Sideway
  OBV: เริ่มสูงขึ้น
  → มีแรงซื้อสะสมก่อนราคาขึ้น → เป็นสัญญาณแรกก่อน Divergence จะเกิด
```

#### จุดเด่น
- ✅ **บอกก่อนใคร** — Volume movement เกิดก่อนราคาเสมอ
- ✅ ใช้ยืนยัน Divergence จาก Indicator อื่น
- ✅ ใช้กับทุก Timeframe ได้ (M15-H1)

#### จุดอ่อน
- ❌ **ไม่มี Overbought/Oversold** — ใช้เดี่ยวลำบาก
- ❌ OBV สามารถวิ่งต่อไปเรื่อยๆ โดยไม่มีขอบเขต
- ❌ Volume ปลอม (spoofing) ทำให้ OBV สัญญาณผิด

#### ใช้กับ SMC ยังไง
```
OBV เป็น "ตัวกรอง" ก่อนใช้ Divergence:

Step 1: เช็ค OBV Divergence — Volume หนุนราคามั้ย?
Step 2: ถ้าไม่หนุน → หา RSI/MACD Divergence
Step 3: ถ้าหาเจอ → รอ Market Structure Shift
Step 4: หา Order Block → Entry

กฎ:
- OBV Divergence + อีก 1 Indicator Divergence → reliability สูง
- OBV ขึ้น แต่ราคา Sideway = แอบสะสมของ → เตรียม Buy
- OBV ลง แต่ราคา Sideway = แอบกระจายของ → เตรียม Sell
```

---

### 5. Awesome Oscillator (AO)

#### ความหมาย
AO วัด **Momentum ปัจจุบันเทียบกับ Momentum อดีต** — SMA 5 กับ SMA 34

```
AO = SMA 5 - SMA 34
→ แสดงเป็น Histogram: เขียว = AO เพิ่มขึ้น, แดง = AO ลดลง
```

#### ใช้หา Divergence ยังไง
AO ใช้ **หน้าตาของ Histogram** — ง่ายที่สุดใน 5 ตัวนี้:

```
Bearish Divergence:
  ราคา: Higher High (HH)
  AO: แท่ง Histogram สูงขึ้นครั้งแรก → แต่ครั้งที่สองกลับสั้นลง
  → โมเมนตัมซื้อลดลง → แม้ราคาจะขึ้นต่อ

Bullish Divergence:
  ราคา: Lower Low (LL)
  AO: Histogram ติดลบ → แต่แท่งติดลบสั้นลง (ใกล้ 0)
  → โมเมนตัมขายลดลง → เตรียมกลับตัวขึ้น
```

#### การอ่านสี AO
```
สีเขียว 2 แท่งติดกัน = โมเมนตัมซื้อ (Momentum เพิ่มขึ้น)
สีแดง 2 แท่งติดกัน = โมเมนตัมขาย (Momentum ลดลง)
เขียว → แดง = โมเมนตัมเริ่มเปลี่ยน (Saucer)
แดง → เขียว = โมเมนตัมเริ่มเปลี่ยน (Saucer)
```

#### จุดเด่น
- ✅ **ภาพชัด** — ดูแค่ Histogram สูง/ต่ำ + สีเขียว/แดง
- ✅ ใช้กับ M15 ได้ดี — ไวกำลังดี
- ✅ Zero Cross = เห็น Momentum เปลี่ยนชัด

#### จุดอ่อน
- ❌ Median-based — อาจตีความต่างกันในตลาด Sideway
- ❌ ขาด Overbought/Oversold — บอกระดับไม่ได้
- ❌ สัญญาณปลอมในตลาดไร้ทิศทาง

#### ใช้กับ SMC ยังไง
```
AO เหมาะกับการยืนยัน Market Structure Shift:

เมื่อ AO Histogram: เขียว → แดง (Saucer Sell)
+ ราคาทำ Higher High = Bearish Divergence
+ รอ External Change of Character ลง
→ Confluence สูงมาก → เตรียม Order Block SELL

เมื่อ AO Histogram: แดง → เขียว (Saucer Buy)
+ ราคาทำ Lower Low = Bullish Divergence
+ รอ External Change of Character ขึ้น
→ Confluence สูงมาก → เตรียม Order Block BUY
```

---

## 🔹 สรุป: Indicator ไหนใช้ตอนไหน

| Indicator      | Timeframe ที่เหมาะ | ความไว  | Overbought/Oversold | ใช้คู่กับ     |
| -------------- | ------------------ | ------- | ------------------- | ------------- |
| **RSI**        | H1+                | ปานกลาง | 70/30               | ทุก Indicator |
| **MACD**       | H1+                | ช้า     | ไม่มี               | RSI (ยืนยัน)  |
| **Stochastic** | M15                | เร็ว    | 80/20               | AO (กรองปลอม) |
| **CCI**        | H1+                | เร็ว    | +100/-100           | MACD          |
| **OBV**        | ทุก Timeframe      | ปานกลาง | ไม่มี               | RSI หรือ MACD |
| **AO**         | M15                | เร็ว    | ไม่มี               | Stochastic    |

### วิธีเลือกใช้

```
M15 (Scalp / เข้าเร็ว):    Stochastic + Awesome Oscillator
H1 (Swing):                RSI + MACD
เช็ค Volume แอบสะสม:       OBV + RSI
ยืนยัน Divergence:         2 Indicator พร้อมกัน → Confluence เพิ่ม
เช่น: RSI Divergence + MACD Divergence = Confluence 2 ก่อนหา Order Block
```

---

## 🔹 ข้อดี
- ✅ มี Indicator หลายแบบให้เลือกตาม Timeframe
- ✅ ใช้ 2 Indicator พร้อมกัน = reliability สูงขึ้น
- ✅ แต่ละตัวมอง Market ต่างมุม — เห็นรอบด้าน

## 🔹 ข้อเสีย
- ❌ Indicator มากไป = Analysis Paralysis (เลือกไม่ถูก)
- ❌ แต่ละตัวให้สัญญาณไม่พร้อมกัน — อาจสับสน
- ❌ ไม่มีตัวไหนใช้เดี่ยวได้ — ต้องใช้ Confluence เสมอ
