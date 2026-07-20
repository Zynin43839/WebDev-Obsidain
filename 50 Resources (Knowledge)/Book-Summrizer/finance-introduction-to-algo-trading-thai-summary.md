# บทสรุปภาษาไทย — Introduction to Algorithmic Trading
> **ต้นฉบับ:** QuantInsti.com (2018) | **ผู้เรียบเรียง:** สรุปจาก 11 chunks ทั้งหมด

---

## 1. ข้อมูลคือทุกสิ่ง (Data is Everything)

### ที่มาของข้อมูลตลาด
- ตลาดหลักทรัพย์ เช่น NSE (อินเดีย) ให้ข้อมูล Quote ผ่านบริษัทลูก DotEx International Ltd. ซึ่ง broadcast ข้อมูลแบบ real-time ให้แก่หน่วยงานต่างๆ
- ข้อมูลมีหลายประเภท ได้แก่ Capital Market, Futures & Options (F&O), Wholesale Debt Market (WDM), Securities Lending & Borrowing (SLBM), Currency Derivatives (CDS)

### ระดับข้อมูล (Data Levels)
- **Level 1**: Best Bid/Ask + ขนาด Bid/Ask — เพียงพอสำหรับนักเทรดทั่วไปในการวิเคราะห์กราฟและสร้างกลยุทธ์
- **Level 2**: Market depth ถึง 5 ระดับ bid/ask — ใช้โดยนักเทรดที่มีประสบการณ์
- **Level 3**: Market depth ถึง 20 ระดับ — ใช้โดย HFT firm และสถาบัน
- **Tick-By-Tick (TBT)**: ทุก order หรือการเปลี่ยนแปลง order ทุกรายการ

### Data Vendors และ Charting Platform
- Data vendors ที่นิยม ได้แก่ eSignal, GlobalDataFeeds, iCharts, ValveNet
- บาง vendor ให้แค่ data feed บางรายให้ทั้ง charting platform + analytics + watch list
- eSignal มี 3 ผลิตภัณฑ์: SIGNATURE (นิยมสุด), CLASSIC, ELITE — รองรับ streaming real-time data, charting, backtesting, historical data 1 ปี, news
- **QLink service** ของ eSignal ช่วย download ข้อมูล real-time ลง Excel สำหรับวิเคราะห์และสร้าง strategy

---

## 2. Charting Platforms

### แพลตฟอร์มยอดนิยม
- **NinjaTrader** — ใช้ C# programming language
- **TradeStation** — ภาษา scripting คล้ายภาษาอังกฤษ
- **MetaStock** — มีหลายรุ่น: Real Time, Xenith, Daily Charts, DataLink, Third Party add-ons
- **AmiBroker**
- **eSignal**

### ฟีเจอร์หลักที่ต้องพิจารณา
- Real-time scanning, technical indicators, expert advisors
- Backtesting, company fundamentals, news services
- Auto trade execution, forecasting, Level 2 data
- **สำคัญ**: ทดลองใช้ demo ก่อนสมัคร ทำความเข้าใจ pricing (ค่า software + data feed + exchange fees + add-ons แยกกัน)

---

## 3. Programming

### ภาษาที่ใช้ใน Algorithmic Trading
- **Python** — ภาษาที่ได้รับความนิยมสูงสุดสำหรับ algo trading ทั่วโลก (เริ่มจาก Van Rossum สร้างตอนคริสต์มาส)
- **Java, MATLAB** — ใช้ร่วมกับ trading platform ผ่าน external wrappers
- รองรับหลายกลยุทธ์: momentum, mean reversion, scalping, machine learning, sentiment-based

### ความรู้ Programming จำเป็นอย่างยิ่ง
- ต้อง coding strategy จากข้อมูล historical/real-time
- บาง platform มี scripting language เฉพาะ สำหรับ backtesting ภายในตัว
- นักเทรดที่ต้องการ success ต้องมี programming knowledge ที่แข็งแรง

---

## 4. การเลือก Broker

