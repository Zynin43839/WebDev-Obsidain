# R-Forex Learn Book — สรุปภาษาไทย

> **Source:** [[R-Forex Learn Book]]
> **Author:** Royal Forex (© 2001)
> ** Covers:** Chapter 1-5 — ความรู้พื้นฐาน Forex, Fundamental Analysis, Technical Analysis, Fibonacci & Elliott Waves

---

## ส่วนที่ 1: ความรู้ทั่วไปเกี่ยวกับ Forex

### 1.1 Forex คืออะไร
- **Forex** คือตลาดแลกเปลี่ยนเงินตราต่างประเทศระดับโลก ผู้ค้าทำกำไรจากการซื้อขายสกุลเงินต่างกัน
- ค่าเงินเปลี่ยนแปลงตลอดเวลาตาม supply/demand ซึ่งได้รับผลกระทบจากปัจจัยเศรษฐกิจ การเมือง และธรรมชาติ
- จุดเด่นของ Forex: ตอบสนองต่อปัจจัยจำนวนมาก, เปิดให้เทรดเดอร์ทุกระดับ, มี liquidity สูง, และเทรดได้ 24 ชั่วโมง

### 1.2 ประวัติ Forex
- การแลกเปลี่ยนเงินตรามีมาตั้งแต่ยุคกลาง จนกระทั่ง Bretton Woods Accord (1944) กำหนดให้ USD เป็น reserve currency หลัก (ตรึงทอง $35/oz)
- ปลายทศวรรษ 1970 กำหนด free-floating ของ currencies → Forex ยุคปัจจุบันเริ่มต้น
- Daily turnover ปี 1977 = $5B → ปี 2000 = $1.5 ล้านล้าน USD

### 1.3 บทบาทของ Central Banks
- **Federal Reserve (FRS)** และ central banks ของ G-7 กระทบตลาด Forex ผ่าน:
  - **Repurchase agreements (Repos):** ขายพันธบัตรแล้วซื้อคืน → inject reserves → สกุลเงินอ่อนค่า
  - **Matched sale-purchase agreements:** ตรงข้าม repos → drain reserves → สกุลเงินแข็งค่า
- **Naked intervention:** ซื้อ/ขายเงินโดยตรง → มีผลกระทบต่อ money supply ระยะยาว
- **Sterilized intervention:** ชดเชยผลกระทบต่อ money supply ด้วยการขายพันธบัตรรัฐบาล → มีผลระยะสั้น-ปานกลาง

### 1.4 ความเสี่ยงใน Forex
| ประเภทความเสี่ยง | คำอธิบาย |
|:--|:--|
| **Exchange rate risk** | ความผันผวนของอัตราแลกเปลี่ยนที่กระทบ position ที่ถืออยู่ → ป้องกันด้วย position limit และ loss limit (stop-loss) |
| **Interest rate risk** | กำไร/ขาดทุนจาก forward spreads ที่ผันผวน → จำกัด size ของ mismatches ตาม maturity dates |
| **Credit risk** | คู่สัญญาอาจไม่ชำระหนี้ → ใช้ credit line limits และ matching systems |
| **Settlement risk** | ช่วงเวลาต่างกันระหว่างทวีป → จ่ายเงินก่อนแล้วคู่สัญญาอาจล้มละลาย |
| **Dictatorship/Sovereign risk** | รัฐบาลแทรกแซงกิจกรรม Forex → เทรดเดอร์ต้องตระหนักข้อจำกัดเชิงนโยบาย |

---

## ส่วนที่ 2: ประเภทของตลาด Forex

### 2.1 Spot Market
- เป็นที่นิยมมากที่สุด (37% ของกิจกรรมทั้งหมด)
- ทําสัญญาแล้วส่งมอบภายใน 2 วันทำการ (CAD = 1 วัน)
- ราคามี 2 ฝั่ง: **Bid** (ราคาขาย) และ **Ask** (ราคาซื้อ) → ต่างกันคือ **Spread** (pip)
- ตลาดมี liquidity สูงและ volatility สูง: EUR/USD เปลี่ยนค่าได้ 18,000 ครั้ง/วัน, 100-200 pips ในไม่กี่วินาที

