# 📘 สรุปหนังสือ Successful Algorithmic Trading — Michael Halls-Moore

> **ที่มา:** [[finance-successful-algorithmic-trading]]
> **ภาษา:** ไทย (สรุปจาก EN chunks 21 ไฟล์)
> **วันที่:** 2026-07-20

---

## 🧭 ภาพรวมหนังสือ

*Successful Algorithmic Trading* โดย Michael Halls-Moore (ผู้ก่อตั้ง QuantStart) เป็นคู่มือเชิงปฏิบัติสำหรับนักเทรดรายย่อยและมืออาชีพที่มีพื้นฐานการเขียนโปรแกรม ต้องการสร้างระบบเทรด algorithmic ที่อัตโนมัติและมีกำไร ด้วยภาษา **Python**

หนังสือแบ่งออกเป็น **6 Parts** (16 บท) ครอบคลุมตั้งแต่แนวคิด—การตั้งค่าสภาพแวดล้อม—การจัดการข้อมูล—การสร้างแบบจำลองทางสถิติ—การวัดผล—ไปจนถึงการสร้างระบบ backtest และ execution แบบ event-driven แบบ end-to-end

---

## 📖 Part I — แนวคิดหลัก (Ch 1-2)

### Ch 1: Introduction

- **QuantStart**: เว็บไซต์ที่ Michael Halls-Moore ก่อตั้งในปี 2010 เพื่อช่วยเหลือ junior quantitative analyst
- **กลุ่มเป้าหมาย**: นักเทรดรายย่อย + professional quants ที่มีความรู้ programming พื้นฐาน
- **เป้าหมาย**: สร้างระบบ algorithmic trading ที่ profitable และ robust ด้วย Python
- **ข้อควรมี**: ความคุ้นเคยกับ trading แบบ discretionary, พื้นฐาน programming (ตัวแปร, if-else, loop)
- **โครงสร้างหนังสือ**: ตั้งแต่เหตุผลที่ควรใช้ algo trading → การพัฒนา → การออกแบบระบบ → การติดตั้ง → time series analysis → optimization → performance measurement → risk management → strategy implementation → execution

### Ch 2: What Is Algorithmic Trading?

- **นิยาม**: การใช้ระบบอัตโนมัติในการเทรด โดยปราศจากการแทรกแซงของมนุษย์

**ข้อดีของ Algo Trading:**
- **Historical Assessment**: สามารถ backtest ย้อนหลังบนข้อมูลในอดีต
- **Efficiency**: ไม่ต้องเฝ้าหน้าจอตลอดเวลา
- **No Discretionary Input**: กำจัด fear/greed ออกจากการตัดสินใจ
- **Comparison**: สามารถเปรียบเทียบ Sharpe ratio, drawdown ระหว่าง strategy ได้อย่างเป็นระบบ
- **Higher Frequencies**: เป็นไปได้เฉพาะระบบ automation

**ข้อเสีย:**
- **Capital Requirements**: ต้องใช้เงินทุนมากกว่า (IB ขั้นต่ำ $10,000, PDT rule $25,000)
- **Programming/Scientific Expertise**: ต้องเขียนโปรแกรมและเข้าใจสถิติ
- **Data costs**: $300-500/เดือนสำหรับ intraday data

**Scientific Method for Trading:**
- ใช้ Observation → Hypothesis → Prediction → Testing → Analysis
- แตกต่างจาก data mining / black box approach ที่พารามิเตอร์ล้นเกิน
- ถ้า strategy พัง สามารถกลับไป revisit hypothesis ได้

**ทำไมต้อง Python?**
- เรียนรู้ง่าย, Libraries ครบ (NumPy, SciPy, pandas, statsmodels, scikit-learn, matplotlib)
- Development speed รวดเร็ว, Execution speed เพียงพอ (ใช้ Cython ถ้าต้องการเร็วขึ้น)
- เชื่อมต่อ broker ได้ (IbPy, FIX)
- **ฟรี**, open source, cross-platform

**Retail vs Institutional:**
- Retail ได้เปรียบ: เล่นตลาดเล็กได้, ไม่ต้อง follow the crowd, ไม่มี market impact
- Retail เสียเปรียบ: ไม่มี prime brokerage, leverage ถูกจำกัด, ไม่มี client news flow
- **ข้อได้เปรียบใหญ่สุด**: เลือก tech stack ได้อิสระ ไม่มี legacy system