### ปัจจัยสำคัญ 6 ข้อ
1. **Speed and reliability** ของ trading platform
2. **Segments offered** — ตลาดที่ broker ให้บริการ
3. **Brokerage** — ค่าธรรมเนียม
4. **Leverage & margin requirements**
5. **Compatibility** ของ charting software กับ broker platform
6. **Gateway API** ที่ broker ให้บริการ

### ตัวอย่าง broker ยอดนิยม (ตลาดอินเดีย)
- Interactive Brokers, MasterTrust, Presto ATS (Symphony Fintech), Composite Edge, Zerodha

### การใช้ API
- Zerodha ให้ HTTP APIs บน web-based trading platform — เข้าถึง profile, funds, order history, positions, live quotes
- รองรับทุกภาษา: Excel VBA, Python, Java, C#
- นักเทรดต้องเข้าใจวิธีใช้งาน API ของ broker

---

## 5. ระบบคอมพิวเตอร์สำหรับ Algo Trading

### ทำไมต้อง Desktop?
- Trading ด้วย laptop ไม่เสถียร จำกัด multi-tasking
- แนะนำ high-end desktop + multiple monitors

### Spec ขั้นต่ำ
- **Processor**: Intel Core i5, 2.40 GHz
- **RAM**: 3GB DDR3
- **OS**: Windows Vista–10, Mac (v10.7–10.13)
- **Software**: Anaconda (Python 2.7 & 3.6), R/R Studio, Microsoft Excel

---

## 6. กลยุทธ์ Algorithmic Trading

### กลยุทธ์หลัก 4 ประเภท

#### 6.1 Momentum / Trend Following
- ติดตามแนวโน้มของตลาด ใช้สถิติว่า trend จะต่อเนื่องหรือกลับตัว
- ตัวอย่าง: Moving Average Crossover (5-day SMA > 20-day SMA = Long, น้อยกว่า = Short)
- ข้อดี: ใช้ประโยชน์จาก behavioral bias ของนักลงทุน
- ข้อเสีย: volatility สูง, trend ไม่คงอยู่ตลอด, ต้อง stop loss ที่ดี

#### 6.2 Arbitrage
- ใช้ price inefficiencies ของ instrument เดียวกันในต่าง market destinations
- Market neutral — ใช้โดย hedge funds และ proprietary traders

#### 6.3 Statistical Arbitrage (Stat Arb)
- ใช้ mean reversion model กับ diversified portfolio
- ถือครองระยะสั้น (วินาทีถึงวัน)
- **Pairs Trading**: จับคู่หุ้นที่เคลื่อนไหวสอดคล้องกัน เมื่อเบี่ยงเบน → short ตัวที่ outperform, long ตัวที่ underperform
- ทำให้ strategy เป็น beta-neutral (ไม่เสี่ยง market direction)

#### 6.4 Market Making
- ให้ liquidity แก่หุ้นที่ไม่ค่อยมีการซื้อขาย
- กำไรจาก bid-ask spread
- ตัวอย่าง: ซื้อ IBM 1000 หุ้น @ $100 (ask) → ขาย @ $100.05 (bid) = กำไร $0.05/หุ้น × volume มหาศาล
- HFT ส่วนใหญ่เป็น passive market making

### Paradigms และ Modelling Ideas

#### Market Making Models
- **Model 1 — Inventory Risk**: มุ่งที่ preferred inventory position + risk appetite
- **Model 2 — Adverse Selection**: แยก informed trades (มีมุมมอง) กับ noise trades (ไม่มีมุมมอง)

#### Statistical Arbitrage — Modelling
- หาหุ้นที่มี historical co-movement → เปิด long/short เมื่อเบี่ยงเบนจาก equilibrium
- ต้องวิเคราะห์ liquidity cost curve (bid-ask spread × volume)

#### Momentum — Modelling
- ตรวจจับ price momentum: หุ้นที่ขึ้นต่อเนื่องหลายวัน/สัปดาห์/เดือน
- ตัวอย่าง: หุ้นที่อยู่ภายใน 10% ของ 52-week high, หรือ % price change 12–24 สัปดาห์
- **Earnings momentum**: กำไรจาก under-reaction ต่อ earnings surprise

