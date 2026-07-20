# AI Algorithmic Trading Vol.1 — Foundations
> **Author:** Marcos López de Prado, Robert F. Connell (Adapted from multi-chapter textbook series)

---

## บทสรุปภาษาไทย

### Part I — Foundations

หนังสือเล่มนี้เป็นพื้นฐานทั้งหมดของ Algo Trading ที่ใช้ AI ครอบคลุม 25 chapters แต่ส่วนที่สรุปนี้คือ Part I (Chapters 1-4) ซึ่งเป็นแกนหลักของความเข้าใจตลาด ข้อมูล และวิศวกรรม Features

---

### Chapter 1 — Markets, Data, Logic of Algo Trading

#### 1. ภาพรวมและสามเสาหลัก

Algo Trading ตั้งอยู่บน 3 เสาหลักที่เชื่อมต่อกัน (หน้า 7-10): เข้าใจตลาด (markets as dynamic systems), จัดการข้อมูลอย่างมีวินัย (disciplined data treatment), และมี architecture ทาง computation ที่เชื่อมโยงข้อมูลเข้ากับการตัดสินใจ ไม่มีเสาไหนทำงานโดดเดี่ยวได้ แม้ model จะเทพแค่ไหน ถ้า architecture อ่อนแอ ก็ล้มเหลว

ตลาดถูกมองว่าเป็น "information engine" — ข่าว, order flow, constraints เป็น inputs; พฤติกรรมการเทรดและกลไก institutional เป็น engine; ราคา, volume, liquidity เป็น outputs ที่สังเกตได้ ทุก strategy ตั้งแต่ simple moving average ถึง neural network ล้วนพยายาม "estimate and act upon latent information states"

โครงสร้างของตลาดมี patterns ที่ exploitable ได้ — ไม่ใช่ fully predictable แต่มี regularity เช่น intraday seasonality, LOB shapes, responses to macro announcements จุดสำคัญคือแยกให้ออกว่า statistical pattern ที่เกิดจาก chance vs economic structure ที่ survive ค่า transaction cost ได้

#### 2. Market Microstructure (หน้า 10-13)

**Limit Order Book (LOB)** เป็นหัวใจของ price formation ทุก electronic market LOB คือ schedule ของ buying/selling interest ที่ update ตลอดเวลา มี three event types: limit orders (rest in book), market orders (immediate execution), cancellations/amendments ทุก event เปลี่ยน shape ของ book แม้แค่ limit order 1 lot ที่ best bid ก็ส่งผลต่อ liquidity measures ได้

LOB ทำงานภายใต้ price-time priority — ราคาดีกว่า execute ก่อน, ราคาเท่ากัน queue ตามลำดับเวลา สำหรับ algo trader สิ่งนี้สำคัญมาก因为它กำหนด queue position, expected waiting time, fill probability

LOB เป็น **partially observable system** — เห็น visible queue แต่ไม่เห็น reservation prices, private information, strategic intentions ของ traders คนอื่น ไม่ได้ observe hidden/iceberg orders ทั้งหมด และ off-book/dark pool trades ทำให้ LOB เป็นแค่ subset ของกิจกรรมเทรดทั้งหมด

**Frictions and Latency** ไม่ใช่ annoyances ที่ต้อง ignore แต่เป็น **design parameters** หลัก (หน้า 11-13): latency differentials ทำให้ HFT ได้เปรียบ, asymmetric information ทำให้ informed traders มี advantage, transaction costs (explicit + implicit) ทำให้ strategy ที่ดูกำไรใน simulation กลายเป็น unviable, market fragmentation ทำให้ "best price" กระจาย across venues, regulation กำหนด rules ของ transparency, priority, minimum tick size

#### 3. Market Data as Informational Object (หน้า 13-19)

Market data ไม่ใช่แค่ price series เดียว แต่เป็น **multilayered representation** ของ evolving beliefs under constraints มี 4 layers หลัก (หน้า 13-14):

- **Level-1 data**: best bid/ask, last traded price, volume — เป็น minimum input ที่ retail platforms ใช้ แต่ **insufficient สำหรับ professional strategies** เพราะ collapse โครงสร้างมากเกินไป ไม่เห็น depth หลัง quotes, ไม่เห็น order arrival rate, ไม่เห็น queue dynamics

- **Level-2 data**: order book หลาย levels — ช่วยสร้าง features เช่น order-book imbalance, book slope, local convexity/stacking สำคัญสำหรับ short-horizon forecasting และ market-making

- **Level-3 data**: event-level granularity — ทุก order submission/cancellation/modification/trade พร้อม timestamp, order ID, side, action, price, quantity เป็น richest microstructure representation แต่ต้องการ data engineering ที่ heavy

