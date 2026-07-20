# สรุปหนังสือ: Derivative Work Book (BSE)

> หนังสือแบบฝึกหัดเรื่องอนุพันธ์ (Derivatives) สำหรับการซื้อขายในตลาดหลักทรัพย์บอมเบย์ (BSE) ประเทศอินเดีย ครอบคลุมตั้งแต่พื้นฐานจนถึงการคำนวณ margins และการบัญชี

---

## บทที่ 1: ความรู้เบื้องต้นเกี่ยวกับอนุพันธ์ (Introduction to Derivatives)

### 1.1 นิยามของอนุพันธ์ (Derivatives Defined)

อนุพันธ์ (Derivative) คือสินค้าที่มีมูลค่ามาจากมูลค่าของสินค้าอ้างอิง (Underlying Asset) ซึ่งอาจเป็นหุ้น ค่าเงิน คอมmodity หรือสินค้าอื่นๆ ตัวอย่างง่ายๆ คือ เกษตรกรผู้ปลูกมะม่วงต้องการขายผลผลิตล่วงหน้าเพื่อลดความเสี่ยงจากราคาที่ผันผวน สัญญาดังกล่าวถือเป็นอนุพันธ์ชนิดหนึ่ง

ตามกฎหมาย SC(R)A ของอินเดีย อนุพันธ์ถูกนิยามว่ารวมถึง:
- หลักทรัพย์ที่มาจากเครื่องมือหนี้ หุ้น เงินกู้ หรือสัญญาซื้อขายความแตกต่าง (Contract for Differences)
- สัญญาที่ได้มูลค่าจากราคาหรือดัชนีราคาของสินค้าอ้างอิง

### 1.2 ปัจจัยที่ขับเคลื่อนการเติบโตของอนุพันธ์

ปัจจัยสำคัญ 5 ประการที่ทำให้ตลาดอนุพันธ์เติบโตอย่างรวดเร็ว:
1. **ความผันผวนของราคาสินทรัพย์** ที่เพิ่มมากขึ้นในตลาดการเงิน
2. **การบูรณาการของตลาดการเงิน** ภายในประเทศกับตลาดระหว่างประเทศ
3. **การปรับปรุงด้านการสื่อสาร** ที่ดีขึ้นอย่างชัดเจนและต้นทุนที่ลดลง
4. **เครื่องมือจัดการความเสี่ยงที่ซับซ้อนมากขึ้น** ทำให้ผู้มีส่วนได้เสียมีทางเลือกมากขึ้น
5. **นวัตกรรมในตลาดอนุพันธ์** ที่รวมความเสี่ยงและผลตอบแทนจากสินทรัพย์ทางการเงินหลายชนิดเข้าด้วยกัน

### 1.3 ผลิตภัณฑ์อนุพันธ์ (Derivative Products)

ผลิตภัณฑ์อนุพันธ์มีหลายประเภท ได้แก่:

- **Forward (สัญญาซื้อขายล่วงหน้า)**: สัญญาที่ปรับแต่งตามความต้องการของทั้งสองฝ่าย กำหนดราคาล่วงหน้า แต่ส่งมอบในอนาคต
- **Future (สัญญาซื้อขายล่วงหน้ามาตรฐาน)**: 类似กับ forward แต่เป็นสัญญาที่มีมาตรฐานและซื้อขายในตลาดหลักทรัพย์
- **Option (ออปชั่น)**: แบ่งเป็น Call (สิทธิ์ซื้อ) และ Put (สิทธิ์ขาย) ผู้ซื้อมีสิทธิ์แต่ไม่มีภาระผูกพัน
- **Warrant**: ออปชั่นที่มีอายุยาวกว่า 1 ปี มักซื้อขายแบบ OTC
- **LEAPS**: ออปชั่นที่มีอายุสูงสุด 3 ปี
- **Basket**: ออปชั่นบนพอร์ตโฟลิโอของสินทรัพย์หลายชนิด
- **Swap**: สัญญาแลกเปลี่ยนกระแสเงินสดในอนาคต มี 2 ประเภทหลักคือ Interest Rate Swap และ Currency Swap
- **Swaption**: สิทธิ์ในการซื้อขาย swap

### 1.4 ผู้เข้าร่วมในตลาดอนุพันธ์

ผู้เข้าร่วมตลาดมี 3 กลุ่มหลัก:
1. **Hedger (ผู้ป้องกันความเสี่ยง)**: ใช้ futures หรือ options เพื่อลดหรือกำจัดความเสี่ยงจากราคา
2. **Speculator (ผู้เก็งกำไร)**: ต้องการเดิมพันกับการเคลื่อนไหวของราคาในอนาคต ใช้ leverage เพื่อเพิ่มผลตอบแทน
3. **Arbitrageur (ผู้ทำ arbitrage)**: ใช้ประโยชน์จากราคาที่เบี่ยงเบนระหว่างตลาดต่างๆ