#### Machine Learning
- ใช้ ML algorithm ทำนาย price movements ระยะสั้น
- ข้อดี: AI ปรับปรุง model ได้เองตามเวลา (ไม่ static เหมือน manual model)
- **Bayesian networks** ใช้ทำนาย market trends
- **Evolutional computation** (เลียนแบบ genetics) + deep learning สร้าง "digital trader" ที่ evolves ได้เอง

---

## 7. ขั้นตอนสร้าง Algorithmic Trading Strategy

### 7.1 ตัดสินใจเลือก Strategy Paradigm
- Market Making, Arbitrage, Alpha generation, Hedging, หรือ Execution-based
- ตัวอย่าง: Pairs Trading (statistical arbitrage, market neutral, beta neutral)

### 7.2 ตรวจสอบ Statistical Significance
- ทดสอบว่า strategy มีนัยสำคัญทางสถิติจริงหรือไม่
- ตัวอย่าง pairs trading: ทดสอบ co-integration ของคู่หุ้น

### 7.3 สร้าง Trading Model
- Code logic สำหรับ buy/sell signals
- กำหนด **Stop Loss** (จำกัดขาดทุน) และ **Take Profit** (ล็อคกำไร)

### 7.4 เลือก Quoting vs Hitting Strategy
- **Quoting**: ส่ง order 之一ฝั่ง → fill น้อยกว่า แต่ประหยัด bid-ask ฝั่งเดียว
- **Hitting**: ส่ง market order ทั้งสองฝั่งพร้อมกัน → fill มากกว่า แต่ slippage สูงกว่า
- ใช้ **Granger Causality Test** หา lead-lag relationship → quote ตัว lead, cover ตัว lag

---

## 8. Backtesting

### ทำไมต้อง Backtest?
- ทดสอบกลยุทธ์บนข้อมูล history ก่อนเสี่ยงเงินจริง
- กลยุทธ์ถือว่าดีถ้า backtest results สนับสนุน hypothesis

### หลักการ Backtest ที่ดี
- เลือกข้อมูล history ที่มี data points มากพอ (อย่างน้อย 100+ trades)
- ครอบคลุม market conditions หลายแบบ (bullish, bearish, sideways)
- **บวกค่า brokerage + slippage** ให้ผลลัพธ์สมจริง
- สำหรับ quoting strategy: สมมติ order fill ที่ last traded price

### เครื่องมือ Backtest
- **Python / R** — นิยมสุด (open source, packages มากมาย, community ใหญ่)
- **MATLAB** — ดีแต่มีค่า license
- **Amibroker** — technical indicators ดี
- **TradeStation** — ภาษา scripting ใกล้ภาษาอังกฤษ
- **NinjaTrader** — ใช้ C# + DotNet Framework

### กระบวนการ Backtest
1. สร้าง strategy ด้วย Python/R/MATLAB/Excel VBA
2. Backtest บน historical data
3. Paper trade ด้วย simulator (ทดสอบ execution ในสภาพจำลอง)
4. ตรวจสอบ performance metrics

### วิธีเลือก Automated Trading Platform
- **Backtesting** — ต้องมี Sharpe Ratio, Information Ratio
- **Programming Language** — ภาษาที่ใช้ backtest อาจต่างจากภาษาที่ใช้ live trading
- **Data** — รองรับ securities ที่ต้องการหรือไม่ (Bloomberg, Reuters, Forex, Equities)
- **Web-based vs Desktop** — Desktop มีฟีเจอร์มากกว่า
- **Complexity** — ต้องตรงกับความเชี่ยวชาญ
- **Number of Strategies** — จำกัดหรือไม่
- **Commissions/Costs** — ตรวจสอบค่าธรรมเนียมทั้งหมด
- **Technical Support & Uptime** — ประวัติ system outage

---

## 9. Risk and Performance Evaluation

