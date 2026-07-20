# Technical Analysis of the Financial Markets - บทสรุปภาษาไทย

> **ผู้เขียน:** John J. Murphy (1999)
> **แหล่งอ้างอิงหลักของ MTA สำหรับการสอบ Chartered Market Technician (CMT)**
> **เดิมชื่อ "Technical Analysis of the Futures Markets" (1985) แล้วขยายเป็น覆盖ทุกตลาดการเงิน**

---

## สารบัญ

1. ปรัชญาของ Technical Analysis
2. Dow Theory
3. Chart Construction
4. Basic Concepts of Trend
5. Major Reversal Patterns
6. Continuation Patterns
7. Volume and Open Interest
8. Long-Term Charts
9. Moving Averages
10. Oscillators and Contrary Opinion
11. Point and Figure Charting
12. Japanese Candlesticks
13. Elliott Wave Theory
14. Time Cycles
15. Computers and Trading Systems
16. Money Management and Trading Tactics
17. Intermarket Analysis
18. Stock Market Indicators
19. Pulling It All Together
- Appendix A: Advanced Technical Indicators
- Appendix B: Market Profile
- Appendix C: Building a Trading System
- Appendix D: Continuous Futures Contracts

---

## 1. ปรัชญาของ Technical Analysis (Philosophy)

### นิยาม
Technical Analysis คือ **การศึกษาการกระทำของตลาด (market action) โดยใช้กราฟเป็นหลัก เพื่อทำนายทิศทางราคาในอนาคต** ข้อมูลที่ใช้มี 3 แหล่งหลัก: **Price, Volume, และ Open Interest**

### หลักการสำคัญ 3 ข้อ (Three Premises)

1. **Market Action Discounts Everything** - ราคาสะท้อนทุกสิ่งที่มีผลต่อตลาดแล้ว ไม่ว่าจะเป็นปัจจัยพื้นฐาน จิตวิทยา นโยบายการเงิน หรือเหตุการณ์ทางการเมือง ทุกอย่างถูก discount เข้าไปในราคาแล้ว ดังนั้นการศึกษาราคาเพียงอย่างเดียวก็เพียงพอ ถ้า demand > supply ราคาก็ขึ้น ถ้า supply > demand ราคาก็ตก ไม่ต้องรู้เหตุผล specific

2. **Prices Move in Trends** - ราคามีแนวโน้ม (trend) และแนวโน้มนั้นมีแนวโน้มที่จะดำเนินต่อไปเรื่อยๆ เหมือนกฎ Newton ข้อที่ 1 ว่า "วัตถุที่เคลื่อนที่จะยังคงเคลื่อนที่ต่อไปจนกว่ามีแรงมาหยุด" วัตถุประสงค์หลักของการ chart คือการ identify trend ตั้งแต่เนิ่นๆ เพื่อเทรดตาม trend

3. **History Repeats Itself** - รูปแบบบนกราฟ (chart patterns) เกิดจากจิตวิทยามนุษย์ซึ่งไม่ค่อยเปลี่ยน ดังนั้นรูปแบบเดิมๆ จึงมักเกิดซ้ำแล้วซ้ำเล่า อนาคตคือการซ้ำรอยอดีต

### Technical vs. Fundamental Analysis

- **Fundamentalist** ศึกษา "สาเหตุ" (cause) ที่ทำให้ราคาเคลื่อนไหว วิเคราะห์ supply/demand ที่แท้จริงหา intrinsic value
- **Technician** ศึกษา "ผลลัพธ์" (effect) ดูว่าราคาทำอะไรอยู่ ไม่ต้องรู้ว่าทำไม
- **Murphy เน้นว่า**: Technical Analysis รวม Fundamental ไว้แล้ว (เพราะราคาสะท้อนทุกอย่าง) แต่ Fundamental ไม่รวม Technical ถ้าต้องเลือกอย่างใดอย่างหนึ่ง ให้เลือก Technical
- ราคา市场มัก lead ข้อมูล known fundamentals เสมอ ข้อมูลใหม่ๆ ถูก discount เข้าไปในราคาก่อนที่จะเป็นที่รู้จัก

### Analysis vs. Timing
- **Analysis** = วิเคราะห์ทิศทางตลาด (ใช้ทั้ง Fundamental และ Technical ได้)
- **Timing** = กำหนดจังหวะเข้า-ออกที่แม่นยำ (เป็นเรื่อง **Technical ล้วนๆ**)
- ในตลาด Futures ที่มี leverage >90% margin requirement แม้วิเคราะห์ทิศทางถูก แต่ timing ผิดก็อาจถูก force out ได้

### ข้อวิจารณ์ Self-Fulfilling Prophecy
- นักวิจารณ์บอกว่าถ้าคนส่วนใหญ่เห็น pattern เดียวกัน ทุกคนจะเข้าพร้อมกัน ทำให้ราคาเคลื่อนไหวตาม pattern เอง
- Murphy ตอบว่า: Charting เป็น "ศิลปะ" (subjective) ไม่ใช่ทุกคนจะเห็นเหมือนกัน บางคนเข้าก่อน breakout บางคนรอ pullback บางคนใช้ stop order บางคนใช้ limit order การกระทำจึงไม่พร้อมกัน
- แม้ self-fulfilling prophecy จะเป็นปัญหา ก็จะเป็น self-correcting เพราะเทรดเดอร์จะปรับตัว

### Random Walk Theory
- ทฤษฎีนี้อ้างว่าราคาก้าวอย่างเป็นอิสระ (serially independent) ไม่มีรูปแบบ และไม่สามารถทำนายได้ กลยุทธ์ที่ดีที่สุดคือ "buy and hold"
- Murphy ตอบว่า: ความจริงที่นักวิชาการหลายคนไม่พบ pattern ไม่ได้พิสูจน์ว่าไม่มี pattern อยู่ ECG ก็เหมือนสุ่มสำหรับคนที่ไม่เข้าใจ แต่หมอเห็นความหมาย
- แม้แต่ efficient market hypothesis ก็ยอมรับว่าราคา reflect ข้อมูลทั้งหมดแล้ว ซึ่งเป็น core premise เดียวกับ Technical Analysis
- Behavioral Finance ที่กำลังเติบโตในมหาวิทยาลัยชั้นนำก็ยอมรับว่าจิตวิทยามนุษย์เชื่อมโยงกับ pricing ซึ่งเป็น foundation ของ Technical Analysis