### 1.5 บทบาททางเศรษฐกิจของตลาดอนุพันธ์

ตลาดอนุพันธ์มีบทบาทสำคัญ 5 ประการ:
1. ช่วยในการ **ค้นหาราคาล่วงหน้าและราคาปัจจุบัน** (Price Discovery)
2. ช่วย **โอนความเสี่ยง** จากผู้ที่ไม่ต้องการรับไปยังผู้ที่ต้องการรับ
3. ช่วยเพิ่ม **สภาพคล่อง** ในตลาดสินค้าอ้างอิง
4. ย้ายการซื้อขายเก็งกำไรไปยัง **สภาพแวดล้อมที่ควบคุมได้**
5. เป็น **ตัวเร่งให้เกิดกิจกรรมผู้ประกอบการใหม่**

### 1.6 Exchange-Traded vs. OTC Derivatives

- **Exchange-Traded Derivatives**: ซื้อขายในตลาดหลักทรัพย์ มีมาตรฐาน มี clearing house รับประกัน
- **OTC Derivatives**: เจรจาระหว่างคู่สัญญาโดยตรง ไม่มีตลาดกลาง มีความยืดหยุ่นสูงแต่มีความเสี่ยง counterparty สูง

ข้อเสียของ OTC Derivatives ได้แก่:
- การจัดการความเสี่ยงแบบกระจายศูนย์
- ไม่มีขีดจำกัดตำแหน่ง levearge หรือ margin ที่เป็นทางการ
- ไม่มีกฎเกณฑ์สำหรับการแบ่งเบาภาระความเสี่ยง
- ไม่มีกลไกการรักษาเสถียรภาพของตลาด
- ไม่ได้รับการกำกับดูแลโดยตรง

---

## บทที่ 2: ดัชนีตลาด (Market Index)

### 2.1 ความเข้าใจเกี่ยวกับดัชนี

ดัชนี (Index) คือตัวเลขที่วัดการเปลี่ยนแปลงของมูลค่ากลุ่มหุ้นที่กำหนดไว้ในช่วงเวลาหนึ่งๆ ดัชนีตลาดหุ้นที่ดีควรสะท้อนพฤติกรรมของตลาดโดยรวม มีความหลากหลาย และมีสภาพคล่องสูง

ดัชนีมีความสำคัญในฐานะ:
- **ตัวชี้วัดพฤติกรรมตลาด** (Barometer)
- **เกณฑ์มาตรฐาน** สำหรับผลตอบแทนพอร์ตโฟลิโอ
- **สินค้าอ้างอิง** ในอนุพันธ์ เช่น index futures
- ใช้ในการ **บริหารกองทุนแบบ passive** (Index Fund)

### 2.2 ความสำคัญทางเศรษฐกิจของmovement of index

การเคลื่อนไหวของดัชนีสะท้อนความคาดหวังที่เปลี่ยนไปของตลาดหุ้นเกี่ยวกับเงินปันผลในอนาคตของภาคธุรกิจ ดัชนีขึ้นเมื่อนักลงทุนคาดว่าเงินปันผลจะดีขึ้น และดัชนีลงเมื่อมีความessimistic

### 2.3 การก่อตั้งดัชนี

ดัชนีที่ดีคือการแลกเปลี่ยนระหว่าง **ความหลากหลาย** (Diversification) กับ **สภาพคล่อง** (Liquidity) การเพิ่มหุ้นจาก 10 เป็น 20 ตัวช่วยลดความเสี่ยงได้มาก แต่การเพิ่มจาก 50 เป็น 100 ตัวลดความเสี่ยงได้น้อยมาก

### 2.4 ประเภทของดัชนี

- **Market Capitalization Weighted Index**: หุ้นแต่ละตัวมีน้ำหนักตามมูลค่าหลักทรัพย์ตามราคาตลาด
- **Price Weighted Index**: หุ้นแต่ละตัวมีน้ำหนักราคาหุ้น
- **Free Float Method**: วิธีที่ใช้กันในปัจจุบันโดยมุ่งเน้นเฉพาะหุ้นที่หมุนเวียนในตลาด

### 2.5 คุณสมบัติที่พึงประสงค์ของดัชนี

1. สะท้อนพฤติกรรมของพอร์ตโฟลิโอที่หลากหลาย
2. หุ้นในดัชนีต้องมีสภาพคล่องสูง
3. ได้รับการดูแลอย่างมืออาชีพ