---

## 📖 Part II — Trading Systems (Ch 3-5)

### Ch 3: Successful Backtesting

**Backtesting = การจำลอง strategy บนข้อมูลในอดีตเพื่อประเมินประสิทธิภาพ**

**ประโยชน์:**
- **Filtration**: คัด strategy ที่ไม่ผ่านเกณฑ์
- **Modelling**: ทดสอบ transaction costs, order routing, latency
- **Optimisation**: ปรับพารามิเตอร์เพื่อเพิ่มประสิทธิภาพ
- **Verification**: ตรวจสอบว่าการ implement strategy ถูกต้อง

**4 Biases สำคัญที่ต้องระวัง:**

1. **Optimisation Bias (Curve Fitting)**: ปรับพารามิเตอร์จน fit ข้อมูลในอดีต แต่พอเทรดจริงพัง → ต้อง minimize จำนวนพารามิเตอร์, ใช้ sensitivity analysis
2. **Look-Ahead Bias**: ใช้ข้อมูลอนาคตในจุดที่不该มี → เกิดจาก index offset, parameter calculation, maxima/minima
3. **Survivorship Bias**: ทดสอบเฉพาะ symbol ที่รอดถึงปัจจุบัน ไม่รวมของที่ delisted → Yahoo Finance มี bias นี้
4. **Cognitive Bias**: นักเทรดทน Drawdown ไม่ไหวทั้งที่ backtest เคยเห็น → ต้องเตรียมใจว่า drawdown จะเกิดขึ้นจริง

**Exchange Issues:**
- **Market orders**: ได้ fill แน่นอนแต่ราคาไม่แน่นอน
- **Limit orders**: ราคาแน่นอนแต่ไม่แน่นอนว่าได้ fill
- OHLC data จาก Yahoo มีปัญหาการ aggregation จากหลาย exchange
- Forex ต้องใช้ bid-ask quote จาก venue เดียวกับที่จะเทรด
- **Shorting constraints**: หุ้นบางตัวอาจยืมมา short ไม่ได้

**Transaction Costs (3 ประเภทหลัก):**
1. **Commission**: ค่านายหน้า, ค่าธรรมเนียม clearing, stamp duty
2. **Slippage**: ผลต่างระหว่างราคาตัดสินใจเทรดกับราคาที่ได้จริง — momentum strategy เสีย slippage มากกว่า mean reversion
3. **Market Impact**: คำสั่งใหญ่ในตลาดสภาพคล่องน้อยจะเคลื่อนราคา — spread ก็เป็น transaction cost

**ข้อควรจำ**: Backtest คือ **idealized upper bound** ไม่ใช่ performance จริง

### Ch 4: Automated Execution

**Software Landscape — เกณฑ์เลือก:**
- Programming Skill → ควบคุม tech stack เองดีที่สุด
- Execution Capability — IB API ดีกว่า TradeStation ที่บังคับ broker
- Customisation — Python/MATLAB ยืดหยุ่นกว่า
- Speed of Development — prototyping ไม่กี่สัปดาห์
- Speed of Execution — HFT ต้อง C/C++, FPGA

**Research Tools vs Event-Driven Backtesters:**
| Research Tools | Event-Driven |
|:---------------|:-------------|
| เร็ว, ไร้เดียงสา | สมจริง, ใช้ code เดียวกับ live |
| Vectorized, reuse | ต้องสร้าง component แยก |
| เหมาะ első pass | เหมาะ production |

**Colocation (Hardware):**
1. **Home Desktop**: ถูกที่สุด แต่ power failure, internet down, ต้อง restart
2. **VPS** (Amazon EC2, Rackspace): 24/7, robust, latency อาจไม่ดีกว่า
3. **Exchange Colocation**: เร็วที่สุด แต่แพงเกิน retail

### Ch 5: Sourcing Strategy Ideas

**"Know Thyself":** สิ่งสำคัญที่สุด — มีวินัย, อดทน, detached emotionally

