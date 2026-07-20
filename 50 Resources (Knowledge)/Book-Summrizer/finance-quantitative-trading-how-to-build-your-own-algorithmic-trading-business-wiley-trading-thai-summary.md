# สรุปหนังสือ Quantitative Trading: How to Build Your Own Algorithmic Trading Business

**ผู้เขียน**: Ernest P. Chan (Ernie Chan)
**สำนักพิมพ์**: Wiley Trading (2009)
**เป้าหมายหลัก**: คู่มือสำหรับนักเทรดอิสระที่ต้องการสร้างธุรกิจ quantitative trading ของตนเอง — เน้น statistical arbitrage บนหุ้น, futures, และ currencies ไม่ใช่ derivatives ที่ซับซ้อน

---

## ภาพรวม

Ernie Chan ถ่ายทอดประสบการณ์จากนักเทรดในสถาบัน (Morgan Stanley, Credit Suisse, hedge funds หลายแห่ง) สู่การเป็นนักเทรดอิสระ ข้อความสำคัญคือ: **กลยุทธ์ที่เรียบง่ายและมีวินัย มักทำกำไรได้มากกว่าโมเดลที่ซับซ้อน** เขาเริ่มต้นจากห้องนอนที่บ้าน ด้วยเงินทุน $100,000 และทำกำไรได้ตั้งแต่เดือนแรก — ต่างจากธุรกิจ dot-com ที่เขาเคยทำซึ่งเสียเงิน 100%

---

## Part I: ภาพรวมและพื้นฐาน (Chapters 1-2)

### Chapter 1 — ทำไมต้อง Quantitative Trading?

**Quantitative Trading คืออะไร?**
- การซื้อขายหลักทรัพย์โดยใช้ algorithm ของคอมพิวเตอร์ที่ตัดสินใจซื้อ/ขายตาม strategy ที่เขียนขึ้น
- ไม่ใช่แค่ technical analysis — ยังรวมถึง fundamental data (รายได้, cash flow, debt-to-equity) 甚至 news parsing
- ต่างจาก discretionary trading ตรงที่ทุก decision ต้อง-encoded เป็น computer program

**ใครเป็น Quant Trader ได้บ้าง?**
- ไม่จำเป็นต้องมี PhD — แค่ความรู้ระดับมัธยมปลายเรื่องคณิตศาสตร์, statistics, programming ก็เพียงพอสำหรับ statistical arbitrage
- Ernie Chan บอกว่าตัวเขาเองมี PhD จาก Cornell ด้าน physics แต่ strategy ที่ซับซ้อนกลับทำให้ขาดทุน จนกระทั่งเขาเปลี่ยนมาใช้ strategy เรียบง่ายจึงทำกำไรได้จริง
- **บทเรียน**: "Make everything as simple as possible. But not simpler." — Einstein

**ข้อดีของ Quantitative Trading เทียบกับธุรกิจอื่น:**

| ปัจจัย | Quantitative Trading | ธุรกิจทั่วไป |
|---------|---------------------|-------------|
| **Scalability** | สูงมาก — scale ด้วย leverage ไม่ต้องเจรจาเงินกู้ | จำกัดด้วย capital และคน |
| **เวลา** | 2-3 ชม./วัน (operational) — ที่เหลือ research + อิสระ | ใช้เวลาทั้งวัน |
| **Marketing** | ไม่จำเป็น — counterparty ตัดสินใจที่ราคาล้วนๆ | ต้องมี marketing หนัก |
| **Capital เริ่มต้น** | ~$50,000-$100,000 | ขึ้นกับธุรกิจ |

**เป้าหมายหลัก**: ไม่ใช่รวยเร็ว — แต่เป็น income stream ที่มั่นคงและเพิ่มขึ้นเรื่อยๆ ด้วย compound growth

---

### Chapter 2 — Fishing for Ideas: การหา Strategy