### 2.6 SENSEX (S&P BSE SENSEX)

SENSEX คือดัชนีหลักของ BSE เริ่มคำนวณครั้งแรกในปี 1986 ประกอบด้วยหุ้น 30 ตัว เป็นหุ้นขนาดใหญ่ที่มีสภาพคล่องและเป็นตัวแทนของตลาด

คุณสมบัติเด่น:
- คำนวณแบบ Free-float Market Capitalization (เปลี่ยนจาก Full Market Cap ตั้งแต่ 1 กันยายน 2003)
- Base Year: 1978-79, Base Value: 100
- คำนวณทุก 15 วินาที ระหว่างเวลาทำการ
- Index Committee ประชุมทุกไตรมาสเพื่อทบทวนองค์ประกอบ

### 2.7 การคัดเลือกหุ้นใน SENSEX

เกณฑ์เชิงปริมาณ:
- ต้องอยู่ใน 100 อันดับแรกตามมูลค่าหลักทรัพย์ตามราคาตลาด
- ซื้อขายทุกวันในช่วง 1 ปีที่ผ่านมา
- อยู่ใน 150 อันดับแรกตามจำนวนธุรกรรมเฉลี่ยต่อวัน

เกณฑ์เชิงคุณภาพ:
- บริษัทต้องมี track record ที่ดี

### 2.8 Beta ของหุ้นใน SENSEX

Beta วัดความอ่อนไหวของราคาหุ้นต่อการเคลื่อนไหวของ SENSEX:
**Beta = Covariance(SENSEX, หุ้น) / Variance(SENSEX)**

### 2.9 การคำนวณ SENSEX

SENSEX คำนวณจาก Free-float Market Capitalization ของหุ้น 30 ตัว หารด้วย Index Divisor ซึ่งเชื่อมโยงกับ base period

### 2.10 F&O Segment และ Mini SENSEX

- **SENSEX Futures**: มี lot size 50 หน่วย หมดอายุเดือนละหนึ่งครั้ง (วันพฤหัสบดีสุดท้ายของเดือน)
- **Chhota (Mini) SENSEX**: lot size 5 หน่วย เปิดตัว 1 มกราคม 2008 สำหรับนักลงทุนรายย่อย

---

## บทที่ 3: Futures และ Options

### 3.1 Forward Contract

Forward คือสัญญาซื้อขายล่วงหน้าระหว่างสองฝ่าย โดย:
- กำหนดราคา จำนวน คุณภาพ วันที่ส่งมอบล่วงหน้า
- ไม่มี margin ที่ต้องจ่าย
- มี **Counterparty Risk** (ความเสี่ยงคู่สัญญา)
- ราคาไม่โปร่งใส

### 3.2 Futures Contract

Futures คล้ายกับ Forward แต่ซื้อขายในตลาดหลักทรัพย์:
- ตลาดหลักทรัพย์เป็นคู่สัญญาทางกฎหมาย
- ราคาโปร่งใส ทุกคนเข้าถึงได้
- มีมาตรฐาน (Lot size, Tick size, วันหมดอายุ)
- ต้องจ่าย Margin ทั้งฝ่ายซื้อและขาย

### 3.3 ข้อจำกัดและข้อดีของ Forward vs. Futures

| Forward | Futures |
|---------|---------|
| ปรับแต่งตามต้องการ | มีมาตรฐาน |
| มี Counterparty Risk | ตลาดรับประกัน |
| ราคาไม่โปร่งใส | ราคาโปร่งใส |
| ไม่มี Margin | มี Margin |

### 3.4 การหมดอายุของ Futures

- ในอินเดียหมดอายุวันพฤหัสบดีสุดท้ายของทุกเดือน
- ถ้าวันพฤหัสบดีเป็นวันหยุด จะหมดอายุวันทำการก่อนหน้า
- Settlement ทำได้ 2 แบบ: **Cash Settlement** (จ่ายเป็นเงินสด) หรือ **Delivery Settlement** (ส่งมอบหุ้นจริง)

### 3.5 สินค้าอ้างอิงของอนุพันธ์

- หุ้นรายตัว (Equity Shares)
- ดัชนีหุ้น (Equity Indices)
- หลักทรัพย์ในตลาดเงิน (Debt Market)
- อัตราดอกเบี้ย (Interest Rates)
- ค่าเงินต่างประเทศ (Foreign Exchange)
- สินค้าโภคภัณฑ์ (Commodities)
- ตัวอนุพันธ์เอง (Derivatives themselves)

### 3.6 กลยุทธ์การซื้อขายใน Futures

