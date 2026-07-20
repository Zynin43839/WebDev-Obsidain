# สรุปภาษาไทย — The Traders Trick Entry (TTE)

> **ต้นฉบับ:** Traders Trick Entry, Pages 9-57  
> **ผู้เขียน:** Joe Ross (cAlgoBot source)  
> **สรุปโดย:** Agent (EN raw chunks → TH summary)

---

## TTE คืออะไร?

TTE (Traders Trick Entry) คือเทคนิคการเข้าเทรดล่วงหน้าก่อนที่ราคาจะ breakout จุดสำคัญ (เช่น #2 point ของ 1-2-3 formation หรือ Ross Hook point) โดยใช้ momentum ของ market mover และกลุ่ม trader อื่นๆ (Fibonacci, Gann) มาเป็นแรงขับให้เทรดได้กำไรก่อนถึง breakout จริง — เรียกได้ว่า "ขี่รถตามคนรวย"

### ข้อได้เปรียบหลัก
- **ได้ free trade**: ปิดกำไรบางส่วนก่อนถึง breakout point จริง → ถ้า breakout เป็น false break ก็ยังได้กำไร
- **ใช้ momentum ของคนอื่น**: market mover ต้อง push ราคาไปที่ breakout point อยู่แล้ว + Fibonacci/Gann traders จะเข้าซื้อที่ retrace level → รวมแรงกันดันราคา

---

## กฎ TTE สำหรับ 1-2-3 Formation

### ข้อบังคับ #1 — ต้องมีพื้นที่พอสำหรับ覆盖 costs + กำไร

ระหว่างจุด entry กับ #2 point ต้องมีระยะห่างพอที่จะ:
- ค่า spread + commission (cover costs)
- เอา profit บางส่วนได้

**เหตุผล**: ถ้า breakout ล้มเหลว (false break) → เรายังเทรดได้กำไรและเทรดฟรี (ได้ profit ที่ offset ค่า costs ไปแล้ว)  
**เงื่อนไข**: ต้องเทรดอย่างน้อย 2 contracts — 1 contract สำหรับทำกำไร, 1 contract สำหรับ trail ไปยัง breakout point  
**ถ้าระยะไม่พอ**: รอ price bar ถัดไปอีก 1-2 bars แล้วค่อยเข้า

### ข้อบังคับ #2 — 1-2-3 Formation ต้องเกิดที่ปลาย trend ห้ามอยู่ใน consolidation

- 1-2-3 formation ที่เกิดในช่วง consolidation ไม่มีความหมาย — มีทั้ง BUY และ SELL ปนกัน ไม่สามารถตีความทิศทางได้
- ต้องเกิดหลังจาก trend ที่ชัดเจนแล้วเท่านั้น

### ข้อบังคับ #3 — ห้ามเกิน 3 bars ของการ correction ก่อนราคากลับไปหา #2 point

**Trade-off ระหว่างความน่าจะเป็น vs. จำนวน pip ที่จะได้:**

| Bar ที่ Violate | % ที่จะผ่าน #2 point | จำนวนห้องสำหรับ cover costs + profit |
|:---:|:---:|:---:|
| Bar 1 | สูงสุด | น้อยที่สุด |
| Bar 2 | ปานกลาง | ปานกลาง |
| Bar 3 | ต่ำสุด | มากที่สุด |

> **คำแนะนำ**: Violate bar ที่ 2 มี probability ดีที่สุดโดยรวม — แม้ chance ต่ำกว่า bar 1 5% แต่มีห้องพอสำหรับ cover costs + profit ดีกว่า

**ข้อยกเว้น**: Double/Triple high/low — 2 bars นับเป็น 1 correcting bar, 3 bars นับเป็น 1 correcting bar  
**เกิน 3 bars**: โอกาสจะ favor consolidation → ไม่ควรเทรด TTE

---

## Ross Hook + TTE

### Ross Hook คืออะไร?
- เมื่อ price breakout จาก #2 point แล้วเกิด reversal bar → จุดสูงสุดของ reversal bar คือ Rh point
- Ross Hook = "จุด hook" ที่เกิดหลัง breakout แล้วมี correction เล็กน้อย

### กฎ TTE สำหรับ Ross Hook

**ข้อบังคับ #1** — เหมือน 1-2-3: ต้องมีพื้นที่พอระหว่าง entry กับ Rh point เพื่อ cover costs + profit

**ข้อบังคับ #2** — Rh formation ต้องเกิดหลังจาก:
- Breakout ของ #2 point จาก 1-2-3 formation
- Breakout ของ Consolidation
- Breakout ของ Ledge

> **เหตุผล**: Violation ของ #2 point / Ledge / Consolidation เป็นตัว "defines trend" แล้ว Rh เป็นตัว "re-establishes trend"

**ข้อบังคับ #3** — เหมือนกัน: ห้ามเกิน 3 bars ของการ correction, exception เดียวกัน

### Correcting Bars ของ Ross Hook
- Bars ที่เกิดหลัง Rh point ที่มี lower highs
- **สำคัญ**: ไม่จำเป็นต้องมี lower lows — lower high ก็พอ

---

## วิธีเทรด TTE — 3 ขั้นตอนง่ายๆ

### ขั้นตอนที่ 1 — ตรวจสอบ formation
- ยืนยันว่า 1-2-3 formation ไม่ได้อยู่ใน consolidation
- ยืนยันว่า Rh เกิดหลังจาก 1-2-3, Ledge หรือ Consolidation ที่ breakout แล้ว

### ขั้นตอนที่ 2 — เตรียม order เมื่อเห็น #2 หรือ Rh point
- ตั้ง buy stop 1 tick เหนือ high ของ correcting bar (หรือ sell stop 1 tick ต่ำกว่า low สำหรับขาลง)

### ขั้นตอนที่ 3 — เลื่อน entry stop ตาม corrected bar
- เมื่อราคาย้าย away จาก #2/Rh point → เลื่อน buy stop ไป 1 tick เหนือ high ของ correcting bar ถัดไป
- ทำซ้ำจนกว่าจะถูก trigger

> **กฎเดียวกันสำหรับขาลง**: Sell stop 1 tick ต่ำกว่า low ของ correcting bar จาก 1-2-3 high, Ledge breakout, Consolidation breakout

---

## ทำไม TTE ใช้ได้จริง — Momentum Stacking

### 1. Market Mover (Smart Money)
- Market mover ต้อง push ราคาไปที่ breakout point (#2 / Rh) อยู่แล้ว
- ถ้าเขาจะ stop เรา เขาต้องหยุด momentum ของตัวเอง → เขาไม่ยอมทำแบบนั้น
- **เรา ride momentum ไปกับเขา**

### 2. Fibonacci Traders
- จะเข้าซื้อที่ retrace level .382, .500, .618
- การซื้อของพวกเขาเพิ่ม momentum → ดันราคาไปทาง breakout point เร็วขึ้น

### 3. Gann Traders
- จะเข้าซื้อที่ retracement 1/3, 1/2, 2/3
- แรงซื้อของ Gann traders + Fibonacci traders = momentum มหาศาล → push ราคาเข้าสู่ TTE trigger

> **สรุป**: 3 กลุ่ม trader รวมแรงกัน (market maker + Fibonacci + Gann) ทำให้ TTE มีความน่าจะเป็นสูงที่ราคาจะถึง trigger point

---

## 1-2-3 Formation vs. Ross Hook — ความแตกต่างหลัง Breakout

| ปัจจัย | 1-2-3 Breakout | Ross Hook Breakout |
|:---|:---:|:---:|
| Chance ที่จะเกิด trend หลัง breakout | **ต่ำกว่า** | **สูงกว่า** |
| เหตุผล | Trend ยังไม่ confirm ชัดเจน | Trend ยืนยันแล้วจาก breakout ของ #2 |
| Bar 3 ของ TTE | เสี่ยงต่อ reversal ได้ | เสี่ยงต่อ reversal ได้เหมือนกัน |
| คำแนะนำ | เลี่ยง bar ที่ 3 | เลี่ยง bar ที่ 3 |

> **สำคัญ**: Bar ที่ 3 ของ TTE เสมอมี danger ที่จะกลับ成为 trend reversal — นี่คือเหตุผลที่ bar ที่ 2 มี probability ดีที่สุดโดยรวม

---

## สรุปสำคัญสำหรับ Algo Trading

1. **TTE = Early Entry ก่อน Breakout** — ไม่ต้องรอ breakout จริง ใช้ correcting bar เป็น trigger
2. **ต้องมี 2 contracts** — partial profit ก่อน + trail ไป breakout
3. **Consolidation = ห้ามเทรด** — TTE ใช้ไม่ได้กับ ranging market
4. **3 bars rule** — เกิน 3 bars ของการ correction = consolidation กำลังจะเกิด → หยุด
5. **Momentum stacking** — Fibonacci + Gann + Smart Money รวมแรงกัน → ทำให้ TTE trigger มี chance สูง
6. **Ross Hook > 1-2-3 Breakout** — สำหรับ trend continuation, Ross Hook มี chance เกิด trend ต่อสูงกว่า
