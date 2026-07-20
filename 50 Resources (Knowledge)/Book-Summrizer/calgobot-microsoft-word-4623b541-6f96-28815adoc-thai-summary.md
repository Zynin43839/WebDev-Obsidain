# สรุปหนังสือ: The Daily Fozzy Method

> **ผู้เขียน:** Michael Dunbar
> **เนื้อหา:** ระบบเทรด Forex แบบ Daily Chart ที่เน้นความเรียบง่ายและใช้งานได้จริง
> **ที่มา:** forex-rewards.com

---

## 📌 ภาพรวม

The Daily Fozzy Method เป็นระบบเทรด Forex ที่ใช้ **Daily Chart** เป็นหลัก โดยมีจุดประสงค์ให้เทรดเดอร์สามารถเทรดได้ในชีวิตประจำวันโดยไม่ต้องนั่งเฝ้ากราฟทั้งวัน เหมาะสำหรับคนที่ต้องทำงานประจำหรือไม่อยากให้การเทรดมาคุมชีวิต

---

## 🔧 การตั้งค่า Indicators

### Indicators ที่ต้องใช้
1. **RSI (Relative Strength Index)** — ตัวหลักของระบบ
2. **EMA 8  Period บน RSI** — Moving Average สำหรับ RSI
3. **ATR (Average True Range)** — สำหรับคำนวณ Stop Loss
4. **EMA 28 Period บน Chart** — สำหรับทิศทางราคาและ Target

### การตั้งค่า
- ใช้ **Daily Chart (D1)** เท่านั้น
- ATR แสดงแค่ค่าตัวเลข ไม่ต้องแสดงกราฟ (ตั้งสีเป็น "none")
- ต้องหลีกเลี่ยงกราฟที่มี "Sunday Candle" (จะทำให้ crossover เพี้ยน)

---

## 📈 กติกาการเข้าเทรด (Entry Rules)

### BUY Signal
- **RSI ตัดขึ้น** ผ่าน EMA 8 จากข้างล่าง

### SELL Signal
- **RSI ตัดลง** ผ่าน EMA 8 จากข้างบน

### กฎสำคัญ
- **ห้ามเข้าเทรดวันที่เกิดสัญญาณ** — ต้องรอให้ Daily Candle ปิดก่อน (cross อาจหายไปตอนสิ้นวัน)
- **เข้าเทรดที่ Opening Price ของแท่งถัดไป**

---

## 🛡️ การตั้ง Stop Loss

### วิธีที่ 1: ATR-Based Stop Loss (แนะนำ)
- ใช้ **70% ของ ATR(20)** เป็น Stop Loss
- ATR แสดงถึงความผันผวนล่าสุดของคู่เงิน
  - ATR ต่ำ (30-50 pips) = ตลาดนิ่ง
  - ATR สูง (100-200 pips) = ตลาดผันผวนมาก
- **กฎส่วนตัว:** ไม่ตั้ง Stop มากกว่า **100% ATR** เด็ดขาด

### วิธีที่ 2: Price Analysis
- ใช้ **High/Low ของเมื่อวาน** เป็น Stop Loss
- ใส่ Buffer 5-10 pips เผื่อราคาไปแตะพอดี

### วิธีที่ 3: ouble/Triple Top/Bottom
- ระดับที่ราคาเด้งหลายครั้ง = ระดับสำคัญ
- **ไม่ควรวาง Stop ภายในระดับเหล่านี้** เพราะมีโอกาสถูก SL ก่อนที่ราคาจะเด้ง

---

## 🚪 การออกจากเทรด (Exit Rules)

### กลยุทธ์ที่ 1: ไม่มี Target — Trailing Stop 70% ATR
- **ไม่ตั้ง Take Profit** — ปล่อยให้ตลาดตัดสิน
- ย้าย Stop Loss ขึ้นทุกวันเมื่อเทรดเป็นกำไร
- สร้างสถานะ **"Free Trade"** = ไม่มีความเสี่ยงแล้ว (profit ล็อคแล้ว)
- **ข้อสำคัญ:** ห้ามย้าย Stop กลับลง

### กลยุทธ์ที่ 2: ใช้ Target Point
- **ATR-Based Target** — ตั้ง TP = 50-70% ATR เท่ากับ Stop
- **Support/Resistance** — ใช้ระดับ S/R ที่ราคาเคยreact เป็น TP
- **EMA 28** — มักเป็นแนว Support/Resistance ใช้เป็นเป้าหมาย
- **Fibonacci Levels** — เน้นที่ระดับ **50.0%** เมื่อเทรด counter-trend

### กลยุทธ์ที่ 3: Multiple Lots
- แบ่ง 1 Standard Lot เป็น **2 Mini Lots**
  - Lot ที่ 1: ใช้ Target Point (scalp)
  - Lot ที่ 2: ไม่มี Target (ride trend)
- ทั้ง 2 Lot ใช้ Stop Loss เท่ากัน

---

## 🎯 การเลือกเทรด (Trade Selection)