### ความแตกต่าง Stocks vs. Futures
- **Pricing Structure**: Futures ซับซ้อนกว่า (หน่วย报价ต่างกัน)
- **Limited Life Span**: Futures มีวันหมดอายุ ต้อง rotate สัญญาใหม่เรื่อยๆ
- **Leverage**: Futures margin <10% (leverage สูงมาก) ทำให้ timing สำคัญกว่า
- **Time Horizon**: Futures trader ต้องการรู้ว่าราคาจะเป็นอย่างไรสัปดาห์หน้า หรือพรุ่งนี้ ไม่ใช่ 3-6 เดือนข้างหน้า
- **Moving Average**: Stocks ใช้ 50-200 วัน Futures ใช้ 4-18 วัน

### ประเภทของนัก Technical Analysis
- **Traditional Chartist** = ใช้กราฟเป็นหลัก เป็น "art"
- **Statistical/Quantitative Technician** = สร้าง trading systems เป็นกลไก (mechanical) ลด subjective element
- ทุก chartist เป็น technician แต่ไม่ใช่ทุก technician เป็น chartist

---

## 2. Dow Theory

### ประวัติ
- **Charles Dow** ก่อตั้ง Dow Jones & Company ปี 1882
- ตีพิมพ์ stock market average ตัวแรก ปี 1884 (หุ้น 11 ตัว: 9 railroad + 2 manufacturing)
- ปี 1897 แบ่งเป็น 2 indices: Industrial (12 หุ้น) และ Rail (20 หุ้น)
- ปี 1928 Industrial ขยายเป็น 30 หุ้น (เท่าที่เป็นอยู่ทุกวันนี้)
- Dow Theory เป็น **รากฐานของ Technical Analysis ทั้งหมด** แม้จะผ่านมานานกว่า 100 ปี

### หลัก 6 ข้อของ Dow Theory

1. **The Averages Discount Everything** - ค่าเฉลี่ยตลาดสะท้อนทุกอย่างที่รู้ทั้งหมดแล้ว รวมแม้แต่ "acts of God" (แผ่นดินไหว) ตลาดจะ discount ทันที

2. **The Market Has Three Trends** (เปรียบกับน้ำทะเล):
   - **Primary trend (tide)** = กระแสน้ำหลัก อยู่นานหลายปี
   - **Secondary/Intermediate trend (waves)** = คลื่น อยู่ 3 สัปดาห์ถึง 3 เดือน ปรับตัว 1/3 ถึง 2/3 ของ trend ก่อนหน้า (มักประมาณ 50%)
   - **Minor trend (ripples)** = คลื่นเล็กๆ อยู่น้อยกว่า 3 สัปดาห์

3. **Major Trends Have Three Phases**:
   - **Phase 1 - Accumulation**: นักลงทุนที่เชี่ยวชาญที่สุดเริ่มซื้อเมื่อทุกคนกลัว "ข่าวร้ายหมดแล้ว" (ตรงข้ามกับ mass psychology)
   - **Phase 2 - Public Participation**: ราคาพุ่งขึ้นเร็ว ข่าวดีเริ่มมา technical traders เริ่มเข้า
   - **Phase 3 - Distribution**: ข่าวดีเต็มหน้าหนังสือพิมพ์ speculative volume สูง คนที่ accumulate ตั้งแต่ Phase 1 เริ่มขายให้คนที่เพิ่งเข้ามา
   - *นัก Elliott Wave จะสังเกตว่า 3 phases นี้เหมือนกับ 5-wave sequence*

4. **The Averages Must Confirm Each Other** - ทั้ง Industrial และ Transportation ต้อง confirm กัน ถ้าตัวหนึ่งขึ้น new high อีกตัวต้องขึ้นด้วย ถ้า divergence กัน = trend เดิมยังทำงานอยู่

5. **Volume Must Confirm the Trend** - Volume ต้อง expand ตามทิศทางหลัก ถ้าราคาขึ้น volume ต้องเพิ่ม ถ้าราคาลง volume ต้องเพิ่มในขาลง

6. **A Trend Is Assumed to Be in Effect Until Definite Signals of Reversal** - สมมติว่าแนวโน้มยังทำงานอยู่จนกว่าจะมีหลักฐานกลับตัวชัดเจน (ใช้ support/resistance, price patterns, trendlines, moving averages ในการจับ reversal)

### Failure Swing vs. Nonfailure Swing
- **Failure Swing**: ราคา fail ที่จะ break previous high แล้วลงต่ำกว่า previous low = sell signal ชัดเจน
- **Nonfailure Swing**: ราคา break previous high ก่อนลงต่ำกว่า previous low = สัญญาณไม่ชัดเจน บางคนอาจรอ lower high อีกที

### ข้อจำกัดของ Dow Theory
- **ช้า**: miss ช่วงต้นของ trend ไป 20-25% (ประมาณที่ trend-following systems ส่วนใหญ่เริ่ม identify)
- **ไม่ใช่จุด top/bottom**: Dow ตั้งใจ capture "ส่วนกลาง" ของ major move ไม่ใช่จุดสุดยอด
- จากปี 1920-1975: Dow Theory signals capture 68% ของ moves ใน Industrials/Transportation และ 67% ใน S&P 500

### Dow Theory สำหรับ Futures
- Dow ให้ความสำคัญกับ major trend แต่ Futures traders ต้องให้ความสำคัญกับ **minor trend ด้วย** เพราะใช้ intermediate trend เป็นหลัก
- Minor dips ใน intermediate uptrend = จังหวะซื้อ Minor bounces ใน intermediate downtrend = จังหวะขาย

### Stocks เป็น Economic Indicators
- Dow ไม่ได้ตั้งใจใช้ theory ทำนายหุ้น แต่ใช้หุ้นเป็น barometer ของเศรษฐกิจ
- หุ้นเป็น leading economic indicator อย่างเป็นทางการ

---

## 3. Chart Construction (โครงสร้างกราฟ)

### ประเภทกราฟที่ใช้

1. **Bar Chart** - แสดง OHLC (Open, High, Low, Close) ของแต่ละวันเป็นแท่งแนวตั้ง
   - เส้นซ้ายแนวนอน = Open, เส้นขวาแนวนอน = Close
   - ใช้กันมากที่สุดใน Technical Analysis

2. **Line Chart** - วาดแค่จุด Close แล้วเชื่อมต่อกันเป็นเส้นต่อเนื่อง
   - บางคนเชื่อว่า Close คือราคาที่สำคัญที่สุด

3. **Point & Figure (P&F) Chart** - แสดงเป็นคอลัมน์ X (ราคาขึ้น) และ O (ราคาลง)
   - ไม่สนใจ time axis จริงๆ focus ที่ price movement
   - Buy/Sell signals ชัดเจนกว่า bar chart