### 2.2 Forward Market
- 57% ของ market share (ปี 1998)
- ประกอบด้วย **Forward outright deals** และ **Swap deals** (spot + forward)
- ไม่มี standard settlement dates (3 วัน ถึง 3 ปี)
- Forward price = Spot rate + Forward spread (forward points/pips)

### 2.3 Futures Market
- สัญญา futures เป็น forward outright deals แบบ standardized
- หมดอายุเฉพาะ 3rd Wednesday ของ Mar/Jun/Sep/Dec
- ซื้อขายบน exchange (CME) → ไม่มี credit risk (Clearinghouse เป็นคู่สัญญาทุกด้าน)
- มี tools เพิ่มเติม: Gap, Volume, Open Interest

### 2.4 Options Market
- ให้สิทธิ์ (ไม่ใช่ภาระ) ในการซื้อขายสกุลเงินที่ราคาและเวลาที่กำหนด
- Premium = ราคา option → คือความเสี่ยงสูงสุดของผู้ซื้อ
- ทั้ง high volatility และ low volatility สามารถสร้างกำไรได้
- 5% ของ Forex market (ปี 1998) — fastest-growing segment

---

## ส่วนที่ 3: วิเคราะห์ปัจจัยพื้นฐาน (Fundamental Analysis)

### 3.1 ทฤษฎีการกำหนดอัตราแลกเปลี่ยน

#### Purchasing Power Parity (PPP)
- ** Absolute version:** อัตราแลกเปลี่ยน = อัตราส่วนของ price levels สองประเทศ → ใช้ยากในทางปฏิบัติ (ค่าขนส่ง, trade barriers, brand names)
- **Relative version:** % change ในอัตราแลกเปลี่ยน = ความแตกต่างระหว่าง % change ในราคาภายใน vs ต่างประเทศ → มีปัญหาเรื่อง base period, trade restrictions

#### Theory of Elasticities
- อัตราแลกเปลี่ยนคือ "ราคา" ของเงินตราต่างประเทศที่รักษาสมดุล balance of payments
- ค่าเงินขึ้น → ส่งออกเพิ่ม → รายได้ในประเทศเพิ่ม → นำเข้าเพิ่ม → ความต้องการเงินตราต่างประเทศเพิ่ม

#### Modern Monetary Theories
- อัตราแลกเปลี่ยนได้รับผลกระทบจาก supply/demand ของ financial assets มากกว่า commodity market ในระยะสั้น
- Financial markets ปรับตัวเร็วกว่า commodity markets → ความผันผวนเกิดจาก speed difference นี้

### 3.2 ตัวชี้วัดทางเศรษฐกิจ

#### GDP/GNP
- **GNP:** มูลค่าสินค้าและบริการทั้งหมดที่ผลิตโดยชาวอเมริกัน (ในและต่างประเทศ)
- **GDP:** มูลค่าสินค้าและบริการทั้งหมดที่ผลิตในสหรัฐฯ (โดยบริษัทในและต่างประเทศ)
- องค์ประกอบ: Consumption Spending + Investment Spending + Government Spending + Net Trade

#### ตัวชี้วัดภาคอุตสาหกรรม
- **Industrial Production:** ผลผลิตทั้งหมดของโรงงาน,สาธารณูปโภค,และเหมือง → สะท้อนความแข็งแกร่งของเศรษฐกิจ
- **Capacity Utilization:** ผลผลิตจริง / ศักยภาพผลผลิต → >85% = เศรษฐกิจร้อนเกินไป → inflation → central bank ขึ้นดอกเบี้ย
- **Durable Goods Orders:** สินค้าที่มีอายุ >3 ปี → สะท้อนความมั่นใจของผู้บริโภค