### Metrics ที่ต้องติดตาม
| Metric | ความหมาย |
|--------|-----------|
| **CAGR (Total Returns)** | อัตราเติบโตเฉลี่ยต่อปี |
| **Hit Ratio** | อัตรา order ต่อ trade |
| **Avg Profit per Trade** | กำไรเฉลี่ยต่อเทรด |
| **Avg Loss per Trade** | ขาดทุนเฉลี่ยต่อเทรด |
| **Maximum Drawdown** | ขาดทุนสูงสุดจากจุดสูงสุดถึงจุดต่ำสุด |
| **Volatility of Returns** | Standard deviation ของ returns |
| **Sharpe Ratio** | ผลตอบแทน risk-adjusted (excess return ต่อหน่วย volatility) |

---

## 10. การตั้ง Algo Trading Desk

### 10.1 จดทะเบียนบริษัท
- จดเป็น Company, Partnership, LLP หรือ Individual
- ถ้าเป็น Hedge Fund ต้องขออนุมัติจาก regulator (SEBI อินเดีย, MAS สิงคโปร์)

### 10.2  капиталrequirements
- **HFT**: เงินทุน trading น้อยกว่า LFT แต่ค่า infrastructure/technology สูงกว่า
- **LFT**: รองรับ trading capital มากกว่า ขยาย scale ได้

### 10.3 Trading Paradigm
- Execution-based (เน้น best price)
- HFT (latency-sensitive: market making, scalping, arbitrage)
- Sentiment/ML/News-based (latency ไม่ critical เท่า HFT)

### 10.4 Market Access
- ประเภท membership: clearing, trading, trading cum clearing, professional clearing
- ผ่าน broker → compliance น้อยกว่า แต่เสียค่า brokerage
- HFT ไวต่อ transaction cost มาก

### 10.5 Infrastructure
- **Colocation**: server อยู่ใกล้ exchange (same LAN) — ลด latency
- **Hardware**: ต้อง update ทุก 1–2 ปี (technology เปลี่ยนเร็ว)
- **Network Equipment**: Router/Modem, Switch, NIC, FPGA
- **Network Lines**:
  - Trading Lease Line (ส่ง order)
  - Market Data Lease Line (รับ data: TBT หรือ Snapshot)
  - Lines between exchanges (Smart Order Routing)
  - Between premises and exchange
  - Test connectivity (test market ของ exchange)

---

## 11. Algo Trading Platform Architecture

### 3 องค์ประกอบหลัก
1. **Market Data Adapter (MDA)** — รับข้อมูลจาก exchange → แปลงเป็น format ที่ระบบเข้าใจ
2. **Complex Events Processing Engine (CEP)** — "สมอง" ของระบบ — logic หลักของ strategy อยู่ที่นี่
3. **Order Routing System (ORS)** — แปลงคำสั่งจาก CEP เป็น format ที่ exchange เข้าใจ
   - ใช้ **FIX protocol** (Financial Information Exchange) เป็น standard
   - บาง exchange มี native format เร็วกว่า FIX

### Backtesting + Paper Trading
- Backtest → ดู simulated P&L + risk statistics
- Paper trade → ทดสอบว่าไม่มี technical glitch ก่อน live

### Risk Management
- **Market Risk**: ตรวจสอบ market conditions
- **Operational Risk** (สำคัญสำหรับ HFT): failure ของ technology, network, data streams → ต้องมี multi-level checks
- ต้อง disconnect ได้ภายใน milliseconds ถ้ามีปัญหา

---

## 12. Conformance, Compliance และทีมงาน

### Conformance & Empanelment (อินเดีย)
- ต้องได้รับอนุมัติจาก exchange ก่อน live strategy
- ผ่าน mock demo ให้ exchange ทดสอบ
- CME ทดสอบ Trading Systems ทั้งระบบ (ไม่ต้อง test ทีละ strategy)

### Audit & Compliance
- HFT firms ในอินเดียต้อง audit ทุก 6 เดือน
- ต้องเก็บ order logs, trade logs, control parameters หลายปี

