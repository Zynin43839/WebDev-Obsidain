# สรุปหนังสือ Building Winning Trading Systems with TradeStation

## ภาพรวมของหนังสือ

หนังสือเล่มนี้เขียนโดย George C. Pruitt ครอบคลุมทุกแง่มุมของการสร้างระบบเทรดแบบมีกลยุทธ์ (Strategy Trading) ตั้งแต่หลักการพื้นฐาน ไปจนถึงการออกแบบ ทดสอบ ปรับแต่ง และบริหารจัดการธุรกิจเทรดอย่างมืออาชีพ โดยใช้ TradeStation เป็นเครื่องมือหลัก

---

## บทที่ 1: หลักการของเทรดเดอร์ที่ประสบความสำเร็จ

### 1.1 อย่าพยายามทำนายอนาคต
- **ไม่มีใครรู้ว่าตลาดจะไปทางไหน** ไม่มีกูรูหรือผู้เชี่ยวชาญคนไหนสามารถทำนายทิศทางตลาดได้อย่างแม่นยำอย่างสม่ำเสมอ แม้แต่เทคนิค Fibonacci, Elliott Wave หรือ cycle analysis ก็ไม่สามารถทำนายได้
- **ไม่มีใครรู้ว่าตลาดจะเคลื่อนไหวเมื่อไหร่** แม้จะรู้ทิศทาง แต่จังหวะเวลาไม่มีใครคาดเดาได้ การพยายามทำนายจังหวะทำให้พลาด big move บ่อยครั้ง
- **กูรูไม่ใช่พ่อมด** พวกเขาไม่ได้ทำกำไรจากการทำนาย แต่ทำกำไรจากการเทรดที่มีวินัย กำไรมาจาก trading discipline ไม่ใช่ prediction

### 1.2 กำไรมาจาก Money Management และ Risk Control
- ร้านอาหารที่ดีไม่ได้สำเร็จเพราะอาหารอร่อยที่สุด แต่เพราะ cash management ดี เช่นเดียวกับการเทรด — indicator ที่ดีไม่ได้รับประกันกำไร แต่การจัดการเงินที่ดีคือกุญแจสู่ความสำเร็จ
- เทรดเดอร์ที่ประสบความสำเร็จไม่กังวลเรื่องทิศทางตลาด แต่กังวลเรื่อง risk ต่อเทรด, ตำแหน่ง stop loss, และจำนวน capital ที่เสี่ยง

### 1.3 ควรอยู่ร่วมกับตลาด (Be in Harmony)
- ทำกำไรเมื่ออยู่ฝั่งเดียวกับตลาด — long เมื่อตลาดขึ้น, short เมื่อตลาดลง
- **อย่าต่อสู้กับตลาด** เพราะนอกจากจะเสียเงินแล้ว ยังกระทบจิตใจ ทำให้ยืนยันความคิดของตัวเองผิดๆ ต่อไป
- เปรียบเหมือนการพายเรือ — พายตามกระแสน้ำง่ายกว่าพายทวนน้ำมาก

### 1.4 ปล่อยให้ตลาดบอกว่าต้องทำอะไร
- ยอมรับว่าเราไม่สามารถควบคุมตลาดได้ สิ่งเดียวที่ควบคุมได้คือการ execute คำสั่งซื้อขาย
- ปล่อยให้ strategy เป็นตัวตัดสินใจ ไม่ใช่ความคิดเห็นส่วนตัว
- **ตลาดให้เงินและตลาดเอาเงินคืน** — เป้าหมายคือสร้าง strategy ที่ให้มากกว่าที่เอาไป

### 1.5 มี Time Horizon ที่เหมาะสม
- อย่าคิดว่าจะรวยเร็ว — การเทรดต้องใช้เวลาพิสูจน์ผลลัพธ์
- ปู่ของผู้เขียนเคยพูดว่า "คุณไม่มีวันหมดตัวถ้าเอาชนะกำไร" — แต่จริงๆ แล้วผิด คุณสามารถหมดตัวได้ถ้าปิดกำไรก่อนเวลาอันควร
- ให้ strategy มีเวลาทำงานของมัน อย่า cut profit ก่อนเวลา