4. **Japanese Candlestick** - เวอร์ชันญี่ปุ่นของ bar chart
   - แสดง OHLC เหมือนกัน แต่ different visual
   - Real Body = ส่วนระหว่าง Open-Close (ถ้า Close > Open = ขาว/bullish, Close < Open = ดำ/bearish)
   - Shadow = เส้นบางๆ สูง-ต่ำของวัน
   - ให้ความสำคัญกับความสัมพันธ์ระหว่าง Open กับ Close

### Arithmetic vs. Logarithmic Scale
- **Arithmetic (linear)** = ระยะห่างเท่ากันทุกหน่วยราคา (เช่น จาก 5 ถึง 6 เท่ากับจาก 50 ถึง 51)
- **Logarithmic (ratio)** = ระยะห่างเท่ากันสำหรับ % เปลี่ยนแปลงเท่ากัน (เช่น จาก 5 ถึง 10 เท่ากับจาก 50 ถึง 100 เพราะทั้งคู่เพิ่ม 100%)
- สำหรับ long-term trend analysis log scale มีประโยชน์กว่า

### โครงสร้าง Daily Bar Chart
- แกน Y = ราคา แกน X = เวลา
- วาดแท่งแนวตั้งจาก high ถึง low ของวัน (daily range)
- เส้นซ้าย = Open, เส้นขวา = Close
- วันเสาร์-อาทิตย์ไม่แสดง (weekdays only)
- Volume = แท่งแนวตั้งเล็กๆ ที่ด้านล่างของกราฟ

### Volume
- จำนวนสัญญา futures หรือหุ้นที่ซื้อขายในวันนั้น
- แสดงเป็นแท่งแนวตั้งที่ด้านล่างของกราฟ (แท่งสูง = volume หนัก)

### Open Interest (เฉพาะ Futures/Options)
- จำนวนสัญญา futures ที่ outstanding ณ สิ้นวัน (ไม่ใช่ total ของ long + short แต่ด้านใดด้านหนึ่ง)
- แสดงเป็นเส้นต่อเนื่องที่ด้านล่างของกราฟ
- **สำคัญ**: ต้องดู total volume/open interest ไม่ใช่ individual delivery month เพราะ contract lifecycle สร้าง noise

### Weekly and Monthly Bar Charts
- **Weekly chart**: 1 แท่ง = 1 สัปดาห์ (วิเคราะห์ได้ ~5 ปี ย้อนหลัง)
- **Monthly chart**: 1 แท่ง = 1 เดือน (วิเคราะห์ได้ ~20 ปี ย้อนหลัง)
- ใช้สำหรับ long-term trend analysis ที่ daily chart มองไม่เห็น

---

## 4. Basic Concepts of Trend (แนวคิดพื้นฐานของแนวโน้ม)

### นิยาม Trend
- **Uptrend** = series of successively higher peaks and troughs (จุดสูงและจุดต่ำสูงขึ้นเรื่อยๆ)
- **Downtrend** = series of successively lower peaks and troughs
- **Sideways/Horizontal** = peaks and troughs อยู่ในแนวนอน = "trendless" (เกิดประมาณ 1/3 ของเวลา)

### Trend มี 3 ทิศทาง
- ขึ้น, ลง, หรือข้าง (sideways)
- **สำคัญ**: Trend-following systems ทำงานแย่มากในช่วง sideways market ความล้มเหลวไม่ได้อยู่ที่ system แต่อยู่ที่เทรดเดอร์ที่เอา system ที่ออกแบบสำหรับ trending market ไปใช้ใน nontrending market

### 3 Classifications ของ Trend (Dow Theory)
- **Major trend** = >6 เดือน (Dow บอก >1 ปี)
- **Intermediate trend** = 3 สัปดาห์ถึง 3 เดือน
- **Near-term trend** = <2-3 สัปดาห์
- ทุก trend เป็นส่วนหนึ่งของ trend ที่ใหญ่กว่า เช่น intermediate trend อาจเป็น correction ของ major uptrend แต่ตัวมันเองก็มี minor waves อยู่ด้วย

### Support and Resistance
- **Support** = ระดับ/พื้นที่ใต้ราคาที่มีแรงซื้อมากพอจะหยุดการตก (previous reaction lows)
- **Resistance** = ระดับ/พื้นที่เหนือราคาที่มีแรงขายนามพอจะหยุดการขึ้น (previous peaks)
- **Reverse Roles**: เมื่อ support ถูก break อย่างมีนัยสำคัญ → support กลายเป็น resistance และ vice versa

### จิตวิทยาของ Support/Resistance
- เมื่อราคาอยู่เหนือ support: longs ดีใจ, shorts เสียใจ, uncommitted รอซื้อ dip → ทุกคน "buy the next dip" = support
- เมื่อราคาหลุด support: longs ติด loss, broker เรียก margin, ต้อง force sell → buy orders กลายเป็น sell orders over the market = resistance

### ความสำคัญของ Support/Resistance
- **เวลาที่ใช้**: trading ยิ่งนาน zone ยิ่งแข็ง
- **Volume**: ยิ่ง volume สูง ณ support/resistance ยิ่งสำคัญ
- **ความสด (recency)**: ยิ่งใหม่ ยิ่ง potent
- **Round Numbers**: 10, 20, 50, 100, 1000 มักเป็น support/resistance ทางจิตวิทยา หลีกเลี่ยงการวาง stop order ที่ round number

### Trendlines
- **Up trendline** = ลากใต้ successive reaction lows
- **Down trendline** = ลากเหนือ successive rally highs
- **2 จุด** = วาด trendline ได้, **3 จุด** = confirm ว่า trendline ใช้ได้จริง
- ยิ่ง touched หลายครั้ง + อยู่นาน = ยิ่งสำคัญ
- **Slant angle**: trendline ที่ดีมักอยู่ประมาณ 45 องศา (สมดุลระหว่าง price กับ time)
- **Break of trendline** = สัญญาณเปลี่ยน trend ที่ดีมาก
- **3% Rule**: สำหรับ trendline ระยะยาว ต้อง break อย่างน้อย 3% จึงจะ valid
- **2 Day Rule**: ราคาต้องปิดนอก trendline 2 วันติดต่อกัน
- Trendlines **reverse roles** เมื่อถูก break (support line กลายเป็น resistance)

### Fan Principle
- หลัง break trendline ราคาจะ rally กลับไปทดสอบ old trendline (ตอนนี้เป็น resistance)
- ลาก trendline ใหม่ผ่าน peak ใหม่ได้ (line 2) → break อีก → line 3
- **Breaking of the 3rd trendline** = สัญญาณกลับ trend ที่แท้จริง