- **Long Position**: ซื้อ Futures เมื่อมองว่าตลาดจะขึ้น (Bullish)
- **Short Position**: ขาย Futures เมื่อมองว่าตลาดจะลง (Bearish)
- **Arbitrage**: ซื้อและขายในตลาดต่างๆ พร้อมกันเพื่อใช้ประโยชน์จากราคาที่เบี่ยงเบน

### 3.7 Hedging

Hedging คือการใช้อนุพันธ์เพื่อลดความเสี่ยง ตัวอย่าง:
- เกษตรกรขาย Rice Futures เพื่อป้องกันราคาข้าวขึ้น
- ช่างทองขาย Gold Futures เพื่อป้องกันราคาทองลง
- ผู้นำเข้า Dollar Buy Futures เพื่อป้องกัน Dollar แข็งค่า
- ผู้ส่งออก Dollar Sell Futures เพื่อป้องกัน Dollar อ่อนค่า

### 3.8 ประเภทของธุรกรรมใน Futures

- **Opening Buy**: เปิด Long Position
- **Opening Sell**: เปิด Short Position
- **Closing Buy**: ปิด Short Position (ซื้อคืน)
- **Closing Sell**: ปิด Long Position (ขายออก)
- **Square Up**: ทำธุรกรรมตรงข้ามเท่ากัน

### 3.9 Bid-Ask Price และ Impact Cost

- **Bid Price**: ราคาที่ผู้ซื้อยินดีจ่าย
- **Ask Price**: ราคาที่ผู้ขายยินดีรับ
- **Bid-Ask Spread**: ความแตกต่างระหว่าง Bid และ Ask
- **Impact Cost**: ค่าใช้จ่ายที่เกิดจากการเคลื่อนไหวของราคาเนื่องจากคำสั่งซื้อขาย

### 3.10 Exotic Derivatives และ OTC

- **Vanilla Derivatives**: ผลิตภัณฑ์มาตรฐานที่ซื้อขายในตลาด
- **Exotic Derivatives**: ผลิตภัณฑ์ที่มีลักษณะพิเศษ ออกแบบตามความต้องการ
- **OTC Derivatives**: ไม่ได้ซื้อขายในตลาด มักไม่ต้องจ่าย margin แต่มี counterparty risk

### 3.11 ความเสี่ยงในตลาดหุ้น

- **Systematic Risk**: ความเสี่ยงที่กระทบตลาดทั้งหมด (เช่น แผ่นดินไหว สงคราม) ใช้ Index Futures ป้องกัน
- **Non-systematic Risk**: ความเสี่ยงเฉพาะหุ้นแต่ละตัว ใช้ Diversification ป้องกัน

### 3.12 Leverage ในอนุพันธ์

การซื้อ Futures หรือ Options ไม่ต้องลงทุนเต็มมูลค่าสัญญา ใช้แค่ Initial Margin ทำให้มี leverage สูง ทั้งผลตอบแทนและความเสี่ยงเพิ่มขึ้นอย่างมีนัยสำคัญ

### 3.13 การกำหนดราคา Futures

ราคา Futures ถูกกำหนดโดย **Cost of Carry Model**:
**Futures Price = Spot Price + Interest (Cost of Carry)**

ถ้าราคาในตลาดไม่ตรงกับโมเดล Arbitrageur จะเข้ามาทำกำไร

### 3.14 Risk Management

กระบวนการจัดการความเสี่ยงประกอบด้วย:
1. ระบุความเสี่ยง
2. ตัดสินใจว่าจะให้เครดิตลูกค้าเท่าใด
3. ตัดสินใจความถี่ในการเก็บ margin
4. ตัดสินใจว่าความเสี่ยงระดับใดยอมรับได้
5. ควบคุมและติดตามความเสี่ยงอย่างต่อเนื่อง

### 3.15 Short Selling

การ Short Selling คือการขายหุ้นที่ยังไม่มีอยู่ในมือ ในตลาด Futures (แบบ Cash Settlement) การ short selling ทำได้อย่างเสรี

### 3.16 Open Interest

Open Interest คือจำนวนธุรกรรมที่ยังไม่ได้ปิด ณ สิ้นวัน ระดับ Open Interest สะท้อน **ความลึกของตลาด**

### 3.17 Beta และ Hedge Ratio

- **Beta**: วัดความอ่อนไหวของหุ้นต่อดัชนี ถ้า Beta = 1.15 หุ้นจะเคลื่อนไหว 1.15 เท่าของดัชนี
- **Hedge Ratio**: จำนวนสัญญา Futures ที่ต้องขายเพื่อ hedging สมบูรณ์

### 3.18 Volatility

Volatility คือระดับความผันผวนของราคา ไม่เกี่ยวกับทิศทาง ระดับ Volatility กำหนดระดับ Margin: สูง = Margin สูง, ต่ำ = Margin ต่ำ