### 1.6 ยอมรับการขาดทุนเป็นค่าใช้จ่ายในการทำธุรกิจ
- เทรดเดอร์ที่ดีที่สุดในโลกก็ขาดทุนมากกว่า 50% ของการเทรดเช่นกัน
- การขาดทุนเป็นค่าใช้จ่ายปกติ เหมือน scrap ในโรงงาน manufacturing

### 1.7 ใช้สถิติย้อนหลัง
- การทดสอบย้อนหลังให้ความมั่นใจในช่วงที่ strategy ขาดทุน — รู้ว่า max loss ที่เคยเกิดคือเท่าไหร่ ช่วยให้ผ่าน drawdown ได้

### 1.8 อย่าเทรดเพื่อเงิน
- คนที่ประสบความสำเร็จทุกคนรักในสิ่งที่ทำ เทรดเดอร์ที่ประสบความสำเร็จก็เช่นกัน — รักการเทรด ไม่ใช่แค่รักเงิน

### 1.9 ให้ความสำคัญกับ Execution
- วิเคราะห์ตลาดและออกแบบ strategy นอกเวลาเทรด เวลานาฬิกาทำงาน ให้ execute อย่างเดียว อย่าตั้งคำถาม

### 1.10 Always Be In the Market
- Trend trader ต้องไม่พลาด big move วิธีเดียวที่จะไม่พลาดคืออยู่ในตลาดเสมอ จัดการ cash flow ระหว่างรอ big move

### 1.11 Buy High, Sell Low (ไม่ใช่ Buy Low, Sell High)
- 95% ของเทรดเดอร์ซื้อถูกขายแพง — และสูญเงิน กลยุทธ์ที่ประสบความสำเร็จต้องทำตรงข้ามกับสัญชาตญาณมนุษย์

---

## บทที่ 2: เส้นทางสู่เทรดที่ประสบความสำเร็จ

### 2.1 ประเภทของเทรดเดอร์ 4 ขั้นตอน

#### Discretionary Trader
- ตัดสินใจด้วย intuition, hot tips, และข้อมูลที่ไม่ใช่เชิงปริมาณ
- ไม่มีกฎเกณฑ์ที่ชัดเจน เปลี่ยนวิธีเทรดไปเรื่อยตามอารมณ์
- คิดว่าตัวเองฉลาดกว่าคนอื่น — คิดว่า "ทำเงินง่ายแค่ฉลาดกว่าคนอื่น"
- **ผลลัพธ์**: ไม่เคยเห็นใครทำกำไรได้จากการเทรดแบบนี้ในระยะยาว

#### Technical Trader
- เริ่มใช้ indicators (Moving Average, RSI, Stochastic, MACD, ADX, CCI ฯลฯ)
- เข้าสู่ "Indicator Fascination" — ค้นหา Holy Grail indicator ที่จะทำกำไรได้เสมอ
- ยังไม่มีวินัยจริงจัง ยังคง override signals ด้วยความคิดเห็นส่วนตัว
- เริ่มเข้าใจความสำคัญของ stop loss

#### Strategy Trader
- ใช้ objective, quantifiable data และ back testing
- เข้าใจว่าไม่มีใครทำนายตลาดได้ และต้องมี conviction จากผลการทดสอบย้อนหลัง
- สร้าง strategy ด้วย TradeStation และ严格执行

#### Complete Strategy Trader
- เชี่ยวชาญ cash management, pyramiding, position sizing
- เทรดหลายตลาดและหลาย strategy
- เปรียบเหมือนนักธุรกิจที่จัดการองค์กรการเงินอย่างมืออาชีพ

