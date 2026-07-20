# สรุป: Forex Report — Predicting Price Action (พฤศจิกายน 2004)

> **ผู้เขียน:** Scott Owens & Omer Lizotte (FX Engines, Inc.)
> **วัตถุประสงค์:** วิเคราะห์ความน่าจะเป็นของ price action ว่าเมื่อคู่เงินขยับ X pips แล้ว จะไปต่ออีกกี่ pip โดยใช้ข้อมูล tic data จากมกราคม 2000 ถึงพฤษภาคม 2004

---

## 1. แนวคิดหลัก — Price Action คือรากฐานของ Technical Analysis

- Price action เป็น **รากฐานของ indicator ทุกตัว** แต่เทรดเดอร์ส่วนใหญ่ไม่ค่อยทำความเข้าใจมันจริงจัง
- ภายในแต่ละ trade, price action สร้าง **inflection point** ที่กำหนดจุดเข้า-ออก
- นักลงทุนที่เข้าใจ price action จะใช้มันเป็น **กรอบตัดสินใจทุก trades**
- รายงานนี้มุ่งตอบคำถามว่า: **เมื่อราคาขยับ 10/15/20/25/50 pips แล้ว จะไปต่ออีกกี่ pip?**

---

## 2. วิธีทำวิจัย — Methodology

### ข้อมูลที่ใช้
- **tic data** จากมกราคม 2000 ถึงพฤษภาคม 2004 (ข้อมูล Day ครอบคลุมถึงตุลาคม 2004)
- คู่เงิน 4 คู่หลัก: **EURUSD, USDJPY, GBPUSD, USDCHF**
- 6 ช่วงเวลา (interval): **5 นาที, 30 นาที, 60 นาที, 2 ชั่วโมง, 4 ชั่วโมง, 1 วัน**
- แต่ละ interval มีอย่างน้อย 5,000 observations (ยกเว้น Day ที่ใช้ daily close ทั้งหมด)

### วิธีจำลอง (Simulation)
- ใช้ **trigger value** เป็นเงื่อนไขเข้า: ดูทุก interval close แล้วสังเกต 24 intervals ถัดไป
- ถ้า trigger ถูก hit ในทิศทางใดก็ได้ → เข้า market
- จากจุดเข้า → วัดความน่าจะเป็นว่าจะ hit target price ภายใน 24 periods
- **Stop loss คงที่: 20 pips** ทุก simulated trade
- **ไม่คิด spread**

### Caveats สำคัญ
- ความน่าจะเป็น **ไม่ใช่กฎตายตัว** — เป็น snapshot ของข้อมูล ณ ช่วงเวลาหนึ่ง
- ได้รับผลกระทบจาก **seasonality** และเงื่อนไขทางสถิติอื่นๆ
- ช่วงเวลาสั้น (5 min) มีโอกาสเกิด anomaly มากกว่า
- ไม่ได้รับประกันความถูกต้อง — จุดประสงค์คือ **เพิ่ม sense of the market** ไม่ใช่ระบบเทรดตายตัว

---

## 3. ผลลัพธ์หลัก — ความน่าจะเป็นของ Price Continuation

### 3.1 ความสัมพันธ์ระหว่าง Timeframe กับ Probability

**กฎที่สำคัญที่สุด:** ยิ่ง timeframe ยาว → ความน่าจะเป็นที่ราคาจะไปต่อ **สูงขึ้นมาก**

**ตัวอย่าง EURUSD 10 pip trigger → ไปต่อ +100 pips:**
| Timeframe | Probability |
|-----------|:-----------:|
| 5 min     | 1.2%        |
| 30 min    | 9.2%        |
| 60 min    | 19.8%       |
| 2 hour    | 38.5%       |
| 4 hour    | 54.7%       |
| Day       | 61.4%       |

→ **Day chart มี probability สูงกว่า 5 min ถึง 51 เท่า** (61.4% vs 1.2%)

### 3.2 ความสัมพันธ์ระหว่าง Trigger Level กับ Probability

**Trigger ยิ่งใหญ่ (10 → 15 → 20 → 25 → 50 pips) → Probability ยิ่งสูงขึ้น**

**ตัวอย่าง EURUSD 60 min chart → ไปต่อ +80 pips:**
| Trigger | Probability |
|---------|:-----------:|
| 10 pip  | 32.4%       |
| 15 pip  | 34.2%       |
| 20 pip  | 35.8%       |
| 25 pip  | 37.5%       |
| 50 pip  | 54.1%       |

→ ยิ่งราคาขยับเยอะก่อนเข้า → ยิ่งมีโอกาสไปต่อ

### 3.3 ความแตกต่างระหว่างคู่เงิน

