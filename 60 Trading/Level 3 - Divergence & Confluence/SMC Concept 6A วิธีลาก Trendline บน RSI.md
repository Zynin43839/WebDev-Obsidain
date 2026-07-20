---
created: 2026-05-23
updated: 2026-05-29
type: note
topic: SMC
status: learning
---

# Divergence — Concept 6A: วิธีลาก Trendline บน RSI

---

## 🔹 ความหมาย

**Divergence** คือจุดที่ **ราคากับโมเมนตัม (momentum) จาก Relative Strength Index ไม่ไปทางเดียวกัน** — ราคาขึ้นต่อแต่โมเมนตัมเริ่มอ่อนลง หรือราคาลงต่อแต่โมเมนตัมเริ่มแข็งขึ้น

```
ปกติ: ราคาขึ้น → โมเมนตัมแรงขึ้น (RSI สูงขึ้น)
Divergence: ราคาขึ้น → โมเมนตัมอ่อนลง (RSI ต่ำลง)

เหมือนรถเร่งเครื่องแต่ความเร็วลด = กำลังจะจอด
```

Divergence ต้องลากเทรนด์ไลน์บน RSI **ให้ถูกจุด** — ถ้าลากผิดที่ = divergence ผิด → trade ผิดทิศ

```
Trendline บน RSI:
- ต้องลากที่ **peaks** (จุดสูงสุดของ RSI) สำหรับ bearish divergence
- ต้องลากที่ **troughs** (จุดต่ำสุดของ RSI) สำหรับ bullish divergence
- ไม่ใช่ลากตามราคา — คนส่วนมากลากผิดตรงนี้
```

---

## 🔹 จุดประสงค์

1. **Early signal** — Divergence เกิดก่อนที่ราคาจะกลับตัวจริง (ก่อน Market Structure Shift)
2. **Confluence booster** — OB + Divergence = Confluence 2, OB + Divergence + Market Structure Shift = Confluence 3
3. **กรอง fakeout** — ถ้า divergence ไม่มี → แสดงว่า trend ยังแข็ง → ไม่ควรฝืน

---

## 🔹 วิธีใช้ — 4 ขั้นตอนลาก RSI Divergence

### ขั้นที่ 1: ตั้ง RSI ให้ถูก

```
RSI Period: 14 (ค่าเริ่มต้น — ใช้ไปก่อน ไม่ต้องปรับ)
Level:      Overbought = 70, Oversold = 30

จดจำ:
- Bearish Divergence → RSI > 70 → กำลัง overbought → เตรียมกลับ
- Bullish Divergence → RSI < 30 → กำลัง oversold → เตรียมกลับ
- Divergence ที่เกิดแถว 40-60 = มีน้ำหนักน้อย
```

### ขั้นที่ 2: หา Swing Point บน RSI ก่อน

```
— อย่าดูราคาก่อน — ดูก่อนว่า RSI มี swing peak/trough หรือยัง —

Bullish Divergence: หา RSI trough (จุดต่ำ) 2 จุด
Bearish Divergence: หา RSI peak (จุดสูง) 2 จุด
```

### ขั้นที่ 3: เทียบกับราคา

กฎตายตัว:

| ประเภท | ราคา | RSI |
|--------|------|-----|
| Bullish Regular | Lower Low (LL) | **Higher Low (HL)** |
| Bearish Regular | Higher High (HH) | **Lower High (LH)** |
| Bullish Hidden | Higher Low (HL) | **Lower Low (LL)** |
| Bearish Hidden | Lower High (LH) | **Higher High (HH)** |

**วิธีเทียบให้ไม่สับสน:**
```
1. ดูราคาก่อน — มันทำอะไร (LL / HH / HL / LH)?
2. ดู RSI — มันทำตรงข้ามไหม?
3. ถ้าตรงข้าม = divergence ✓
4. ถ้าไปทางเดียวกัน = ไม่ใช่ divergence ✗
```