- **Derived features**: volatility measures, realised variance, imbalance indicators, microprice, trade direction (Lee-Ready), order-flow toxicity (ELO, VPIN) — ทำหน้าที่เป็น bridge จาก raw market events สู่ model-ready signals

**Time Bars vs Volume Bars vs Tick Bars** (หน้า 17-19): Time bars สะดวกเพราะ align กับ human time แต่assume ว่า information arrives สม่ำเสมอ ซึ่งไม่จริง Volume/tick bars ทำให้แต่ละ bar มี "equal-information" มากกว่า สำหรับ ML models ที่assume stationarity input, volume/tick bars มักให้ cleaner signals

#### 4. Information Flow and Price Formation (หน้า 18-21)

**Semi-Strong EMH** (หน้า 20-21): ราคา incorporates public information อย่างรวดเร็ว แต่ microstructure mechanics ทำให้ efficiency ไม่สมบูรณ์ — order-book imbalances, net aggressive order flow, execution patterns รอบ hidden orders ล้วนสร้าง predictability ได้

**Predictability and Autocorrelation** (หน้า 21): ที่ very short horizons มักมี negative autocorrelation (mean reversion จาก bid-ask bounce, inventory management) ที่ intermediate horizons มักมี positive autocorrelation (momentum จาก institutional flows, trend-following) Structural breaks ทำให้ autocorrelation estimates จาก long windows misleading

**Liquidity as State Variable** (หน้า 22-24): Liquidity ไม่ใช่ static attribute แต่เป็น dynamic state variable ที่ fluctuates ใช้ Kyle's lambda (price impact), Amihud's illiquidity ratio (|return|/volume), spread-based proxies วัด liquidity ทั้ง opportunity side (narrow spreads สนับสนุน high-turnover strategies) และ risk side (execution risk เพิ่มใน low-liquidity states)

#### 5. Algorithmic Trading System Architecture (หน้า 21-28)

System มี 4 core components (หน้า 22-23):

- **Signal Engine**: หัวใจ intellectual ของ system — แปลง features เป็น predictions/scores/decisions ตั้งแต่ linear regression, tree-based methods, deep learning, reinforcement learning ถึง generative models และ large reasoning models ใน agentic architectures 信号 engine ไม่ใช่ single monolithic model แต่เป็น "society of specialised agents" ที่ reasoning agent ข้างบน integrate

- **Execution Engine**: แปลง decisions เป็น orders — TWAP, VWAP, participation algorithms, optimal control Microstructure-aware execution ต้อง model price impact, fill probabilities, adverse selection AI enhances execution: RL learns adaptive policies, generative models simulate synthetic LOB trajectories, agentic architectures แยก venue-selection/timing/stealth agents

- **Risk Engine**: guardian ของ system — monitors exposures, enforces limits, provides feedback Modern architectures ใช้ ML models สำหรับ forecast volatility/liquidity, generative models สร้าง stress scenarios, agentic risk controllers negotiate de-risking actions

- **Governance and Logging**: ตอบ 3 คำถาม — What did you do? Why did you do it? Under what assumptions/constraints? ต้องมี trace logs, feature audits, retrospective scenario analysis, model versioning Generative/Reasoning models ช่วย generate human-readable summaries, act as governance agents, assist root-cause analysis

#### 6. Mathematical Foundations (หน้า 26-36)

**Probability spaces and filtrations** (หน้า 28-30): Ω (sample space), F (σ-algebra), P (probability measure) Filtration {Ft} แสดง information flow ที่ increasing — trading decisions at time t ต้อง depend only on Ft

**Stochastic Processes** (หน้า 30): Brownian motion (Wt), Geometric Brownian motion (GBM: dSt = μSdt + σSdW), Mean-reverting processes (OU: dXt = κ(θ-Xt)dt + σdWt), Jump processes (dSt = μSdt + σSdW + SdJ) These are priors/building blocks ไม่ใช่ literal truth

**Volatility Estimation** (หน้า 31): Realised variance (Σr²), high-frequency estimators (realised kernels, bipower variation), model-based (GARCH, stochastic volatility), ML-based (learn σ²_{t+1} ≈ fθ(xt))

**Linear Models** (หน้า 31-32): r̂_{t+1} = β0 + β'xt, MSE loss, closed-form solution β̂ = (X'X)⁻¹X'y, Regularization (ridge p=2, LASSO p=1) สำหรับ control complexity

**Machine Learning Foundations** (หน้า 32): Empirical risk minimization (ERM), loss functions (regression: MSE, classification: cross-entropy, ranking: pairwise), Regularization Ω(θ), SGD with mini-batches