#### ตัวชี้วัดเงินเฟ้อ
- **PPI (Producer Price Index):** ค่าเฉลี่ยราคาสินค้า ~3,400 รายการ (ไม่รวมสินค้านำเข้า)
- **CPI (Consumer Price Index):** ค่าเฉลี่ยราคา retail (อาหาร 19%, ที่พัก 38%, เชื้อเพลิง 8%)
- **CRB Futures Index:** ราคา futures ของสินค้า 21 ชนิด → สัญญาณเงินเฟ้อ
- **GNP/GDP Implicit Deflator:** ค่าเงินปัจจุบัน / ค่าเงินคงที่ → มาตรการเงินเฟ้อที่สำคัญที่สุด

#### ดุลการค้า (Merchandise Trade Balance)
- ส่วนต่างระหว่างส่งออกและนำเข้า → ตัวชี้วัดที่สำคัญที่สุดตัวหนึ่ง
- กระทบนโยบายการเงินและการต่างประเทศในระยะยาว

#### ตัวชี้วัดการจ้างงาน
- **Nonfarm Payrolls:** ตัวเลขการจ้างงานที่สำคัญที่สุดสำหรับ Forex
- **Employment Cost Index (ECI):** ค่าจ้าง + เงินเดือน + สวัสดิการ → วัดแรงกดดันด้านค่าจ้าง

#### ตัวชี้วัดการใช้จ่ายผู้บริโภค
- **Retail Sales:** ความแข็งแกร่งของความต้องการบริโภค → เดือนที่สำคัญ: ธันวาคม (วันหยุด), กันยายน (เปิดเทอม), พฤศจิกายน
- **Consumer Sentiment:** ความเชื่อมั่นผู้บริโภค → สะท้อนแนวโน้มการใช้จ่าย

#### Leading Indicators
- ชั่วโมงทำงานเฉลี่ย, จำนวนผู้ว่างงาน, คำสั่งซื้อสินค้า, Vendor performance, สัญญาจ้างเครื่องจักร, Permit สร้างบ้านใหม่, คำสั่งซื้อคงค้าง, ราคาสินค้าวัตถุดิบ

### 3.3 ปัจจัยทางการเงินและสังคมการเมือง

#### บทบาทของอัตราดอกเบี้ย
- Forex ให้ความสำคัญกับ **interest rate differential** (ส่วนต่างดอกเบี้ย) ไม่ใช่ดอกเบี้ยตัวเลขเดี่ยว
- ถ้า G-5 พร้อมกันลดดอกเบี้ย 0.5% → เป็นกลางต่อ Forex (differential ไม่เปลี่ยน)
- ตลาดซื้อขายบน **expectations** — ข่าวลือทำให้ขายก่อน fact, แล้วซื้อคืนหลังประกาศ

#### วิกฤตการเมือง
- อันตรายต่อ Forex: volume ลดลง, spread กระโดดจาก 5 → 100 pips
- เทรดเดอร์ต้องตัดสินใจภายในไม่กี่วินาที → ยากที่จะกลับเข้าตลาดหลังวิกฤต

---

## ส่วนที่ 4: วิเคราะห์ทางเทคนิค (Technical Analysis)

### 4.1 ทฤษฎี Dow — หลักการพื้นฐาน
1. **ราคาสะท้อนทุกอย่าง** ("The market knows everything")
2. **ราคาเคลื่อนที่ตามแนวโน้ม** ("Trend is your friend") — Uptrend (bullish), Downtrend (bearish), Sideways
3. **ประวัติศาสตร์ซ้ำรอย** — รูปแบบเดิมปรากฏบนกราฟซ้ำๆ
4. **ตลาดมี 3 แนวโน้ม:** Primary (≈1 ปี), Secondary (1 เดือน+), Minor (ไม่กี่วัน-สัปดาห์)
   - Primary trend มี 3  phases: Accumulation → Run-up/Run-down → Distribution

### 4.2 ประเภทของกราฟ