**Source ของ Strategy Ideas:**
- **Textbooks**: Ernest Chan ("Quantitative Trading", "Algorithmic Trading"), Rishi Narang ("Inside The Black Box"), Euan Sinclair ("Volatility Trading")
- **Internet**: Quant Blogs (QuantStart, Epchan, Quantivity), Aggregators (Quantocracy, Reddit r/algotrading), Forums (Elite Trader, Wilmott)
- **Academic Journals**: arXiv (q-fin), SSRN, Journal of Computational Finance
- **Independent Research**: Market microstructure, Fund structure arbitrage, Machine learning

**Criteria ในการประเมิน Strategy:**
- Methodology (momentum? mean-reverting? เข้าใจไหม?)
- Sharpe Ratio (risk-adjusted return)
- Leverage (ต้องใช้ leverage ขนาดไหน?)
- Frequency (เชื่อมโยงกับ tech stack + transaction costs)
- Volatility / Win-Loss Profile
- Max Drawdown (สำคัญที่สุดในการตัดสินใจส่วนบุคคล)
- Capacity / Liquidity
- Parameters (ยิ่งมาก = ยิ่งเสี่ยง curve fitting)
- Benchmark (S&P500 สำหรับ US equities)

**Historical Data Considerations:**
- Fundamental data (interest rates, earnings)
- News data (sentiment analysis)
- Asset price data (OHLCV)
- Frequency: daily → intraday → tick/order book
- Technology stack: RDBMS (MySQL), NoSQL (MongoDB), HDF5

---

## 📖 Part III — Data Platform Development (Ch 6-8)

### Ch 6: Software Installation

- **OS เลือก Ubuntu Linux** — package management ดีที่สุดสำหรับ Python trading stack
- **Python Libraries**: NumPy, SciPy, pandas, IPython, matplotlib, scikit-learn, statsmodels
- **Broker Connection**: IbPy (Python wrapper สำหรับ Interactive Brokers API), Trader Workstation (TWS)
- ติดตั้งผ่าน `pip` + `apt-get` สำหรับ dependencies

### Ch 7: Financial Data Storage

**Securities Master Database** = ระบบฐานข้อมูลกลางสำหรับเก็บราคาหุ้น/ETF/ฟิวเจอร์

**3 รูปแบบการจัดเก็บ:**
1. **Flat-File (CSV)**: ง่าย, เหมาะ archive, แต่ query ช้า
2. **Document Stores/NoSQL** (MongoDB): เหมาะ fundamental data ไม่เหมาะ time series
3. **RDBMS** (MySQL): เหมาะที่สุดสำหรับ OHLCV — transaction safe, query ได้

**Schema Design:**
- `exchange` table: abbreviation, name, city, currency
- `data_vendor` table: name, website (Yahoo Finance, Google Finance)
- `symbol` table: ticker, instrument type, sector
- `daily_price` table: OHLCV + adj_close, **ใช้ DECIMAL(19,4) ไม่ใช่ FLOAT** (precision matters)

**การดึงข้อมูล:**
- Wikipedia scraping → insert symbols
- Yahoo Finance API → download OHLCV → insert daily_price
- ใช้ `pandas.read_sql_query()` เพื่อดึงข้อมูลมาวิเคราะห์

**Data Accuracy Issues:**
- Corporate actions (stock splits, dividends)
- Price spikes (Flash Crash)
- Missing data (trading holidays)
- Backfilling (vendor แก้ไขข้อมูลอดีต → backtest เปลี่ยน)

### Ch 8: Processing Financial Data

**ประเภทข้อมูล:**
| ความถี่ | Bars/Symbol | Storage |
|:--------|:-----------:|:-------|
| Daily (EOD) | 2,520/10yr | RDBMS |
| Minutely | ~1M/10yr | RDBMS |
| Tick/Order Book | ~60M/10yr | HDF5, kdb |

**Source ข้อมูลฟรี:**
- **Yahoo Finance**: ฟรี แต่ backfilling, aggregation errors, API บางครั้งให้ผลต่าง
- **Quandl**: API ดี, 50-500 calls/day, มี futures data

**Source ข้อมูลเสียเงิน:**
- **DTN IQFeed**: $50-200/month, tick-by-tick data, Windows-only server
- **QuantQuote**: S&P500 survivorship-bias-free minutely data $895

