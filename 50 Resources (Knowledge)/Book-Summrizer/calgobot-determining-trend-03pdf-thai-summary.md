# สรุป: Determining Trend — John Hayden

## 1. RSI เป็นเครื่องมือหลักในการหา Trend

John Hayden ใช้ **RSI (Relative Strength Index)** ของ J. Welles Wilder Jr. เป็น indicator หลักในการ identify trend RSI ดีกว่า momentum oscillators ตัวอื่น (Momentum, Rate of Change, Stochastics) เพราะโครงสร้างของ RSI ช่วย **smooth out distortion** ที่เกิดจาก big price moves ที่ถูก dropped จาก lookback period

- RSI อยู่ในช่วง 0–100 เสมอ ไม่ต้องเปรียบเทียบกับ歷史 data
- ค่าเริ่มต้น 14-period เป็นค่าที่ใช้ได้ดีทุก timeframe (ครึ่งหนึ่งของ lunar cycle)
- ใน gold/silver/crude/financials ใช้ 25-period (ครึ่งของ 50-day cycle)
- ต้องมี data อย่างน้อย 90 periods ก่อนใช้ RSI วิเคราะห์ trend

## 2. RSI Range Shifts — Bull vs Bear Market

แนวคิดสำคัญจาก **Andrew Cardwell**:

| ตลาด | RSI Support | RSI Resistance |
|------|:-----------:|:--------------:|
| **Bull market** | **40** | **80** |
| **Bear market** | **20** | **60** |

- ใน uptrend: RSI จะอยู่ช่วง 40–80, ทำ higher highs และ higher bottoms
- ใน downtrend: RSI จะอยู่ช่วง 20–60, ทำ lower highs และ lower bottoms
- **สัญญาณ trend change** แรก: RSI ที่เคย respect 60 พุ่งขึ้นถึง 70+ แล้ว decline กลับมา respect 40 แทน
- **แนวคิดระดับต่อไป**: ใน bull market ที่แข็งแรง 60 จะกลายเป็น support (ไม่ใช่ resistance) — และใน bear market ที่แข็งแรง 40 จะกลายเป็น resistance

**กรณีศึกษา Japanese Yen**:
- RSI ที่ 60 = resistance → decline violation 40 → bullish divergence → 60 กลายเป็น support → ยืนยันว่ากลับมาเป็น bull market
- นักเทรดควรมองหา entry ที่ old resistance levels กลายเป็น support (L) ใน bull market

## 3. Divergence — สัญญาณ Trend Change

- **Bullish divergence**: price ทำ new low แต่ RSI ไม่ทำ new low → เตือนว่า trend อาจเปลี่ยนเป็นขึ้น
- **Bearish divergence**: price ทำ new high แต่ RSI ไม่ทำ new high → เตือนว่า trend อาจเปลี่ยนเป็นลง

**ข้อแตกต่างจากตำราทั่วไป**: Hayden กลับหัวแนวคิด —
> "Bearish divergence = เราอยู่ใน **BULL** market, Bullish divergence = เราอยู่ใน **BEAR** market"

เพราะ divergence จะเกิดซ้ำๆ ได้เฉพาะใน trend ที่แข็งแรงเท่านั้น:
- Bearish divergence → bull market ที่ overbought กำลังจะ correction
- Bullish divergence → bear market ที่ oversold กำลังจะ rebound

**การนับระยะ divergence**:
- 2–6 periods = มีนัยสำคัญที่สุด (price detour มีโอกาสเกิดขึ้นสูง)
- ระยะยาว (weeks–months) = มีนัยสำคัญน้อยกว่า
- divergence จะ "locked in" เมื่อราคาหยุดไม่ให้ RSI เกินค่า peak ก่อนหน้า

## 4. Moving Averages — Trend Confirmation

Hayden ใช้ **9-period SMA + 45-period WMA** ทั้งบน price และ RSI:

| 9SMA(price) vs 45WMA(price) | 9SMA(RSI) vs 45WMA(RSI) | Trend |
|:---------------------------:|:------------------------:|:-----:|
| 9 > 45 | 9 > 45 | **Up** |
| 9 < 45 | 9 < 45 | **Down** |
| 9 > 45 | 9 < 45 | Sideways to up |
| 9 < 45 | 9 > 45 | Sideways to down |

- RSI มีความ volatile กว่า price → crossover ของ RSI จะเกิดก่อน price crossover เสมอ
- **45-period WMA** ทำหน้าที่เป็น support/resistance ทั้งบน price และ RSI
- การที่ราคาไม่สามารถ distancing จาก 45 MA ได้ = สัญญาณ trend จะยังคงเดิม

## 5. Support & Resistance Transformation

- ใน uptrend: former resistance levels → กลายเป็น **current support**
- ใน downtrend: former support levels → ถูก violate → กลายเป็น **current resistance**
- RSI chart ช่วยบอกจุดที่ RSI เคยเจอ effective resistance/support → ใช้เป็น reference สำหรับ price action

## 6. Checklist 5 ข้อ — Determining Trend

เวลาดู chart ใหม่ ให้ถามตัวเอง:

1. **Range**: RSI อยู่ใน range ไหน? มี range shift หรือไม่?
2. **Support/Resistance**: กำลัง respect หรือ violate? มี role reversal หรือไม่?
3. **Trendline**: เส้น trend บน price หรือ RSI ถูกหักหรือไม่?
4. **Divergence**: มี divergence แบบไหน? (และมันบอก trend อะไร)
5. **Moving Averages**: MA บน price และ RSI บอกอะไร?

## สรุป

John Hayden เสนอแนวทาง Quick, Accurate & Effective ในการ determine trend โดยใช้ RSI เป็นแกนหลัก ร่วมกับแนวคิด Cardwell เรื่อง range shift (40/80 = bull, 20/60 = bear), divergence analysis (กลับหัวกับ textbooks), และ moving average crossover confirmation เป้าหมายสูงสุดคือ identify **trend direction ก่อน** แล้วค่อยหา entry/exit — ไม่ใช่ divergence ที่บอกให้สวน trend