**Neural Networks and Backpropagation** (หน้า 33): Feed-forward: h(ℓ) = σ(W(ℓ)h(ℓ-1) + b(ℓ)), Backpropagation ใช้ chain rule คำนวณ gradients efficiently

**Graph Neural Networks and Transformers** (หน้า 33-35): GNNs ใช้ message passing บน graphs ของ assets (correlations, sector links) Transformers ใช้ self-attention (Q, K, V) สำหรับ long-range dependencies, multi-modal integration

**Generative and Reasoning Models** (หน้า 35-36): VAEs (latent z, variational lower bound), Diffusion models (forward noising → reverse denoising), Autoregressive transformers (p(x1,...,xT) = Πp(xt|x<t)), Reasoning models (fθ: X×H → Y×H) ทำ chain-of-thought inference, Multi-agent systems on product state space Ht = (h(1)t,...,h(k)t)

#### 7. Designing Algo Trading Workflow (หน้า 34-41)

**Universe Selection** (หนา 36-37): Liquidity filters (volume, turnover, spread), data quality, economic coherence, operational feasibility Universe ควรเป็น time-varying set Ut

**Data Pipeline Setup** (หน้า 37-39): Raw data → Cleaning/normalization → Feature engineering → Labelling ต้อง handle missing values, corporate actions, outliers, timestamp alignment Normalization: returns (ไม่ใช่ prices), log volumes, z-scored features Labelling: regression (future returns), classification (direction), structured outputs (volatility surfaces)

**Model Development** (หน้า 39-40): Training (architecture selection, hyperparameters, regularization), Validation (walk-forward, rolling/expanding windows, nested CV), Backtesting (transaction costs, slippage, capital constraints, risk limits) Backtest ต้อง reproduce live workflow ทั้งหมด

**Live Deployment** (หน้า 40-42): Paper trading → Productionisation (small capital, robust infrastructure) → Real-time monitoring (P&L dashboards, alert breaches, performance attribution) Generative AI ช่วย summarize system status, analyze logs, propose remedial actions

---

### Chapter 2 — Python Quant Toolbox: Market Data as Structured Object

#### 1. Introduction — Market Data คือ Interface ระหว่าง Reality กับ Models (หน้า 43-49)

Market data ไม่ใช่ passive record แต่เป็น **active informational environment** ที่เกิดจาก heterogeneous agents, institutional rules, venue microstructure, imperfect measurement (หน้า 112-113) วิธี define "price" แต่ละแบบ = เลือก measurement instrument ต่างกัน

Data เป็น interface ระหว่าง reality กับ models — model ไม่ interact กับ market โดยตรง แต่ interact กับ data streams ความล้มเหลวของ strategies ส่วนใหญ่ไม่ได้เกิดจาก model sophistication ไม่พอ แต่เกิดจาก **mismatches ที่ interface layer** — inputs เป็น poor representation ของ economic quantities

Precise definitions เป็น **structural regularization** — เมื่อ specifies ชัดเจนว่า log returns จาก split-adjusted, dividend-inclusive prices sampled at daily close ใน local time จะ constrain model-building exercise ให้ focus กับ economic object ที่ชัดเจน

#### 2. Dimensions of Market Data (หน้า 49-56)

**Cross-Section**: Assets, Venues, Instruments — trading universe ไม่ใช่ single instrument แต่เป็น collection ที่มี identifiers, currencies, listing venues Venue-specific information สำคัญสำหรับ high-frequency execution

**Time**: Clock time (calendar seconds/minutes), Event time (k-th trade/update), Business/Volume time (rescale by trading intensity) แต่ละ clock นิยาม "distance" ระหว่าง observations ต่างกัน

**Attributes**: Prices (last, bid, ask, mid, indicative), Volumes (trade sizes, cumulative, depth), Quotes/LOB states (best bid/ask, full depth), Derived states (realised volatility, spread measures, regime labels)

**Market Data Tensor** (หน้า 51): Xt = {P(ℓ)t, V(ℓ)t, Q(ℓ)t, L(ℓ)t, ...} ที่ ℓ indexes resolution layers (tick, second, minute, hour, day) ขยายเป็น Xi,t สำหรับ cross-section i และ tensor indices (i, t, ℓ, a) ที่ a คือ attributes

**Resolution Layers and Aggregation** (หน้า 52-54): OHLC bars, VWAP, TWAP เป็น operators ที่ map fine-grained layer สู่ coarser layer Event-based sampling (volume bars, tick bars) ปรับตัวตาม trading activity แต่ละ aggregation มี information loss — OHLC จำ path ไม่ได้, VWAP ซ่อน distribution