**Annual Volatility = Daily Volatility x 16** (จำนวนวันทำการประมาณ 256 วัน หรือ รากที่ 256)

---

## บทที่ 3 (ต่อ): Options

### 3.19 ความหมายของ Option

Option คือสัญญาที่ผู้ขาย (Writer) ให้สิทธิ์ผู้ซื้อ (Holder) ในการซื้อหรือขายสินค้าอ้างอิงในราคาที่กำหนดไว้ล่วงหน้า

- **Call Option**: สิทธิ์ในการซื้อ
- **Put Option**: สิทธิ์ในการขาย

### 3.20 ศัพท์สำคัญใน Options

- **Strike/Exercise Price**: ราคานัดหมายสำหรับการซื้อขาย
- **Exercise Date**: วันที่สัญญาครบกำหนด
- **Expiration Period**: ระยะเวลาที่สามารถใช้สิทธิ์ได้
- **Option Premium**: ราคาที่ผู้ซื้อต้องจ่าย
- **Expiration Cycle**: รอบการหมดอายุ

### 3.21 American vs. European Options

- **American Options**: ใช้สิทธิ์ได้ทุกวันจนถึงวันหมดอายุ
- **European Options**: ใช้สิทธิ์ได้เฉพาะวันหมดอายุเท่านั้น

ในอินเดีย Index Options เป็น European style, Stock Options เป็น American style

### 3.22 มูลค่าของ Option (Components of Option Value)

มูลค่า Option ประกอบด้วย 2 ส่วน:
- **Intrinsic Value**: กำไรที่จะได้รับถ้าใช้สิทธิ์ทันที (ไม่ติดลบ)
- **Time Value**: มูลค่าเพิ่มเติมนอกเหนือจาก Intrinsic Value

**Total Option Value = Intrinsic Value + Time Value**

### 3.23 ปัจจัยที่มีผลต่อมูลค่า Option

ปัจจัย 6 ประการ:

| ปัจจัย | Call Option | Put Option |
|--------|-------------|------------|
| ราคาตลาดสูงขึ้น | มูลค่าเพิ่ม | มูลค่าลดลง |
| Strike Price สูงขึ้น | มูลค่าลดลง | มูลค่าเพิ่ม |
| Volatility สูงขึ้น | มูลค่าเพิ่ม | มูลค่าเพิ่ม |
| เวลาเหลือมากขึ้น | มูลค่าเพิ่ม | มูลค่าเพิ่ม |
| อัตราดอกเบี้ยสูงขึ้น | มูลค่าเพิ่ม | มูลค่าลดลง |
| เงินปันผลสูงขึ้น | มูลค่าลดลง | มูลค่าเพิ่ม |

### 3.24 The Greeks

- **Delta**: อัตราการเปลี่ยนแปลงของราคา Option ต่อการเปลี่ยนแปลง 1 หน่วยของราคาสินค้าอ้างอิง สะท้อน Hedge Ratio
- **Gamma**: อัตราการเปลี่ยนแปลงของ Delta ต่อการเปลี่ยนแปลง 1 หน่วยของราคาสินค้าอ้างอิง
- **Theta**: อัตราการเปลี่ยนแปลงมูลค่า portfolio ต่อการผ่านไปของเวลา (Time Decay)
- **Vega**: อัตราการเปลี่ยนแปลงมูลค่า portfolio ต่อการเปลี่ยนแปลง Volatility
- **Rho**: อัตราการเปลี่ยนแปลงมูลค่า portfolio ต่อการเปลี่ยนแปลงอัตราดอกเบี้ย

### 3.25 In the Money, At the Money, Out of the Money

- **In the Money (ITM)**: Option ที่ถ้าใช้สิทธิ์จะมีกำไร (มี Intrinsic Value > 0)
- **At the Money (ATM)**: Strike Price เท่ากับราคาตลาด
- **Out of the Money (OTM)**: Option ที่ถ้าใช้สิทธิ์จะไม่มีกำไร (มี Intrinsic Value = 0)

### 3.26 Delta แบบตัวเลข

ถ้า Delta = 0.6 และราคาสินค้าอ้างอิงขึ้น 20 บาท ราคา Option จะขึ้นประมาณ 12 บาท (0.6 x 20)

### 3.27 Time Decay

เมื่อหมดอายุ มูลค่า Option จะเป็นศูนย์ (ถ้า OTM) หรือเท่ากับ Intrinsic Value (ถ้า ITM) Option จึงถูกเรียกว่า "Wasting Assets"

### 3.28 แบบจำลองการ定价 Option

