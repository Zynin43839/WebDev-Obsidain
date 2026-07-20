# Money Management (Pt. I-IV) — บทสรุปภาษาไทย

> **ผู้เขียน**: Dave Landry | **แหล่ง**: cAlgoBot Money Management Series
> **หัวข้อหลัก**: กฎและบทเรียน money management สำหรับนักเทรด จาก 17 กฎสากล + สัมภาษณ์มืออาชีพ 4 คน

---

## 1. กฎ 6 ข้อแรก: หลักพื้นฐาน risk management

### ข้อ 1 — เสี่ยงแค่ % เล็กๆ ต่อเทรด
- เสี่ยงไม่เกิน **2% ของพอร์ต** ต่อ trades หนึ่ง
- นักเทรดมืออาชีพ 2 คนที่สร้างความมั่งคั่งได้จริง ทั้งคู่ risk **ต่ำกว่า 1%** ต่อเทรด และใช้ stop loss ที่แน่นอน — ไม่ใช่ "mental stop"
- หลักการ: **รอดให้พอ ถึงจะรวยได้**

### ข้อ 2 — จำกัด total portfolio risk ที่ 20%
- ถ้าถูก stop out ทุก position พร้อมกัน ยังเหลือ **80% ของทุน** — นี่คือ safety net สูงสุด

### ข้อ 3 — Reward-to-risk ratio ต้อง ≥ 2:1
- เสี่ยง 1 จุด ต้องได้กำไรอย่างน้อย 2 จุด (preferably 3:1)
- ระบบ S&P ที่เสี่ยง 3 จุดเพื่อจะได้ 1 จุด — **ล้มละลายตั้งแต่ drawdown แรก** เพราะต้องใช้ 3 เทรดถูกชดเชย 1 เทรดผิด

### ข้อ 4 — เข้าใจความเสี่ยงจริงของตลาดที่เทรด
- อย่าหลอกตัวเองว่าเสี่ยงน้อย ถ้าถือ position ข้ามคืนในหุ้น tech ที่ผันผวน หรือ S&P futures ที่มี leverage สูง

### ข้อ 5 — ปรับ position size ตาม volatility
- ตลาดผันผวน → เล่นน้อยลง
- volatility เปลี่ยนตลอดเวลาตามสภาวะตลาด — ต้อง adapt

### ข้อ 6 — เข้าใจ correlation ระหว่าง positions
- Long heating oil + crude oil + unleaded gas = ไม่ใช่ 3 positions — แต่เป็น **1 position ในพลังงานที่มี risk 3 เท่า**
- ตลาดที่ corr กันสูง = risk ที่ซ้อนทับโดยไม่รู้ตัว

---

## 2. กฎข้อ 7-9: กำไรก้อน + trade frequency + ทุน

### ข้อ 7 — เก็บกำไรก้อนบางส่วนเสมอ
- ถ้าได้กำไรก้อนใหญ่เร็ว → **ขายออกบางส่วนทันที** โดยเฉพาะ short-term trading ที่กำไรก้อนหายาก

### ข้อ 8 — ยิ่งเทรดบ่อย ยิ่งเสี่ยงน้อยต่อเทรด
- Day trader (สิบกว่ารอบ/วัน) → risk ต่ำกว่า 2% ต่อเทรด — วันเดียว bad day ก็เกือบหมด
- Position trader (3-4 เทรด/ปี) → risk ได้ 3-5% ต่อเทรด
- **ไม่ว่าจะเทรดแบบไหน รวม risk ต้อง ≤ 20%**

### ข้อ 9 — มีทุนให้พอ (Adequately Capitalized)
- **ไม่มี Holy Grail** แต่ถ้ามี — คือ "มีเงินพอเล่น + เสี่ยงน้อย"
- นักเทรดหลายคนล้างพอร์ตเล็กๆ ตอนเริ่มต้น จนกว่าจะมีทุนพอถึงจะรอด long-term
- **ต้องเตรียมทุน ≥ 2× historical drawdown** — กำไรจริงจะเป็นแค่ครึ่งเดียวของ backtest
- ถ้า backtest บอก drawdown $10,000 → ต้องมี **$20,000-$50,000** ถึงจะรอดจริง

---

## 3. กฎข้อ 10-16: วินัยและการจัดการ position