**Multivariate and Panel Representations** (หน้า 54-56): Single asset time series vs multi-asset panels, Synchronous vs asynchronous observations (ต้อง decide interpolation method), Implications for covariance/dependence modelling

#### 3. Data Sources, Conventions, and Structural Biases (หน้า 55-61)

**Primary Sources**: Exchange data (order book, trades, reference), Broker/ECN feeds (pass-through + internalisation + synthetic prices), Index providers (levels, constituents, corporate action treatments)

**Secondary Sources**: Data vendors (consolidated quotes/trades, derived fields) — ผลลัพธ์อาจ vendor-dependent, Back-adjusted databases (split/dividend-adjusted, back-adjusted futures) — สะดวกแต่ dangerous (erase roll yields, change interpretation), Survivorship bias (excluding delisted firms inflates backtests)

**Alternative Datasets**: News/social media/sentiment (structured records, sentiment scores — biases จาก coverage patterns, language differences, timestamp meaning), Macro/fundamental data (release times, revision history, look-ahead bias ถ้าใช้ revised data), Proprietary order/client data (selection bias, behavioral bias, survivorship/regime bias)

**Calendar Effects** (หน้า 60-61): Trading calendars (holidays, half-days, sessions), Time zones + daylight saving, Currency conventions (rollover, settlement T+1/T+2)

#### 4. Prices: Definitions and Transformations (หน้า 61-67)