**Continuous Futures Contracts:**
- ปัญหา: ฟิวเจอร์มี expiration — ต้อง stitch series
- 3 วิธี:
  1. **Back/Forward (Panama) Adjustment**: shift prices — ทำให้ trend bias
  2. **Proportionality Adjustment**: ใช้ ratio (เหมือน stock split) — ใช้คำนวณ % return ได้
  3. **Rollover/Perpetual Series**: linear weighted blend — เหมาะ backtesting

**Data Cleansing:**
- Filter spike, handle missing data (pad forward, interpolate, or ignore)
- ต้อง logging เมื่อมีการแก้ไข historical data point

---

## 📖 Part IV — Modelling (Ch 9-11)

### Ch 9: Statistical Learning

- **เป้าหมาย**: สร้าง model ที่ประมาณ `Y = f(X) + ε`
- **Prediction**: ทำนาย Y จาก X ใหม่ — ไม่สนใจรูปแบบของ f
- **Inference**: เข้าใจความสัมพันธ์ระหว่าง X และ Y
- **Parametric Models**: กำหนดรูปแบบ f ก่อน (เช่น linear) — estimate แค่ parameter (β)
- **Non-Parametric Models**: ยืดหยุ่นกว่า แต่ต้องใช้ข้อมูลมากกว่า และ overfit ง่าย
- **Supervised Learning**: มี X และ Y — พวก regression, classification
- **Unsupervised Learning**: มีแค่ X — พวก clustering, PCA

**เทคนิคสำคัญ:**
- **Regression**: Linear (OLS), Logistic (classification)
- **Classification**: LDA, QDA, SVM, Random Forest, Decision Trees
- **Time Series Models**: ARIMA (ค่า), ARCH/GARCH (volatility)
- **Continuous Models**: GBM, Heston, Ornstein-Uhlenbeck

### Ch 10: Time Series Analysis

**Mean Reversion Testing:**

1. **Augmented Dickey-Fuller (ADF) Test**
   - ทดสอบว่า `γ = 0` (random walk) หรือไม่
   - ค่า test statistic ที่ negative มาก → reject null → mean reverting

2. **Hurst Exponent (H)**
   - H < 0.5 = mean reverting
   - H = 0.5 = Geometric Brownian Motion (random walk)
   - H > 0.5 = trending

**Cointegration (CADF):**
- หา hedge ratio β ระหว่าง 2 หุ้น (OLS) → ทดสอบว่า residual stationary หรือไม่
- **AREX และ WLL**: CADF test ที่ 5% significant → pair trading ได้
- CADF ดีกว่า ADF ตรงที่หา β ให้เอง (ไม่ต้องเดา)

### Ch 11: Forecasting

- **เป้าหมาย**: ทำนายทิศทาง S&P500 (+1 หรือ -1) ด้วย lagged returns

**Measuring Accuracy:**
- **Hit Rate**: สัดส่วนที่ predict ถูก
- **Confusion Matrix**: UP-UP, UP-DOWN, DOWN-UP, DOWN-DOWN

**Classifiers (6 ตัว):**
1. **Logistic Regression**: ให้ probability, probabilistic interpretation
2. **LDA**: สมมติ multivariate Gaussian, covariance matrix เดียวกัน
3. **QDA**: covariance matrix แยก — ดีเมื่อ non-linear boundary + มี data เยอะ
4. **Linear SVC**: linear separating boundary
5. **RSVM (Radial SVM)**: non-linear boundary via kernel trick — computational expensive
6. **Random Forest**: ensemble ของ decision trees — interpretable, ทน overfit

**ผลลัพธ์บน S&P500 (2005):**
- QDA = 59.9% (best)
- LR/LDA/LSVC = 56%
- RSVM = 56.3%
- RF = 50.4%
- **สรุป**: lagged returns มี predictive power น้อย — ทุกตัว hit rate 50-60%

---

## 📖 Part V — Performance & Risk Management (Ch 12-13)

### Ch 12: Performance Measurement

**Trade Analysis:**
- Total PnL, Avg Period PnL, Max Win/Loss, Win/Loss Ratio
- Trend-following: ชนะน้อยครั้งแต่กำไรเยอะ, Mean-reversion: ชนะบ่อยแต่เสียครั้งเดียวเยอะ

**Strategy/Portfolio Analysis:**