**หา idea ได้จากที่ไหน?**
- Academic papers (SSRN, NBER, business school seminars)
- Financial blogs ( Seeking Alpha, TradingMarkets, Elite Trader)
- Trader forums — มี strategy มากมายที่ share กัน free
- ข้อค้นพบ: **การแลกเปลี่ยน idea ง่ายกว่าตอนทำงานใน hedge fund** เพราะคนไม่กลัวว่าคุณจะเอา strategy ไปเทรด $100M

**วิธีเลือก Strategy ที่เหมาะกับตัวเอง:**

1. **เวลาที่มี** — ถ้าเทรด part-time ต้องเลือก overnight strategy หรือไม่ก็ automate ทั้งหมด
2. **ทักษะ programming** — ถ้าเขียน Visual Basic/Java/C++ ได้ ทำ high-frequency ได้; ไม่ได้ก็เทรดวันละครั้ง
3. **เงินทุน**:
   - **ทุนน้อย (< $100K)**: เทรด futures/currencies (leverage สูง), intraday (Reg T 4:1), directional ไม่ต้อง hedge
   - **ทุนมาก (> $100K)**: เทรดได้ทุกอย่าง, market-neutral ได้, มี data คุณภาพสูง
4. **เป้าหมาย**: income รายเดือน vs capital growth ระยะยาว — ต้องเลือก holding period ให้สอดคล้อง

**เกณฑ์ประเมิน Strategy (ก่อน backtest เต็มรูปแบบ):**

| เกณฑ์ | สิ่งที่ต้องตรวจสอบ |
|--------|-------------------|
| Benchmark | Long-only strategy ต้องชนะ index; dollar-neutral เทียบ risk-free rate |
| Sharpe Ratio | >1 สำหรับ standalone strategy; >2 = กำไรทุกเดือน; >3 = กำไรทุกวัน |
| Drawdown | ต้องถามตัวเองว่าทนได้แค่ไหน — 20% 3 เดือน? 10% 1 เดือน? |
| Transaction Costs | ต้อง incorporate เข้าไป — strategy ที่ดีอาจกลายเป็นขาดทุนหลังหักค่าธรรมเนียม |
| Survivorship Bias | ถ้าไม่มี survivorship bias-free data → ต้อง skepticism |
| Performance over time | กำไรที่ดีในอดีตอาจหายไปถ้ามี regime shift |
| Data-snooping bias | ยิ่งมี parameters มาก → ยิ่งเสี่ยง overfitting |
| "Fly under the radar" | เลือก strategy ที่ institutional money ไม่สนใจ — capacity ต่ำ แต่ competition น้อย |

**AI และ Stock Picking — ความเห็นของ Ernie:**
- Neural networks, decision trees, genetic algorithms มี parameters มากเกินไปสำหรับ financial data ที่ limited
- ใช้ได้ใน consumer marketing (ข้อมูล billions of records) แต่ financial data มี serial correlation และมีจำนวนน้อย
- หลักการที่ใช้ได้จริง: **มี econometric basis, parameters น้อย, linear regression เท่านั้น, ง่าย, optimize ใน lookback window เท่านั้น** — ใช้ Occam's Razor

---

## Part II: Backtesting (Chapter 3)

### Chapter 3 — Backtesting: การทดสอบกลยุทธ์กับข้อมูลในอดีต

** Platforms สำหรับ Backtest:**

| Platform | ข้อดี | ข้อเสีย |
|----------|-------|---------|
| **Excel** | WYSIWYG, ง่าย, ไม่มี look-ahead bias, backtest = live execution | ใช้กับ model ซับซ้อนไม่ได้ |
| **MATLAB** | Portfolio ใหญ่ได้, built-in stats, web scraping, third-party packages เยอะ | ราคาแพง (~$1,000+), ใช้ execute live ไม่ได้ดี |
| **TradeStation** | All-in-one (data + backtest + execution), มี DDE link | ผูกกับ TradeStation, language จำกัด |
| **High-end** (FactSet, Alphacet, Barra) | สถาบันใช้, มี ML + execution | ราคาแพงมาก |