### ข้อ 10 — ห้าม average down
- ถ้าผิด → **ยอมรับแล้วออก** — สอง wrong ไม่ make a right

### ข้อ 11 — ห้าม pyramid หรือทำให้ถูกวิธี
- ถ้าจะเพิ่ม position → เพิ่มเฉพาะ position ที่ **มีกำไรแล้ว** (ไม่ใช่ position ที่ขาดทุน)
- โครงสร้าง pyramid: ซื้อ 600 → บวก 300 → บวก 100 (largest ที่ base)
- **ต้องไม่เกิน risk limits เดิม** (2% per trade, 20% total)

### ข้อ 12 — ต้องมี stop order จริงเสมอ
- Mental stop ไม่ work — market ไม่สนใจว่าคุณ "จะหยุด" ตรงไหน

### ข้อ 13 — เก็บกำไรบางส่วนเมื่อเทรดบวก (2-for-1 money management)
- กำไรเกิน initial risk → **ขายออกครึ่งหนึ่ง + ย้าย stop ไป breakeven**
- ผลลัพธ์: เลวร้ายที่สุดก็แค่ breakeven สำหรับที่เหลือ แต่ยังมี upside

### ข้อ 14 — เข้าใจ market ที่เทรด (โดยเฉพาะ derivatives)
- ตัวอย่าง: นักเทรดขาย put options จนถูก exercise → ถูก "put" หุ้น → ขาดทุนวันละหลายพันโดยไม่รู้ตัว จน broker call margin

### ข้อ 15 — รักษา max drawdown ≤ 20-25%
- เกิน 25% → **กู้คืนยากมาก** (loss 60% ต้องได้กำไร 150% ถึง breakeven, loss 70% ต้อง 233%)

### ข้อ 16 — หยุดเทรด + ทบทวนเมื่อมี losing streak
> *"เมื่อเทรดผิด 1-3 ครั้งติด ไม่ว่าจะขาดทุนมากหรือน้อย แสดงว่ามีบางอย่างผิดที่ตัวคุณ ไม่ใช่ตลาด trend อาจเปลี่ยนไปแล้ว กฎของผมคือ ออกมาก่อน รอ แล้วศึกษาว่าทำไมถึงขาดทุน จำไว้: คุณไม่มีวันเสียเงินจากการนั่งดูเฉยๆ"* — W.D. Gann

---

## 4. กฎข้อ 17 + บทสัมภาษณ์มืออาชีพ: จิตวิทยาและ insight จาก pro

### ข้อ 17 — ผลกระทบทางจิตวิทยาของการขาดทุน
- ไม่เหมือนกฎอื่น — ข้อนี้ **quantify ไม่ได้**
- ต้องถามตัวเองตรงๆ: "ถ้าขาดทุน X% จะกระทบ lifestyle หรือไม่?"
- ต้อง **emotional comfortable** กับ risk ที่รับ — ไม่ใช่แค่รู้ว่า risk เท่าไหร่

---

## 5. บทเรียนจากมืออาชีพ 4 คน

### Manuel Ochoa — "ถูก wipe out ตั้งแต่เรียนมหาลัย"
- มีพอร์ต $20,000 → คืนเดียวถูก margin call เหลือ **-$2,000** เพราะข่าว UK PM
- **บทเรียน**: คิดเรื่อง loss ก่อน ไม่ใช่แค่กำไร
- ไม่เคย risk มากกว่า **2.5%** ต่อเทรด — เพราะรู้ว่า exceptional event จะทำให้ loss จริง **เป็น 2 เท่า** ของที่คำนวณ
- วาง stop ให้อยู่นอก **average range 3-4 วันล่าสุด** — ถ้าไม่ทำจะถูก noise หยิบออกทุกครั้ง
- **System testing = historical estimates ไม่ใช่ fact** — ยิ่งเทรดนาน drawdown สูงสุดจะยิ่งใหญ่ขึ้น (ไม่ show ใน backtest)
- *"Controlling risk is much more important than the system itself"*