| Metric | สูตร |
|:-------|:-----|
| **Sharpe Ratio** | `E(Ra - Rb) / σ(Ra - Rb)` |
| **Sortino Ratio** | ใช้ downside deviation แทน std |
| **CALMAR Ratio** | `E(Ra - Rb) / max drawdown` |
| **CAGR** | ((Pf/Pi)^(1/n) - 1) × 100 |

**Sharpe Ratio — ข้อควรรู้:**
- N = 252 (daily), 252×6.5 (hourly), 252×6.5×60 (minutely)
- Market-neutral → ไม่ต้อง subtract risk-free rate
- **ข้อเสีย**: backward looking, สมมติ normal distribution, ignore tail risk
- "Good" Sharpe: S > 1 (rule of thumb), S > 2 (retail ดีมาก), S > 3 (fund ระดับ top)
- **ต้องคำนวณ NET of transaction costs เสมอ**

**Drawdown Analysis:**
- Maximum Drawdown (MDD) = จุดสูงสุดถึงต่ำสุด
- Drawdown Duration = ระยะเวลาที่ equity อยู่ under water
- **สำคัญที่สุด**: ถ้า equity หมด ไม่มี metric ไหนสำคัญอีก
- ต้องตั้ง "stop loss" ระดับ portfolio ไว้ล่วงหน้า

### Ch 13: Risk and Money Management

**5 ประเภทความเสี่ยง:**
1. **Strategy/Model Risk**: curve-fitting, bias, assumption mismatch
2. **Portfolio Risk**: factor loading, sector concentration, correlation
3. **Counterparty Risk**: broker ล้มละลาย
4. **Market Risk**: regime change
5. **Operational Risk**: IT failure, single point of failure, regulatory change

**Kelly Criterion:**
```
f_i = μ_i / σ_i²    (optimal leverage)
g = r + S²/2        (expected growth rate)
```
- ตัวอย่าง: μ=7.7%, σ=12.4% → f=5.01 (leverage 5×), g=22%
- **Kelly ปรับสมดุลทุกวัน**: กำไร → borrow more, ขาดทุน → sell into loss (emotional แต่ถูกทางคณิตศาสตร์)
- **Half-Kelly** เพื่อ safety — ใช้ Kelly เป็น upper bound
- ต้อง recalculation ทุก 3-6 เดือน

**Value-at-Risk (VaR):**
- **นิยาม**: ขาดทุนสูงสุดที่ confidence level 95% หรือ 99%
- **Variance-Covariance Method**: ใช้ normality assumption
- **ข้อเสีย**: ไม่ครอบคลุม extreme event (tail risk), backward looking
- David Einhorn: "VaR is an airbag that works all the time, except when you have a car accident."

---

## 📖 Part VI — Automated Trading (Ch 14-16)

### Ch 14: Event-Driven Trading Engine

**แนวคิด**: Event Loop — คอยรับ event → dispatch ไปยัง component ที่เหมาะสม

**6 Component หลัก:**
1. **Event**: Base class → MarketEvent, SignalEvent, OrderEvent, FillEvent
2. **Event Queue**: Python `Queue.Queue()`
3. **DataHandler**: ABC ที่ให้ interface เดียวกันสำหรับ historical และ live data → `HistoricCSVDataHandler`
4. **Strategy**: ABC method `calculate_signals()` → สร้าง SignalEvent
5. **Portfolio**: จัดการ position, holdings, risk → สร้าง OrderEvent
6. **ExecutionHandler**: `SimulatedExecutionHandler` (backtest) → `IBExecutionHandler` (live)

**Interaction Flow:**
```
MarketEvent → Strategy.calculate_signals() → SignalEvent
→ Portfolio.update_signal() → OrderEvent
→ ExecutionHandler.execute_order() → FillEvent
→ Portfolio.update_fill()
```

**Performance helper functions:**
- `create_sharpe_ratio(returns, periods)` — ปรับ periods ให้ตรง TF
- `create_drawdowns(pnl)` — คำนวณ high water mark, drawdown, duration

**Live Execution via Interactive Brokers:**
- ใช้ IbPy `ibConnection`, `Contract`, `Order`
- ต้อง `time.sleep(1)` หลัง placeOrder มิฉะนั้น API inconsistent

### Ch 15: Trading Strategy Implementation

**3 Strategy Implemented:**

