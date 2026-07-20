---
created: 2026-06-30
type: note
topic: technical-analysis
status: seedling
---

# 03 Price Action & Timeframe Analysis

---

## 🔹 Part 1: Price Action — อ่านการเคลื่อนที่ของราคา

**Price Action = การเคลื่อนไหวของราคาที่เกิดขึ้นจริงบนกราฟ — ไม่มี indicator, ไม่มี lag**

ข้อดี: ข้อมูล real-time ไม่มีการคำนวณล่าช้า  
ข้อเสีย: subjective — คนตีความได้ต่างกัน

แต่ในตลาดมีรูปแบบการเคลื่อนที่เพียง **4 แบบ** ที่เกิดซ้ำๆ ไม่รู้จบ

---

### ราคาเคลื่อนที่ 4 แบบ — ต้องแยกให้ออก

```
1. Impulse — วิ่งตรงไปในทิศทาง trend (มีแรง)
2. Pullback — ย่อ/ดีดชั่วคราวในทางตรงข้าม (อ่อนแรง)
3. Consolidation — สะสมกำลังในกรอบแคบ (ลังเล)
4. Reversal — เปลี่ยนทิศ (หมดแรง)
```

**การแยก 4 แบบนี้คือ ทักษะที่สำคัญที่สุดอย่างหนึ่งของเทรดเดอร์**  
เพราะถ้าคุณแยกเป็น:
- ขา Impulse → รู้ว่า "กำลังไป" → อย่าเทรดสวน
- ขา Pullback → รู้ว่า "กำลังย่อ" → รอสัญญาณกลับเข้า trend
- Consolidation → รู้ว่า "กำลังสะสม" → รอ breakout
- Reversal → รู้ว่า "กำลังกลับ" → เปลี่ยนกลยุทธ์

---

### 1. Impulse Move

**Impulse = ช่วงที่ราคาวิ่งแรงในทิศทางเดียว**

```
ลักษณะของ Impulse:
  ✅ Body candle ใหญ่กว่าปกติ (อย่างน้อย 2× body เฉลี่ย)
  ✅ Volume สูงกว่าปกติ
  ✅ Wick เล็ก (ไม่มี rejection)
  ✅ ต่อเนื่อง 3-5 candle หรือมากกว่า
  ✅ ไปทางเดียว ไม่กลับ
```

**ทำไม Impulse ถึงเกิด?**

Impulse = Smart Money กำลังผลักราคาอย่างจริงจัง  

นึกภาพ: Smart Money ต้องการ Buy 100 สัญญาทอง → ถ้าซื้อทีเดียว → ราคาพุ่งทันที → เสียของ  
ดังนั้นมันจะ:
1. Accumulate (สะสม) ในช่วง consolidation — ซื้อทีละน้อย
2. พอสะสมพอ → Impulse (ผลักราคา) → ขึ้นไปหา Take Profit
3. Impulse = ช่วงที่ Smart Money "เปิดเผยตัว"

**วิธีใช้ Impulse:**

```
1. หา Impulse ล่าสุด
2. ใช้จุดเริ่มต้น impulse หา Order Block (OB)
3. รอราคา retest OB → เข้า trade ตามทิศ impulse
4. Impulse ที่แรง = OB ที่น่าเชื่อถือ
```

| ลักษณะ Impulse | กำลัง | Reliability |
|---------------|-------|-------------|
| Body ใหญ่ + Volume พุ่ง | แรงมาก | สูง — OB ที่เกิดจาก impulse นี้เชื่อถือได้ |
| Body กลาง + Volume ปานกลาง | ปานกลาง | กลาง — ต้องมี OB ซ้อนกัน 2 TF |
| Body เล็ก + Volume ต่ำ | อ่อน | ต่ำ — อาจเป็นแค่ noise หรือ retail-driven |

---

### 2. Pullback (การย่อตัว)

**Pullback = การเคลื่อนที่ชั่วคราวในทิศทางตรงข้าม trend**