### ทีมงานที่ต้องมี
- **Trader/Strategist** — ออกแบบกลยุทธ์
- **IT Professional** — พัฒนาระบบ
- **Network Manager** — ดูแล infrastructure
- **Risk Manager** — จัดการความเสี่ยง
- **HR & Legal**
- **เริ่มต้นได้**: 3–5 คน + support staff ≈ 7–10 คน (startup 4–5 คนก็ได้)

---

## 13. การเรียนรู้ Algorithmic Trading

### 3 Core Areas
1. **Quantitative Analysis/Modeling** — statistics, time-series, statistical packages
2. **Programming Skills** — Python, R, MATLAB, C++, Java
3. **Trading/Financial Markets Knowledge** — instruments, strategies, risk management

### วิธีเรียนรู้
- **หนังสือ**: "Options, Futures, and Derivatives" (John C. Hull), "Algorithmic Trading: Winning Strategies" (Dr. Ernest Chan)
- **Online courses**: Quantra, Coursera, EPAT™ by QuantInsti
- **YouTube, podcasts** (เช่น Better System Trader), webinars
- **Competitions**: Kaggle, NumerAI, Topcoder, CrowdANALYTIX, DrivenData
- **เรียนจากผู้เชี่ยวชาญ**: เข้า workshop, เรียน online กับ practitioner

---

## 14. Machine Learning ใน Trading

### ML Algorithms ที่นิยม
- Linear Regression, Logistic Regression
- Random Forests (RF), Support Vector Machine (SVM)
- k-Nearest Neighbor (kNN)
- Classification and Regression Tree (CART)
- Deep Learning

### การประยุกต์ใช้
- วิเคราะห์ historical market behavior ด้วย large data sets
- หา optimal inputs (predictors) สำหรับ strategy
- หา parameter set ที่ดีที่สุด
- ทำนายการเทรด

### กองทุนที่ใช้ ML
- Medallion Fund, Citadel, D.E. Shaw (extent ไม่เปิดเผย)
- Rebellion Research, KFL Capital (pure AI-based)

---

## 15. อาชีพใน Algorithmic Trading

### ตลาดงาน
- Global algo trading market โต CAGR 10.3% (2016–2020)
- Goldman Sachs แทนที่ trader 600 คน ด้วย engineer 200 คน

### ตำแหน่ง Quant Researcher
- **หน้าที่**: พัฒนา quantitative trading strategy, ดูแล automated systems, วิเคราะห์ performance
- **ทักษะ**: derivatives, option strategies, ML/ANN, Python/R, problem solving
- **ประสบการณ์**: equity markets, backtesting, deploying strategies

### Case Studies
- **Maxime Fages**: นักวิศวกรจากฝรั่งเศส → M&A → CME → ก่อตั้ง Golden Compass (quant research firm)
- **คุณ V. Sankar Narayanan**: database manager 22 ปี → เริ่มเป็น Quant ตอนอายุ 40+ ปี — พิสูจน์ว่าอายุไม่ใช่อุปสรรค
- **Rohit Gupta**: Fixed income trader → MBA HK → เรียน EPAT → เปลี่ยนเป็น Quantimental trader

### ข้อคิดสำคัญ
- อายุและพื้นฐานไม่ใช่อุปสรรค — สำคัญที่ drive, initiative, competence
- ตลาดให้รางวัลกับคนที่ perform จริง

---

## 16. Disclaimer

- เนื้อหาใน ebook เป็นความคิดเห็นส่วนบุคคล ไม่ใช่คำแนะนำในการลงทุน
- ไม่รับประกันว่าข้อมูลถูกต้องครบถ้วน หรือจะนำไปสู่ผลกำไร
- สงวนลิขสิทธิ์ QuantInsti® ห้ามคัดลอกโดยไม่ได้รับอนุญาต

---

> **สรุปจาก**: Introduction to Algorithmic Trading (QuantInsti, 2018)
> **จำนวน chunks**: 11 files, หน้า 19–50
> **เนื้อหาหลัก**: ครอบคลุมตั้งแต่ data → charting → programming → broker → strategies → backtesting → risk → infrastructure → regulation → career