### The Importance of Number Three
- 3 phases ของ bull market (Dow Theory + Elliott Wave)
- 3 types of gaps
- 3 ระดับของ trend (major, intermediate, minor)
- 3 ทิศทางของ trend (up, down, sideways)
- 3 ประเภทของ triangle (symmetrical, ascending, descending)
- 3 แหล่งข้อมูล (price, volume, open interest)

### Channel Lines
- **Uptrend**: วาด up trendline ใต้ lows แล้วลากเส้นคู่ขนาน (channel line) ผ่าน peak แรก
- **Downtrend**: ทำ reverse
- **ใช้**: Trendline = ซื้อ, Channel line = ขายทำกำไร
- **Failure to reach channel line** = สัญญาณเตือนว่า trend กำลังอ่อน → มักจะ break trendline หลัก
- **Breaking of channel line** (ขึ้นเหนือ) = signal trend acceleration

### Percentage Retracements
- หลัง market move ราคามักจะ retrace กลับมาส่วนหนึ่งก่อนจะไปต่อ
- **50% retracement** = ที่รู้จักกันมากที่สุด ( Dow Theory)
- **Minimum = 33%, Maximum = 66%** (Dow Theory)
- **Fibonacci: 38% และ 62%**
- Murphy แนะนำใช้ combined zone: **33-38% (minimum) ถึง 62-66% (maximum)**
- ถ้า price retrace เกิน 66% → มีโอกาสสูงที่ trend จะกลับตัว (ไม่ใช่แค่ retracement)
- Gann ใช้ eighths: 1/8, 2/8 ... 8/8 โดยให้ความสำคัญกับ 3/8 (38%), 4/8 (50%), 5/8 (62%)

### Speed Resistance Lines
- วัด **rate of ascent/descend** (speed) ของ trend ไม่ใช่แค่ราคา
- วิธี: ลาก vertical line จากจุดสูงสุดถึงต้น trend แล้วแบ่งเป็น 3 ส่วน ลาก trendline ผ่านจุด 1/3 และ 2/3
- ใช้เป็น support ระหว่าง correction ถ้า break ทีละเส้น → ราคาจะลง/ขึ้นไปที่เส้นถัดไป

### Reversal Days
- **Top Reversal Day**: new high ตามด้วย close ต่ำกว่าเมื่อวาน (มักเกิดตอนเปิดตลาด)
- **Bottom Reversal Day (Selling Climax)**: new low ตามด้วย close สูงกว่าเมื่อวาน
- ยิ่ง range ใหญ่ + volume หนัก = ยิ่งสำคัญ
- Weekly/Monthly reversals สำคัญกว่า daily มาก

### Price Gaps
3 ประเภทหลัก:

1. **Breakaway Gap** - เกิดตอน breakout จาก price pattern สำคัญ มักเกิดบน heavy volume **มักไม่ถูก fill**
2. **Runaway/Measuring Gap** - เกิดกลางของ trend move (ใช้วัดเป้าหมายราคา) **มักไม่ถูก fill**
3. **Exhaustion Gap** - เกิดปลาย trend move สุดท้าย **มักถูก fill เร็ว**

- "Gaps are always filled" = **ไม่จริง** breakaway/runaway gaps หลายตัวไม่เคยถูก fill
- **Island Reversal** = exhaustion gap + breakaway gap ในทิศตรงข้าม = สัญญาณกลับตัวที่แรง

---

## 5. Major Reversal Patterns (รูปแบบกลับตัวขนาดใหญ่)

### ประเภทของ Patterns
- **Reversal patterns** = สัญญาณกลับ trend
- **Continuation patterns** = สัญญาณ pause แล้วไปต่อในทิศเดิม

### Head and Shoulders Top (H&S Top)
- รูปแบบ reversal ที่รู้จักกันดีที่สุด
- โครงสร้าง: Left Shoulder (peak) → Head (peak สูงกว่า) → Right Shoulder (peak ต่ำกว่า head)
- **Neckline** = เส้น support ใต้ shoulders/head (เชื่อมจุดต่ำระหว่าง left shoulder-head และ head-right shoulder)
- **Sell Signal**: ราคา break ลงใต้ neckline
- **Price Objective**: ระยะจาก head ถึง neckline → ลบลงมาจาก breakout point
- **Volume**: ควรลดลงจาก left → head → right shoulder (สัญญาณ weakening)

### Head and Shoulders Bottom (Inverse H&S)
- ตรงกันข้ามกับ top
- **Buy Signal**: ราคา break ขึ้นเหนือ neckline
- Volume ควรเพิ่มขึ้นตอน breakout

### Triple Tops/Bottoms
- 3 peaks/troughs ที่ระดับใกล้เคียงกัน
- Sell/Buy signal เหมือน H&S แต่ไม่ต้องมี head ที่สูง/ต่ำกว่าอย่างชัดเจน

### Double Tops/Bottoms
- 2 peaks/troughs ที่ระดับใกล้เคียงกัน
- **สำคัญ**: ต้องมี "significant decline" ระหว่าง tops (มักประมาณ 20% สำหรับ stocks) จึงจะเป็น double top จริง
- Double tops ที่แท้จริงหายากกว่าที่คนคิด

### Saucers (Rounding Bottoms)
- รูปแบบกลับตัวช้าๆ ที่ก้นกราฟ ดูเหมือน basin หรือ rounding bottom
- มักใช้เวลาหลายสัปดาห์ถึงหลายเดือน

---

## 6. Continuation Patterns (รูปแบบต่อเนื่อง)

### Symmetrical Triangle
- ราคา converge ลงมา形成รูปสามเหลี่ยม
- Breakout อาจเกิดทั้งขึ้นและลง (ไม่มี bias)
- **Measuring Rule**: วัดความสูงของ triangle ที่ widest point แล้ว project จาก breakout point
- Breakout ที่ดีมักเกิด 1/2 ถึง 2/3 ของ way ผ่าน triangle
- ยิ่ง convergence ช้าลง ยิ่งมีพลังเมื่อ breakout

### Ascending Triangle
- Flat top (resistance) + rising bottom (support)
- **Bullish bias** - มีโอกาส breakout ขึ้นมากกว่าลง
- ราคาซื้อที่ support line (rising)

### Descending Triangle
- Flat bottom (support) + declining top (resistance)
- **Bearish bias** - มีโอกาส breakout ลงมากกว่าขึ้น

### Broadening Formation
- ราคา diverge ออกกว้างขึ้นเรื่อยๆ (ตรงข้ามกับ triangle)
- สะท้อนความไม่แน่นอนที่เพิ่มขึ้น มักเกิดที่ market tops
- Breakout มักเกิดลง

