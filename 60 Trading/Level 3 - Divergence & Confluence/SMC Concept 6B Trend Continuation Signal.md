---
created: 2026-05-29
type: note
topic: SMC
status: learning
---

# Hidden Divergence — Concept 6B: Trend Continuation Signal

---

## 🔹 ความหมาย

**Hidden Divergence** เกิดขึ้น **ใน trend ปกติ** — ตลาดพักตัว แต่โมเมนตัมยังอยู่ — บอกว่า trend ยังไม่จบ แค่ย่อตัว

```
Hidden Bullish Divergence (ใน uptrend):
ราคา: Higher Low (HL) → แสดงว่าย่อตัวไม่ลึก
RSI:  Lower Low (LL) → แต่โมเมนตัมดูเหมือนลดลง

→ จริงๆ = ตลาดกำลังสะสมแรง → ต่อไปจะขึ้นต่อ
→ "ซ่อน" ความแรงไว้ใน RSI ที่ดูเหมือนอ่อน

เปรียบ: นักกีฬาหมอบก่อนกระโดด = ดูอ่อนแต่กำลังจะออกตัว
```

### Regular vs Hidden — ต่างกันยังไง?

| | Regular Divergence | Hidden Divergence |
|--|-------------------|-------------------|
| ใช้ตอน | Trend หมดแรง | Trend กำลังไป |
| สอนอะไร | "เตรียมกลับ" | "เทรนด์จะไปต่อ" |
| Entry | B หรือ C (ระวัง) | A (มั่นใจ) |
| Confluence | Order Block ฝั่งตรงข้าม | Order Block ฝั่งเดียวกับ trend |
| Risk | สูง (กลับตัว = uncertainty) | ต่ำ (ไปทาง trend) |

---

## 🔹 จุดประสงค์

1. **ไม่หลุดออกจาก trend เร็วเกินไป** เพราะเห็น RSI ลดแล้วเข้าใจผิดว่า trend หมด
2. **ใช้เป็น Confluence สำหรับ Entry แบบ A (Limit Order)** — เพราะมั่นใจว่า trend จะต่อ
3. **เพิ่ม Win Rate** — Hidden Divergence มี reliability สูงกว่า Regular เพราะไปทาง trend

---

## 🔹 วิธีใช้

### Hidden Bullish Divergence (Uptrend)

```
เงื่อนไข:
1. ราคา: Higher Low (HL) — ย่อแล้วเด้ง
2. RSI: Lower Low (LL) — จุดต่ำกว่าเดิม
3. RSI ยังอยู่เหนือ 40 (ไม่อ่อนเกิน)

→ สรุป: trend ขึ้นยังแข็ง → รอรีเทสต์ → BUY
→ Entry: แบบ A (Limit) หรือ B (Retest) ก็ได้
```

### Hidden Bearish Divergence (Downtrend)

```
เงื่อนไข:
1. ราคา: Lower High (LH) — ดีดแล้วลง
2. RSI: Higher High (HH) — จุดสูงกว่าเดิม
3. RSI ยังอยู่ใต้ 60 (ไม่แรงเกิน)

→ สรุป: trend ลงยังแข็ง → รอรีเทสต์ → SELL
→ Entry: แบบ A (Limit) หรือ B (Retest)
```

### วิธีแยก Regular vs Hidden (ไม่ให้สับสน)

```
ถาม 2 ข้อนี้:

1. ราคาทำอะไร?
   - Lower Low / Higher High = Regular (กำลังจะกลับ)
   - Higher Low / Lower High = Hidden (กำลังจะไปต่อ)

2. Trend ปัจจุบันคืออะไร?
   - ไม่มี trend ชัด / หมดแรง → Regular
   - Trend แรง + แค่ย่อ → Hidden

ถ้าตอบไม่ได้ → ใช้ข้อมูลเพิ่ม:
   - RSI อยู่เหนือ 60 = trend ขึ้นยังแข็ง → มองหา Hidden Bullish
   - RSI อยู่ใต้ 40 = trend ลงยังแข็ง → มองหา Hidden Bearish
```

---

## 🔹 ข้อดี

- ✅ Hidden = reliability สูงกว่า Regular (ไปทาง trend)
- ✅ ใช้ Entry A ได้ = Risk to Reward Ratio ดี
- ✅ ช่วยไม่ให้หลุดจาก trend ก่อนเวลา

## 🔹 ข้อเสีย

- ❌ มองหาก็จริงแต่อาจเป็น Regular ก็ได้ — ต้องแยกให้ออก
- ❌ ต้องมี trend ชัดเจนก่อน — Sideway ใช้ Hidden ไม่ได้
- ❌ Hidden Divergence หลายครั้งก่อน trend จบ = จับจุด Exit ไม่ได้

---

## 🔹 Use Case

```
XAUUSD H1 Uptrend: Higher High → Higher Low → Higher High → Higher Low → Higher High
ราคาย่อมา Higher Low = 4650 (Higher Low)
RSI: trough 1 = 48, trough 2 = 42 (Lower Low)
→ Hidden Bullish Divergence ✓ (ราคา Higher Low, RSI Lower Low)

เช็ค:
- RSI ยัง > 40 ✓ (ไม่อ่อน)
- Trend ชัด ✓ (Higher High + Higher Low)
- Order Block BUY ล่าสุดที่ 4640 ✓

Action:
→ Enter: Limit Buy ที่ 4650 (Entry A — มั่นใจ trend)
→ Stop Loss: 4645 (ใต้ Order Block 5 pip)
→ TP1: 4700 (Swing High)
→ TP2: 4730 (Higher High ถัดไป)
```

---

## 🔹 ตารางเทียบ Regular vs Hidden Divergence (เต็ม)

| มิติ | Regular Divergence | Hidden Divergence |
|-----|-------------------|-------------------|
| ความหมาย | Trend กำลังจะกลับ | Trend จะไปต่อ |
| ราคา | Lower Low / Higher High | Higher Low / Lower High |
| RSI | Higher Low / Lower High | Lower Low / Higher High |
| reliability | 40-60% (ต้อง Confluence) | 60-80% (ไปทาง trend) |
| Entry ที่เหมาะ | B หรือ C | A หรือ B |
| Stop Loss | กว้าง (unsertainty สูง) | แคบ (มั่นใจ trend) |
| Timeframe ที่เหมาะ | H1+ | M15 / H1 |
| ใช้กับ | หาจุดกลับตัว | หาจุดเข้าเพิ่ม |
| ตลาด | Trend สิ้นสุด | Trend กำลังไป |
| Indicator | RSI / MACD / Stochastic | RSI / MACD / AO |