### 2.2 หลักฐานเชิงประจักษ์: Objective Linear Model ดีที่สุด
จากการศึกษาวิจัย 9 ด้าน 3 วิธีตัดสินใจ (Intuitive, Subjective Linear, Objective Linear):
- **Intuitive (Discretionary)**: ค่าเฉลี่ย 0.33
- **Subjective Linear (Technical)**: ค่าเฉลี่ย 0.39 (ดีกว่าเล็กน้อย)
- **Objective Linear (Strategy)**: ค่าเฉลี่ย 0.64 — **ดีที่สุดอย่างมีนัยสำคัญ**
- ในด้านการเปลี่ยนแปลงราคาหุ้น: Intuitive=0.23, Subjective=0.29, **Objective=0.80**

---

## บทที่ 3: ตลาด, กลยุทธ์ และ Time Frame

### 3.1 ประเภทตลาด 3 ประเภท

#### Trending Market (ตลาดแนวโน้ม)
- การเพิ่มหรือลดลงอย่างต่อเนื่องของราคา
- ตัวอย่าง: Coca-Cola ตั้งแต่ปี 1994, Swiss Franc  downtrend ปี 1996-97
- ตัวชี้วัด: Moving Average Crossover

#### Directionless Market (ตลาดไม่มีทิศทาง)
- ราคาเคลื่อนไหวขึ้นลงเล็กน้อยแบบ sideway
- ตัวอย่าง: Caterpillar ปี 1996 (ซื้อขายระหว่าง 31-37)
- ตัวชี้วัด: Stochastic Oscillator (buy ที่ oversold, sell ที่ overbought)

#### Volatile Market (ตลาดผันผวน)
- ราคากระโดดอย่างรุนแรง (volatility expansion)
- ตัวอย่าง: S&P futures ช่วง gap openings
- ตัวชี้วัด: Gap openings, daily range expansion

### 3.2 กลยุทธ์ 3 ประเภท

#### Trend Following
- **จุดมุ่งหมาย**: จับ big move
- ต้องอยู่ในตลาดเสมอ (มี position เสมอ หรือมือ market วางไว้)
- Win rate ต่ำ (30-45%) แต่ profit จาก big move ชดเชย
- **ปัญหา**: Whipsaw จำนวนมากในช่วง sideway, drawdown นาน
- ตัวอย่าง: MA Crossover Disney — ชนะ 39% เท่านั้น

#### Support & Resistance (S/R)
- ซื้อเมื่อราคาต่ำ ขายเมื่อราคาสูง (counter-trend)
- Win rate สูง (60-70%) แต่ profit ต่อเทรดจำกัด
- ใช้ Stochastic, RSI เป็นตัวชี้วัด overbought/oversold
- **ปัญหา**: ไม่ได้จับ big trend move, ยากต่อการ sustain profit

#### Volatility Expansion
- จับช่วงราคาที่ผันผวนอย่างรุนแรง (gap, range expansion)
- Win rate สูง, เทรดสั้น, อยู่นอกตลาดนาน
- เหมาะกับ S&P futures ที่มี volatility สูง
- ตัวอย่าง: Gap Open strategy — exit วันถัดไป

### 3.3 เปรียบเทียบ Time Frame

| Time Frame | ข้อดี | ข้อเสีย |
|:---------:|:------|:--------|
| Weekly | Smoother, ลด whipsaw, กำไรดีกว่า daily | ต้องมีวินัยสูง, ดูตลาดสัปดาห์ละครั้ง |
| Daily | นิยมมากที่สุด, สะดวก | Price noise มาก |
| Intra-day | กำไรสูงสุดต่อหน่วยเวลา, competition น้อย | ต้อง full-time, commission สูง, ต้องมี software/data |

**ตัวอย่าง Channel Breakout IBM**: Weekly (10-wk) outperformed Daily (50-day) ในทุก category — กำไรสูงกว่า, trades น้อยกว่า, drawdown ต่ำกว่า

---

## บทที่ 4: Profile ของ Strategy ที่ชนะ