```
Uptrend: ขึ้น → ▌ย่อ▌ → ขึ้น → ▌ย่อ▌ → ขึ้น
Downtrend: ลง → ▌ดีด▌ → ลง → ▌ดีด▌ → ลง
```

**ทำไมราคาถึง Pullback?**

1. **Take Profit:** คนที่ซื้อที่ OB กำไรมากแล้ว → ขายทำกำไร → ราคาย่อลงนิดหน่อย
2. **Stop Hunting:** Smart Money ดึงราคากลับไปเก็บ Stop Loss ของคนที่เข้าเทรดสาย
3. **New Buyers:** Smart Money ต้องการราคาถูกเพิ่ม → ดึงกลับมาเติมของ

**Pullback vs Reversal — ข้อแตกต่างที่สำคัญที่สุด:**

| ดูอะไร | Pullback | Reversal (MSS) |
|--------|----------|----------------|
| **โครงสร้าง** | HL/LH ยัง intact | HL/LH ถูกทำลาย |
| **Volume ตอนย่อ** | Volume ลด — แรงขายน้อย | Volume เพิ่ม — มีคนสวน trend เยอะ |
| **หลังจากย่อ** | กลับไปทาง trend ต่อ | ไปทางตรงข้ามต่อ |
| **Candle ปฏิเสธ** | ปกติ | Engulfing / Pin Bar กลับทิศ |

**ตัวอย่างแยก Pullback vs Reversal:**

```
Uptrend: 4500 → 4550 → 4520 (ย่อ) → 4540 (ขึ้นต่อ)
→ 4520 = Pullback — HL ยังสูงขึ้น → กลับขึ้นต่อ ✅

Uptrend: 4500 → 4550 → 4480 (ย่อ) → 4490 (ขึ้นนิดเดียว) → 4460 (ลงต่อ)
→ 4480 < HL ก่อนหน้า = MSS → Reversal ❌
```

---

### 3. Consolidation (Sideway / การสะสม)

**Consolidation = ราคาแกว่งในกรอบแคบๆ — ไม่มีทิศทางชัดเจน**

```
ลักษณะ:
  - ราวขึ้นแล้วลงในกรอบเดิม
  - Body เล็ก
  - Volume ค่อยๆ ลด หรือเพิ่ม (ขึ้นกับว่า accumulation หรือ distribution)
```

**ทำไม Consolidation ถึงเกิด?**

มี 2 สาเหตุ:

1. **Accumulation (สะสมของ):** Smart Money ซื้อทีละน้อยในราคาเดิม  
   → Volume ค่อยๆ เพิ่มขึ้น  
   → ราคาไม่ขึ้น (Smart Money กดราคาไว้)  
   → พร้อมแล้ว → Breakout ขึ้น

2. **Distribution (กระจายของ):** Smart Money ขายทีละน้อย  
   → Volume ค่อยๆ เพิ่มขึ้น  
   → ราคาไม่ลง (Smart Money พยุงราคาไว้)  
   → ของหมดแล้ว → Breakout ลง

**วิธีแยก:**

| | Accumulation | Distribution |
|--|-------------|--------------|
| Volume | เพิ่มขึ้นเรื่อยๆ | เพิ่มขึ้นเรื่อยๆ |
| ราคา | Sideway ไม่ขึ้น | Sideway ไม่ลง |
| หลัง breakout | ขึ้น | ลง |
| Bottom ของกรอบ | มักถูกลากลงเก็บ liquidity ก่อนขึ้น | มักถูก push ขึ้นก่อนลง |

---

### 4. Reversal

**Reversal = Trend เปลี่ยนทิศ — ไม่ใช่แค่ pullback**

Reversal จริงต้องมี:
1. **Structure break** (MSS) — HL/LH หลุด
2. **Volume ยืนยัน** — Volume เพิ่มตอนสวน trend
3. **Reversal candle** — Engulfing / Pin Bar / Doji หลัง trend