### Flags and Pennants
- **Flags** = รูปสี่เหลี่ยมผืนผ้าเล็กๆ ที่เอียงไปทางตรงข้ามกับ trend
- **Pennants** = รูปสามเหลี่ยมเล็กๆ คล้าย symmetrical triangle เล็กมาก
- ทั้งสองเกิดกลาง trend move อย่างรวดเร็ว + volume สูง
- **Measuring Rule**: วัด "flagpole" (move ก่อน pattern) แล้ว project จาก breakout
- **Duration**: มัก 1-3 สัปดาห์ ไม่เกิน 4 สัปดาห์

### Wedge Formation (Falling/Rising Wedge)
- เหมือน symmetrical triangle แต่ทั้งสองเส้นเอียงไปทิศทางเดียวกัน
- **Rising Wedge** = bearish (เกิดกลาง uptrend)
- **Falling Wedge** = bullish (เกิดกลาง downtrend)

### Rectangle Formation (Trading Range)
- ราคาแกว่งระหว่าง support/resistance ที่ชัดเจน
- ดูเหมือน "box" หรือ "trading range"
- Breakout ไปทิศทางไหนก็ได้ แต่ breakout ไปทิศทางเดิมของ trend ก่อนหน้ามีโอกาสสำเร็จมากกว่า

### Measured Move
- หลัง breakout จาก consolidation pattern ราคาจะ move ระยะเท่ากับ move ก่อนหน้า

---

## 7. Volume and Open Interest

### Volume Rules (ทั่วไป)
- **Volume ควร.expand ตามทิศทางของ trend** - ถ้าราคาขึ้น volume ต้องเพิ่ม, ถ้าราคาลง volume ต้องเพิ่ม
- **Volume ควรลดลงใน correction** - ชี้ว่า correction ไม่ใช่ reversal
- **Volume climax (blowoff)** = volume หนักมากที่ปลาย trend = สัญญาณกลับตัว

### Volume Rules (Futures-Specific)
- Volume + Open Interest เพิ่ม = trend แข็งแรง
- Volume เพิ่ม + Open Interest ลด = short covering (อ่อนแอ)
- Volume ลด + Open Interest เพิ่ม = accumulation/distribution (ไม่ชัดเจน)
- Volume ลด + Open Interest ลด = trend กำลังจาง

### Open Interest Interpretation
- OI เพิ่ม + ราคาขึ้น = bullish (new longs entering)
- OI เพิ่ม + ราคาลง = bearish (new shorts entering)
- OI ลด + ราคาขึ้น = bearish (short covering, trend อ่อนแอ)
- OI ลด + ราคาลง = bullish (long liquidation, trend จาง)

### Blowoffs and Selling Climaxes
- **Blowoff** = volume สูงสุดที่ price top = สัญญาณ sell
- **Selling Climax** = volume สูงสุดที่ price bottom = สัญญาณ buy (panic selling หมดแล้ว)

### Commitments of Traders (COT) Report
- **Commercials** (hedgers) = มักถูกต้อง (smart money)
- **Large Traders** = trend followers
- **Small Traders** = มักผิด (dumb money)
- ดู positions ของ commercials เป็น contrarian indicator

### Options: Put/Call Ratio
- Put/Call ratio สูง = bearish sentiment สูง = contrarian buy signal
- Put/Call ratio ต่ำ = bullish sentiment สูง = contrarian sell signal

---

## 8. Long-Term Charts (กราฟระยะยาว)

### ทำไมต้องดู Long-Term Charts?
- ให้ "bigger picture" ที่ daily charts มองไม่เห็น
- Long-term trends โต้แย้ง Random Walk Theory
- Patterns บน weekly/monthly charts สำคัญกว่า daily charts

### Continuous Contracts
- ปัญหาของ Futures: สัญญาหมดอายุ ต้อง combine สัญญาหลายๆ เดือนเข้าด้วยกัน
- **Perpetual Contract** = ราคาต่อเนื่องโดยไม่สะดุด
- มีวิธีต่างๆ: Nearest contract, Gann contract, Constant Forward

### ควรปรับ Inflation ไหม?
- สำหรับ long-term charts (10-20 ปี) inflation adjustment มีประโยชน์
- แต่สำหรับ trading ระยะสั้น ไม่จำเป็น

### สำคัญ: Long-term charts ไม่ได้สร้างมาเพื่อ trading
- ใช้สำหรับ **directional analysis** ไม่ใช่ timing

---

## 9. Moving Averages (ค่าเฉลี่ยเลื่อน)

### หลักการพื้นฐาน
- Moving Average = "smoothing device" ที่ลด noise ของราคา
- เป็น "curvilinear trendline" ที่ปรับตัวตามเวลา
- มี **time lag** เพราะเป็นค่าเฉลี่ยของข้อมูลในอดีต

### Simple Moving Average (SMA)
- คำนวณจากค่าเฉลี่ยของราคาปิด X วันล่าสุด
- Popular: 4, 9, 18 วัน (futures), 50, 200 วัน (stocks)

### Exponential Moving Average (EMA)
- ให้น้ำหนักกับข้อมูลล่าสุดมากกว่า
- ตอบสนองเร็วกว่า SMA
- เป็นที่นิยมในระบบเทรดสมัยใหม่

### Moving Average Envelopes
- วาดเส้น包围 MA ด้วย % ที่กำหนด (เช่น +/- 3%)
- ราคาชน upper envelope = overbought
- ราคาชน lower envelope = oversold

### Bollinger Bands
- ใช้ standard deviation แทน % คงที่ (จะ expand/contract ตาม volatility)
- Upper band = MA + 2 std dev, Lower band = MA - 2 std dev
- **Upper/Lower bands ใช้เป็น price targets**
- **Band Width** วัด volatility

### How to Use Moving Averages
- **Crossover**: ราคาตัดขึ้นเหนือ MA = buy, ตัดลงใต้ MA = sell
- **Dual MA System**: MA สั้นตัดขึ้นเหนือ MA ยาว = buy crossover, ตัดลง = sell crossover
- **Multiple MAs**: ใช้ 3-4 MAs (เช่น 4, 9, 18) เพื่อดู trend alignment

### Fibonacci Numbers 作为 Moving Averages
- ใช้ Fibonacci numbers (5, 8, 13, 21, 34, 55, 89, 144) เป็น periods

### Moving Averages on Long-Term Charts
- Weekly MA สำคัญกว่า daily MA สำหรับ long-term analysis