** GBPUSD มี probability สูงสุด** ในเกือบทุก timeframe/trigger combo

**EURUSD 10 pip trigger → +100 pips:**
| Pair    | 5 min | 60 min | Day  |
|---------|:-----:|:------:|:----:|
| EURUSD  | 1.2%  | 19.8%  | 61.4%|
| USDJPY  | 1.8%  | 14.1%  | 61.7%|
| GBPUSD  | 1.2%  | 38.7%  | 64.2%|
| USDCHF  | 1.8%  | 34.4%  | 70.3%|

→ **USDCHF มี momentum ต่อเนื่องสูงสุดใน Day chart** (70.3%)
→ **GBPUSD มี momentum สูงสุดใน 60 min** (38.7%)

---

## 4. ข้อมูล Detail — แต่ละ Trigger Level

### 4.1 10 Pip Trigger

| Pair    | Timeframe | +20    | +50    | +80    | +100+  |
|---------|-----------|:------:|:------:|:------:|:------:|
| EURUSD  | 60 min    | 87.5%  | 57.6%  | 32.4%  | 19.8%  |
| EURUSD  | 4 hour    | 91.4%  | 74.8%  | 63.3%  | 54.7%  |
| USDJPY  | 60 min    | 85.4%  | 44.7%  | 22.7%  | 14.1%  |
| USDJPY  | 4 hour    | 93.6%  | 77.7%  | 61.2%  | 51.4%  |
| GBPUSD  | 60 min    | 90.0%  | 67.6%  | 50.1%  | 38.7%  |
| GBPUSD  | 4 hour    | 92.7%  | 77.1%  | 66.2%  | 58.7%  |
| USDCHF  | 60 min    | 90.8%  | 68.3%  | 47.3%  | 34.4%  |
| USDCHF  | 4 hour    | 93.6%  | 79.6%  | 71.1%  | 65.7%  |

**Observation:** ที่ 60 min, GBPUSD มี chance ไปต่อ +80 สูงถึง 50.1% — เกือบครึ่ง!

### 4.2 25 Pip Trigger

| Pair    | Timeframe | +50    | +75    | +100+  |
|---------|-----------|:------:|:------:|:------:|
| EURUSD  | 60 min    | 68.5%  | 46.4%  | 23.0%  |
| EURUSD  | Day       | 86.0%  | 79.2%  | 67.6%  |
| USDJPY  | 60 min    | 56.5%  | 36.0%  | 17.4%  |
| USDJPY  | Day       | 86.7%  | 76.1%  | 66.3%  |
| GBPUSD  | 60 min    | 77.6%  | 61.6%  | 42.2%  |
| GBPUSD  | Day       | 88.3%  | 79.7%  | 68.9%  |
| USDCHF  | 60 min    | 78.1%  | 61.5%  | 38.4%  |
| USDCHF  | Day       | 89.4%  | 82.2%  | 74.8%  |

### 4.3 50 Pip Trigger (Trigger ใหญ่สุด)

| Pair    | Timeframe | +60    | +80    | +100+  |
|---------|-----------|:------:|:------:|:------:|
| EURUSD  | 60 min    | 82.3%  | 54.1%  | 33.1%  |
| EURUSD  | Day       | 95.0%  | 87.2%  | 78.1%  |
| USDJPY  | 60 min    | 77.8%  | 49.4%  | 30.6%  |
| USDJPY  | Day       | 93.6%  | 83.8%  | 76.5%  |
| GBPUSD  | 60 min    | 89.0%  | 69.8%  | 51.7%  |
| GBPUSD  | Day       | 94.8%  | 84.8%  | 77.4%  |
| USDCHF  | 60 min    | 89.3%  | 66.4%  | 47.3%  |
| USDCHF  | Day       | 95.5%  | 89.5%  | 83.1%  |

**Key insight:** ที่ Day chart + 50 pip trigger, ทุกคู่มี chance ไปต่อ +100 pips **สูงกว่า 76%** — นั่นคือ momentum บน daily มีความแข็งแกร่งมาก

---

## 5. ผลลัพธ์เชิงกลยุทธ์ — Action Items

### 5.1 กลยุทธ์ที่แนะนำ

1. **Fixed Exit + Limit Pips**
   - ใช้ fixed exit และ limit ออกเพื่อจับ pips ในช่วงคงที่
   - ทดสอบระดับต่างๆ ของ limit exit เพื่อหาระดับที่ optimal ที่สุด

2. **Breakout Signals สำหรับ Market Entry**
   - ใช้ breakout signal เป็น trigger เข้า market ใน interval ต่างๆ
   - แต่ละ interval ใช้ exit strategy ต่างกันเพื่อใช้ประโยชน์จาก probability ของ price move ยาว/สั้น

