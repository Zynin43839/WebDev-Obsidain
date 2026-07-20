# Trading with Bollinger Bands — สรุปภาษาไทย

> **ผู้เขียน:** Toni Turner (President, TrendStar Trading Group, Inc.)
> **แหล่งที่มา:** PowerPoint Presentation (32 slides)
> **หนังสืออื่นของผู้เขียน:** A Beginner's Guide to Day Trading Online, A Beginner's Guide to Short Selling, A Beginner's Guide to Short-Term Trading

---

## 1. Bollinger Bands คืออะไร

- **ผู้สร้าง:** John Bollinger ในช่วงต้นทศวรรษ 1980
- **โครงสร้าง:** ประกอบด้วย band บน (Upper Band), band ล่าง (Lower Band), และ Middle Band ที่ล้อมรอบกราฟราคาของหุ้นหรือ index
- **หลักการทำงาน:** Band จะ **ขยายและหดตัว** ตามระดับ Volatility ของราคา — เมื่อราคาผันผวนสูง band จะกว้าง, เมื่อราคาสงบ band จะแคบ
- **คำถามหลักที่ BB ตอบ:** ราคา ณ ตอนนี้ "สูง" หรือ "ต่ำ" เมื่อเทียบกับค่าเฉลี่ย? — ราคาสูงที่ Upper Band, ราคาต่ำที่ Lower Band

---

## 2. สูตรคำนวณ (Default Settings)

| องค์ประกอบ | ค่าเริ่มต้น |
|:---|:---|
| **Middle Band** | Simple Moving Average (SMA) 20  периодов |
| **Upper Band** | SMA20 + 2 × Standard Deviation |
| **Lower Band** | SMA20 − 2 × Standard Deviation |

- Default จะครอบคลุม **~95% ของ price action** ทั้งหมด

### การปรับค่าตาม Time Frame
- **Long-term:** 50-period MA, 2.1 Standard Deviation
- **Short-term:** 10-period MA, 1.9 Standard Deviation

---

## 3. ขอบเขตการใช้งาน (Applications)

- **เครื่องมือ Decision Support** สำหรับ: หุ้น, index, options, commodities, financial futures, currencies, mutual funds
- **ใช้ได้ทุก Time Frame:** ตั้งแต่ yearly, quarterly, monthly, weekly, daily, hourly, minute ลงไปถึง tick!
- **หลักการสำคัญ:** BB ใช้ **เปรียบเทียบราคา和技术指标** เพื่อตัดสินใจซื้อขาย

---

## 4. การใช้งานหลัก 3 ประการ (Three Primary Uses)

### 4.1 Pattern Recognition (การจำแนกรูปแบบ)
- จำแนก Double Tops, Head-and-Shoulders, Double Bottoms

### 4.2 Reversal Signals (สัญญาณกลับตัว)
- ตรวจจับสัญญาณเตือนล่วงหน้าว่าราคาอาจกลับทิศทาง

### 4.3 Trend Analysis (การวิเคราะห์แนวโน้ม)
- ตรวจจับการดำเนินต่อเนื่องของแนวโน้ม (Trend Continuation) และจุดสิ้นสุดของแนวโน้ม (Trend Conclusion)

---

## 5. แนวคิดพื้นฐานที่สำคัญ (Key Concepts)

### 5.1 การ Breakout ของ Band
- **Tags of bands ไม่ใช่สัญญาณซื้อ/ขายโดยอัตโนมัติ** — ต้องมี indicator ยืนยัน
- **Closes นอก BB อาจเป็นสัญญาณต่อเนื่อง (Continuation)** ถ้าได้รับการ confirm จาก indicator อื่น

### 5.2 Squeeze → Expansion → Squeeze
- **The Squeeze** (Band หดแคบ) ตามด้วย **Expansion** (Band ขยาย) แล้ววนกลับมา Squeeze อีกครั้ง
- เป็นวัฏจักรธรรมชาติของ Volatility

### 5.3 Price Walking the Band
- ราคาสามารถ **"เดินขึ้น" ตาม Upper Band** หรือ **"เดินลง" ตาม Lower Band** ได้ — ไม่ใช่ชน band แล้วต้องกลับทิศเสมอ