### The 4-9-18 Weekly Rule
- สัญญาณซื้อ: 4-week MA อยู่เหนือ 9-week MA และทั้งคู่อยู่เหนือ 18-week MA
- สัญญาณขาย: reverse

### Optimization
- Murphy เตือนเรื่อง over-optimization (.curve fitting) ไม่ใช่ optimized parameters จะดีกว่าเสมอ

### Adaptive Moving Average (AMA)
- ปรับ sensitivity ตาม market conditions: trending = responsive, nontrending = smooth

---

## 10. Oscillators and Contrary Opinion

### Oscillator คืออะไร?
- ตัวชี้วัดที่แกว่งอยู่ในช่วงที่กำหนด (เช่น 0-100 หรือ -100 ถึง +100)
- ใช้วัด **momentum** (อัตราการเปลี่ยนแปลงของราคา)

### Momentum Oscillators
- ** Momentum** = ราคาปัจจุบัน - ราคา X วันก่อน
- **Rate of Change (ROC)** = (ราคาปัจจุบัน / ราคา X วันก่อน) x 100

### Moving Average Oscillator (MACD)
- **MACD Line** = 12-period EMA - 26-period EMA
- **Signal Line** = 9-period EMA ของ MACD Line
- **Histogram** = MACD - Signal
- **Buy Signal**: MACD ตัดขึ้นเหนือ Signal Line
- **Sell Signal**: MACD ตัดลงใต้ Signal Line
- **Divergence**: ราคา new high แต่ MACD ไม่ new high = bearish divergence (สัญญาณ sell)

### Relative Strength Index (RSI)
- วัด strength ของราคาขึ้น vs. ราคาลง
- ช่วง 0-100
- **Overbought**: >70
- **Oversold**: <30
- **Divergence**: ราคา new high แต่ RSI ไม่ new high = สัญญาณ sell
- Wilder แนะนำ 14-period RSI

### Stochastics (%K, %D)
- วัดตำแหน่งของราคาปัจจุบันเทียบกับ high-low range ของ X วัน
- **%K** = fast, **%D** = slow (smoothed %K)
- **Overbought**: >80
- **Oversold**: <20
- Buy: %K ตัดขึ้นเหนือ %D ในโซน oversold
- Sell: %K ตัดลงใต้ %D ในโซน overbought

### Larry Williams %R
- คล้าย Stochastics แต่กลับทิศ (0 = top, -100 = bottom)
- Overbought: >-20, Oversold: <-80

### Commodity Channel Index (CCI)
- วัดระยะห่างของราคาจากค่าเฉลี่ยทางสถิติ
- Overbought: >+100, Oversold: <-100

### สำคัญ: Oscillators ใช้ได้ดีใน **Sideways/Trending Markets**
- ใน strong uptrend: RSI อาจ stay >70 ได้นาน = "overbought" ไม่ได้หมายความว่าต้อง sell
- **Combine oscillator กับ trend**: ใช้ oscillator หา divergence เมื่อ trend อ่อนแอลง

### MACD Histogram
- วัดระยะห่างระหว่าง MACD กับ Signal Line
- Histogram หดตัว = momentum จาง = สัญญาณเตือน

### ใช้ Weekly + Daily ร่วมกัน
- Weekly oscillator ให้ direction หลัก
- Daily oscillator ให้ timing (จังหวะเข้า)

### Contrary Opinion (ความเห็นสวน)
- **Crowd psychology** มักผิดที่จุดสุดยอดและจุดก้น
- ใช้ sentiment indicators:
  - **Put/Call Ratio**: สูง = bearish sentiment = buy signal
  - **Investors Intelligence**: % bullish สูง = sell signal
  - **Commitments of Traders**: Commercials = smart money

---
---

## 11. Point and Figure Charting

### ��念
- Point and Figure (P&F) ไม่ใช้ time axis — ใช้ price movement เท่านั้น
- X = ราคาขึ้น, O = ราคาลง
- ต้องมี reversal amount (เช่น 3-box reversal) ก่อนจะเปลี่ยน column

### ข้อดี
- ตัด noise ออกได้ดี (ไม่แสดง time-based noise)
- ให้ price objective ชัดเจน (counting boxes)
- Support/Resistance ชัดเจนกว่า bar chart
- ใช้กับ long-term chart ได้ดี (ไม่ต้องการข้อมูลทุกช่วงเวลา)

### วิธีสร้าง
1. ถ้าขึ้น X amount จาก X ตัวสุดท้าย = เพิ่ม X
2. ถ้าลง O amount จาก O ตัวสุดท้าย = เพิ่ม O
3. ถ้าราคาเปลี่ยนทิศทาง reversal amount = เปลี่ยน column

---

## 12. Japanese Candlesticks

### โครงสร้าง
- **Real body**: ช่วงระหว่าง Open กับ Close
- **Upper shadow (wick)**: ช่วงระหว่าง High กับ body
- **Lower shadow (tail)**: ช่วงระหว่าง Low กับ body
- **White/green body**: Close > Open (bullish)
- **Black/red body**: Close < Open (bearish)

### Single Candlestick Patterns
- **Doji**: Open = Close ( indecision)
- **Hammer / Hanging Man**: Body เล็ก, lower shadow ยาว 2x body
  - Hammer หลัง downtrend = bullish reversal
  - Hanging Man หลัง uptrend = bearish reversal
- **Inverted Hammer / Shooting Star**: Upper shadow ยาว
- **Marubozu**: ไม่มี shadow (strong momentum)

### Multi-Candlestick Patterns
- **Engulfing**: Body ของวันที่สองครอบคลุม body ของวันแรก
  - Bullish engulfing หลัง downtrend = reversal
  - Bearish engulfing หลัง uptrend = reversal
- **Harami**: Body ของวันที่สองอยู่ inside body ของวันแรก
- **Morning Star / Evening Star**: 3-candle reversal pattern
- **Three White Soldiers / Three Black Crows**: 3-candle continuation

### ข้อจำกัด
- ต้องดู context (trend ก่อนหน้า)
- ต้อง confirm ด้วย volume
- ไม่ควรใช้ pattern เดียวในการตัดสินใจ

---

## 13. Elliott Wave Theory

### โครงสร้างพื้นฐาน
- **Impulse waves (1-2-3-4-5)**: ตาม trend หลัก
- **Corrective waves (A-B-C)**: ย้อน trend หลัก

### กฎของ Elliott Wave
1. **Wave 2** ไม่เคย retracement มากกว่า 100% ของ Wave 1
2. **Wave 3** ไม่เคยสั้นที่สุดใน impulse waves (มักจะยาวที่สุด)
3. **Wave 4** ไม่เคยเข้าไป overlap กับ territory ของ Wave 1