- **Binomial Model**: สมมติว่าราคาหุ้น follow Binomial Distribution
- **Black-Scholes Model**: สมมติว่าราคาหุ้น follow Log-Normal Distribution

### 3.29 Profit Profile

- **ผู้ซื้อ**: กำไรไม่จำกัด ขาดทุนจำกัด (เท่ากับ Premium)
- **ผู้ขาย**: กำไรมากจำกัด (เท่ากับ Premium) ขาดทุนไม่จำกัด

### 3.30 มุมมองการเก็งกำไร

- Call Buyer: Bullish
- Call Seller: Bearish หรือ Neutral
- Put Buyer: Bearish
- Put Seller: Bullish หรือ Neutral

### 3.31 Option Strategies

กลยุทธ์ที่พบบ่อย:
- **Covered Call**: ถือหุ้นแล้วขาย Call
- **Protective Put**: ถือหุ้นแล้วซื้อ Put
- **Bull Spread**: ซื้อ Call ต่ำ ขาย Call สูง
- **Bear Spread**: ซื้อ Call สูง ขาย Call ต่ำ
- **Straddle**: ซื้อ Call และ Put Strike เดียวกัน
- **Strangle**: ซื้อ Call และ Put Strike ต่างกัน

---

## บทที่ 4: Trading, Clearing และ Settlement

### 4.1 Trading Rules

การซื้อขายอนุพันธ์ที่ BSE ดำเนินการผ่านระบบ **DTSS (Derivatives Trading and Settlement System)** ซึ่งเป็นระบบอัตโนมัติแบบ screen-based

### 4.2 ประเภทของ Order

- **Limit Order**: ซื้อขายที่ราคาที่กำหนดหรือดีกว่า
- **Market Order**: ซื้อขายที่ราคาที่ดีที่สุดในตลาด
  - **Partial Fill Rest Kill (PF)**: ดำเนินการจำนวนที่มีแล้วยกเลิกส่วนที่เหลือ
  - **Partial Fill Rest Convert (PC)**: ดำเนินการจำนวนที่มีแล้วแปลงส่วนที่เหลือเป็น Limit Order
- **Stop Loss**: Order ที่จะกลายเป็น Limit Order เมื่อราคาถึงระดับที่กำหนด

### 4.3 Order Retention Types

- **GFD (Good For Day)**: มีอายุแค่วันนั้น
- **GTD (Good Till Date)**: มีอายุตามจำนวนวันที่กำหนด
- **GTC (Good Till Cancelled)**: อยู่ในระบบจนกว่าจะยกเลิกหรือหมดอายุ

### 4.4 Clearing Entities

- **Clearing Members**: สมาชิกที่รับผิดชอบการ clearing และ settlement
  - Self Clearing Member
  - Trading-cum-Clearing Member
  - Professional Clearing Member
- **Clearing Banks**: ธนาคารสำหรับการ settlement ของกองทุน

### 4.5 Settlement Mechanism

ทุกสัญญา Futures และ Options ใช้ **Cash Settlement** (จ่ายเป็นเงินสด) ในปัจจุบัน

**Futures Settlement**:
- **MTM Settlement**: ทำทุกวัน คิดกำไร/ขาดทุนจาก price movement
- **Final Settlement**: วันสุดท้ายที่หมดอายุ

**Options Settlement**:
- **Daily Premium Settlement**: ผู้ซื้อจ่าย premium ผู้รับ premium
- **Exercise Settlement**: เมื่อผู้ซื้อใช้สิทธิ์
- **Interim Exercise Settlement**: สำหรับ Stock Options ใช้สิทธิ์ได้ระหว่างอายุ
- **Final Exercise Settlement**: อัตโนมัติสำหรับ ITM options วันหมดอายุ

### 4.6 Adjustments for Corporate Actions

การปรับเงื่อนไขสัญญาเมื่อมี Corporate Actions เช่น:
- Bonus, Rights, Merger/Demerger, Amalgamation, Splits
- Cash Dividend
- ปรับได้ทั้ง Strike Price, Position, Market Lot/Multiplier

### 4.7 Risk Management

#### Stock Futures
- **Initial Margin**: 基于 Worst Case Scenario Loss ครอบคลุม 99% VaR 1 วัน
  - ใช้ Price Scan Range = 3.5 sigma (sigma = daily volatility)
  - Minimum Margin = 7.5% ของ notional value
- **Calendar Spread**: ได้ส่วนลด margin สำหรับ spread position
- **Exposure Limit**: ไม่เกิน 20 เท่าของ Liquid Net Worth
- **Position Limits**: Market Wide, Trading Member Level, Client Level, FII Level

