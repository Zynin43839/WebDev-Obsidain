# สรุปภาษาไทย: A Practical Guide to Swing Trading
**ผู้เขียน**: Larry Swing  
**แหล่งที่มา**: calgobot-to-all-those-who-did-not-dedicate (PDF)

---

## 1. ภาพรวมหนังสือ

หนังสือเล่มนี้เป็นคู่มือ Practical สำหรับ Swing Trading ที่เน้นระบบRULE-BASED ที่เรียกว่า **"The Master Plan"** — เป็นชุดกฎที่กำหนดว่าเมื่อไหร่เข้า-ออกเทรดอย่างชัดเจน ไม่ต้องใช้ดุลยพินิจ Larry Swing ใช้เวลาหลายปีพัฒนาระบบนี้จากประสบการณ์ตรง และเปิดให้ใช้ฟรีบนเว็บ mrswing.com

**หลักการหลัก**: Swing Trading ทำกำไรจาก "คลื่น" ของราคา — ซื้อเมื่อย่อตัว (pullback) ในขาขึ้น, ขาย short เมื่อเด้งขึ้น (pull-up) ในขาลง

---

## 2. Swing Trading คืออะไร?

- **นิยาม**: การเทรดที่ถือ position ตั้งแต่ไม่กี่วันถึงไม่กี่สัปดาห์ ไม่ใช่ day trading แต่ก็ไม่ใช่ buy-and-hold
- **ทำไมทำได้**: เพราะราคามีลักษณะเป็นคลื่น (wave) — ขึ้นแล้วย่อ, ลงแล้วดีด ซ้ำไปมาเป็น pattern
- **ทำกำไรได้ทุกสภาวะตลาด**: bullish, bearish, หรือ sideways
- **ต้องมีวินัย**: กฎ Master Plan เป็น mechanical rules — ตัดอารมณ์ fear/greed ออก

---

## 3. ขั้นตอน Swing Trading (5 Steps)

1. **ระบุหุ้นที่อยู่ในขาขึ้นหรือขาลง** — ใช้ technical analysis
2. **รอ pullback (ขาขึ้น) หรือ pull-up (ขาลง)** — 3 วันติดต่อกัน
3. **สั่ง order** ตาม Master Plan (buy stop หรือ sell stop)
4. **วาง stop-loss และ take-profit** ทันทีหลังเทรดถูก Execute
5. **ปรับ trailing stop** ทุกวันตามกฎ

### เกณฑ์คัดกรองหุ้น
- ราคาอย่างน้อย **$7-12**
- Volume เฉลี่ย 20 วันอย่างน้อย **500,000 หุ้น**
- หลีกเลี่ยงหุ้นราคาถูกและ volume ต่ำ (market maker จัดการง่าย)

---

## 4. The Master Plan — ระบบเข้า-ออกเทรด

### หลักการสำคัญ
- **Take Profit**: ขาย half ที่ราคา **สูงกว่า entry 7%** (ใช้ sell limit order)
- **Stop Loss**: ขายทั้งหมดที่ราคา **ต่ำกว่า entry 4%** หรือ **6 เซนต์ต่ำกว่า low ของวัน** (แล้วแต่ราคาไหนสูงกว่า)
- **Half ที่เหลือ**: ถือต่อโดยใช้ trailing stop — "ride the wave"

### จังหวะเข้าเทรด (Long)
- **ปกติ (ไม่มี gap)**: ซื้อเมื่อราคา **สูงกว่า high ของวันก่อน 6 เซนต์** (ใช้ buy stop)
- **Gap Up  ≥ $0.50**: รอ **30 นาที** หลัง market open แล้วเข้า
- **Gap Down ≥ $0.50**: รอ **5 นาที** หลัง market open แล้วเข้า
- **เหตุผล**: ซื้อหลังเปิดตลาดดีกว่า — ให้ market "หายใจ" ก่อน

### Trailing Stop วันถัดไป
- ถ้า gap น้อยกว่า $0.50: ปรับ stop เป็น **6 เซนต์ต่ำกว่า low ของวันก่อน**
- ถ้า gap ≥ $0.50: ใช้ low ของวันนี้แทน
- **ทำซ้ำ** จนกว่าจะโดน stop หรือ hit target