| ประเภท | ลักษณะ | ข้อดี/ข้อเสีย |
|:--|:--|:--|
| **Line Chart** | เชื่อม closing prices | เข้าใจง่าย, แต่ไม่เห็น price activity ภายใน period |
| **Bar Chart** | Histogram แสดง OHLC | เห็น range ทั้งหมด, แสดง gaps ได้ |
| **Candlestick Chart** | Body (jittai) + Shadow (uwakage/shitakage) | มองเห็น pattern ได้ชัดเจน, มี interpretation เฉพาะ |

### 4.3 Trend Lines, Support และ Resistance

#### Trendline
- เชื่อมจุด peaks (bearish) หรือ troughs (bullish) → เส้น trend
- Trendline + เส้นขนาน = **Trade Channel**
- ใช้ 2 จุดในการลากเส้น, จุดที่ 3 เป็นการ confirm

#### Support และ Resistance
- **Resistance:** จุด peaks ที่แรงขาย > แรงซื้อ
- **Support:** จุด troughs ที่แรงซื้อ > แรงขาย
- **Rule สำคัญ:** เมื่อ break แล้วจะสลับกัน — support กลายเป็น resistance และในทางกลับกัน
- ความสำคัญของ trend แปรผันตามเวลาและ volume

#### กฎ可靠性ของ Channel
1. Channel ยิ่งนานยิ่งน่าเชื่อถือ (แต่เก่ามาก >1 ปี reliability ลดลง)
2. ยิ่งกว้างยิ่งน่าเชื่อถือ ("It takes time to break channel")
3. Resistance ต้องใช้ volume มากในการ break ("It takes volume to break resistance")
4. Steep channel น่าเชื่อถือน้อยกว่า gentle channel
5. Support สามารถ break ได้โดยไม่ต้องใช้ volume ("under own weight")

### 4.4 Trend Reversal Patterns (รูปแบบกลับตัว)

#### Head-and-Shoulders (H&S)
- 3 rallies: ไหล่ซ้าย, หัว, ไหล่ขวา → หัวสูงสุด, ไหล่สองข้างสูงเท่ากัน
- **Neckline:** เส้น support ที่เชื่อมจุด B และ C
- Break neckline ด้วย heavy volume → ยืนยัน reversal
- **Price target:** วัดจาก breakout point ลงมา = ระยะห่างระหว่างหัวกับ neckline
- **Inverted H&S:** รูปแบบกระจกเงา → สัญญาณ bullish

#### Double Top / Double Bottom
- **Double Top:** 2 peaks ระดับเดียวกัน → bearish reversal
- **Double Bottom:** 2 troughs ระดับเดียวกัน → bullish reversal
- Neckline = เส้นเชื่อมจุดกึ่งกลาง → break ด้วย heavy volume เท่านั้น
- **Price target:** วัดจาก neckline breakout = ความสูงเฉลี่ยของ formation

#### Triple Top / Triple Bottom
- 3 peaks/troughs ระดับเดียวกัน → hybrid ของ H&S และ Double Top/Bottom
- ความน่าเชื่อถือสูงกว่า Double Top/Bottom

#### Rounded Top / Rounded Bottom (Saucer/Inverted Saucer)
- การเปลี่ยนทิศทางช้าๆ ค่อยเป็นค่อยไป
- ยิ่งใช้เวลานานยิ่งมีโอกาสที่ราคาจะเคลื่อนไหวแรงในทิศทางใหม่

### 4.5 Trend Continuation Patterns (รูปแบบต่อเนื่อง)