#### Stock Options
- ใช้ Portfolio-based margining model
- Price Scan Range = 3.5 sigma, Volatility Scan Range = 10%
- Short Option Minimum Margin = 7.5% ของ notional value
- ใช้ Black-Scholes Model ในการ估值

#### Index Futures
- Price Scan Range = 3 sigma
- Minimum Margin = 5% ของ notional value
- Exposure Limit = 33 1/3 เท่าของ Liquid Net Worth

#### Index Options
- Price Scan Range = 3 sigma, Volatility Scan Range = 4%
- Short Option Minimum Margin = 3% ของ notional value
- ใช้ Black-Scholes Model

### 4.8 Position Limits

Position Limits มีหลายระดับ:
1. **Market Level**: 20% ของ free float
2. **Trading Member Level**: 20% ของ MWPL หรือ Rs. 300 crore (แล้วแต่ค่าใดต่ำกว่า)
3. **Client Level**: 1% ของ free float market cap หรือ 5% ของ Open Interest
4. **FII Level**: เหมือน Trading Member Level
5. **Sub-account Level, NRI Level, Mutual Fund Level**: มีข้อจำกัดเฉพาะ

---

## บทที่ 5: Regulatory Framework

### 5.1 Securities Contracts (Regulation) Act, 1956 [SC(R)A]

กฎหมายหลักที่กำกับดูแลการซื้อขายหลักทรัพย์ในอินเดีย นิยาม "securities" รวมถึงอนุพันธ์

Section 18A กำหนดว่าสัญญาอนุพันธ์จะถูกกฎหมายและมีผลบังคับใช้ถ้า:
- ซื้อขายในตลาดหลักทรัพย์ที่ได้รับการยอมรับ
- Settlement ผ่าน clearing house ของตลาดหลักทรัพย์

### 5.2 SEBI Act, 1992

จัดตั้ง Securities and Exchange Board of India (SEBI) ด้วยอำนาจทางกฎหมายเพื่อ:
- คุ้มครองผลประโยชน์ของนักลงทุน
- ส่งเสริมการพัฒนาตลาดหลักทรัพย์
- กำกับดูแลตลาดหลักทรัพย์

### 5.3 Regulation for Derivatives Trading

SEBI ตั้งคณะกรรมการ Dr. L. C. Gupta 24 คน เพื่อพัฒนาระบบการกำกับดูแลการซื้อขายอนุพันธ์

ข้อกำหนดสำคัญ:
1. ตลาดหลักทรัพย์ต้องมี Governing Council แยกต่างหาก
2. สมาชิกตลาดหลักทรัพย์ต้องมีอย่างน้อย 50 ราย
3. สมาชิกเดิมไม่ได้เป็นสมาชิกอนุพันธ์อัตโนมัติ
4. Clearing ต้องผ่าน Clearing Corporation ที่ SEBI อนุมัติ
5. สมาชิก clearing ต้องมี Net Worth อย่างน้อย Rs. 300 Lakh
6. มูลค่าสัญญาขั้นต่ำ Rs. 2 Lakh
7. ต้องมี Risk Disclosure Document และ Know Your Customer Rule

### 5.4 Accounting for Derivatives

#### 5.4.1 การบัญชี Futures

ICAI ออกแนวปฏิบัติการบัญชี Futures ดังนี้:

**เมื่อเริ่มสัญญา**:
- บันทึก Initial Margin เป็น Current Asset

**Daily Settlement**:
- บันทึก MTM margin เข้า/ออกจากบัญชี MTM margin

**Open Positions ณ วันงบดุล**:
- สร้าง Provision สำหรับกำไร/ขาดทุนที่คาดการณ์ตามหลักความระมัดระวัง
- Debit balance แสดงเป็น Current Asset หัก provision
- Credit balance แสดงเป็น Current Liability

**Final Settlement**:
- คำนวณกำไร/ขาดทุนจากความแตกต่างระหว่าง Final Settlement Price กับ Contract Price
- ใช้ FIFO method สำหรับหลายสัญญา

**Default**:
- ปรับ Initial Margin กับ MTM margin ที่ค้างชำระ
- ส่วนเกินเป็นหนี้สิน Current Liability

#### 5.4.2 การบัญชี Options

**ผู้ขาย (Writer)**:
- จ่าย Initial Margin → บันทึกเป็น Current Asset
- รับ Premium → บันทึกเป็น Current Asset

**ผู้ซื้อ (Holder)**:
- จ่าย Premium → บันทึกเป็น Current Asset
- ไม่ต้องจ่าย Margin

**Open Positions ณ วันงบดุล**:
- สร้าง Provision สำหรับกำไร/ขาดทุนที่อาจเกิดขึ้น
- กำไร/ขาดทุนจะrecognized เมื่อใช้สิทธิ์หรือ expire

