---
created: 2026-06-30
type: note
topic: SMC
status: seedling
---

# SMC Foundations — Market Structure, Liquidity, FVG, Smart Money Theory

> **วางรากฐานก่อนเข้า SMC Concepts 1-6C** — ถ้าคุณไม่เข้าใจพื้นฐานเหล่านี้ SMC Concepts จะดูเหมือน "เดา" แต่ถ้าเข้าใจ จะเห็นว่ามันเป็นระบบที่สมเหตุสมผล

---

## 🔹 Part 1: Smart Money คือใคร?

### ใครคือ Smart Money?

Smart Money (หรือเรียก Institutional Money, "เงินก้อนโต") คือ:
- **ธนาคารกลาง (Central Banks)** — FED, ECB, BOJ
- **ธนาคารพาณิชย์ (Investment Banks)** — JP Morgan, Goldman Sachs
- **กองทุนเฮดจ์ฟันด์ (Hedge Funds)** — Bridgewater, Renaissance
- **กองทุนรวม (Mutual Funds)** — BlackRock, Vanguard
- **Market Makers** — ผู้ดูแลสภาพคล่อง

### Smart Money vs Retail — ต่างกันยังไง?

| มิติ | Smart Money | Retail Trader (คุณ) |
|------|------------|-------------------|
| เงินทุน | พันล้าน — ล้านล้าน | พัน — แสน |
| คำสั่งซื้อขาย | Iceberg orders (แบ่งเป็นชิ้นเล็ก) | Market orders (ก้อนเดียว) |
| ข้อมูล | News feed จริง, Dark pool, Order flow | Chart + Indicators |
| Time horizon | เดือน — ปี | นาที — วัน |
| กำไรที่ต้องการ | 10-20% ต่อปี | 100-1000% ต่อเดือน (แล้วเจ๊ง) |
| วิธีทำกำไร | สร้างสภาพคล่อง → ล่า Stop Loss → แล้วค่อยวิ่ง | หาจุดเข้าเป๊ะๆ โดยไม่เข้าใจเบื้องหลัง |

### หลักการทำงานของ Smart Money

```
1. ACCUMULATION (สะสม)
   Smart Money ซื้อของถูกโดยไม่ให้ราคาขึ้น — แบ่งซื้อทีละน้อย
   → ราคา Sideway หรือลงเล็กน้อย
   → Retail เห็นว่า trend ลง → ขายทิ้ง → Smart Money รับซื้อ

2. MANIPULATION (หลอก)
   Smart Money ดันราคาทะลุแนวรับเพื่อ:
   - เก็บ Stop Loss ของคนที่ Short
   - หลอกให้คนที่观望เข้าขายเพิ่ม
   → ราคาทะลุ = Liquidity Grab (Stop Hunt)

3. DISTRIBUTION (กระจาย)
   หลังจากได้ของถูก → Smart Money ดันราคาขึ้น
   → Retail เห็นราคาขึ้น → อยากได้ → เข้าซื้อ
   → Smart Money ขายของแพงให้ Retail = Distribution

4. MARKDOWN (ลง)
   หลังจากขายหมด → Smart Money ไม่มีแรงซื้อ → ราคาร่วง
   → Retail ที่ซื้อของแพง → ติดดอย
```

---

## 🔹 Part 2: Market Structure — ภาษากราฟที่ทุกคนต้องอ่านเป็น

### Swing Points — จุดเปลี่ยนของราคา

```


        HH────HH────HH────HH────HH
       /  \  /  \  /  \  /  \  /
      /    \/    \/    \/    \/
     HL    HL    HL    HL    HL

   HH = Higher High (จุดสูงสุดที่สูงขึ้น)
   HL = Higher Low (จุดต่ำสุดที่สูงขึ้น)
   → UPTREND (แนวโน้มขาขึ้น)




   LH────LH────LH────LH
  /  \  /  \  /  \  /  \
 /    \/    \/    \/    \
LL    LL    LL    LL

   LH = Lower High (จุดสูงสุดที่ต่ำลง)
   LL = Lower Low (จุดต่ำสุดที่ต่ำลง)
   → DOWNTREND (แนวโน้มขาลง)
```

### โครงสร้างตลาด 3 แบบ

