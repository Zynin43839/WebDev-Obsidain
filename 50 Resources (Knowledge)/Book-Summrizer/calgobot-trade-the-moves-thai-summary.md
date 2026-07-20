# สรุปไทย — Trade the Moves!
> ที่มา: Trade the Moves! | หน้า 1-57

---

## 1. แก่นของหนังสือ — ทำกำไรได้ทุกสภาวะตลาด

- ** Markets Changed**: แม้ index จะลง 29% ใน 1 ปี (NASDAQ 2001) แต่ถ้าซื้อขาขึ้น 4 รอบ + ขายชอร์ตขาลง 4 รอบ → **ผลรวม +205%**
- **หลักการ**: ไม่ต้องเดาว่าตลาดจะขึ้นหรือลง แค่จับ "ทิศทาง" แล้วเทรดให้ถูกฝั่ง — Long เมื่อขาขึ้น, Short เมื่อขาลง

---

## 2. การประเมินทิศทาง Index — MACD Scoring System

### วิธีทำ
1. Plot **30-period Moving Average** บน index
2. Plot **MACD** ใต้ index เดียวกัน
3. หา **Chart Patterns** บน MACD โดยตรง

### 7 ปัจจัยให้คะแนน (Score)

| ปัจจัย | คะแนน |
|--------|:------:|
| Bullish Divergence | **+3** |
| Bearish Divergence | **-3** |
| Trend Line Break ขึ้น | **+2** |
| Trend Line Break ลง | **-2** |
| Saucer หันขึ้น | **+2** |
| Saucer หันลง | **-2** |
| Resistance (Consolidation) Break Up | **+2** |
| Support (Consolidation) Break Down | **-2** |
| ราคาอยู่เหนือ 30 MA | **+1** |
| ราคาอยู่ต่ำกว่า 30 MA | **-1** |
| MACD สูงกว่า trigger line | **+1** |
| MACD ต่ำกว่า trigger line | **-1** |

### การตีความ
- **Score (+)** → Long / Cover Short
- **Score (0)** → ไม่เปิด position
- **Score (-)** → Short / Close Long
- **Weighting**: ถ้าเทรดปกติ 10% ต่อไม้ → score +2 ใส่ 20%, score +3 ใส่ 30%

### ตัวอย่างจริง — NASDAQ 2001-01-02
- 30 MA: -1, Bullish Divergence: +3, MACD Below Trigger: -1
- **Total = +1** → Long 60%, Short 40%

---

## 3. Long/Short Mix — ถ่วงน้ำหนักพอร์ต

ตาราง Long/Short ตาม Score:

| Score | Long% | Short% |
|:-----:|:-----:|:------:|
| -4 | 10 | 90 |
| -3 | 20 | 80 |
| -2 | 30 | 70 |
| -1 | 40 | 60 |
| 0 | 50 | 50 |
| +1 | 60 | 40 |
| +2 | 70 | 30 |
| +3 | 80 | 20 |
| +4 | 90 | 10 |

- **หลักการ**: ถ่วงน้ำหนัก Long/Short ตาม score → ไม่เสียสมดุลแม้ตลาดเปลี่ยนทิศ
- เปรียบเหมือน "Running your own Mutual Fund" แต่ควบคุมเอง

---

## 4. ทำไมเทรดหุ้น Individual ดีกว่าเทรด Index

1. **หุ้น individual move แรงกว่า index** — MERQ ทำได้ +370%, PDLI ทำได้ +460% (ในขณะที่ index ลง 29%)
2. **มี Chart Patterns ชัดเจน** — ยืนยัน entry ได้ดีกว่า
3. **ความเสี่ยงกระจาย** — ไม่ต้อง "put all eggs in one basket"
4. PeopleSoft: ซื้อขาย range-bound → ได้ +360%

> ⚠️ Trading index โดยตรง (QQQ) มีข้อมูลน้อยกว่า ทำนายทิศทางยากกว่า

---

## 5. Entry Patterns — 7 Chart Patterns ยืนยันการเข้า

ใช้ 7 pattern นี้เป็น confirmation:

| # | Pattern | ตัวอย่างหุ้น |
|---|---------|------------|
| 1 | **Trend Line Break** | Lexmark International |
| 2 | **Support and Resistance** | America OnLine |
| 3 | **Saucer** | Perot Systems Corp. |
| 4 | **Fibonacci Retracement** | Thomson Multimedia (50%, 62%) |
| 5 | **Gaps** (Breakaway, Measured, Exhaustion) | Kemet Corporation |
| 6 | **Consolidation** | Ameren Corporation |
| 7 | **Volume Climax and Trend** | Worthington Industries |