| Pattern | ลักษณะ | Price Target |
|:--|:--|:--|
| **Flags** | ช่วง consolidation สั้นๆ ระหว่าง trend แข็งแกร่ง, border ขนานกัน | = ความยาว flagpole จาก breakout point |
| **Pennants** | เหมือน flags แต่ support/resistance converge | = ความยาว pennant pole จาก breakout point |
| **Symmetrical Triangle** | 2 เส้น converge สมมาตร → break ได้ทั้ง 2 ฝั่ง | = ความกว้างฐานสามเหลี่ยม |
| **Descending Triangle** | Support flat + Resistance sloping down → bearish | = ความกว้างฐาน |
| **Expanding Triangle (Megaphone)** | Diverging lines → เพิ่มขึ้นเรื่อยๆ | = ความกว้างฐาน |
| **Wedges** | เหมือน triangle แต่มี slope ชัดเจน → break ตรงข้าม slope | มี direction แต่ไม่มี price target ที่แน่นอน |
| **Rectangles** | Sideways consolidation ชัดเจน | = ความสูงของ rectangle จาก breakout point |

### 4.6 Gaps (ช่องว่างราคา)

| ประเภท | ลักษณะ | การเติม |
|:--|:--|:--|
| **Common Gaps** | เกิดในช่วงเงียบๆ หรือ illiquid market | เติมเร็ว (≤12 ชม.) |
| **Breakaway Gaps** | เกิดตอนเริ่ม trend ใหม่, หลัง break consolidation | ไม่เติมเร็ว |
| **Runaway Gaps** | เกิดกลาง trend ที่แข็งแกร่ง → "measurement gap" | ใช้คำนวณจุดสิ้นสุด trend |
| **Exhaustion Gaps** | เกิดจุดสูงสุด/ต่ำสุด → สัญญาณกลับตัวเร็ว | เติมเร็ว |

### 4.7 Technical Indicators (ตัวชี้วัดทางเทคนิค)

#### Moving Averages

| ประเภท | สูตร | ลักษณะ |
|:--|:--|:--|
| **SMA (Simple)** | ค่าเฉลี่ยธรรมดา | smooth น้อยสุด |
| **LMA (Linearly Weighted)** | น้ำหนักเพิ่มตามวันล่าสุด (วันล่าสุด × 4, 3, 2, 1) | responsive กว่า SMA |
| **EMA (Exponential)** | ถ่วงน้ำหนักแบบ exponential | smooth ดีที่สุด, responsive สูง |

- **สัญญาณ:** ราคาตัด MA จากล่างขึ้นบน = Buy, จากบนลงล่าง = Sell
- **Multiple MAs:** ใช้ 2-3 MAs (เช่น 4/9 วัน, 4/9/18 วัน) → Buy เมื่อ short-term ตัด long-term ขึ้นบน

#### Envelopes
- MA ± % (แนะนำ 2% สำหรับ Forex) → ราคาตัด envelope จากบนลงล่าง = Buy

#### Bollinger Bands
- 20-day SMA ± 2 standard deviations
- Band หดตัว → volatility ต่ำ → จะ expand เร็วๆ นี้
- Double top ข้างนอก band แล้วข้างใน = สัญญาณ

#### ATR (Average True Range)
- วัด volatility → ค่าต่ำสุด/สูงสุด = สัญญาณกลับตัว

#### Oscillators

| Indicator | ช่วง | สัญญาณ Buy | สัญญาณ Sell |
|:--|:--|:--|:--|
| **CCI** | -100 ถึง +100 | < -100 (oversold) | > +100 (overbought) |
| **DMI/ADX** | 0-100% | +DI > -DI | -DI > +DI |
| **Stochastic** | 0-100 | < 10% | > 90% |
| **MACD** |围绕 zero line | Upward crossover | Downward crossover |
| **Momentum** |围绕 100 | M > 100 ("market caught") | M < 100 ("market lost") |
| **RSI** | 0-100 | < 15 (oversold) | > 85 (overbought) |
| **ROC** |围绕 100 | CCP/OCP > 1 | CCP/OCP < 1 |
| **Williams %R** | 0-100 (กลับหัว) | < 80% | > 20% |