### Fibonacci Relationships
- Wave 2 มัก retracement 50% หรือ 61.8% ของ Wave 1
- Wave 3 มัก extend 161.8% ของ Wave 1
- Wave 4 มัก retracement 38.2% ของ Wave 3
- Wave 5 มักเท่ากับ Wave 1 หรือ 61.8% ของ Wave 1

### Corrective Patterns
- **Zigzag (5-3-5)**: แก้ไขแรง
- **Flat (3-3-5)**: แก้ไขแบบ sideways
- **Triangle (3-3-3-3-3)**: แก้ไขแบบ consolidation
- **Complex combinations**: W-X-Y, double/triple threes

### ข้อจำกัด
- Subjective — นักวิเคราะห์คนเดียวกันอาจนับ wave ต่างกัน
- ต้องใช้实践经验 (experience) มาก
- Fibonacci ratios เป็น guideline ไม่ใช่กฎตายตัว

---

## 14. Time Cycles

### ��念
- ตลาดมีรอบ (cycle) ที่เกิดซ้ำ
- Cycle length = ระยะห่างระหว่างจุดต่ำสุด (trough to trough)

### ประเภทของ Cycles
- **Kitchin Cycle**: 40 เดือน (business cycle)
- **Juglar Cycle**: 6-7 ปี (intermediate)
- **Kondratieff Wave**: 50-60 ปี (long wave)
- **Hurst Cycles**: ชุด cycles หลายระดับ

### วิธีใช้
- ระบุ cycle length จากข้อมูล historical
- ใช้ moving average ของ cycle length
- คำนวณ expects cycle high/low dates
- ใช้ cycle together กับ patterns และ indicators

### ข้อจำกัด
- Cycles ไม่สม่ำเสมอ (variable length)
- ต้อง combine กับ price patterns
- ไม่ควรใช้ cycle เดียวในการตัดสินใจ

---

## 15. Computers and Trading Systems

### บทบาทของ Computer
- คำนวณ indicators อัตโนมัติ
- สร้างและวิเคราะห์ chart
- Backtest trading systems
- Real-time data feed และ alert

### ประเภทของ Trading Systems
- **Mechanical systems**: กฎตายตัว (ไม่มี discretionary)
- **Discretionary systems**: ใช้ judgment ของ trader
- **Automated systems**: Computer  executes อัตโนมัติ

### Moving Average Trading Systems
- **Dual MA crossover**: Short-term MA ตัด Long-term MA
  - Golden cross = bullish
  - Death cross = bearish
- **Envelopes / channels**: MA +/- percentage

### Oscillator-Based Systems
- **MACD crossover**: MACD line ตัด signal line
- **RSI overbought/oversold**: ซื้อเมื่อ RSI < 30, ขายเมื่อ RSI > 70
- **Stochastic crossover**: %K ตัด %D

### Backtesting
- ทดสอบ system กับ historical data
- ดู Sharpe ratio, max drawdown, win rate
- ระวัง overfitting (curve fitting)
- ต้อง out-of-sample test

### VIX (Volatility Index)
- วัด implied volatility ของ S&P 500 options
- สูง = ตลาดกลัว (bearish sentiment)
- ต่ำ = ตลาดสงบ (bullish sentiment)
- ใช้เป็น contrarian indicator

---

## 16. Money Management and Trading Tactics

### Money Management Rules
1. **Always use stop-loss**: ไม่ถือ position โดยไม่มีจุดออก
2. **Risk per trade**: ไม่เกิน 1-2% ของ portfolio ต่อ trade
3. **Position sizing**: คำนวณ size จาก stop-loss distance
4. **Risk/Reward ratio**: อย่างน้อย 2:1 (reward มากกว่า risk)

### Trading Psychology
- **Discipline**: ทำตาม rules ที่ตั้งไว้
- **Patience**: รอ setup ที่ดีที่สุด
- **Emotional control**: ไม่เทรดเพราะอารมณ์
- **Record keeping**: บันทึกทุก trade เพื่อ review

### Trading Tactics
- **Entry signals**: ใช้ indicators + patterns + volume confirm
- **Exit strategies**: Trail stop, target price, time-based exit
- **Scaling in/out**: เพิ่ม/ลด position ทีละส่วน
- **Diversification**: กระจาย across markets

### Common Mistakes
- Overtrading (เทรดบ่อยเกินไป)
- Averaging down (ถัวเฉลี่ยขาล)
- Ignoring stop-loss
- Revenge trading (เทรดแก้แค้นหลังขาดทุน)

---

## 17. Intermarket Analysis

### ความสัมพันธ์ระหว่าง Markets
- **Stocks vs Bonds**: ปกติ inverse correlation
  - หุ้นขึ้น → bond ลง (interest rate rise)
  - หุ้นลง → bond ขึ้น (flight to quality)
- **Dollar vs Commodities**: inverse correlation
  - Dollar แข็ง → commodities ลง ( priced in dollar)
  - Dollar อ่อน → commodities ขึ้น
- **Oil vs Airlines**: inverse correlation
  - น้ำมันขึ้น → สายการบินลง
- **Gold vs Dollar**: inverse correlation
  - Dollar แข็ง → Gold ลง
  - Dollar อ่อน → Gold ขึ้น

### วิธีใช้ Intermarket Analysis
1. ดู trend ของ market ที่เกี่ยวข้อง
2. ยืนยัน direction ของ market หลัก
3. ถ้ามี divergence = อาจมีจุดเปลี่ยน
4. ใช้ intermarket เป็น overlay ไม่ใช่ primary signal

### Key Relationships for Traders
- **TIPS spread** (inflation expectations): กระทบ commodities และ gold
- **Credit spreads**: กระทบ risk appetite
- **Yield curve**: กระทบ bank stocks และ economy

---

## 18. Stock Market Indicators

### Market Breadth
- **Advance-Decline Line (AD Line)**: จำนวนหุ้นที่ขึ้น ลบ หุ้นที่ลง
  - AD line ขึ้นพร้อม market = bullish confirmation
  - AD line ลง แม้ market ขึ้น = bearish divergence
- **Advance-Decline Ratio**: หุ้นขึ้น / หุ้นลง
- **New Highs - New Lows**: หุ้นทำ new high ลบ หุ้นทำ new low

### Volume Indicators
- **TRIN (Arms Index)**: (Advancing Issues / Declining Issues) / (Advancing Volume / Declining Volume)
  - < 1.0 = bullish
  - > 1.0 = bearish
- **On-Balance Volume (OBV)**: สะสม volume ตาม direction
- **Money Flow Index (MFI)**: RSI-weighted by volume