### ถ้าเทรดไม่ถูก Execute
- ทำซ้ำได้สูงสุด **5 trading days**
- ปรับ entry/exit ตามราคาตลาดปัจจุบัน

### Short Swing (ขาย short)
- เป็น mirror image ของ long
- เข้าเมื่อราคา **ต่ำกว่า low ของวันก่อน 6 เซนต์** (sell stop)
- Stop loss = buy stop ที่ **สูงกว่า entry 4%** หรือ **6 เซนต์สูงกว่า high ของวัน**
- Take profit = buy limit ที่ **ต่ำกว่า entry 7%** สำหรับ half ของ position
- ต้องมี **margin account**

---

## 5. Technical Analysis ที่ใช้

### Japanese Candlestick
- แสดง Open, High, Low, Close ในรูปแบบที่เน้นความสัมพันธ์ระหว่าง open กับ close
- ไม่คำนวณอะไรเพิ่ม — เป็น visual representation ล้วน

### Volume
- **Volume สูง + ราคาขยับน้อย** = จุดกลับตัว (reversal)
- **Volume สูง + ราคาขยับมาก** = แนวโน้มเดินหน้า (trend continuation)
- **Volume ต่ำ + ไม่มีการขยับ** = trend ยังดำเนินต่อ

### Equivolume (Richard Arms)
- กล่องที่ width = volume, height = price range
- **กล่องสั้นกว้าง** (vol สูง, ราคาขยับน้อย) = จุดกลับตัว
- **กล่องยาวแคบ** (vol ต่ำ, ราคาขยับมาก) = trend ชัดเจน
- **Power Box**: ทั้งสูงและกว้าง = breakout ที่ยืนยันด้วย volume

### Moving Average (SMA)
- ใช้ **SMA 10, 20, 50 วัน** บน closing price
- Uptrend: ราคาอยู่เหนือ MA20/MA50, MA10 > MA20
- Downtrend: ราคาอยู่ใต้ MA20/MA50, MA10 < MA20
- MA ทำหน้าที่เป็น support/resistance

### Force Index (Dr. Alexander Elder)
- **Force Index = Volume × (Close วันนี้ - Close เมื่อวาน)**
- ใช้ MA 3 วัน (สั้น) และ MA 13 วัน (ยาว)
- **BUY**: FI-13MA ≥ 0 AND FI-3MA ≤ 0 (หมีชนะระยะสั้น แต่ วัวยังคุมระยะยาว)
- **SELL**: FI-13MA ≤ 0 AND FI-3MA ≥ 0

### Directional Moving Index (DMI) — J. Welles Wilder
- **+DI**: ทิศทางบวก, **-DI**: ทิศทางลบ, **ADX**: ความแข็งแกร่งของ trend
- **Uptrend**: ADX > 30, +DI > -DI
- **Downtrend**: ADX > 30, -DI > +DI
- ตลาดอยู่ใน trend จริงๆ แค่ ~30% ของเวลา — 70% เป็น sideways

### Up/Down/In/Out
- **Up (เขียว)**: High สูงกว่าเมื่อวาน + Low สูงกว่าเมื่อวาน
- **Down (แดง)**: High ต่ำกว่า + Low ต่ำกว่า
- **In (เหลือง)**: High ต่ำกว่า + Low สูงกว่า = inside bar
- **Out (น้ำเงิน)**: High สูงกว่า + Low ต่ำกว่า = outside bar

---

## 6. Pattern Recognition Criteria (SwingLab Scans)

### Forceswing LONG
```
MAV20 >= 500,000 AND    // volume เพียงพอ
CLOSE > 12 AND          // ราคาไม่ถูกเกินไป
CLOSE > SMA10 AND CLOSE > SMA20 AND  // ยังอยู่ใน uptrend
HIGH < HIGH1 AND HIGH1 < HIGH2 AND   // pullback 3 วัน
FORCE3 <= 0 AND FORCE13 >= 0 AND     // short-term pullback, long-term bullish
ADX20 > 30                           // trend แข็ง
```