**แหล่งข้อมูล:**
- Yahoo! Finance (free, มี survivorship bias)
- HQuotes.com (low cost, bulk download)
- CSIdata.com (source ของ Yahoo/Google)
- CRSP.com (survivorship bias-free แต่แพง)
- GainCapital.com (forex intraday free)

**ปัญหาสำคัญของข้อมูล:**

1. **Split/Dividend Adjustment** — ถ้าไม่ adjust จะเกิด false signal ที่ ex-date (ราคา drop ผิดปกติ)
2. **Survivorship Bias** — ข้อมูลที่ไม่รวมหุ้นที่ delisted/ bankrupt → ทำให้ backtest ดูดีเกินจริง ตัวอย่าง: strategy "ซื้อหุ้นราคาถูก" ได้ return -42% (จริง) vs +388% ( bias)
3. **High/Low Data Noise** — High/Low ของวัน noise มาก, ไม่ควรใช้สำหรับ limit order backtest

**Performance Measurement:**

| Metric | สูตร / วิธีคำนวณ | ความหมาย |
|--------|-------------------|----------|
| **Sharpe Ratio** | √N × (mean excess return / std dev) | วัด risk-adjusted return; N = จำนวน period/ปี |
| **Maximum Drawdown** | ราคาสูงสุด → ต่ำสุด ถัดมา (เป็น %) | ความเสียหายสูงสุดจาก peak |
| **Max Drawdown Duration** | จำนวนวันที่ต้องใช้ recovery นานที่สุด | ความเจ็บปวดนานแค่ไหน |

**Sharpe Ratio สำคัญแค่ไหน?**
- SAC Capital Advisors เคยปฏิเสธ strategy ที่ Sharpe สูง บอกว่าขอ return สูงกว่า
- **ผิด**: Sharpe สูง → leverage ได้สูง → leveraged return สูงกว่า return แบบไม่ leverage
- อัตราส่วน leverage ที่ Kelly formula แนะนำคือ f = m / σ² — ยิ่ง Sharpe สูง ยิ่ง leverage ได้มาก

**Look-Ahead Bias:**
- ใช้ข้อมูลจากอนาคตในการตัดสินใจ trading signal เช่น ใช้ daily low ทั้งวันมาตัดสิน buy ทั้งๆ ที่ไม่มีทางรู้ low จนตลาดปิด
- **วิธีป้องกัน**: ใช้ lagged data เสมอ; test ด้วย truncation method — ตัดข้อมูลส่วนท้าย N วัน แล้วดูว่า positions ต่างกันหรือไม่

**Data-Snooping Bias:**
- ยิ่งมี parameters มาก ยิ่งเสี่ยง overfitting
- **กฎ**: ไม่เกิน 5 parameters (entry/exit threshold, holding period, lookback period)
- วิธีลด: sample size พอ, out-of-sample testing, sensitivity analysis, simplify โมเดล

**Out-of-Sample Testing:**
- แบ่งข้อมูลเป็น training set + test set
- Optimize parameters ใน training set → ทดสอบใน test set
- **Paper trading** = out-of-sample test จริงที่สุด

**Transaction Costs:**
- ตัวอย่าง Bollinger Band mean-reverting strategy: Sharpe = 3 (ไม่หักค่า) → Sharpe = -3 (หัก 1bp) — กลายเป็นขาดทุนรุนแรง
- Round-trip S&P 500 stocks ≈ 10 bps; ES futures ≈ 2 bps

**Strategy Refinement:**
- เปลี่ยนเล็กน้อย เช่น เปลี่ยนจาก market close → market open → Sharpe 0.25 → 4.43 (ก่อนค่า), 0.78 (หลังค่า)
- ต้องมี rationale ทางเศรษฐศาสตร์ ไม่ใช่ trial-and-error

---

## Part III: Setting Up Business + Execution (Chapters 4-5)

### Chapter 4 — Setting Up Your Business

**Retail vs Proprietary Trading:**