Reversal ปลอม (Fake Reversal) = แค่ pullback ลึก

---

## 🔹 Part 2: Breakout, Retest, Fakeout

### Breakout (การทะลุ)

**Breakout = ราคาทะลุแนวรับ/แนวต้านที่สำคัญ**

```
Breakout จริง:
  1. ราคาทะลุ S/R
  2. ปิดนอก S/R — candle นี้ปิดเหนือ Resistance หรือต่ำกว่า Support
  3. 2-3 candle ติดกันยังอยู่นอก S/R
  4. Volume พุ่งและต่อเนื่อง
  5. ราคาไม่กลับเข้าไปใน S/R อีก
```

**ทำไม Breakout ถึงเกิด?**  
เมื่อ Supply (Sell) / Demand (Buy) ที่แนว S/R หมดลง  
→ ราคาวิ่งไปหา S/R ถัดไป

เหมือนมัดเชือกที่ยึดเรือไว้ — เชือกขาด → เรือลอยออกไป

### Retest (การกลับมาเทส)

**Retest = หลังจาก breakout แล้วราคากลับมาเทส S/R ที่เพิ่งทะลุ**

```
ตัวอย่าง:
  ทองมี Support ที่ 4500 → ราคาทะลุลงไป 4470
  → กลับมาที่ 4500 อีกครั้ง
  → 4500 ตอนนี้ = Resistance (Role Reversal)
  → ถ้า 4500 ไม่กลับไปเหนือ → Sell signal
```

**ทำไมต้องรอ Retest? — 3 เหตุผล:**

1. **ลดความเสี่ยง:** ถ้าเข้า breakout ทันที → ถ้า fakeout → ติดดอย
2. **Role Reversal ยืนยัน:** การกลับมาเทส = แสดงว่า S/R เปลี่ยนบทบาทจริง
3. **Stop Loss ชัดเจน:** SL ไว้เหนือ Resistance ที่เพิ่งกลับมาเทส (ไม่ไกล)

> **ข้อควรจำ:** ไม่ต้องเข้า trade ทุก retest  
> ถ้าคุณภาพ retest ไม่ดี (volume ไม่ขึ้น / candle ไม่ confirm) → ข้าม

### Fakeout / Liquidity Grab

**Fakeout = Breakout ปลอม — ทะลุแล้วกลับเข้ามาทันที**

```
ลักษณะ Fakeout:
  1. Wick ทะลุ S/R แต่ปิดในกรอบ
  2. Volume พุ่งตอนทะลุแล้วลดทันที
  3. 1-2 candle ถัดมากลับเข้ากรอบ
  4. มักเกิดที่ EQH/EQL — จุดที่มี Stop Loss เยอะ
```

**ทำไม Fakeout ถึงเกิด?**  
Smart Money ต้องการ **Liquidity** — คำสั่ง Stop Loss ของคนที่ตั้งไว้เหนือ EQH หรือต่ำกว่า EQL

```
ตัวอย่าง:
  EQH ที่ 4500 — มีคน Short จำนวนมาก (รอ Sell ที่ 4500)
  → พวกเขาตั้ง SL เหนือ 4500 (ที่ 4505)
  → Smart Money ดันราคาทะลุ 4500 → 4505
  → Stop Loss คน Short ทั้งหมด = Buy order = Liquidity
  → Smart Money ซื้อของที่ 4505 (ใช้ SL คนอื่นเป็นของ)
  → พอได้ของ → ราคากลับลงมา
  → Fakeout
```

**Fakeout vs Real Breakout:**

| ดูอะไร | Fakeout | Real Breakout |
|--------|---------|---------------|
| ปิดนอกแนว | ไม่ — แค่ wick | ใช่ — body ปิดนอก |
| กลับเข้า | ใช่ (1-2 candle) | ไม่ |
| Volume | พุ่งแล้วลด | พุ่งต่อเนื่อง |
| Candle ถัดไป | กลับทิศ | ไปต่อ |
| โครงสร้าง | ยัง intact | MSS หรือไปต่อ |