### Dr. Paul Ruggieri — หมอศัลยกรรม + นักลงทุนหุ้น medical
- ไม่ใช่ technical trader — ใช้ **fundamental research** คัดหุ้น
- กฎเหล็ก: **ไม่เคยซื้อ position ขนาดใหญ่พอที่จะกระทบ lifestyle**
- หุ้น single-focus (มี pipeline ตัวเดียว) → รู้ว่าโอกาสเป็น 0 มีสูง → **เล่นน้อยมาก** หวัง homerun
- หุ้น diversified → กล้าซื้อตอน drop เพราะ fundamentals ไม่เปลี่ยน
- ขายทำกำไร **ก่อน drug approval** เพราะ side effects ที่ไม่คาดคิดจะเกิดเมื่อใช้จริง

### Jeff Cooper — "พ่อ lose หมดจากตลาด → Jeff เกือบlose หมดเหมือนกัน"
- ครอบครัวถูกทำลายโดย buy-and-hold + margin ตอนพ่อ — แล้ว Jeff ก็ **ทำซ้ำเหมือนพ่อ**
- บทเรียน: human nature ต้อง "รู้สึกเอง" ไม่ใช่แค่รู้จากคนอื่น
- **Risk = ¼ ถึง ½% ต่อเทรด** แล้วไม่เปลี่ยนขนาดเทรด → risk ยิ่งลดเรื่อยๆ ตาม equity growth
- ใช้ **4 ประเภท stop**: price stop (ไม่เกิน 1 จุด), pivot stop (ใต้ breakout point), time stop (ไม่ move = ออก), size stop (ใต้ large bid)
- *"Any fool can enter, it takes talent to exit consistently and profitably"*
- Market choppy → เล็กกว่า, tight stop, เอา profit เร็ว
- Market trending → ปล่อยให้วิ่ง, press position เมื่อจังหวะมา

### Kevin Haggerty — Institutional → Day trader
- Day trader ต้องมี **tight stop** (¼ ถึง ⅜ จุด) — ไม่มีทางหลีกเลี่ยง
- **ถูก 40% ผิด 60%** แต่ยังกำไร → ดีลsize เล็ก, loss เล็ก, win ใหญ่
- ตัวอย่าง: ผิด 3 ครั้ง (-$275 × 3 = -$825) แล้วถูก 1 ครั้ง (+$975) = **กำไร $150** แม้ผิด 75%
- แต่ถ้า lose ¼ + profit ¼ + commission = **ล้มละลาย**
- นักเทรดมือใหม่: เล่น size เล็ก + stop กว้างกว่า (ไม่ต้อง day trade ถ้าไม่มี direct access)
- เข้า position → **สมมติว่าผิดจนกว่า market ثبتว่าถูก** — ไม่ต้องรอ stop hit, exit ถ้า dynamic เปลี่ยน

---

## 6. สรุปหลักการ Money Management ทั้งหมด

| หลักการ | รายละเอียด |
|---------|-----------|
| **Risk ต่อเทรด** | ≤ 2% (มืออาชีพใช้ 0.25-2.5%) |
| **Total portfolio risk** | ≤ 20% |
| **Reward:Risk** | ≥ 2:1, prefer 3:1 |
| **Max drawdown** | ≤ 20-25% (เกินกู้คืนยากมาก) |
| **Stop loss** | ต้องมีจริงเสมอ — mental stop ไม่ work |
| **Adequate capital** | ≥ 2× historical drawdown |
| **Pyramiding** | ทำเฉพาะ position ที่กำไร, largest ที่ base |
| **Profit taking** | ครึ่งหนึ่งเมื่อ profit > initial risk → move stop to breakeven |
| **Correlation** | ดูภาพรวม risk ไม่ใช่แค่จำนวน positions |
| **จิตวิทยา** | ต้อง comfortable กับ loss ทั้งทางการเงินและอารมณ์ |
| **Learning loop** | หยุดเทรด + ทบทวนเมื่อมี losing streak — ตลาดอยู่ตรงนี้เสมอ |

---

## 7. หนังสือแนะนำต่อ
- Mark Boucher — *The Hedge Fund Edge* (1999)
- Mark Etzkorn — *TradingMarkets.com Guide to Conquering the Markets* (1999)
- Larry Connors & Linda Raschke — *Street Smarts* (1995)
- Robert Rottela — *Elements of Successful Trading* (1992)

---

*สรุปจาก Money Management Series Parts I-IV โดย Dave Landry — 4 chunks, ~15 หน้า*