#### Ichimoku Kinko Hyo
- รวม trend direction, support/resistance, สัญญาณ Buy/Sell ไว้ใน indicator เดียว
- **Tenkan-sen:** ค่าเฉลี่ยราคาช่วงแรก (9 วัน) → ถ้าขึ้น/ลง = trend มีอยู่
- **Kijun-sen:** ค่าเฉลี่ยราคาช่วงที่สอง (26 วัน) → "main line" วัดการเคลื่อนไหว
- **Senkou Span A/B:** สร้าง "Cloud" → ราคาอยู่ใน cloud = trendless, อยู่เหนือ cloud = bullish, อยู่ใต้ cloud = bearish
- **Chinkou Span:** close ของ candlestick ย้อนหลัง 26 วัน → ตัดราคาจากล่างขึ้นบน = Buy

---

## ส่วนที่ 5: Fibonacci Constants และ Elliott Waves Theory

### 5.1 Fibonacci Constants
- **Fibonacci sequence:** 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233, ...
- อัตราส่วนระหว่างเลขตัวใดกับตัวถัดไป ≈ **0.618** (Fibonacci ratio / "Golden Spiral")
- อัตราส่วนกลับกัน ≈ **1.618**
- ใช้ในการคำนวณ **price targets** และ **placing stops** ในตลาดการเงิน
- ตัวอย่าง: ถ้า corrective wave จะ retracement 61.8% → วาง stop ใต้ระดับนั้น

### 5.2 Elliott Waves Theory
- ทุกการเคลื่อนไหวของตลาดประกอบด้วย cycle ละ **8 waves**:
  - **5 waves ตาม trend** (impulse waves — หมายเลข 1-5)
  - **3 waves ต่อต้าน trend** (corrective waves — ตัวอักษร A-B-C)
- **กฎที่ต้องปฏิบัติ:**
  - Wave 2 ไม่สามารถ retracement มากกว่า 100% ของ wave 1
  - Wave 3 ไม่เคยเป็น wave ที่สั้นที่สุด (มักจะยาวที่สุด)
  - Wave 4 ไม่สามารถเข้าไปใน price range ของ wave 1

#### Extensions
- หนึ่งใน 3 impulse waves (1, 3, หรือ 5) มักจะ "extended" — ยาวผิดปกติ
- ถ้า wave 1 และ 3 ยาวเท่ากัน → wave 5 มักจะ extended

#### Diagonal Triangles
- **Type 1:** เกิดใน wave 5 หรือ wave C → สัญญาณว่า move "ไปไกลเกินไปเร็วเกินไป" → bearish (rising) หรือ bullish (falling)
- **Type 2:** หายากมาก, เกิดใน wave 1 หรือ A → สัญญาณ **continuation** ของ trend

#### Failures (Truncated Fifths)
- Wave 5 ไม่สามารถทะลุจุดสูงสุดของ wave 3 → สัญญาณ **weakness** ของ trend → มักตามด้วย reversal รุนแรง

---

## หมายเหตุสำหรับการเทรด

### ความเชื่อมโยงกับระบบเทรดอัตโนมัติ
- **Fundamental indicators** ใช้เป็นข้อมูลป้อนเข้าสำหรับ News Calendar Filter (เช่น NFP/FOMC/CPI gate)
- **ATR** ใช้เป็นตัวชี้วัด volatility สำหรับ SL/TP distance ในระบบ Multi-Strategy
- **Stochastic, RSI, CCI** ใช้เป็น oscillator สำหรับ overbought/oversold filtering
- **Bollinger Bands** ใช้สำหรับ volatility expansion/contraction detection
- **Ichimoku** ใช้สำหรับ trend direction confirmation
- **Fibonacci retracement levels (38.2%, 50%, 61.8%)** ใช้สำหรับ Support/Resistance zones

### แหล่งอ้างอิง
1. Luca C. Trading in the Global Currency Markets (2nd Ed., 1999)
2. Luca C. Technical Analysis Applications in the Global Currency Markets (2nd Ed., 2000)
3. Achelis S.B. Technical Analysis from A to Z (2nd Ed., 2000)
4. Reuters. An Introduction to Technical Analysis (1999)
5. Edwards R.D., Magee John. Technical Analysis of Stock Trends (7th Ed., 1998)