---

## 🔹 Part 3: Multiple Timeframe Analysis (MTF)

### ทำไมต้อง MTF? — ไม่ใช่แค่ดูกราฟเดียว

**ปัญหาของการดู TF เดียว:**

```
ดูแค่ M15 → หา trend ขึ้น → เปิด Buy
→ ที่ไม่รู้คือ D1 กำลังเป็น downtrend ใหญ่
→ Buy ที่ทำ = ว่ายทวนน้ำ → ซวย
```

**MTF = การวิเคราะห์หลาย Timeframe พร้อมกัน — เพื่อให้เห็นทั้งป่าและต้นไม้**

```
D1 (Daily):   ป่า — trend ใหญ่ไปทางไหน?
H4 (4 Hour):  ป่ากลาง — โซน OB/FVG หลักอยู่ตรงไหน?
H1 (1 Hour):  ต้นไม้ — refine โซน, รอ retest
M15 (15 min): ใบไม้ — จังหวะเข้า
```

### หลักการ MTF: HTF > LTF

```
HTF = Higher Timeframe (D1, W1) — ทิศทางหลัก
LTF = Lower Timeframe (H1, M15, M5) — จังหวะเข้า

กฏเหล็ก:
  1. HTF direction = direction ที่คุณเทรด — ไม่มีข้อแม้
  2. LTF ใช้เพื่อจังหวะเข้า — ไม่ใช่เพื่อเปลี่ยน direction
  3. ถ้า HTF กับ LTF ไม่ match → ข้าม หรือ ลด Lot
```

**ตัวอย่างการทำ MTF จริง:**

```
Step 1: เปิด D1
  → เห็น HH + HL → Uptrend ✅
  → direction = Buy

Step 2: เปิด H4
  → หา OB ล่าสุดที่สร้างจาก impulse ขึ้น
  → เจอ OB Buy ที่ 4500 → โซนรอซื้อ

Step 3: เปิด H1
  → ราคากำลัง retest 4500
  → ดูว่า candle ที่ 4500 มี rejection (Pin Bar) หรือไม่

Step 4: เปิด M15
  → ดูจังหวะเข้า — Bullish Engulfing ที่ OB → Entry B
  → SL = 4490 (ต่ำกว่า OB เล็กน้อย)
  → TP = 4550 (HH ล่าสุด)
```

**ข้อผิดพลาด MTF ที่พบบ่อย:**

```
❌ D1 = ขึ้น → เปิด Buy
   → H4 = OB Sell → ไม่สน
   → ผิด — ต้องให้ HTF กับ LTF สอดคล้องกัน

❌ D1 = ขึ้น → H4 = ขึ้น
   → เปิด Buy → M15 ลง → panic ขาย
   → ผิด — M15 = noise — direction = D1 (HTF)

✅ D1 = ขึ้น → รอ LTF ย่อ → เข้า Buy ที่ OB
```

### ต้องดู TF อะไรบ้าง?

| TF | ใช้ดูอะไร | เหมาะกับ |
|----|-----------|----------|
| W1 (Weekly) | แนวโน้มระยะยาวมาก | Position Trader |
| D1 (Daily) | Trend หลักของวัน | Swing Trader |
| H4 (4 Hour) | OB/FVG หลัก, S/R ใหญ่ | Intraday |
| H1 (1 Hour) | Refine OB, Structure กลาง | Scalp/Intraday |
| M15 (15 Min) | Entry, A/B/C | Scalp |
| M5 (5 Min) | Entry ละเอียด, Confirmation | Scalp |

> **กฎ: เลือก HTF ที่ TF สูงกว่าที่คุณเทรด 1-2 ระดับ**  
> เช่น ถ้าเข้า M15 → HTF = H1 หรือ H4  
> ถ้าเข้า H1 → HTF = D1

---

## 🔹 Part 4: Volume Analysis

### Volume บอกอะไร?