### 4.1 แนวคิด Set-Up และ Entry (สำคัญมาก!)

**Set-Up** = เงื่อนไขที่บ่งบอกว่าเทรดกำลังจะเกิดขึ้น (บอกทิศทาง)
- ตัวอย่าง趋势-following: MA cross up, ADX trending
- ตัวอย่าง S/R: RSI oversold, Stochastic crossover

**Entry** = สัญญาณที่ใช้เข้าตลาดจริง (ยืนยันทิศทาง)
- ต้องทำตาม **Entry Rule 1**: ราคายืนยันทิศทางของ Set-Up ก่อนเข้า
- ต้องทำตาม **Entry Rule 2**: ต้องรับประกันว่าจะไม่พลาด big move

### 4.2 ประเภท Order สำหรับ Entry

| Order Type | ข้อดี | ข้อเสีย |
|:----------:|:------|:--------|
| Market | เข้าง่าย | ไม่ยืนยันทิศทาง (ผิด Rule 1) |
| Stop Close Only | ยืนยันทิศทาง | ต้องรอปิดตลาด |
| Stop | ดีที่สุด — รับประกันเข้าตลาดทุกครั้ง | อาจมี slippage |
| Limit | ได้ราคาดี | ไม่ยืนยันทิศทาง + อาจไม่ได้ fill (ผิดทั้ง Rule 1 & 2) |

### 4.3 ตัวอย่างการพัฒนา Strategy: Coca-Cola (KO)

**ขั้นตอนที่ 1** — Set-Up เท่านั้น (MA turns): กำไร $0.48/trade — ยังไม่ดี

**ขั้นตอนที่ 2** — เพิ่ม Entry (%R < 20): กำไรเพิ่มเป็น $3.01/trade, Win% 32%→67%, ROMID 587%→1088%

**ขั้นตอนที่ 3** — เปลี่ยนเป็น Bar Breakout: MAXID ลดลง, ROMID เพิ่มเป็น 3,500%+

**ขั้นตอนที่ 4** — แก้ปัญหา missed big move: เพิ่ม failsafe entry (50-week high/low breakout) — ไม่พลาด big move อีกต่อไป

**บทเรียน**:  Combine Set-Up + Entry = ผลลัพธ์ที่ดีกว่าใช้แค่อย่างใดอย่างหนึ่งมาก

---

## บทที่ 5: ศิลปะการออกแบบ Strategy — ภาคทฤษฎี

### 5.1 ขั้นตอน 10 ขั้นตอนของการออกแบบ Strategy

1. **เลือกประเภทตลาด** (trending, directionless, volatile) — พิจารณาจิตวิทยาก่อนผลกำไร
2. **เลือก Time Frame** (intra-day, daily, weekly)
3. **ออกแบบและ plot indicator** — หลีกเลี่ยง indicators ที่มาจากข้อมูลราคาเดียวกัน
4. **เขียน ShowMe Study** — visual สำคัญมาก ช่วยป้องกัน confirmation bias
5. **ปรับปรุงไอเดียผ่าน ShowMe Study**
6. **เขียน Alert เพื่อ simulate trading**
7. **ออกแบบ Strategy** (Set-Up + Entry + Exits + Stops)
8. **ทดสอบและ optimize**
9. **Implement และเทรด**
10. **ปรับปรุงจากประสบการณ์เทรดจริง**

### 5.2 หลักสำคัญในการเลือก Indicator

- **ใช้ indicator ในทางที่ไม่ conventional** — ถ้าใช้ตามปกติจะไม่ทำกำไร (เพราะ 95% ของเทรดเดอร์ก็ใช้แบบเดียวกัน)
- **อย่าใช้ indicator ที่Derived จากข้อมูลราคาเดียวกัน** — จะซ้ำซ้อนกัน (RSI, Stochastic, CCI ดูคล้ายกันเพราะมาจาก close price)
- **Indicator ควร simple** — ความซับซ้อนแปรผกผันกับความมีประโยชน์
- **ใช้ oscillator สร้างจาก indicator สองตัว** — เช่น ความต่างระหว่าง MA สองตัว