#### 1. Uptrend (Bullish)
```
HH → HL → HH → HL → HH → HL
เงื่อนไข: จุดต่ำสุดสูงขึ้น (Higher Low) + จุดสูงสุดสูงขึ้น (Higher High)
ตลาด: แรงซื้อมากกว่าแรงขาย → ซื้อได้ (Buy)
```

#### 2. Downtrend (Bearish)
```
LH → LL → LH → LL → LH → LL
เงื่อนไข: จุดสูงสุดต่ำลง (Lower High) + จุดต่ำสุดต่ำลง (Lower Low)
ตลาด: แรงขายมากกว่าแรงซื้อ → ขายได้ (Sell)
```

#### 3. Sideway (Ranging / Consolidation)
```
Equal Highs ←→ Equal Highs
     ↑              ↑
  Equal Lows ←→ Equal Lows
ตลาด: แรงซื้อ = แรงขาย → เลือก direction รอ หรือ ข้าม
```

### วิธี Identify Trend — 3-Step

```
Step 1: ดู Higher High/Lower High
  - มี Higher High = trend ขึ้น
  - มี Lower High = trend ลง
  - ไม่มีทั้งคู่ = Sideway

Step 2: ดู Higher Low/Lower Low
  - มี Higher Low = trend ขึ้น
  - มี Lower Low = trend ลง

Step 3: สรุป
  - Higher High + Higher Low = Uptrend ✅
  - Lower High + Lower Low = Downtrend ✅
  - ไม่ชัด = Sideway → ใช้ confluence มากขึ้น
```

### EQH / EQL — Equal Highs/Lows (แนวต้าน/แนวรับ)

```
EQH (Equal Highs) = จุดสูงสุดเท่ากัน 2 ครั้งขึ้นไป
  → แนวต้านแข็ง → ถ้าทะลุ = ขึ้นแรง
  → ถ้าไม่ทะลุ = ลง

EQL (Equal Lows) = จุดต่ำสุดเท่ากัน 2 ครั้งขึ้นไป
  → แนวรับแข็ง → ถ้าทะลุ = ลงแรง
  → ถ้าไม่ทะลุ = ขึ้น
```

---

## 🔹 Part 3: Liquidity — หัวใจของ SMC

### Liquidity คืออะไร?

**Liquidity (สภาพคล่อง)** = กลุ่มของ Stop Loss / Limit Order ที่รวมกันอยู่ ณ ระดับราคาเดียว — Smart Money ต้องการ "ล่า" ตรงนี้เพื่อเข้า order ก้อนใหญ่

### ประเภทของ Liquidity

#### 1. Buy-Side Liquidity (BSL) — ด้านบน
```
ราคา
  ↑
  │   ═══ EQH / Swing High ═══  ← Stop Loss ของคน Short อยู่ตรงนี้
  │                              ← Limit Sell Order อยู่ตรงนี้
  │
  │     ราคาปัจจุบัน
  │
  │   ═══ Swing Low ═══
  ↓
```

**BSL = Liquidity ด้าน Buy:** Stop Loss ของคนที่ Short + Limit Sell Order
→ Smart Money ดันราคาขึ้นไปกิน BSL ก่อนลงจริง

#### 2. Sell-Side Liquidity (SSL) — ด้านล่าง
```
  ↑
  │   ═══ Swing High ═══
  │
  │     ราคาปัจจุบัน
  │
  │   ═══ EQH / Swing Low ═══  ← Stop Loss ของคน Long อยู่ตรงนี้
  │                              ← Limit Buy Order อยู่ตรงนี้
  ↓
```

**SSL = Liquidity ด้าน Sell:** Stop Loss ของคนที่ Long + Limit Buy Order
→ Smart Money ดันราคาลงไปกิน SSL ก่อนขึ้นจริง

### Liquidity Grab (Stop Hunt)

```
1. ราคาอยู่ในแนวโน้มขึ้น
2. จู่ๆ ราคาทะลุแนวรับลงมา — ดูเหมือน trend จะกลับ
3. Retail ที่ Long ตื่น → โดน Stop Loss (SL ใต้แนวรับ)
4. ราคากลับขึ้นไปต่อทันที — Smart Money ได้ order ก้อนใหญ่
5. → Liquidity Grab เสร็จสิ้น → ราคาวิ่งต่อ

เปรียบ: Smart Money เหยียบหนู (Stop Loss) แล้วค่อยเดิน
```