### Forceswing SHORT
- เหมือน LONG แต่กลับทิศ: **CLOSE < SMA10/SMA20**, **LOW > LOW1 > LOW2** (pull-up 3 วัน)

---

## 7. Money Management

- **แบ่งทุนเป็น 15 ส่วน** — แต่ละเทรดใช้ 1 ส่วน (6.66%)
- ถ้าจัดการได้มากขึ้น อาจเพิ่มเป็น 20 ส่วน
- เริ่มต้น 2-3 trades พร้อมกันก็ได้
- **ไม่ต้องกังวลเรื่อง cash หมด** — ถ้าไม่มีเงินเหลือ order จะไม่ถูก fill เอง

---

## 8. เครื่องมือที่แนะนำ

### SwingTracker (Software)
- Real-time charts, quotes, portfolio tracker
- Stock Scan: ค้นหาหุ้นจาก 100+ criteria (price, volume, candlestick patterns, indicators)
- Equivolume, Force Index built-in
- ใช้ได้บน Windows และ Mac

### SwingLab (Free)
- Scan formulas ที่ Larry ใช้จริง — คัดลอกไปใส่ SwingTracker ได้เลย
- อัพเดทสม่ำเสมอที่ mrswing.com

### Broker แนะนำ
- **Interactive Brokers**: รองรับ OCO orders, margin, short selling
- **optionsXpress**: มี OCO feature, autotrading (XecuteSM), streaming quotes, charting

---

## 9. กรณีศึกษา (Case Studies)

| เทรด | วันที่ | Entry | Exit | กำไร |
|------|--------|-------|------|------|
| DFXI | 04/26/02 | $42.48 | $45.45 + $43.44 | **+4.63%** |
| PFG | 04/12/02 | $27.26 | $27.94 (trailing stop) | **+2.49%** |
| PTEN | 03/26/02 | $26.84 | $28.72 + $29.99 | **+9.37%** |

**รูปแบบที่ใช้**: Forceswing Long — CLOSE > SMA50, SMA20, pullback 3 วัน, Force Index ยืนยัน

---

## 10. Short Selling (Appendix)

- ขายหุ้นที่ไม่ได้เป็นเจ้าของ — ยืมจาก broker แล้วขาย หวังซื้อคืนราคาถูกลง
- **ต้องมี margin account** — portfolio ต้องมีมูลค่า ≥ 50% ของ short transaction
- **Up-tick Rule**: short sell ได้เฉพาะตอนที่ราคากำลังขึ้น (up-tick) — ห้าม short ตอนราคาลง
- **ความเสี่ยง**: ถ้าราคาขึ้น ต้องเติม margin หรือซื้อคืน
- **ทำไมขาย short**: (1) เก็งกำไรหุ้น overvalued, (2) protect portfolio จาก market downturn

---

## 11. ความเกี่ยวข้องกับ Algo Trading

### สำหรับระบบอัตโนมัติ

1. **Master Plan เป็น rule-based 100%** — เขียนเป็น algorithm ได้ทันที (mechanical entry/exit/trailing stop)
2. **Scan formulas มีอยู่แล้ว** — Forceswing LONG/SHORT เป็น boolean logic ที่คอมพิวเตอร์ execute ได้
3. **Money management ตายตัว** — position sizing = ทุน/15, stop 4%, target 7%
4. **Trailing stop ปรับรายวัน** — สามารถ automate ได้ด้วย logic ง่ายๆ

### ข้อจำกัดสำหรับ algo
- ระบบนี้ออกแบบมาสำหรับ **หุ้นสหรัฐ (NYSE/AMEX/NASDAQ)** ไม่ใช่ forex หรือ crypto
- ต้องการ **real-time data feed** และ **broker ที่รองรับ OCO orders**
- Short selling มี up-tick rule ที่ต้อง enforce

### ประยุกต์ใช้กับ forex/crypto
- แนวคิด pullback trading + trailing stop + volume confirmation ใช้ได้กับทุก market
- แต่ต้องปรับ parameters (target %, stop %, volume threshold) ตาม market characteristics
- Force Index และ ADX ใช้ได้ดีกับทุก instrument