### ขั้นที่ 4: เช็ค Confluence

```
Divergence 1 จุด (เปล่าๆ) → reliability 40%
Divergence + Order Block overlap   → reliability 70%
Divergence + Order Block + Market Structure Shift     → reliability 90%
```

---

## 🔹 Regular Divergence (กลับตัว)

| ประเภท | ราคา | RSI | ความหมาย |
|--------|------|-----|----------|
| Bullish Divergence | Lower Low (LL) | **Higher Low (HL)** | ราคาลงต่ำกว่าเดิม แต่ RSI ไม่ลงตาม → แรงขายหมด → เตรียมขึ้น |
| Bearish Divergence | Higher High (HH) | **Lower High (LH)** | ราคาขึ้นสูงกว่าเดิม แต่ RSI ไม่ขึ้นตาม → แรงซื้อหมด → เตรียมลง |

### Regular Bullish Divergence

```
ราคาทำ Lower Low (LL) — จุดต่ำกว่า Low ก่อนหน้า
RSI ทำ Higher Low (HL) — จุดสูงกว่า Low ก่อนหน้า
→ ถึงราคาจะลงต่อ แต่โมเมนตัมขายเริ่มอ่อน
→ ตลาดกำลังบอกว่า "จะกลับตัวขึ้นแล้ว"
```

### Regular Bearish Divergence

```
ราคาทำ Higher High (HH) — จุดสูงกว่า High ก่อนหน้า
RSI ทำ Lower High (LH) — จุดต่ำกว่า High ก่อนหน้า
→ ถึงราคาจะขึ้นต่อ แต่โมเมนตัมซื้อเริ่มอ่อน
→ ตลาดกำลังบอกว่า "จะกลับตัวลงแล้ว"
```

---

## 🔹 Hidden Divergence (ต่อเนื่อง)

| ประเภท | ราคา | RSI | ความหมาย |
|--------|------|-----|----------|
| Hidden Bullish | Higher Low (HL) | **Lower Low (LL)** | ราคาย่อสูงขึ้น แต่ RSI ย่อต่ำลง → กำลังสะสมแรง → เตรียมขึ้นต่อ |
| Hidden Bearish | Lower High (LH) | **Higher High (HH)** | ราคาดีดต่ำลง แต่ RSI ดีดสูงขึ้น → กำลังสะสมแรงขาย → เตรียมลงต่อ |

> ดูรายละเอียดเต็มใน **Hidden Divergence.md**

---

## 🔹 ข้อดี

- ✅ Early signal ก่อน trend เปลี่ยน (เห็นก่อน Market Structure Shift)
- ✅ Risk to Reward Ratio ดีมาก — เข้าใกล้จุดกลับตัว
- ✅ ใช้ Timeframe ไหนก็ได้ (M15, H1, H4)
- ✅ RSI trendline = objective (ไม่ subjective เท่าราคา)

## 🔹 ข้อเสีย

- ❌ False signal บ่อย — โดยเฉพาะในตลาด Sideway
- ❌ ต้องมี indicator ติดกราฟ
- ❌ RSI oscillator = lagging indicator (เกิดหลังจากราคา)
- ❌ ถ้า trend แรงมาก → divergence ได้หลายครั้งก่อนกลับจริง

---

## 🔹 Use Case

```
GOLD M15: ราคาทำ Higher High = 4720 > 4700
RSI: peak 1 = 75, peak 2 = 68 (Lower High)
→ Bearish Regular Divergence ✓

เช็ค:
- RSI > 70 ก่อน divergence (OK — overbought)
- RSI peak ลดลง (Lower High)
- Order Block ที่ 4700 อยู่ใต้ราคา → Order Block + Divergence = Confluence 2

Action: รอราคาลงมา Order Block 4700 → รอ confirmation → SELL
→ ตั้ง Stop Loss 4725, TP1 4650, TP2 4600
```