- ยืนยันด้วย software ที่มี (MetaStock Explorer, OmniTrader)
- **Confirmed Signal** = ราคาตัด 30 MA + มี pattern หนึ่งใน 7 ตัวนี้

---

## 6. Exit Strategy — Eighths/ATR Scale

### วิธีใช้
1. กำหนด **ATR** ณ จุด Entry
2. ลาก scale จาก **Relative Low** ขึ้นไป โดยแต่ละขีด = **1/8 × ATR**
3. ทุกครั้งที่ราคาปิดเกินขีด → **เลื่อน Stop Loss ขึ้น**
4. **Exit เมื่อ CLOSE ของ bar เกินเส้นก่อนหน้า** (range crossing ไม่นับ)

### หลักการ
- ใช้ ATR เป็นหน่วย → ปรับตามความผันผวนของหุ้นแต่ละตัว
- ให้กำไร "วิ่ง" ไปเรื่อยๆ จนราคาย้อนกลับมาแตะ trailing stop

---

## 7. Hedging — ลด Market Risk

### ทฤษฎี
- หุ้นขึ้น + หุ้นลง **พร้อมกัน** → long ตัวหนึ่ง + short อีกตัว
- ถ้าตัวหนึ่งขาดทุน อีกตัวอาจกำไร → **พอร์ตยังลอย**

### ตัวอย่างจริง — BMET vs BRCD
- BMET ขึ้น, BRCD ลง **在同一ช่วงเวลา**
- Long BMET + Short BRCD = กำไรทั้งสองขา

### เป้าหมาย
- **BUY หุ้นที่มีสัญญาณขาขึ้น** + **SHORT หุ้นที่มีสัญญาณขาลง** → พร้อมกัน
- สร้าง "convoy" ของเทรด → ถ้าเรือลำหนึ่งจม พอร์ตยังรอด

---

## 8. ค่า Transaction Cost — ต้นทุนต่ำเป็นกุญแจ

| Broker | Short? | Rate |
|--------|:------:|------|
| InteractiveBrokers.com | Yes | $0.01/share, $0.005 >500 shares |
| FreeTrade.com | Yes | From $0-$3/trade |
| America.com | Yes | FREE (1,000+ shares, $5 น้อยกว่า) |
| FOLIOfn.com | No | 500 trades/mo, 3,000 stocks, $14.95/mo |

- **ค่าธรรมเนียมต่ำ** สำคัญมากสำหรับกลยุทธ์ที่เทรดหลาย position พร้อมกัน

---

## 9. ขั้นตอน Daily Routine (สรุป)

1. **วิเคราะห์ Index** → หา Score → กำหนด Long/Short Mix
2. **Scan หุ้น candidates** → ใช้ MetaStock / OmniTrader
3. **ยืนยัน Entry** → 7 Chart Patterns
4. **ตั้ง Stop Loss** → Eighths/ATR Scale
5. **ปิด position เก่าสุดก่อน** → maintain mix ตาม score ใหม่
6. ทำซ้ำทุกวัน → **ใช้เวลา ~30 นาที**

---

## 10. ความเชื่อมโยงกับ Algo Trading

| หลักการ Trade the Moves | วิธีนำไปใช้ใน Algo System |
|-------------------------|--------------------------|
| MACD Scoring หาทิศทาง Index | ใช้ BOCPD / Regime Detector แทน — วัดว่าตลาด trending หรือ ranging |
| Long/Short Mix ตาม Score | Strategy Selector: เลือก long หรือ short ratio ตาม regime confidence |
| 7 Chart Patterns ยืนยัน Entry | OB + FVG + Trend Line Break → ใช้ใน multi_strategy.py |
| Eighths/ATR Exit Scale | ATR-based trailing stop (พิสูจน์แล้วว่าได้ผลใน backtest — Session 4.19-4.21) |
| Hedging (Long + Short พร้อมกัน) | Multi-Strategy: SMC Range (long) + Breakout Retest (short) พร้อมกัน |
| Scan → Confirm → Enter | Pipeline: detect OB/FVG → sweep + MSS confirm → entry |
| Daily 30 นาที | Daemon run_cycle ทุก 5 นาที → auto-detect + auto-execute |

> **ข้อสังเกต**: หนังสือเน้น "mechanical system" — ไม่ใช้อารมณ์ ตัดสินใจตาม score + pattern เท่านั้น → สอดคล้องกับ algo trading ที่ต้อง rule-based 100%