### วิธีสังเกต Liquidity Grab

```
ลักษณะของ Liquidity Grab:
1. ราคาทะลุ Swing Point (High/Low) แต่ไม่ปิดต่ำกว่า
2. มี wick ยาวทะลุ → กลับมาปิดในโซนเดิม
3. Volume พุ่งตอนทะลุ → กลับมาปกติ
4. ทะลุแล้วกลับทันทีใน 1-2 candle

ลักษณะของ Breakout จริง (ไม่ใช่ Grab):
1. ราคาทะลุ + ปิดเหนือ/ต่ำกว่า
2. 2-3 candle ติดกันในทิศที่ทะลุ
3. Volume พุ่งและยังสูงต่อเนื่อง
4. ราคาไม่กลับมาเขตเดิม
```

---

## 🔹 Part 4: FVG (Fair Value Gap) — ช่องว่างของราคา

### FVG คืออะไร?

**FVG (Fair Value Gap)** หรือ Imbalance คือช่องว่างระหว่าง candle 3 แท่งที่ราคาวิ่งแรงเกินไป — เกิดจากคำสั่งซื้อ/ขายที่ไม่สมดุล

```
    ┌─────┐
    │  3  │
    │     │────  ← ช่องว่าง = FVG
    │     │
────│─────│────
│   │  1  │
│ 2 │     │
│   │     │
└───┴─────┘

Candle 1: เปิดสูง → ปิดต่ำ (Bullish)
Candle 2: เปิดต่ำ → ปิดสูง (Bearish — wick บน/ล่าง)
Candle 3: เปิดสูง → ปิดสูง (Bullish — gap ระหว่าง Low 1 กับ High 3)

FVG = จุดที่ราคาวิ่งผ่านไปเร็วเกินไป → ราคามักกลับมาเติม (rebalance)
```

### วิธีการ Identify FVG

```
หา FVG:
1. ดู 3 candles เรียงกัน
2. เช็คว่า High ของแท่ง 1 < Low ของแท่ง 3 (Bullish FVG)
3. หรือ Low ของแท่ง 1 > High ของแท่ง 3 (Bearish FVG)
4. ถ้ามี gap → FVG เกิดขึ้น → ราคามักกลับมาเติม

ประเภทของ FVG:
- Bullish FVG: Gap ด้านขึ้น (ราคาจะกลับมาเติมก่อนขึ้นต่อ)
- Bearish FVG: Gap ด้านลง (ราคาจะกลับมาเติมก่อนลงต่อ)
```

### ประเภทของ FVG ตาม Timeframe

| TF | ชื่อเรียก | ความสำคัญ |
|----|----------|----------|
| M15 | Micro FVG | เติมเร็ว 1-2 ชม., ใช้สำหรับ Scalp |
| H1 | Swing FVG | เติมใน 4-8 ชม., ใช้ Swing Trade |
| H4 | Daily FVG | เติมใน 1-3 วัน, แนวรับ/แนวต้านแข็ง |
| D1 | Major FVG | เติมใน 1-4 สัปดาห์, จุดกลับตัวใหญ่ |

### Confluence ของ FVG

```
FVG อย่างเดียว = 40% reliability
FVG + OB ซ้อน     = 70% reliability
FVG + OB + MSS    = 90%+ reliability
FVG หลาย TF ซ้อน  = "FVG หนา" — reliability สูงมาก
```

### FVG vs OB — ต่างกันยังไง?

| | FVG (Fair Value Gap) | OB (Order Block) |
|--|---------------------|-----------------|
| คืออะไร | ช่องว่างที่ราคาวิ่งไม่สมดุล | แท่งเทียนก่อน impulse |
| วิธีหา | Gap ระหว่าง candle 1 กับ 3 | แท่งสุดท้ายก่อนราคาวิ่งแรง |
| Reliability | ปานกลาง | สูง (ถ้าแท่งใหญ่) |
| ใช้ยังไง | แนวรับ/แนวต้าน + รอเติม | Entry zone สำหรับเข้า |