| ประเด็น | Retail | Proprietary |
|---------|--------|-------------|
| เงินทุนเริ่มต้น | มาก ($50K+) | น้อย |
| Leverage | Reg T: 2x (overnight), 4x (intraday) | ถึง 20x+ |
| ความเสียหาย | ไม่จำกัด (除非 LLC) | จำกัดแค่เงินลงทุนเริ่มต้น |
| ค่า commission | ต่ำ | สูงกว่า |
| ความเสี่ยงล้มละลาย | ไม่มี (SIPC insurance) | มี |
| Training/guidance | ไม่มี | มี (บางครั้งเสียค่า) |
| Strategy disclosure | ความเสี่ยงต่ำ | สูง — firm อาจ piggyback |

**ปัจจัยในการเลือก Brokerage:**
1. Commission rate (สำคัญ แต่ไม่ใช่ทุกอย่าง)
2. Execution speed + dark pool liquidity access
3. Range of products (futures, forex)
4. **API** — สำคัญที่สุดสำหรับ quantitative trading; ไม่มี API = ไม่สามารถ automate ได้
5. Paper trading account
6. ชื่อเสียงและความมั่นคงทางการเงิน

**Physical Infrastructure:**
- เริ่มต้น: PC dual-core + internet speed + UPS (~$2,000 + $50/เดือน)
- Scale ขึ้น: multiple monitors, T1 line, server hosting/collocation
- ข้อคิด: เทรด portfolio มูลค่า $1M ได้ด้วย infrastructure แค่ไม่กี่พันดอลลาร์

---

### Chapter 5 — Execution Systems

**Semi-automated vs Fully Automated:**

| ระบบ | Semi-automated | Fully Automated |
|------|---------------|-----------------|
| ขั้นตอน | MATLAB/Excel → generate order file → upload ไป basket trader | Loop ตลอดวัน, API → auto submit |
| เหมาะกับ | เทรดวันละครั้ง, ไม่กี่浪 | High-frequency, หลาย浪 |
| ภาษา | MATLAB, Excel VBA | Java, C#, C++ |
| ข้อดี | ง่าย, ไม่ต้อง pro มาก | ไม่มี human error, ไม่มี delay |

**DDE Link** — เชื่อม Excel กับ brokerage real-time data; แต่ช้าสำหรับ high-frequency

**Paper Trading — สำคัญมาก:**
- ตรวจ bug ใน ATS
- ตรวจ look-ahead bias
- เรียนรู้ operational difficulties (download data ทันก่อน market open ไหม?)
- ประมาณ transaction costs จริง
- **ข้อเสีย**: เริ่มเบื่อหลังจากเทรดจริงไปแล้ว → paper trading system อาจเสียเพราะ neglect

**Live Trading ขาดทุน — Checklist:**
1. มี bug ใน ATS software?
2. Trades ตรงกับ backtest?
3. Transaction costs สูงกว่าคาด?
4. เทรดหุ้น illiquid ทำให้ market impact สูง?
5. Data-snooping bias? → ลด parameters
6. Regime shift? → ตลาดเปลี่ยน structure เช่น decimalization (2001), plus-tick rule elimination (2007)

---

## Part IV: Money and Risk Management (Chapter 6)

### Chapter 6 — Kelly Formula และ Risk Management

**Kelly Formula:**
```
f* = m / σ²
```
- f* = optimal fraction ของ equity ที่ควร allocate ให้ strategy นี้
- m = expected return (excess return)
- σ² = variance ของ return

**หลักการสำคัญ:**
- Maximize long-term compounded growth rate: g = r + S²/2 (S = Sharpe ratio)
- ยิ่ง Sharpe สูง ยิ่ง growth rate สูง
- Kelly leverage = independent of time scale

**ตัวอย่าง SPY:**
- Mean annual return: 11.23%, Std dev: 16.91%, Risk-free: 4%
- Kelly leverage: f = 0.07231 / 0.1691² = **2.528**
- Compounded growth rate: **13.14%** (เทียบกับไม่ leverage = 9.8%)

**Multi-Strategy Allocation:**
```
F* = C⁻¹ M
```
- C = covariance matrix ของ strategy returns
- M = mean returns vector
- ตัวอย่าง OIH/RKH/RTH: Kelly leverage = [1.29, 1.17, -1.49], Portfolio Sharpe = 0.475