### 5.3 การใช้ Stop Loss

- **Money Management Stop**: ป้องกัน capital, ไม่ควร hit บ่อยเกิน 10%
- **Trailing Stop**: ป้องกันกำไรเมื่อเทรดเข้าสู่แดนบวก
- **Stops protect capital; Exits respond to market conditions**
- ถ้า set-up และ entry ดี stop อาจไม่จำเป็น

### 5.4 ไม่มี Holy Grail
- ไม่มี indicator เดียวที่จะทำกำไรได้ 100%
- เลือก indicator ที่เหมาะกับ personality และ type of strategy
- ทำกำไรได้จริงคือ trader ที่ดี ไม่ใช่ indicator ที่ดี

---

## บทที่ 6: ศิลปะการออกแบบ Strategy — ภาคปฏิบัติ

### 6.1 ตัวอย่าง: Up Key Reversal (UKR) Volatility Strategy

**ขั้นตอนการพัฒนา**:

1. **UKR Breakout (ไม่มี filter)**: กำไรน้อย, avg loss > avg win
2. **เพิ่ม 10-day low filter**: กำไรเพิ่มขึ้นมาก (market ต้องอยู่ใน downtrend ก่อน)
3. **เปลี่ยน exit เป็น 5-day close**: ทำกำไรได้จริงแล้ว!
4. **combine ทั้งสอง**: กำไรเพิ่มขึ้นอีก (synergy)
5. **Optimize**: parameters ที่ดีที่สุด = 10-day low + 8-day exit
6. **เพิ่ม $5,000 MMS**: แย่ลง — stop รบกวน market action
7. **เพิ่ม $5,000 profit target**: Win% เพิ่มเป็น 68%
8. **ลบวันหลัง crash 1987**: กำไรเพิ่มขึ้นอีก
9. **ทดสอบโดยไม่มี stop**: กำไรสูงสุด 75% profitable, drawdown ต่ำกว่า

### 6.2 บทเรียนสำคัญ
- กลยุทธ์ที่ทำกำไรสูงสุดมักยากที่สุดในการเทรด (profit target + no stop)
- **ต้องเลือกระหว่างกำไรมากกับเทรดง่าย** — trade-off ที่ trader ทุกคนต้องเผชิญ
- ลอง paper trade ก่อนเทรดจริงเสมอ

---

## บทที่ 7: Optimization — ดาบสองคม

### 7.1 Optimization คืออะไร?
- ทดสอบ parameter ต่างๆ เพื่อหาค่าที่ดีที่สุดจากข้อมูลย้อนหลัง
- **ไม่ใช่การสร้าง strategy** แต่เป็นการ refine strategy ที่ทำกำไรอยู่แล้ว

### 7.2 ข้อถกเถียง
- **ฝ่ายค้าน**: ข้อมูลในอดีตไม่เหมือนอนาคต → over-optimization = curve fitting
- **ฝ่ายสนับสนุน**: ข้อมูลย้อนหลังเป็นสิ่งเดียวที่เรามี ทุกการลงทุนล้วนอ้างอิง past history
- **วิธีรู้ว่า over-optimized**: ถ้า strategy พลาด big move ที่ออกแบบมา → curve-fitted

### 7.3 วิธีป้องกัน Over-Optimization

1. **สร้างจาก market theory** — ต้องอธิบายได้ว่าทำไม indicator นี้ทำงาน
2. **Keep it simple** — signals น้อย = curve-fitting น้อย
3. **ใช้ Set-Up + Entry**
4. **ทดสอบบนหลาย securities** — ถ้าใช้ได้แค่ตัวเดียว = suspect
5. **ดู surrounding parameters** — ถ้า parameter ข้างๆ ไม่กำไร = suspect
6. **Forward + Backward testing** — optimize ช่วงหนึ่ง ทดสอบอีกช่วงหนึ่ง