### 1. เทรดตาม Trend
- ดูราคา 4-5 เดือนย้อนหลัง ว่ามี trend ชัดเจนหรือไม่
- ใช้ **EMA 28** เป็นตัววัด — ถ้าราคาอยู่ฝั่งเดียว = trend ชัด
- **หลีกเลี่ยง** สัญญาณที่ขัดกับ trend ใหญ่
- ดู Weekly Chart ด้วยเพื่อยืนยัน

### 2. Counter-Trend กับ Candlestick Patterns
- ใช้ **Double Top/Bottom** และ **Exhaustion Spike (Hammer/Shooting Star)** จับจุดกลับตัว
- **ต้องมี Fozzy Cross ยืนยัน** ด้วย — ห้ามเข้าแค่เพราะเห็น candle pattern

### 3. Convergence (การมาเจอกันของ indicators)
- เมื่อ **Fibonacci + EMA 28 + Trendline + S/R + Candle Pattern** มาเจอกัน = สัญญาณแข็งมาก
- ยิ่งมี indicators ยืนยัน越多 ยิ่งมั่นใจ

### 4. หลีกเลี่ยง Back-to-Back Signals
- ถ้ามีสัญญาณติดกันหลายวัน + EMA 8 ราบเรียบ = ตลาด Range
- **หยุดเทรด** หรือตั้ง TP 保守มากขึ้น
- อาจเปลี่ยนไปเทรด Breakout แทน

### 5. ระวัง News Releases
- ตรวจสอบ Forex Factory Calendar ก่อนเทรด
- หลีกเลี่ยงสัญญาณที่เกิดจากข่าวกระแทก
- **ข้อยกเว้น:** การขึ้นดอกเบี้ยแบบ Surprise อาจเปิด trend ใหม่ได้

### 6. รอ Retrace สำหรับการเคลื่อนไหวใหญ่
- ถ้าสัญญาณเกิดจากราคาเคลื่อนไหวมาก (ข่าวหรือไม่ก็ตาม)
- **รอ retrace** ที่ Fibonacci 23.6% หรือ 38.2% ก่อนเข้า
- ได้ราคาดีกว่า + Stop Loss สั้นลง
- ยอมพลาดบางเทรดที่ราคาไม่ retrace

---

## 💰 การจัดการความเสี่ยง

### Position Sizing
- **เสี่ยงไม่เกิน 3% ของ Equity** ต่อ 1 เทรด
- เริ่มต้นใหม่ให้ใช้ **1-2%** ก่อน
- **ห้ามลด Stop Loss เพื่อเทรด Lot ใหญ่ขึ้น**
  - กำหนด Stop ก่อน แล้วค่อยคำนวณ Lot Size

### Correlation
- หลีกเลี่ยงเทรดคู่เงินที่ **เคลื่อนไหวตรงกันข้าม** เช่น EUR/USD กับ USD/CHF
- เทรดทั้ง 2 = เพิ่มความเสี่ยงเป็น 2 เท่า
- เลือกคู่เงินที่ **เคลื่อนไหวอิสระ** เช่น EUR/USD กับ USD/CAD หรือ GBP/JPY

---

## 🧪 Backtesting

- **backtesting คือความมั่นใจ** — เมื่อขาดทุนติดต่อกัน 5 เทรด ความมั่นใจจาก backtesting จะช่วยให้ไม่ยอมแพ้
- ทดสอบกราฟทีละแท่งด้วยตัวเอง
- **_demo trading** อย่างน้อย 1-2 เดือน ก่อนเทรดจริง
- เทรดหลายคู่เงินใน demo เพื่อสะสมประสบการณ์

---

## 📊 สรุปจุดแข็ง-จุดอ่อน

### ✅ จุดแข็ง
- **เรียบง่าย** — ใช้แค่ RSI, EMA, ATR, Fibonacci
- **ไม่ต้องนั่งเฝ้ากราฟ** — Daily Chart เทรดวันละครั้ง
- **มีหลักการจัดการความเสี่ยงชัดเจน** — ATR-based stop, 3% risk rule
- **Multiple Lots** ผสมผสาน scalp กับ trend riding

### ⚠️ จุดอ่อน
- **สัญญาณ False Cross** บ่อยในช่วง Range
- **ต้องรอ Daily Close** — อาจพลาดบาง move
- **Backtesting ต้องทำเอง** — ไม่มี automated tool มาให้

---

## 💡 ประเด็นสำคัญสำหรับ Trader

1. **Daily Chart = ชีวิตปกติ** — ไม่ต้องเสียสุขภาพ
2. **Stop Loss คือชีวิต** — 70% ATR คือมาตรฐาน
3. **Free Trade คือเป้าหมาย** — เทรดที่ล็อค profit แล้วไม่มีความเสี่ยง
4. **Convergence = ความมั่นใจ** — หลาย indicators ยืนยันกัน
5. **Backtesting สร้างความมั่นใจ** — ไม่ยอมแพ้ตอน losing streak
6. **Correlation Management** — อย่าเทรดคู่เงินที่เคลื่อนไหวเหมือนกัน