1. **Moving Average Crossover (AAPL, daily)**
   - Short SMA 100 / Long SMA 400
   - **Result**: Total Return -0.79%, Sharpe -0.09, Max DD 2.56% ❌
   - ประโยชน์: non-trivial strategy แรกที่ใช้ทดสอบ backtest engine

2. **S&P500 Forecast Trade (SPY, daily)**
   - ใช้ QDA + lag 2 วัน ทำนายทิศทาง → LONG เมื่อ predict +1, EXIT เมื่อ -1
   - **Result**: Total Return +9.84%, Sharpe 0.54, Max DD 5.99% ⚠️
   - ถือว่า buy-and-hold ให้ผลใกล้เคียง — daily data สร้าง edge ยาก

3. **Mean-Reverting Equity Pairs (AREX/WLL, minutely)**
   - **Key concept**: pairs trading = ซื้อตัวหนึ่งขายอีกตัวหนึ่งเมื่อ spread กว้าง
   - ใช้ OLS หา hedge ratio → z-score → entry/exit threshold
   - **Result (100 shares)**: Total Return +15.90%, Sharpe 1.89, Max DD 3.03% ✅
   - **Result (2000 shares)**: +392.85%, Sharpe 2.29, Max DD 45.69%
   - ต้องปรับ DataHandler และ Portfolio ให้ใช้ "close" แทน "adj_close" (DTN format)
   - Sharpe ratio ต้องใช้ `periods=252*6.5*60` (minutely)

### Ch 16: Strategy Optimisation

**3 ปัญหาหลักของ Parameter Optimisation:**
1. **Overfitting**: optimize จน performance ตอน backtest ดีแต่ live พัง — สัมพันธ์กับ bias-variance tradeoff
2. **Computational Cost**: 3 parameters × 5 values each = 125 simulations
3. **Which Parameters to Optimise?**: Model parameters (C, γ) ≠ Trading parameters (entry threshold)

**Model Selection Techniques:**

1. **Train/Test Split**: 2-fold — 80% train, 20% test — แต่ sequential data ควรใช้ chronological split
2. **K-Fold Cross Validation**: K=10 — ทุก fold เป็น train 1 ครั้ง, test 1 ครั้ง — Scikit-Learn `KFold`
3. **Grid Search + Cross Validation**: `GridSearchCV` — Search C, γ สำหรับ SVM → optimize hit rate

**Trading Strategy Optimisation — Intraday Pairs:**
- 3 parameters: OLS window (50/100/200), Z-entry (2.0/3.0/4.0), Z-exit (0.5/1.0/1.5)
- **27 simulations**, ~3 ชั่วโมง
- **Best Sharpe (3.71)**: OLS=50, Z_entry=3.0, Z_exit=1.5 → Return 294.91%, DD 8.04%
- **Best Return (461.99%)**: OLS=200, Z_entry=2.0, Z_exit=1.0 → Sharpe 3.64, DD 10.53%
- Heatmap แสดง: entry/exit threshold สูง → Sharpe สูง, DD ต่ำ

---

## 🎯 Takeaways หลักจากหนังสือ

1. **Scientific Method > Data Mining**: เริ่มจาก hypothesis แล้วทดสอบ ไม่ใช่ fitting อินดิเคเตอร์มั่ว
2. **Transaction Costs Kill Everything**: ไม่รวม costs = backtest ไร้ความหมาย — slippage + commission + spread
3. **Higher Frequency = Higher Sharpe**: intraday pairs Sharpe 1.89-3.71 vs daily -0.09 ถึง 0.54
4. **Event-Driven > Vectorized**: เหมาะ production เพราะใช้ code เดียวกับ live trading
5. **Retail ยังสู้ institution ได้**: capacity, tech stack, correlation — แต่ต้องมีวินัยและ patience
6. **Kelly + Half-Kelly**: ใช้เป็น upper bound ของ leverage — อย่าใช้ full Kelly โดยตรง
7. **Backtest = Upper Bound**: expectation ต้องต่ำกว่าผล backtest เสมอ
8. **Risk Management > Alpha Model**: drawdown คือสิ่งที่ kill account จริงๆ
9. **Data Quality = Foundation**: garbage in, garbage out — securities master ต้อง robust
10. **Param Optimisation ต้องมี CV**: grid search + k-fold — minimize overfitting

---

*Summarized in Thai from 21 EN chunks — Successful Algorithmic Trading by Michael Halls-Moore*