**Canonical Price Definitions**: Last-traded price (stale in illiquid, doesn't reveal bid/ask), Bid/Ask/Mid-quote (reduces bounce, symmetric valuation point), Indicative/Reference prices (fixings, auctions, official closes)

**Price Representation by Market**: Equities/ETFs (tick size, corporate actions, ETF structure), Bonds/Swaps (clean vs dirty price, yield quotes), FX (spot, forwards, crosses, interest-rate differentials), Futures/Options (underlying vs derivative price, implied vol)

**Corporate Actions**: Splits/reverse splits (adjusted series ไม่ correspond กับ actual transaction prices), Cash dividends (price-only vs total-return series), Mergers/spin-offs/delistings

**Transformations for Modelling**: Levels vs logs (logs ดีกว่า: positivity, relative changes, scale invariance), Normalisation by benchmarks (P̃t = Pt/It), Scaling/standardisation for ML (z-score, min-max, rank/quantile transforms)

#### 5. Returns and Their Statistical Structure (หน้า 67-73)

**Return Definitions**: Simple returns Rt = Pt/Pt-1 - 1 (additive in wealth), Log returns rt = log(Pt/Pt-1) (additive over time), Excess returns (Rt - Rft)

**Properties of Log Returns** (หน้า 69-71): Additivity over time (Σlog(Pt+k/Pt+k-1) = log(Pt+n/Pt)), Approximation to continuously compounded returns (GBM solution), Implications for volatility/portfolio aggregation (simple returns aggregate cross-sectionally, log returns aggregate temporally)

**Multi-Horizon Returns** (หน้า 70-72): Overlapping vs non-overlapping returns (overlapping มี serial correlation แม้ 1-period returns เป็น independent), Annualisation conventions (σ_annual ≈ √N × σ_periodic), Serial correlation complications

**Return Decomposition** (หน้า 72-73): Directional vs relative/spread returns, Carry/roll-down/term-structure components (carry = return จาก yield/coupon/interest-rate differentials, roll-down = price change จาก moving along curve), Event-driven returns (discrete jumps จาก announcements — continuous + jump components)

#### 6. Volume, Liquidity, and Order Flow (หน้า 73-80)

**Volume Measures** (หน้า 73-74): Trade count vs traded volume (จับ different aspects — trade intensity vs risk transferred), On-exchange vs off-exchange volume (dark pools, internalisation — visible volume เป็น lower bound ของ true liquidity)

**Liquidity Metrics** (หน้า 75-77): Bid-ask spread (most immediate measure — half-spread ≈ lower bound ของ round-trip cost), Depth at best and in book (top-of-book depth, cumulative depth, order-book slope), Amihud illiquidity (|return|/volume — high value = illiquid)

**Order Flow and Direction** (หน้า 77-78): OFI = Bt - St (net buying/selling pressure), Initiator classification (Lee-Ready, tick rule, hybrid), Short-term predictability (order flow persistence จาก order splitting/herding — key for market-making)

**Volume-Volatility Relationship** (หน้า 78-80): Mixture-of-distributions hypothesis (volume = proxy สำหรับ information arrival rate), Volume as information arrival (conditional volatility, adaptive risk limits, clock choice), Implications for intraday strategy design

#### 7. Market Microstructure Noise and Efficient Prices (หน้า 80-86)

**Observed vs Efficient Price**: P_obs = P* + ε (efficient price + microstructure noise) At high frequency noise can dominate; at low frequency noise may be small relative to genuine moves

**Sources of Noise** (หน้า 80-81): Bid-ask bounce (alternating buy/sell → sawtooth returns), Tick size constraints (prices quantised to discrete grid), Order processing/latency, Inventory/strategic behaviour, Reporting errors

**Consequences for Estimation** (หน้า 81-82): Inflated volatility estimates, spurious negative autocorrelation, mis-specified models

**Bid-Ask Bounce** (หน้า 82-84): สร้าง strong negative first-order autocorrelation ใน trade-to-trade returns แม้ efficient price เป็น random walk Filtering techniques: mid-quote returns, lower-frequency sampling, sparse sampling, state-space/Kalman filtering

**Realised Volatility and Noise-Adjusted Estimators** (หน้า 83-86): RV_obs = Σ(r* + η)² → upward biased ที่ high frequency Subsampling (use every k-th observation), Pre-averaging (smooth before differencing), Two-scale/multi-scale estimators, Kernel-based estimators

#### 8. Market Microstructure Layers (หน้า 86-91)

**Institutional and Infrastructural Layer** (หน้า 86-88): Trading venues (exchanges, dark pools, ECNs), Order types (market, limit, stop, iceberg, hidden, pegged), Matching engines (price-time priority, pro-rata), Clearing/settlement/custody (CCPs, T+1/T+2), Regulatory rules (tick size, circuit breakers, transparency)

**Information Layer** (หน้า 88-89): Quotes/trades/reference prices (granularity, timing, sequencing), News/announcements/signals (structured headlines, sentiment, event dummies), Latency/transmission (physical, technical, logical delays)

**Statistical Layer** (หน้า 89-91): Observed time series เป็น projections ของ microstructure dynamics, Sampling schemes (calendar/event/volume time), Implications for econometric modelling (model misspecification, parameter instability, non-standard errors)

#### 9. Statistical Properties and Stylised Facts (หน้า 91-98)

**Fat Tails and Kurtosis** (หน้า 92-93): Extreme moves occur more frequently than Gaussian predicts Tail index estimation (Hill estimator — α < 2 หมายถึง infinite variance), Non-Gaussianity consequences (variance เป็น incomplete risk measure)

**Autocorrelation of Returns vs Volatility** (หน้า 93-94): Returns: weak/negligible autocorrelation (hard to exploit), |returns| or r²: strong persistent autocorrelation (volatility is predictable!) — GARCH(1,1): σ²t = ω + αr²t-1 + βσ²t-1

**Leverage Effect** (หน้า 94): Negative returns → higher subsequent volatility Corr(rt, σ²t+1) < 0 EGARCH/GJR-GARCH capture this

**Cross-Sectional Dependence** (หน้า 95-97): Assets in same sector/region → higher correlations, Correlations spike during stress, Factor structures explain large portion of cross-sectional variation Dynamic correlations (DCC-GARCH, regime-switching)

**Regime Dependence** (หน้า 97-98): High/low volatility regimes persist for months/years, Liquidity crises (widening spreads, thinning depth, air pockets), HMMs สำหรับ formalise regime dependence, Structural breaks ต้อง re-estimate/re-design models

#### 10. Data Quality, Filtering, and Preprocessing (หน้า 98-102)

**Error Types**: Outliers/spikes (fat-finger, vendor glitches), Stale prices/gaps, Duplicated/inconsistent records

**Cleaning Rules**: Range checks + circuit-breaker logic, Cross-sectional sanity checks (futures vs spot, ETF vs NAV), Vendor vs in-house cleaning pipelines

**Corporate Action/Calendar Adjustments**: Price/volume adjustments, Index rebalancing/inclusion effects, Rolling/stitching futures (back-adjusted, Panama method, perpetual futures)

**Synchronisation**: Previous-tick, next-tick, refresh time sampling — ทุก method trade off bias vs efficiency

**Missing Data**: Market closure vs data issues, Forward-fill/interpolation/model-based imputation, Impact on covariance/signal estimation

#### 11. Implications for Feature Engineering and Modelling (หน้า 102-107)

**From Raw Data to Features** (หน้า 102-104): Return-based features (momentum, volatility, skewness), Volume/liquidity features (turnover, cost proxies, OFI), Cross-asset/cross-market features (relative value, factor exposures, global linkages)

**Sampling Frequency Choice** (หน้า 103-105): High-frequency vs daily — signal-to-noise ratio tradeoff, Microstructure noise vs information content, Align sampling with strategy horizon

**Robustness and Stability** (หน้า 104-106): Sensitivity to cleaning/adjustments, Data revisions/look-ahead bias, Backtesting artefacts from data handling

**AI/ML Pipeline Interaction** (หน้า 105-107): Feature scaling/normalisation/stationarity (scalers fit only on training data), Train-test splits and temporal leakage (random splits invalid for time series), Data documentation and governance

---

### Chapter 3 — Returns, Risk, Portfolio Math / Feature Engineering

#### 1. Introduction — Data as Active Informational Environment (หน้า 111-115)

Market data ไม่ใช่ neutral historical record แต่เป็น active informational environment ที่เกิดจาก heterogeneous agents, institutional rules, venue microstructure (หน้า 112) Feature engineering ไม่ใช่ decorative step แต่เป็น **epistemic decision** ว่า data หมายความว่าอะไร, miss อะไร, distort อะไร

Raw observations มีค่าเฉพาะหลัง stating explicitly ว่า data type คืออะไรและ process ไหน generate มัน — Price series มีหลายรูปแบบ (last-traded, mid-quote, bid/ask, VWAP), Volume/liquidity measures ครอบคลุม trade volume, dollar volume, turnover, spread estimates, impact proxies, Order book data เป็น richest แต่ most fragile (hidden orders, iceberg, venue fragmentation)

#### 2. Taxonomy of Market Data (หน้า 104-107)

**Price Series** (หน้า 115): Family of signals — last trade (irregular, subject to bounce), quote (bid/ask — may be stale), mid-price (reduces bounce), OHLC (auction/cross effects) เลือก price definition = เลือก measurement instrument

**Volume and Liquidity Metrics** (หน้า 115-116): Trade volume, dollar volume, turnover, number of trades, quoted/effective/realized spread, depth, Amihud, Kyle-type impact Many measures are **endogenous** (spreads widen because market makers demand compensation)

**High-Frequency and Tick Data** (หน้า 116): Granularity = value, but dominated by microstructure noise Distinguish trade prints from quote updates; define event-time vs clock-time

**Order Book and Microstructure Signals** (หน้า 116-117): Depth, cumulative depth, slope, imbalance, queue position, cancellation rates, OFI — แต่ visible book is **incomplete** (hidden liquidity, dark pools, strategic cancellations)

**Fundamental and Accounting Indicators** (หน้า 117): Low frequency, delayed release, revision — look-ahead bias, survivorship bias, backfill bias

**Alternative Data** (หน้า 117-118): News, social media, web traffic, satellite — แต่ exposed to selection bias, non-stationarity, platform changes

**Regime-Dependent Data Behaviors** (หน้า 118): Each category behaves differently under different regimes — taxonomy ต้อง include conditionality

#### 3. Statistical Properties of Market Data (หน้า 107-111)

**Stationarity vs Non-Stationarity** (หน้า 118): Prices non-stationary (drift, structural breaks); returns closer to stationary but not truly (distribution changes with volatility regimes) Practical: local estimation (rolling windows, adaptive filters), time-aware validation

**Volatility Clustering and Heteroskedasticity** (หน้า 118-119): Conditional variance is time-varying — variance is dynamic state variable Motivates: realised volatility measures, conditional scaling (rt/σ̂t), volatility modelling as first-order component

**Heavy Tails and Non-Gaussian** (หน้า 119-120): Extreme events more frequent than Gaussian, Standard deviation incomplete risk measure, Robust statistics (median-based, trimmed means, Huber losses), Winsorization must be justified by data-quality logic

**Long Memory and Autocorrelation** (หน้า 120): Raw returns: weak autocorrelation; |returns|/r²: strong persistence Predictability is in magnitude of risk, not direction of returns, Effective sample size < dataset length

**Market Microstructure Noise** (หน้า 120-121): At intraday/HF, noise becomes dominant — bid-ask bounce, tick discreteness, quote flickering Even lower-frequency strategies affected (open/close shaped by auctions, closing crosses)

**Seasonality and Calendar Effects** (หน้า 121-122): U-shaped intraday volume, day-of-week, month-end, quarter-end, option expiration, macro release schedules Seasonality is institutional → can drift as structure evolves

#### 4. Transformations and Normalizations (หน้า 111-114)

**Log Returns and Partial Differencing** (หน้า 122-123): rt = log(Pt) - log(Pt-1) — additive over time, reduces scale dependence Must respect sampling conventions (close-to-close vs open-to-close vs mid-quote)

**Rolling Windows** (หน้า 123-124): Local estimation under non-stationarity — rolling mean (drift), rolling variance (risk), rolling higher moments Trade-off: short windows adapt but noisy, long windows stable but lag

**Detrending and De-seasonalization** (หน้า 123-124): Remove persistent components (returns already detrend prices), De-seasonalize by conditional baselines (compare volume to its typical level for that time segment)

**Volatility Scaling** (หน้า 124): r̃t = rt / σ̂t — stabilizes risk content, enables comparability across regimes/assets Introduces estimation error — robust estimators preferred

**Outlier Treatment and Winsorization** (หน้า 124-125): Classify first: genuine extreme events vs data errors, Winsorize sparingly with documented rationale, Alternatives: robust scaling (median, MAD), Huber losses, rank/quantile transforms

#### 5. Feature Engineering Framework (หน้า 114-117)

**Economic Interpretability** (หน้า 125-126): Feature ต้องมี plausible mechanism — "who would trade on this, why would it move prices, why would it persist?" Supports governance: ถ้า model breaks, interpretable features ให้ diagnostic handles

**Statistical Reliability** (หน้า 126-127): Measurable with reasonable SNR, stable distribution, some persistence/effective sample size Feature ต้อง stable ภายใต้ modest changes ของ window length, frequency, subperiod

**Robustness to Regime Shifts** (หน้า 126-128): Regime-conditioned features, robust aggregation across horizons, defensive normalization, explicit monitoring of feature distributions Target itself can change (transaction costs, crowding)

**Real-Time Feasibility** (หน้า 127-129): Feature ต้อง computable ด้วย information set It เท่านั้น — ห้าม look-ahead bias Compute constraints ด้วย (cross-sectional optimization อาจ infeasible ที่ intraday)

**Feature Classes** (หน้า 129-130): Technical (price/volume functions — universal but crowded), Statistical (distributional properties — model risk but estimation error), Structural (microstructure — implementability but data-dependent), Alternative (external info — differentiation but governance burden) Robust systems combine all classes

#### 6. Technical Indicators as Features (หน้า 117-124)

**Moving Averages and Trend Filters** (หน้า 129-130): SMA, deviation features (Pt - SMA)/SMA, crossover signals, EWMA สำหรับ smoother response Economic interpretation: trends persist จาก slow-moving capital, underreaction, institutional mechanics Works แต่ fail ใน choppy regimes → pair with volatility scaling

**Momentum and Reversal** (หน้า 130-131): Cumulative returns mt(W) = Σrt-i, skip-period momentum (avoid microstructure contamination), risk-adjusted momentum (cumulative return / realised vol) Short-horizon reversal (microstructure), medium-horizon momentum (information diffusion)

**Volatility Indicators** (หน้า 131-132): Rolling std, EWMA vol, range-based estimators (Parkinson: log(H/L)²), vol-of-vol, jump proxies, downside semivariance, volatility regime features (percentile rank) Critical for normalization: z-scores put features in comparable units

**Market Breadth and Participation** (หน้า 132-133): Fraction advancing, advance-decline line, cross-sectional dispersion — วัดว่า move นั้นsupported by broad participation หรือ concentrated

**Volume-Weighted Signals** (หน้า 133-134): VWAP, OBV (on-balance volume), volume-weighted momentum, Amihud-type impact proxies Normalisation สำคัญมาก (seasonal volume patterns)

#### 7. Microstructure-Informed Features (หน้า 124-138)

**Bid-Ask Dynamics** (หน้า 134-135): Mid, spread, relative spread, quote revision frequency, mid-price change relative to spread, bid-ask bounce contamination, effective spread, realized spread

**Order Flow Imbalance** (หน้า 135-137): Signed volume OFI (Lee-Ready direction inference), book-based OFI (∆Q_bid - ∆Q_ask) Normalisation สำคัญ (bounded between -1 and +1) Economic: temporary price pressure, informed trading, short-lived predictive power

**Execution Cost Proxies** (หน้า 137-138): Spread-based, Amihud illiquidity, participation rate-based — สำคัญเพราะ predictive models fail in practice จาก ignoring cost of turning forecasts into trades

**Spread, Impact, and Latency** (หน้า 137-138): Kyle-like κ (∆Mid ≈ κ·OFI), edge-to-cost ratios, microstructure instability measures Features ทำหน้าที่ contextual: บอกว่า market ถูก/แพงที่จะเทรด, price movements มีข้อมูลหรือเป็นแค่ noise

#### 8. Machine-Learning-Ready Features (หน้า 138-142)

**Feature Scaling and Encoding** (หน้า 139-140): Z-scoring (rolling window), robust scaling (median, MAD), rank/percentile encoding (cross-sectional), cyclic encoding สำหรับ calendar variables (sine/cosine)

**Dimensionality Reduction** (หน้า 140-141): PCA (linear, interpretable — extract latent factors), Autoencoders (nonlinear compression — denoising version), Cross-sectional vs temporal reduction

**Nonlinear Interaction Features** (หน้า 141-142): Product terms (momentum × vol, trend × liquidity), gating via indicator functions (regime logic), log/square/saturation transforms Must be guided by economic hypotheses to avoid feature explosion

**Regime-Conditioned Features** (หน้า 142-143): Hard regime labels (volatility regime binning), soft probabilities (HMM), embedded in normalization (vol scaling, de-seasonalization, rank encoding) Pair with monitoring: track feature drift, regime transition rates

#### 9. Data Quality and Governance (หน้า 133-145)

**Missing Data Mechanisms** (หน้า 143-144): Not random — missingness itself can be signal (liquidity withdrawal) Preserve missingness flags as features; don't impute by default Monitor in real time with alerts

**Survivorship and Look-Ahead Bias** (หน้า 144-145): Survivorship: include delisted constituents; define universe as-of each date, Look-ahead: use point-in-time datasets, store timestamps/release times, test causality with unit tests

**Data Snooping and Overfitting** (หน้า 145): Walk-forward validation preferred; strict separation of dev/evaluation; pre-registration of experiments; track feature inventories and model lineage

**Auditability, Lineage, and Reproducibility** (หน้า 145-146): Version everything: raw snapshots, cleaned datasets, feature configs, code commits, random seeds, library versions Record all transformation parameters; build reproducible pipeline from raw inputs

---

### Chapter 4 — Time Series Anatomy for Trading

#### 1. Financial Data as Time Series Objects (หน้า 149-156)

**Discrete-Time Representation** (หน้า 150-151): X: {t0,...,tT} → ℝd, dimension d depends on data layer (close: d=1, OHLCV: d=5) Time index is part of the object — ไม่ใช่ bag of observations Index set itself is design choice (daily vs minute vs event)

**Prices, Returns, and Observability** (หน้า 151-153): Pk = Vk + εk (latent value + microstructure noise), Simple returns Rt = Pt/Pt-1 - 1, Log returns rt = log(Pt/Pt-1), Returns inherit all data issues from prices Observability: object is observable at tk if knowable without future information

**Sampling Frequency** (หน้า 153-154): Daily (clean but hides intraday dynamics), Intraday time bars (richer structure + more noise), Event time (irregular spacing) One step = interval between decision opportunities — matching horizon สำคัญมาก

**Time Ordering, Causality, and Information Sets** (หน้า 154-156): σ-algebra Fk = information set at time k, Causality contract: Zk = g(Fk) — any derived quantity at index k must use only data ≤k Common violations: future-adjusted data, finalized bars treated as known mid-bar, shuffled time series, normalization on all data

#### 2. Trends, Seasonality, and Structural Change (หน้า 156-160)

**Trend as Low-Frequency Drift** (หน้า 156-158): Pt = mt + εt, Rt = µt + ut Trend is scale-dependent (clear uptrend at monthly, noise at minute), Signal-to-noise tradeoff: short windows adapt but noisy, long windows stable but lag Distinguish deterministic trend (at + b) vs stochastic trend (unit-root-like)

**Seasonality** (หน้า 158-159): Institutional origins (human schedules, market design, reporting calendars) Daily scale: day-of-week, month-end effects; Intraday: U-shaped volume patterns Often small relative to noise — requires careful aggregation and baselines

**Calendar Effects and Microstructure Rhythms** (หน้า 158-160): Strong intraday cycles in volume/spread/depth/volatility → normalisation must be conditional on phase, Month-end flows, quarter-end effects, option expiration, Opening/closing auctions shape returns differently

**Structural Breaks and Regime Shifts** (หน้า 159-161): Xt | (St=s) ∼ Ds — observations depend on unobserved regime indicator Operational approach: rolling diagnostics to detect instability (rolling vol changes → vol regime shift, rolling correlations jump → dependence regime shift) Design implication: robust systems are regime-aware even without explicit regime models

#### 3. Stationarity: What Matters for Trading (หน้า 160+)

**Why Stationarity is Useful (Even When False)** (หน้า 160-161): Justifies learning from past → rolling statistics, volatility estimation, correlation estimation, signal normalization all presuppose some stationarity approximation

**Weak Stationarity**: [Xt] constant and (Xt, Xt-k) depends only on lag k — แต่ strict stationarity rarely plausible over long horizons in adaptive markets

Stationarity is not binary — it's a question of **time scale and object choice**: prices non-stationary in level; returns closer to stationary in mean but not variance; spreads/volumes have intraday seasonality Practical goal: "identify the least wrong stationary approximation that supports a causal, governable pipeline"

---

*สรุปครบทุก sections ของ Part I (Chapters 1-4) ของหนังสือ AI Algorithmic Trading Vol.1 — Foundations*

---