3. **Tapering Trailing Exits**
   - ใช้ trailing stop ที่แคบลงเมื่อกำไรมากขึ้น
   - สะท้อน expectation ว่า momentum จะอ่อนลงเมื่อไปไกล

### 5.2 วิธีใช้ข้อมูลนี้ในทางปฏิบัติ

- **สำหรับ日内交易 (intraday):** ที่ 60 min chart, เมื่อ EURUSD ขยับ 10 pip → มี 87.5% chance ไปต่ออีก 10 pip (net +10) — แต่ chance ไปต่อ +70 pip มีแค่ 39.9% → **ใช้ target สั้น**
- **สำหรับ Swing Trading:** ที่ Day chart, momentum แข็งมาก → ใช้ trailing stop ยาวได้
- **Risk Management:** Stop loss 20 pip เป็น baseline → probability ที่จะถูก stop ก่อนไปถึง target ก็มีอยู่

---

## 6. ข้อจำกัดของงานวิจัย

1. **ข้อมูลปี 2000-2004** — สภาพตลาดอาจเปลี่ยนไปมาก ( algo trading ยังไม่แพร่หลายเท่าตอนนี้)
2. **ไม่คิด spread** — ในความเป็นจริง spread ลด profit margin ลงมาก
3. **Trigger strategy จำลองยากใน live trading** — ไม่สามารถ replicate entry conditions ได้เหมือนใน simulation
4. **Seasonality** — ผลลัพธ์อาจผันผวนตามฤดูกาล (ปีใหม่, summer, etc.)
5. **ไม่รวม slippage, commission** — ต้นทุนจริงสูงกว่าใน simulation

---

## 7. บทเรียนสำคัญสำหรับระบบเทรดของเรา

### Momentum Continuation ที่ควรพิจารณา

1. **Timeframe效应 ชัดเจน**: ยิ่ง TF ยาว → momentum ต่อเนื่องสูง → **system ของเราควรมี multi-timeframe confirmation** เช่น H1 entry + H4/D1 momentum
2. **Trigger level matters**: ไม่ใช่ทุก breakout จะเท่ากัน — ยิ่ง breakout แรง (25-50 pip) → ยิ่งมีโอกาสไปต่อ → **พิจารณา minimum breakout threshold ก่อนเข้า**
3. **GBPUSD & USDCHF มี momentum ต่อเนื่องสูง** — อาจเหมาะกับ trend-following strategy มากกว่า pair อื่น
4. **5-min chart มี probability ต่ำมาก** — momentum continuation แทบไม่มีความหมายที่ TF นี้ → **หลีกเลี่ยง scalping บน momentum**
5. **Day chart momentum แข็งแกร่งมาก (>76% สำหรับ 50 pip trigger)** — ยืนยันว่า **daily trend สำคัญที่สุด** สำหรับ position trading

### เปรียบเทียบกับระบบปัจจุบัน

| แนวคิด | Report แนะนำ | ระบบเราทำได้ |
|---------|:-------------|:-------------|
| Multi-TF confirmation | H1 + Day momentum | ✅ ใช้ H1 entry + regime detection |
| Minimum breakout threshold | 25-50 pip trigger | ⚠️ ยังไม่มี — พิจารณาเพิ่ม |
| Trailing stop (tapering) | แคบลงเมื่อกำไรมาก | ✅ ใช้ ATR-based trailing |
| GBPUSD/USDCHF focus | Momentum สูง | ❌ ยังไม่ได้ test |
| 5-min avoidance | Probability ต่ำ | ✅ ใช้ M15 เป็น minimum |

---

## 8. สรุป

รายงานนี้เป็น **baseline data ที่มีค่า** สำหรับการเข้าใจว่า momentum continuation มี probability เท่าใดใน market จริง (ข้อมูล 2000-2004) แม้ว่าข้อมูลจะเก่า แต่ **หลักการพื้นฐานยังใช้ได้:**

1. **Momentum มีจริง** — ไม่ใช่ random walk ที่ 100%
2. **Timeframe สำคัญมาก** — Day chart momentum แข็งกว่า 5-min หลายสิบเท่า
3. **Trigger level สำคัญ** — ยิ่ง breakout แรง ยิ่งมีโอกาสไปต่อ
4. **คู่เงินต่างกัน momentum ต่างกัน** — GBPUSD/USDCHF มี continuation สูง, EURUSD/USDJPY กลางๆ

> ข้อความปิดท้ายจาก Hoffer (อ้าง Montaigne): *"All I say is by way of discourse, and nothing by way of advice. I should not speak so boldly if it were my due to be believed."*