### 7.4 ตัวอย่าง: HH/LL Channel Breakout (Dow Jones)

- **50/50 parameters**: กำไร $4,642 จาก Dow 7,200 จุด (จับ 65% ของ move)
- **Optimized 20/50**: กำไร $6,146 — ดีกว่าชัดเจน
- **ทดสอบแยกทศวรรษ**:
  - 1970s: 20/50 กำไร $325 (sideways market — trend strategy ทำเงินได้!)
  - 1980s: parameters อื่นดีกว่า ( LL=15)
  - 1990s: 20/50 กำไร $5,084
- **สรุป**: 20/50 กำไรทุกทศวรรษ → robust strategy

---

## บทที่ 8: วิทยาศาสตร์การประเมิน Strategy

### 8.1 Financial Evaluation

#### Risk-Free Rate of Return
- เปรียบเทียบกับ T-Bill rate
- หุ้น: ต้องกำไรมากกว่า T-Bill 2 เท่า
- Futures: ต้องกำไรมากกว่า T-Bill 4 เท่า
- ถ้า T-Bill = 10% → strategy futures ต้องกำไรมากกว่า 40%/ปี

#### ROMID (Return on Maximum Intra-day Drawdown)
- MAXID = จำนวนเงินสูงสุดที่ต้องสูญเสียระหว่างทางไป equity high ใหม่
- ROMID = Net Profit / MAXID — เป็น ROI ที่ใช้เปรียบเทียบ strategy ต่างๆ
- ไม่รวม margin (เพราะ margin ควรอยู่ใน T-Bills)

### 8.2 Statistical Evaluation — 4 ตัวเลขหลัก

| ตัวเลข | เกณฑ์ | หมายเหตุ |
|:-------:|:------|:---------|
| Total Number of Trades | >= 30 | ยิ่งมากยิ่งดี, น้อยกว่า 30 = suspect |
| Average Profit/Trade | >= $200 | หลัง slippage+commission, มี margin สำหรับ slippage เพิ่ม |
| Largest Winning Trade | <= 50% gross profit | ถ้า profit มาจากเทรดเดียว = เสี่ยง |
| Profit Factor | >= 2.0 | Gross Profit / Gross Loss = risk/reward ratio |

### 8.3 Personal Evaluation

- **MAXID**: ต้องรับไหวทั้งทางการเงินและจิตใจ
- **Percent Profitable**: แล้วแต่ personality — 35% ก็ได้ถ้ามั่นใจใน long-term
- **Maximum Consecutive Losers**: ต้องรู้ว่ารับได้กี่ losing trades ติดต่อกัน

### 8.4 วิธีรู้ว่า Strategy หยุดทำงานแล้ว

- **Trend strategy**: พลาด big move ที่ออกแบบมา → bust
- **Volatility strategy**: win rate ลดลงอย่างมีนัยสำคัญ, ไม่จับ volatility pops → bust
- **Drawdown เกิน 2 เท่า ของ historical MAXID** → หยุดเทรดและตรวจสอบ

---

## บทที่ 9: การเทรดเป็นธุรกิจ

### 9.1 Trading เป็น Low Barrier Business
- เทรดง่าย (computer + broker + เงิน) → competition สูง
- ต้องฉลาดกว่า วินัยมากกว่า หรือสร้างสรรค์กว่าคนส่วนใหญ่

### 9.2 Product vs. Business
- **Product** = indicator และ strategy
- **Business** = cash flow management หลังจากมี strategy แล้ว
- ร้านอาหารที่ดีไม่ใช่แค่อาหารอร่อย แต่ต้องจัดการต้นทุน พนักงาน cash flow ได้ดี
- เช่นกัน: indicator ที่ดีไม่ได้ทำให้เทรดเดอร์สำเร็จ ต้องมี cash management

### 9.3 Contribution Analysis

```
Revenue (Gross Trading Profit) - Variable Costs (Slippage + Commission) = Contribution
Contribution - Fixed Costs (Office Expenses) = Net Profit
```