**Half-Kelly:**
- ใช้ครึ่งหนึ่งของ Kelly leverage — เป็นที่นิยมเพราะ:
  - Parameter estimation ไม่แน่นอน
  - Return distribution ไม่ Gaussian จริง (fat tails)
  - ลดขนาดการ selling ตอน risk management

**Risk Management Rules:**
1. **ขายตอนขาดทุน** — Kelly บังคับให้ reduce position size เมื่อ equity ลดลง
2. **ซื้อตอนกำไร** — เพิ่ม position size เมื่อ equity เพิ่ม
3. **Fat Tail Events** — ใช้ historical max loss มากำหนด leverage cap; ถ้า max one-day loss = 20% และทนได้แค่ 20% → leverage ≤ 1.0

**Stop Loss — ไม่ใช่ risk management ที่ดีเสมอไป:**
- ใน momentum regime: stop loss มีประโยชน์ (ราคาจะแย่ลงเรื่อยๆ)
- ใน mean-reverting regime: stop loss 有害 (exit ตรงจุดที่แย่ที่สุด)
- วิธีดีกว่า: ใช้ entry signal ใหม่เป็น exit signal

**Psychological Preparedness:**
1. **Endowment effect / Loss aversion** — ถือ position ที่ขาดทุนนานเกินไป, ตัดกำไรเร็วเกินไป
2. **Representativeness bias** — แก้ parameters หลัง loss ใหญ่ → อาจกำจัด profit opportunities
3. **Despair** — ปิด model ทั้งหมดตอน drawdown ใหญ่
4. **Greed** — เพิ่ม leverage เร็วเกินไปตอนกำไรมาก
5. **Overleveraging** — Long-Term Capital Management (2000), Amaranth Advisors (2006, $6B loss)

**วิธีฝึก psychological discipline:**
- เริ่มจาก portfolio เล็กๆ → ค่อยๆ scale ขึ้น
- มี income อื่น/ธุรกิจอื่น เพื่อไม่ให้กดดันเกินไป

---

## Part V: Special Topics (Chapter 7)

### Chapter 7 — หัวข้อพิเศษใน Quantitative Trading

#### 1. Mean-Reverting vs Momentum Strategies

| ประเภท | Mean-Reverting | Momentum |
|--------|---------------|----------|
| สมมติฐาน | ราคากลับสู่ mean | ราคาไปต่อในทิศทางเดิม |
| สาเหตุ | Liquidity events, mispricing | Slow diffusion of info, large orders, herding |
| Holding period | กำหนดด้วย half-life (Ornstein-Uhlenbeck) | กำหนดด้วย backtest (fragile) |
| ผลของ competition | กำจัด arbitrage ทีละน้อย | ลด holding period ลง |
| Stop loss | เหมาะกับ holding period/profit cap มากกว่า | เหมาะกับ stop loss |

**Mean reversion มี prevalent กว่า** — แต่ backtest ยากเพราะ survivorship bias + data errors ทำให้ดูดีเกินจริง

**Momentum** เกิดจาก:
- Slow diffusion of information (PEAD — Post Earnings Announcement Drift)
- Incremental execution ของ large institutional orders
- Herd-like behavior ของ investors

#### 2. Regime Switching

- "Regimes" เช่น bull/bear, high/low volatility, mean-reverting/trending
- **Markov Regime-Switching Models** — กำหนด transition probability แต่ใช้งานจริงไม่ได้เพราะ assumes constant transition probability
- **Turning Points Models** — data mining approach, ใส่ variables ทั้งหมด (volatility, macroeconomic data, media chatter)
- **Alphacet Discovery** — ใช้ perceptron (neural network) กับ technical indicators + holding period optimization → ได้ return 37.93% ใน 6 เดือน backtest

#### 3. Stationarity and Cointegration