### 5.4 Confirming Indicators
- **Confirming indicators ไม่ควรเกี่ยวข้องโดยตรงกับ BB** — เช่น OBV, RSI ซึ่งเป็น indicator คนละประเภท จะให้ confirm ที่น่าเชื่อถือกว่า

---

## 6. กลยุทธ์ที่นำเสนอ (Strategies)

### 6.1 Double Bottom — Bottom Fishing (กลยุทธ์ซื้อ)
1. หาหุ้นที่กำลังสร้าง Double Bottom (เลือก time frame ตามความถนัด)
2. **Bottom แรก** อาจทะลุ Lower Band
3. **Bottom ที่สอง** อาจต่ำกว่า bottom แรก (!) แต่ **ไม่ทะลุ band** → สัญญาณขาขึ้น
4. **Indicator อื่นต้องแสดง Bullish Divergence**
5. **Breakout ขึ้นต้องมี Volume สูง** ยืนยัน
6. **Protective Stop:** วางต่ำกว่าจุดต่ำสุดล่าสุดเล็กน้อย

### 6.2 Double Top / Head-and-Shoulders — Shorting Strategy (กลยุทธ์ขายชอร์ต)
1. หาหุ้นที่กำลังสร้าง Double/Triple Top หรือ Head-and-Shoulders
2. **Top แรก (หรือ Left Shoulder + Head)** จะทะลุ Upper Band
3. **Top ที่สอง (หรือ Right Shoulder)** อาจ **ไม่ทะลุ Upper Band** → สัญญาณขาลง
4. Indicator อื่นต้องแสดง Bearish Divergence

#### วิธีเข้า Short:
- **Double Top:** ขายเมื่อราคาหลุด Low ของวันที่ราคาสูงสุด (sell below low of high day) หรือหลุด Support area
- **Head-and-Shoulders:** ขายเมื่อราคาหลุด Neckline
- **Low Risk Entry:** รอ Throwback Rally แล้วล้มเหลว (fail) ค่อยเข้า

### 6.3 Play "The Squeeze" (กลยุทธ์ Squeeze)
1. หาหุ้นที่กำลังอยู่ใน "tight Squeeze" (Band หดแคบมาก)
2. ระบุแนวโน้มหลัก (Prevailing Trend)
3. ตรวจสอบ indicator อื่นเพื่อเตรียมเข้า
4. **เข้าแล้วตั้ง Trailing Stop** ทันที
5. **ระวัง Head Fake!** — ราคาอาจหลอกวิ่งผิดทางก่อนจะวิ่งจริง

---

## 7. บทสรุป (Conclusion)

- Bollinger Bands ช่วยให้เทรดเดอร์และนักลงทุน **ระบุรูปแบบราคา, ทิศทางแนวโน้ม, และจุดกลับตัว**
- ความรู้นี้ช่วยให้วางเทรดที่ **คิดมาอย่างดี, มีวินัย, และมีความเสี่ยงต่ำ**
- **กฎเหล็ก:** ลำดับแรกสุดคือ **รักษาเงินทุนของคุณเสมอ** (Conserve your capital!)

---

## สรุปสำหรับการนำไปใช้ในระบบเทรดอัตโนมัติ

| แนวคิด | วิธีนำไปใช้ |
|:---|:---|
| **Squeeze Detection** | ตรวจจับ ATR หรือ Band Width หดแคบ → เตรียมสัญญาณ Breakout |
| **Band Walk** | ไม่ต้อง counter-trend เมื่อราคาเดินตาม band — ใช้ Trend Following |
| **Divergence Confirm** | ใช้ RSI/OBV ยืนยัน Bottom/Top ก่อนเข้าเทรด |
| **Volume Confirmation** | Breakout ต้องมี Volume สูงกว่าค่าเฉลี่ย ยืนยัน |
| **ATR-based Stop** | วาง止损 ตาม Volatility ของ BB แทน pip คงที่ |
| **Time Frame Flexibility** | BB ใช้ได้ทุก TF — แต่ต้องปรับ MA Period ให้เหมาะสม |