- **Variable Costs**: Commission (คงที่), Slippage (แปรผัน)
- **Fixed Costs**: ค่าเช่า, software, data, books, seminars
- Slippage สำคัญมาก — Strategy A (125 trades) vs B (29 trades) เท่ากันก่อนค่าใช้จ่าย แต่หลัง $100/trade: A กำไร $12,500 vs B กำไร $22,098

### 9.4 Cash Flow Management — หัวใจของธุรกิจเทรด

#### ANP Pyramid (Accumulated Net Profit)
- เทรด 1 สัญญาตอนเริ่มต้น (ใช้ทุนตัวเอง)
- เมื่อกำไรสะสมพอ เพิ่มจำนวนสัญญา (เสี่ยงด้วยกำไร ไม่ใช่ทุนตัวเอง)
- สูตร: `Number of contracts = (ANP x %Risk) / MMS per contract`

#### ตัวอย่าง Swiss Franc Dual MA Strategy
| Parameter | 1 Contract | ANP Pyramid + Stops |
|:---------:|:----------:|:-------------------:|
| Net Profit | $90,450 | $898,312 |
| Avg Trade | $838 | $8,318 |
| Profit Factor | 1.94 | 3.65 |
| ROMID | 810% | 1,060% |
| MAXID | $11,163 | $84,713 |

#### Risk Control เมื่อใช้ ANP Pyramid
1. **Money Management Stop** ($4,000): จำกัด loss ต่อสัญญา
2. **Breakeven Stop** ($1,000 profit → move to breakeven): ป้องกันไม่ให้เทรดกำไรกลายเป็นเทรดขาดทุน
3. **Trailing Stop** ($3,500): ป้องกันกำไรเมื่อเทรดเข้าสู่แดนบวก

### 9.5 บทสรุปหลักการ Cash Management

1. Optimize strategy parameters ก่อน (ถ้าเหมาะสม)
2. จำกัด risk ต่อเทรดด้วย Money Management Stop
3. ทดสอบ %ANP risk (10%-100%)
4. กำหนด %ANP ที่จะ risk → คำนวณจำนวนสัญญา
5. ทดสอบ risk control stops กับ ANP Pyramid

---

## หลักการสำคัญสำหรับ Algo Trading

### 1. ทุกอย่างต้องเป็น Rule-Based
- ไม่มี discretionary — ทุก signal ต้องมีเงื่อนไขชัดเจน
- ต้องทดสอบย้อนหลังได้และ replicate ได้

### 2. Set-Up + Entry = หัวใจของ Strategy
- Set-Up บอกทิศทาง (aim)
- Entry ยืนยันและเข้าตลาด (pull trigger)
- อย่าใช้แค่อย่างใดอย่างหนึ่ง

### 3. Cash Management สำคัญกว่า Indicator
- Strategy ธรรมดา + cash management ดี = กำไรมหาศาล
- Indicator ดี + ไม่มี cash management = ยังขาดทุนได้

### 4. ต้องไม่พลาด Big Move
- Trend strategy ต้องเข้าตลาดทุกครั้งที่มี big move
- ใช้ failsafe entries (เช่น breakout 52-week high/low) เป็น backup

### 5. ทดสอบความ Robust
- Surrounding parameters ต้องกำไรด้วย
- Forward + backward testing
- ทดสอบบนหลาย securities
- กำไรแยกทศวรรษต้องสม่ำเสมอ

### 6. ยอมรับความเสี่ยง
- ไม่มี strategy ที่ไม่มี drawdown
- ต้องรู้ pain threshold ของตัวเอง
- ออกแบบ strategy ให้เหมาะกับ personality

---

*สรุปจากหนังสือ "Building Winning Trading Systems with TradeStation" ครอบคลุมทั้ง 9 บท สำหรับใช้เป็นแนวทางในการออกแบบระบบเทรดอัตโนมัติแบบ rule-based*