- **Stationary** = ไม่ drift ออกจากค่าเริ่มต้น → เหมาะกับ mean reversion
- **Cointegration** = long ตัวหนึ่ง + short อีกตัว → spread เป็น stationary
- ตัวอย่าง: GLD vs GDX — long 1 หุ้น GLD + short 1.6766 หุ้น GDX → spread stationary (t-stat = -3.36, >95% confidence)
- **Correlation ≠ Cointegration**: KO vs PEP มี correlation 0.48 แต่ไม่ cointegrated (spread drift)

**Cointegration Test (CADF):**
- ใช้ Augmented Dickey-Fuller test
- t-statistic < critical value → cointegrated
- หา hedge ratio จาก OLS regression

#### 4. Factor Models (APT)

- Returns = Factor exposures × Factor returns + Specific returns
- **Fama-French Three-Factor**: beta + market cap + book-to-price ratio
- Factor returns มักมี momentum → ทำนาย returns ได้
- **PCA (Principal Component Analysis)** — ใช้ historical returns สร้าง factors; แต่ผลไม่ดี (avg return = -1.81)

#### 5. Exit Strategies

| Exit Type | เหมาะกับ | วิธีกำหนด |
|-----------|----------|----------|
| Fixed holding period | ทุกแบบ (default) | Backtest หา optimal |
| Target price / Profit cap | Mean-reverting | Mean value จาก Ornstein-Uhlenbeck |
| Latest entry signal | ทุกแบบ (เมื่อ model รันบ่อยกว่า holding period) | ถ้า signal ตรงข้าม position → exit |
| Stop loss | Momentum เท่านั้น | ไม่แนะนำสำหรับ reversal |

**Ornstein-Uhlenbeck Half-Life:**
- `dz(t) = -θ(z(t) - μ)dt + dW`
- Half-life = ln(2)/θ — ใช้กำหนด optimal holding period สำหรับ mean-reverting strategy
- ตัวอย่าง GLD-GDX: half-life ≈ 10 วัน

#### 6. Seasonal Trading

- **หุ้น (January Effect)** — ผลตอบแทนลดลงในปีหลังๆ (competition + awareness สูง)
- **Commodity Futures** — ยังมีกำไร:
  - Gasoline futures: ซื้อ mid-April, ขาย end-April — กำไร 13 ปีติดต่อกัน
  - Natural gas: ซื้อ end-Feb (June contract), ขาย mid-April — กำไร 14 ปีติดต่อกัน
  - สาเหตุ: demand จริงทางเศรษฐกิจ (summer driving, air conditioning)

#### 7. High-Frequency Trading

- ไม่ถือ position overnight
- Sharpe ratio สูงเพราะ **law of large numbers** — ยิ่งเทรดมาก ยิ่ง variance ต่ำ
- Leverage ได้สูง → return สูง
- แต่ backtest ยาก — ต้องมี bid/ask data, order book data
- Execution speed สำคัญมาก — ใช้ C, collocate servers ที่ exchange

#### 8. High-Leverage vs High-Beta Portfolio

- Kelly formula บอกว่า growth rate ∝ Sharpe ratio² ไม่ใช่ return
- Low-beta portfolio + leverage > High-beta portfolio (ที่ Sharpe สูงกว่า)
- ตลาด underprice high-beta stocks เรื้อรัง → ควรเลือก low-beta แล้ว leverage ขึ้น

---

## Part VI: Conclusion (Chapter 8)

### Chapter 8 — นักเทรดอิสระจะประสบความสำเร็จได้หรือไม่?

**ทำไมนักเทรดอิสระถึงชนะ Institutional Traders ได้:**

1. **Capacity** — Strategy ที่มี capacity ต่ำ (เช่น high-frequency, low-volume) ทำกำไรได้ดีสำหรับบัญชีเล็ก แต่ hedge fund ที่มี billions ไม่สามารถเทรดได้เพราะจะสร้าง market impact
2. **Constraints ของสถาบัน** — ถูกบังคับให้ market-neutral, sector-neutral, ไม่เทรด long-only, ไม่เทรด futures — ทุก constraint ลด optimal return
3. **Pressure จาก management** — สั่ง scale up เร็วตอนกำไร, สั่ง liquidate ทั้งหมดตอนขาดทุน
4. **Moral hazard** —  trader ในสถาบันมี upside ไม่จำกัด แต่ downside แค่ถูกไล่ออก → incentive เทรดเสี่ยงเกินไป
5. **Freedom** — นักเทรดอิสระไม่มี constraints เหล่านี้ → environment ใกล้เคียง optimal มากกว่า