### Tick Index
- **Tick Index**: จำนวนหุ้นที่ last trade ราคาขึ้น ลบ หุ้นที่ลง
  - Extreme positive (> +1000) = overbought
  - Extreme negative (< -1000) = oversold

### Put/Call Ratio
- Ratio ของ put volume / call volume
- สูง = bearish sentiment = contrarian bullish
- ต่ำ = bullish sentiment = contrarian bearish

### Volatility Indicators
- **VIX**: Implied volatility ของ S&P 500
- **Bollinger Band Width**: ความกว้างของ bands

---

## 19. Pulling It All Together

### Trading Plan Framework
1. **Identify the trend**: ใช้ long-term chart + MA
2. **Wait for setup**: ใช้ intermediate patterns + indicators
3. **Time the entry**: ใช้ short-term signals + oscillators
4. **Manage the trade**: Stop-loss, position sizing, trail stop

### Checklist ก่อนเทรด
- [ ] Trend identified (long-term)
- [ ] Setup identified (intermediate)
- [ ] Entry signal (short-term)
- [ ] Stop-loss placed
- [ ] Risk/Reward calculated (>= 2:1)
- [ ] Position size calculated (<= 2% risk)
- [ ] No conflicting news/events

### หลักการสำคัญ
1. **Trend is your friend**: ไม่เทรดสวน trend หลัก
2. **The trend is intact until proven otherwise**: อย่าเทรด reversal โดยไม่มีหลักฐาน
3. **Volume confirms the trend**: Volume ควร follow direction
4. **Multiple timeframes**: ดูทุกระดับ time frame
5. **Keep it simple**: ไม่ใช้ indicators มากเกินไป (2-3 พอ)

### สรุป
Technical analysis เป็นเครื่องมือที่มีประสิทธิภาพสำหรับ trading แต่ต้องใช้ร่วมกับ money management และ discipline ที่ดี ไม่มี system ที่ win ทุก trade — สำคัญคือ manage risk ให้ดี

---

## Appendix A: Advanced Technical Indicators

### Parabolic SAR (Stop and Reverse)
- จุด dots แสดงจุดกลับทิศ
- SAR อยู่ใต้ราคา = bullish
- SAR อยู่เหนือราคา = bearish
- ใช้ trailing stop เหมาะกับ trending market
- ไม่เหมาะกับ sideways market (whipsaw)

### ADX (Average Directional Index)
- วัด strength ของ trend (ไม่บอก direction)
- ADX > 25 = strong trend
- ADX < 20 = weak trend / sideways
- +DI > -DI = bullish
- -DI > +DI = bearish

### Commodity Channel Index (CCI)
- วัดระยะห่างระหว่าง price กับ statistical average
- > +100 = overbought
- < -100 = oversold
- ใช้ divergence และ zero line crossover

### Momentum / Rate of Change
- Momentum = ราคาปัจจุบัน - ราคา X วันก่อน
- ROC = (ราคาปัจจุบัน / ราคา X วันก่อน) x 100
- ใช้ divergence กับ price

---

## Appendix B: Market Profile

### ��念
- สร้าง distribution ของราคาตาม time
- **Time Price Opportunity (TPO)**: แต่ละ letter = 30 minutes
- **Value Area**: ช่วงราคาที่ trade 70% ของเวลา
  - Value Area High (VAH)
  - Value Area Low (VAL)
  - Point of Control (POC): ราคาที่ trade มากที่สุด

### วิธีใช้
- **Initiative buying**: ราคาเปิดเหนือ value area = bullish
- **Initiative selling**: ราคาเปิดต่ำ value area = bearish
- **Responsive buying**: ซื้อที่ VAL (expect bounce)
- **Responsive selling**: ขายที่ VAH (expect pullback)

### Opening Range
- ช่วงราคา 30 นาทีแรกของ session
- Breakout จาก opening range = signal
- ใช้กับ intraday trading

---

## Appendix C: Building a Trading System

### ขั้นตอน
1. **Define objectives**: Return, risk tolerance, time frame
2. **Identify edge**: หาสิ่งที่ market ไม่ efficient (anomaly)
3. **Formulate rules**: เขียน rules ชัดเจน (entry, exit, sizing)
4. **Backtest**: ทดสอบกับ historical data
5. **Optimize**: ปรับ parameters (ระวัง overfitting)
6. **Forward test**: ทดสอบกับ paper trading
7. **Go live**: เริ่มด้วย size เล็ก

### วัดผล
- **Profit factor**: Gross profit / Gross loss
- **Sharpe ratio**: Risk-adjusted return
- **Max drawdown**: ขาดทุนสูงสุดจาก peak
- **Win rate**: % ของ trades ที่กำไร
- **Expectancy**: (Win% x Avg win) - (Loss% x Avg loss)

### Common Pitfalls
- Curve fitting (over-optimization)
- Overtrading
- Changing rules mid-test
- Ignoring transaction costs
- Not accounting for slippage

---

## Appendix D: Continuous Futures Contracts

### ปัญหา
- Futures contracts มี expiration
- Historical data ต้อง stitch สัญญาต่างรุ่นเข้าด้วยกัน

### วิธีทำ
- **Rolled contract**: เปลี่ยนสัญญาเมื่อใกล้ expiry
- **Continuous contract**: ต่อเนื่องจาก rollover โดยไม่ adjust price
- **Adjusted continuous**: ปรับราคาตาม rollover gap
- **Unadjusted**: ไม่ปรับ (may have gaps at rollover)

### วิธีใช้
- สำหรับ backtesting ต้องใช้ continuous contract
- ระวัง rollover gaps ที่อาจทำให้ indicators เพี้ยน
- ใช้ adjusted contract สำหรับ indicators
- ใช้ unadjusted contract สำหรับ price analysis

---

## สรุปรวม

หนังสือเล่มนี้เป็น>manual ที่ครอบคลุม technical analysis ทุกด้าน ตั้งแต่:
- **ปรัชญา**: Why technical analysis works
- **เครื่องมือ**: Charts, patterns, indicators
- **การประยุกต์**: Trading systems, money management
- **มุมมองกว้าง**: Intermarket analysis, stock market indicators

สำหรับ algo trading สิ่งที่สำคัญที่สุดคือ:
1. นำ indicators (MA, RSI, MACD, Stochastic) มาคำนวณอัตโนมัติ
2. ใช้ pattern recognition (head & shoulders, triangles) เป็น rule-based logic
3. ใช้ Elliott Wave + Fibonacci เป็น target/exit levels
4. ใช้ money management rules (position sizing, risk per trade)
5. Backtest ทุกอย่างก่อน live trading