### 5.5 Taxation of Derivative Transactions

#### 5.5.1 ภาษีกำไร/ขาดทุน

ก่อนปี Financial Year 2005-06: ธุรกรรมอนุพันธ์ถือเป็น speculative transaction

Finance Act 2005 แก้ไข Section 43(5): ธุรกรรมอนุพันธ์ใน recognized stock exchange ไม่ถือเป็น speculative แล้ว
- กำไร/ขาดทุนสามารถ set off กับรายได้อื่นได้
- ขาดทุน carry forward ได้สูงสุด 8 assessment years

#### 5.5.2 Securities Transaction Tax (STT)

อัตรา STT สำหรับอนุพันธ์ (ตั้งแต่ 1 มิถุนายน 2008):
- ขาย Option: 0.017% (จ่ายโดยผู้ขาย)
- ขาย Option ที่ใช้สิทธิ์: 0.125% (จ่ายโดยผู้ซื้อ)
- ขาย Futures: 0.017% (จ่ายโดยผู้ขาย)

---

## ภาคผนวก: แบบฝึกหัดและโจทย์คำนวณ

### ภาคผนวก I: แบบทดสอบ

หนังสือเล่มนี้มีแบบทดสอบ 50 ข้อ ครอบคลุม:
- ประวัติความเป็นมาของ Futures (CBOT, CME)
- ผู้เข้าร่วมตลาด (Hedger, Arbitrageur, Speculator)
- ความเสี่ยงของ OTC Derivatives
- ประเภทของผลิตภัณฑ์อนุพันธ์
- SENSEX (30 หุ้น)
- Impact Cost และ Liquidity
- วิธีคำนวณดัชนีแบบ Market Capitalization Weighted
- Forward vs. Futures
- Tick size
- Hedging ด้วย Beta
- Option Terminology (Call, Put, Strike, Premium)
- The Greeks (Delta, Gamma)
- American vs. European Options
- ITM/ATM/OTM
- Black-Scholes Model
- Option Strategies (Covered Call, Protective Put, Bull Spread)
- SPAN Margining System

### ภาคผนวก II: โจทย์ Options คำนวณ

ตัวอย่างโจทย์พร้อมวิธีแก้:
- คำนวณกำไร/ขาดทุนจากการซื้อ/ขาย Call/Put Options
- คำนวณ Intrinsic Value และ Time Value
- หา Breakeven Point ของ Options
- กำหนด Maximum Gain/Loss สำหรับผู้ซื้อ/ผู้ขาย

### ภาคผนวก III: โจทย์ Margins คำนวณ

ตัวอย่างโจทย์พร้อมวิธีแก้:
- คำนวณ Liquid Net Worth
- คำนวณ Margin Available for Trading
- คำนวณ Initial Margin ของ Futures
- คำนวณ Mark-to-Market Margin
- คำนวณ Calendar Spread Margin
- คำนวณ Outstanding Liability

### ภาคผนวก IV: โจทย์ Futures คำนวณ

ตัวอย่างโจทย์พร้อมวิธีแก้:
- คำนวณกำไร/ขาดทุนจาก Calendar Spread
- คำนวณมูลค่าสัญญา Futures
- คำนวณ Forward Price ของหุ้น
- คำนวณ Hedge Ratio สำหรับ Perfect Hedge
- คำนวณ Futures Price จาก Cost of Carry Model

---

## สรุปประเด็นสำคัญสำหรับการเทรด

1. **Leverage**: อนุพันธ์ให้ leverage สูง ทั้งผลตอบแทนและความเสี่ยงเพิ่มขึ้นมาก
2. **Margin System**: Initial Margin + Daily MTM + Exposure Limit รักษาเสถียรภาพของระบบ
3. **Clearing House**: ตลาดหลักทรัพย์เป็นคู่สัญญา ลด counterparty risk
4. **Cost of Carry Model**: ใช้กำหนดราคา Futures ที่เหมาะสม
5. **Black-Scholes Model**: ใช้ในการ定价 Options
6. **The Greeks**: Delta, Gamma, Theta, Vega, Rho ช่วยเข้าใจพฤติกรรมของ Options
7. **Hedging**: ใช้ Futures/Options เพื่อลดความเสี่ยง ไม่ใช่เพื่อทำกำไรสูงสุด
8. **Position Limits**: ป้องกันการผูกขาดและรักษาเสถียรภาพของตลาด
9. **Accounting Treatment**: ต้องบันทึกอย่างถูกต้องตาม ICAI guidelines
10. **Taxation**: หลัง Finance Act 2005 กำไร/ขาดทุนอนุพันธ์ใน recognized exchange ไม่ถือเป็น speculative