**ข้อความสำคัญ:**
> "_far, far easier to generate a high Sharpe ratio trading a $100,000 account than a $100 million account. There are many simple and profitable strategies that can work at the low capacity end that would be totally unsuitable to hedge funds. This is the niche for independent traders like us."

**Next Steps สำหรับการเติบโต:**
1. เพิ่ม number of strategies (higher frequency หรือ longer holding period)
2. ขยายไป futures/currencies (capacity สูงกว่า)
3. Collaborate กับนักเทรดคนอื่น / จ้าง consultants
4. ลงทุนใน infrastructure, data, automation
5. เมื่อ capacity มากกว่าที่ Kelly แนะนำ → รับ investors หรือ profit-sharing กับ hedge fund

---

## บทเรียนสำคัญสำหรับระบบเทรดอัตโนมัติ (Automate Trade Agent Hub)

### 1. หลักการพื้นฐานที่ใช้ได้จริงกับระบบของเรา
- **Sharpe Ratio เป็นพระเอก** — ไม่ใช่ absolute return; Sharpe สูง → leverage ได้ → long-term growth สูง
- **Kelly Formula** สำหรับ position sizing — ใช้ half-Kelly เพื่อ safety
- **Survivorship bias** — ข้อมูลต้องมี quality; backtest ที่ดีต้องมี survivorship bias-free data
- **Data-snooping bias** — ไม่ควรมี parameters มากเกินไป; ใช้ out-of-sample testing เสมอ

### 2. Transaction Cost Analysis
- Strategy ที่ดูดีมาก่อนหักค่า → อาจแย่มากหลังหัก
- ต้องประมาณค่า transaction cost อย่าง realistic: round-trip S&P ≈ 10 bps, futures ≈ 2 bps
- **Paper trading** เป็น step สำคัญก่อนเทรดจริง

### 3. Risk Management
- **ไม่ overleverage** — ใช้ half-Kelly หรือต่ำกว่า
- **Cut losses** เมื่อ model ไม่ work แล้ว — ค่อยๆ ลด leverage ลงถึง 0
- **Fat tail events** — Black Monday (-20.47% วันเดียว) แสดงว่า Gaussian assumption มีข้อจำกัด
- **Psychological discipline** — อย่า override model ด้วยอารมณ์

### 4. Strategy Design Principles
- **Mean-reverting** strategies มี prevalent กว่า แต่ต้องระวัง data quality
- **Momentum** strategies มี holding period ที่สั้นลงเรื่อยๆ (competition)
- **Regime switching** ตรวจจับด้วย data mining + machine learning
- **Ornstein-Uhlenbeck** ใช้หา half-life สำหรับ mean-reverting holding period
- **Cointegration** ≠ Correlation — ต้องใช้ test ที่ถูกต้อง
- **Seasonal strategies** ใน commodity futures ยังทำงานได้ดี

### 5. การนำไปใช้กับ MeowQuant SMC v4
- หนังสือเน้น **statistical arbitrage บน equity** แต่หลักการ Sharpe/Kelly/risk management ใช้ได้กับทุก asset class
- **Simple strategies work best** — สอดคล้องกับ SMC approach ที่ใช้ OB/FVG/MSB patterns
- **Per-symbol optimization** — เหมือนที่ Ernie บอกว่า strategy ต้อง fit กับ circumstances ของ trader
- **Backtest pitfalls** — look-ahead bias, data-snooping, transaction costs ต้องตรวจทุกครั้ง
- **Regime awareness** — ควรรู้ว่าตลาดกำลัง mean-reverting หรือ trending เพื่อเลือก strategy ที่ถูกต้อง