---

## 🔹 Part 5: Iceberg Orders & Order Flow

### Iceberg Orders

Smart Money ไม่สามารถส่ง order ก้อนใหญ่ (100,000 lots) ในครั้งเดียวได้ — เพราะจะทำให้ราคากระโดดทันที

```
❌ ส่งก้อนใหญ่ครั้งเดียว:
  Order: SELL 100,000 lots → ราคาร่วงทันที → Smart Money ได้ราคาแย่

✅ แบ่งเป็น Iceberg:
  Order: SELL 100,000 lots → แบ่งเป็น 100 ชิ้น ชิ้นละ 1,000 lots
  → ส่งทีละชิ้น → ราคาไม่กระโดด → Smart Money ได้ราคาดี
```

**Iceberg Order = คำสั่งซื้อ/ขายก้อนใหญ่ที่ซ่อนบางส่วนไว้**

### Absorption

เมื่อราคาไม่เคลื่อนที่ทั้งที่มี order มาก — แสดงว่ามีคนรับ(order ฝั่งตรงข้าม) อยู่

```
ราคา Sideway ที่แนวรับ แต่มี Volume สูงผิดปกติ
→ Smart Money กำลังรับซื้อ (absorption)
→ ราคากำลังจะขึ้น (accumulation phase)
```

### Order Flow Concepts

| Concept | ความหมาย | สัญญาณ |
|---------|----------|--------|
| Absorption | คำสั่งซื้อ/ขายก้อนใหญ่ถูกดูดซับ | ราคา Sideway + Volume สูง |
| Exhaustion | คำสั่งซื้อ/ขายหมดแรง | Volume ลด + Momentum อ่อน (Divergence) |
| Rejection | ราคาถูกปฏิเสธที่ระดับ | Wick ยาว + ปิดกลับ |
| Acceptance | ราคายอมรับระดับ | ปิดเหนือ/ใต้ + ไปต่อ |

---

## 🔹 Part 6: Complete SMC Framework

### ความสัมพันธ์ขององค์ประกอบ SMC ทั้งหมด

```
                    ┌──────────────────┐
                    │   SMART MONEY    │
                    │    CONCEPTS      │
                    └────────┬─────────┘
                             │
          ┌──────────────────┼──────────────────┐
          ▼                  ▼                  ▼
   ┌────────────┐    ┌──────────────┐    ┌────────────┐
   │  LIQUIDITY │    │   STRUCTURE  │    │   IMBALANCE│
   │  (BSL/SSL) │    │(HH/HL/LH/LL) │    │(OB/FVG)    │
   └────────────┘    └──────────────┘    └────────────┘
          │                  │                  │
          │         ┌────────┴────────┐         │
          │         ▼                 ▼         │
          │   ┌──────────┐    ┌──────────┐      │
          │   │  MSS /   │    │DIVERGENCE│      │
          │   │  CHoCH   │    │(RSI/etc) │      │
          │   └──────────┘    └──────────┘      │
          │         │                 │         │
          └─────────┼─────────────────┼─────────┘
                    ▼                 ▼
            ┌───────────────────────────────┐
            │      CONFLUENCE 3             │
            │  LIQUIDITY + MSS + IMBALANCE  │
            └───────────────┬───────────────┘
                            │
                            ▼
                    ┌───────────────┐
                    │   ENTRY       │
                    │  (A / B / C)  │
                    ├───────────────┤
                    │  SL / TP      │
                    ├───────────────┤
                    │  CHECKLIST    │
                    └───────────────┘
```

### วงจรการทำงานของ Smart Money (ครบ)

```
Phase 1: Accumulation
  - ราคา Sideway
  - Volume ต่ำ → ค่อยๆ สูง
  - Smart Money สะสม order

Phase 2: Manipulation (Liquidity Grab)
  - ราคาทะลุแนวรับ/แนวต้านเพื่อเก็บ SL
  - Wick ยาว → ปิดกลับ
  - Volume พุ่ง

Phase 3: Distribution (Impulse)
  - ราคาวิ่งแรงในทิศทางจริง
  - สร้าง Order Block + FVG
  - MSS/CHoCH เกิดขึ้น

Phase 4: Retest (Entry)
  - ราคากลับมาเทส OB/FVG
  - = โอกาสเข้า trade

Phase 5: Continuation / Reversal
  - ราคาไปต่อ (ถ้า trend ยัง)
  - หรือเริ่ม accumulation ใหม่ (ถ้าจบ)
```