**Volume = จำนวนสัญญาที่ซื้อขายในเวลานั้น**

```  
Volume สูง = มีคนสนใจ = การเคลื่อนที่มีนัยสำคัญ  
Volume ต่ำ = มีคนสนใจน้อย = การเคลื่อนที่อาจเป็น noise  
```

| Volume | ราคา | แปลผล |
|--------|------|--------|
| เพิ่ม | ขึ้น | **Bullish** — มีแรงซื้อจริง — trend นี้แข็ง |
| เพิ่ม | ลง | **Bearish** — มีแรงขายจริง — trend นี้แข็ง |
| ลด | ขึ้น | **Bearish divergence** — ขึ้นแต่ไม่มีแรงซื้อ — ใกล้กลับ |
| ลด | ลง | **Bullish divergence** — ลงแต่ไม่มีแรงขาย — ใกล้กลับ |
| ลด | Sideway | **Accumulation** — กำลังสะสม |
| เพิ่ม | Sideway | **Distribution** — กำลังกระจาย |

### Volume Spike (volume > 2× ปกติ)

```
Volume Spike = Volume มากกว่าค่าเฉลี่ย 2 เท่า

เกิดขึ้น 3 กรณี:
  1. Breakout จริง — volume ตามราคาทะลุ S/R
     → Volume ยังพุ่งต่อเนื่อง → breakout จริง
  2. Liquidity Grab — volume พุ่งแค่ตอนทะลุ
     → Volume แล้วลดทันที → fakeout
  3. News Event — ข่าวสำคัญ
     → volume พุ่งสูงมาก → ตลาดผิดปกติ — ควรรอ
```

**วิธีใช้ Volume ร่วมกับ Price Action:**

```
เห็นราคาทะลุ Resistance → เช็ค Volume:
  Volume พุ่ง → Breakout จริง — รอ Retest → Entry
  Volume ปกติ → Fakeout — ข้าม
  Volume พุ่งแล้วลด → Fakeout — รอดู
```

---

## 🔹 สรุป: Price Action Workflow

```
1. เปิด HTF → trend อะไร? (D1/H4)
2. หา OB/FVG หลักบน HTF
3. รอราคาวิ่งมาหาโซน
4. ดู behavior ที่โซน:
   - Impulse → ไปต่อ — เลยโซนไปแล้ว → ข้าม
   - Pullback → กำลังมาโซน → รอสัญญาณ
   - Consolidation → กำลังสะสม → ระวัง breakout
5. รอ Retest — ราคาแตะโซน + rejection
6. เช็ค LTF — M15/M5 — หา Entry
   🟢 Entry B — แตะ OB + rejection candle
   🟢 Entry C — ทะลุ + reclaim (volume confirm)
   🟢 Entry A — Limit order ที่ OB (รอได้)
7. จัดการ SL/TP ตาม Concept
```

### คำสำคัญที่ต้องจำ

| คำ | แปลว่า | ใช้ยังไง |
|----|--------|----------|
| **Impulse** | การเคลื่อนที่แรง | หา OB |
| **Pullback** | การย่อตัว | รอเข้าตาม trend |
| **Reversal** | การกลับตัว | รอ MSS + confirm |
| **Fakeout** | Breakout ปลอม | รอ confirm ก่อนเข้า |
| **Retest** | กลับมาเทส | จังหวะเข้า |
| **Volume** | ปริมาณซื้อขาย | confirm ความแรง |
| **Consolidation** | การสะสม | รอ breakout |
| **MTF** | หลาย Timeframe | HTF=ทิศ, LTF=entry |

---

> **"ราคาไม่รู้อนาคต — แต่มันทิ้งร่องรอยไว้ให้อ่านเสมอ"**

---

## 🔹 Links

- ก่อนหน้า: [[02 Market Structure & Support Resistance]]
- SMC: [[SMC Concept 1 - Entry A B C]], [[SMC Concept 4+5 MSS]]
- กลับไป: [[_Curriculum]]
