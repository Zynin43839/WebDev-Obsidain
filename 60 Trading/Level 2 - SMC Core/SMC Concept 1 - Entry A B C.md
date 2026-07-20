---
created: 2026-05-22
type: note
topic: SMC
status: learning
---

# SMC Concept 1 — OB → Entry A/B/C

## 🔹 แบบ A: Limit Order (รอที่ OB)

**ความหมาย:** เข้าซื้อ/ขายทันทีที่ราคาแตะ Order Block โดยไม่รอ confirmation

**จุดประสงค์:** ไม่พลาดตอนราคาเด้งจาก OB แบบรวดเดียว

**วิธีใช้:**
1. หา OB ที่ชัดเจน (candle ก่อน impulse)
2. ตั้ง Limit Order (Buy/Sell Limit) ไว้ที่โซน OB
3. SL ใต้ OB, TP ไว้ที่ FVG/High/Low ถัดไป

**ข้อดี:** entry ดีที่สุด, ไม่พลาด, ไม่ต้องนั่งจ้อง
**ข้อเสีย:** เสี่ยง false break ถ้า OB อ่อน, ไม่มี confirmation

**Use Case:** OB 4500 + FVG ซ้อน 4498-4502 → Limit Buy ที่ 4500 → TP 4520

---

## 🔹 แบบ B: Retest Confirmation (รอเด้งก่อน)

**ความหมาย:** รอราคามาแตะ OB แล้วเกิดแท่งยืนยัน (bullish/bearish candle ปิด) ก่อนเข้า

**จุดประสงค์:** ยืนยันว่า OB นี้เด้งจริง

**วิธีใช้:**
1. หา OB
2. รอราคามาแตะ OB
3. รอแท่ง candle ปิดใน direction ที่จะเข้า
4. เข้าหลังแท่งยืนยัน

**ข้อดี:** confirmation สูง, false break น้อย
**ข้อเสีย:** พลาดได้ถ้าราคาเด้งไปเลย, entry แย่กว่า A

**Use Case:** OB 4500 ไม่แน่ใจ → รอ bullish candle ปิด → เข้า 4501

---

## 🔹 แบบ C: Breakout Reclaim (false break)

**ความหมาย:** ราคาทะลุ OB แล้วกลับมาเหนือ OB → เข้า

**จุดประสงค์:** จับ reversal ตอน trend หมดแรง

**วิธีใช้:**
1. หา OB
2. ราคาทะลุ OB
3. รอราคากลับมาเหนือ OB
4. เข้า

**ข้อดี:** RR สูงมาก
**ข้อเสีย:** เสี่ยงโดน trend real ทับ

**Use Case:** ทะลุ OB 4500 ลงไป 4495 → กลับมา 4500 → Buy → TP 4530

---

## ตารางเลือกใช้

| สถานการณ์ | ใช้ |
|-----------|-----|
| OB แรง + FVG ซ้อน | A |
| ไม่มั่นใจ, sideways | B |
| Trend เริ่มหมดแรง | C |
| Confluence น้อย | **ข้าม** |

## คำศัพท์

- **OB แรง**: candle ใหญ่, ราคาเคยเด้งหลายครั้ง
- **FVG ซ้อน**: FVG หลาย TF ซ้อนกันในโซนเดียวกัน
- **Confluence**: จำนวนปัจจัยหนุน (OB+FVG=2, OB+FVG+MSS=3)