---

## 🔹 คำศัพท์ SMC ฉบับสมบูรณ์ (Glossary)

| คำศัพท์ | ตัวย่อ | ความหมาย |
|---------|--------|----------|
| Smart Money | — | เงินสถาบัน, เจ้ามือ, คนที่อยู่เบื้องหลังราคา |
| Retail Trader | — | เทรดเดอร์รายย่อย — คุณและผม |
| Market Structure | — | โครงสร้างตลาด (HH/HL/LH/LL) |
| Higher High | HH | จุดสูงสุดที่สูงขึ้น — สัญญาณ uptrend |
| Higher Low | HL | จุดต่ำสุดที่สูงขึ้น — สัญญาณ uptrend |
| Lower High | LH | จุดสูงสุดที่ต่ำลง — สัญญาณ downtrend |
| Lower Low | LL | จุดต่ำสุดที่ต่ำลง — สัญญาณ downtrend |
| Equal High | EQH | จุดสูงสุดเท่ากัน — แนวต้าน |
| Equal Low | EQL | จุดต่ำสุดเท่ากัน — แนวรับ |
| Change of Character | CHoCH | จุดที่ market structure เปลี่ยน |
| Market Structure Shift | MSS | เหมือน CHoCH — โครงสร้างตลาดเปลี่ยนทิศ |
| Internal CHoCH | — | เปลี่ยนโครงสร้างย่อย — trend หลักยังอยู่ |
| External CHoCH | — | เปลี่ยนโครงสร้างหลัก — trend เปลี่ยน |
| Order Block | OB | แท่งเทียนก่อน impulse — จุดที่ Smart Money เข้า |
| Fair Value Gap | FVG | ช่องว่างที่ราคาวิ่งไม่สมดุล — แนวรับ/แนวต้าน |
| Imbalance | — | เหมือน FVG — ความไม่สมดุลของ order |
| Liquidity | — | สภาพคล่อง — Stop Loss + Limit Order |
| Buy-Side Liquidity | BSL | SL ของคน Short + Sell Limit — อยู่ด้านบน |
| Sell-Side Liquidity | SSL | SL ของคน Long + Buy Limit — อยู่ด้านล่าง |
| Liquidity Grab | — | การล่า Stop Loss — ราคาทะลุแล้วกลับ |
| Stop Hunt | — | เหมือน Liquidity Grab |
| Inducement | — | ราคาที่ถูกออกแบบมาให้เข้าใจผิด |
| Accumulation | — | Smart Money สะสม order (ซื้อของถูก) |
| Distribution | — | Smart Money กระจาย order (ขายของแพง) |
| Manipulation | — | Smart Money หลอก Retail |
| Markup | — | ราคาขึ้นหลังจาก Accumulation |
| Markdown | — | ราคาลงหลังจาก Distribution |
| Impulse Move | — | การเคลื่อนที่แรงในทิศทางจริง |
| Pullback | — | การย่อตัวชั่วคราว — trend ยังไปต่อ |
| Retest | — | ราคากลับมาเทสโซนเดิมอีกครั้ง |
| Rebalancing | — | ราคากลับมาเติม FVG |
| Volume | Vol | ปริมาณการซื้อขาย |
| Absorption | — | คำสั่งก้อนใหญ่ถูกดูดซับโดยไม่เปลี่ยนราคา |
| Iceberg Order | — | คำสั่งก้อนใหญ่ที่ซ่อนบางส่วน |
| Divergence | — | ราคากับโมเมนตัมไปคนละทาง |
| Confluence | — | จำนวนปัจจัยที่หนุนการเทรดครั้งนี้ |
| Risk to Reward Ratio | RR | อัตราส่วนความเสี่ยงต่อผลตอบแทน |

---

## 🔹 Links

- Related: [[SMC Concept 1 - Entry A B C]], [[SMC Concept 4+5 MSS]], [[Pre-Trade Checklist]]
- ถัดไป: [[_Curriculum]]
