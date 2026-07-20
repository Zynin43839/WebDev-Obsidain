# สรุป Advances in Financial Machine Learning (AFML)
## ผู้แต่ง: Marcos López de Prado (2018)

> สรุปจาก SSRN paper/book — 22 บท, 5 ส่วนหลัก
> สร้าง: 2026-07-20

---

## ภาพรวมของหนังสือ

**Advances in Financial Machine Learning** เป็นหนังสือที่เขียนโดย **Marcos López de Prado** ซึ่งเป็นผู้จัดการกองทุนหลายพันล้านดอลลาร์สำหรับนักลงทุนสถาบัน โดยใช้ algorithm ML หนังสือเล่มนี้เป็นเล่มแรกที่นำเสนอ **Financial Machine Learning** (FML) 作为一种 subject ที่แยกออกจาก Machine Learning (ML) ทั่วไปอย่างชัดเจน เนื่องจาก finance มีลักษณะเฉพาะที่ท้าทายมากกว่า domain อื่นๆ เช่น computer vision หรือ NLP

**จุดประสงค์หลัก**: เชื่อมช่องว่างระหว่าง academia (ทฤษฎีสวยแต่ใช้ไม่ได้จริง) กับ industry (ใช้งานจริงแต่ไม่มีทฤษฎีรองรับ) — โดยให้ความรู้ที่มาจากการปฏิบัติจริง แล้ว formalize อย่างมีระบบ

**กลุ่มเป้าหมาย**: นักลงทุนมืออาชีพที่มีพื้นฐาน ML, Data Scientist ที่มาจาก domain อื่นแล้วอยากเข้า finance

---

## บทที่ 1: Financial Machine Learning as a Distinct Subject

### 1.1 ทำไม Financial ML ถึงล้มเหลวบ่อย
- **Sisyphus Paradigm**: การจ้าง 50 PhDs แล้วให้แต่ละคนคิด strategy เอง — ผลลัพธ์คือได้ 50 false positives (overfit) หรือ factor investing ที่มี Sharpe ต่ำ (overcrowded) — ไม่คุ้มค่าจ้าง
- **Meta-Strategy Paradigm** ✅: ใช้โรงงานวิจัยแบบ systematic — แต่ละคน specialize ใน station เดียว (data curation, feature analysis, backtesting, deployment) — เหมือน Berkeley Lab ที่ค้นพบธาตุในตารางธาตุ 16 ตัว ไม่ใช่เพราะคนเก่งคนเดียว แต่เป็น systematic team work

### 1.2 โครงสร้างหนังสือตาม Production Chain
หนังสือแบ่งเป็น **5 stations** ตามสายการผลิต:

| Station | หน้าที่ | บทที่เกี่ยวข้อง |
|---------|---------|----------------|
| **Data Curators** | รวบรวม ทำความสะอาด จัดเก็บข้อมูล — ต้องเข้าใจ FIX protocol, market microstructure | บท 2-5, 17-19 |
| **Feature Analysts** | แปลง raw data เป็น informative signals ที่มี predictive power | บท 2-9, 17-19 |
| **Strategists** | แปลง features เป็น investment algorithms — ต้องเข้าใจ economic mechanism ที่ทำให้คนอื่นเสียเงินให้เรา | บท 10, 16 |
| **Backtesters** | ประเมิน profitability โดยพิจารณา **backtest overfitting probability** — จำนวนครั้งที่ทดลอง strategy | บท 11-16 |
| **Deployment Team** | รวม strategy เข้า production — vectorization, multiprocessing, GPU, distributed computing | บท 20-22 |

### 1.3 Portfolio Oversight Lifecycle
1. **Embargo**: ทดสอบบนข้อมูลหลัง backtest end date
2. **Paper Trading**: รันบน live feed จริง (real-time)
3. **Graduation**: จัดการ position จริง
4. **Re-allocation**: ปรับ allocation ตาม performance (concave function — เริ่มน้อย เพิ่มเมื่อดี ลดเมื่อเสื่อม)
5. **Decommission**: ยุติ strategy เมื่อ performance ต่ำกว่าคาดเป็นเวลานาน

### 1.4 Common Pitfalls in Financial ML (Table 1.2)

| # | ปัญหา | วิธีแก้ | บท |
|---|-------|---------|---|
| 1 | Sisyphus paradigm | Meta-strategy paradigm | 1 |
| 2 | Research through backtesting | Feature importance analysis | 8 |
| 3 | Chronological sampling | Volume clock | 2 |
| 4 | Integer differentiation | Fractional differentiation | 5 |
| 5 | Fixed-time horizon labeling | Triple-barrier method | 3 |
| 6 | Learning side+size simultaneously | Meta-labeling | 3 |
| 7 | Weighting non-IID samples | Uniqueness weighting; sequential bootstrapping | 4 |
| 8 | Cross-validation leakage | Purging + embargoing | 7, 9 |
| 9 | Walk-forward backtesting | Combinatorial purged cross-validation | 11, 12 |
| 10 | Backtest overfitting | Synthetic data backtesting; deflated Sharpe ratio | 10-16 |

### 1.5 FAQs สำคัญ
- **ML vs Econometrics**: Econometrics ใช้ linear regression (18th-century technology) — ไม่ "learn" — เป็นสาเหตุหลักที่ finance ไม่มี progress 70 ปี (เปรียบเทียบ: นักดาราศาสตร์ยุคกลางไม่ยอมรับ elliptical orbits เพราะ "unholy" — ถ้ายอมรับ non-linear functions ตั้งแต่แรก จะไม่ต้องรอ Kepler)
- **Backtest overfitting**: ปัญหาที่สำคัญที่สุดใน mathematical finance — เทียบเท่า "P vs NP" ของ CS
- **Alpha ที่เหลือ**: ทุกวันนี้ alpha เหลือแต่แบบ "microscopic" — ต้องใช้ ML tools หนักๆ ขุดออกมา (เปรียบเทียบกับ gold — สมัย Spanish conquistadors ขุดทองก้อนใหญ่ได้ 1.54 ตัน/ปี แต่ปัจจุบันขุด micro particles ได้ 2,500 ตัน/ปี)

---

## PART 1: DATA ANALYSIS (บท 2-5)

---

### บทที่ 2: Financial Data Structures

**ปัญหาหลัก**: ข้อมูล finance ไม่ได้มี structure ที่เป็นธรรมชาติสำหรับ ML — ต้อง transform ก่อน

#### 2.1 ประเภทข้อมูล finance

| ประเภท | ตัวอย่าง |
|--------|----------|
| **Fundamental Data** | งบการเงิน, macroeconomic indicators |
| **Market Data** | OHLCV, order book, tick data |
| **Analytics** | analyst ratings, sentiment scores |
| **Alternative Data** | satellite imagery, social media, credit card transactions |

#### 2.2 Bars (Table Rows) — วิธีแบ่งข้อมูล

**Standard Bars:**
- **Time Bars**: แบ่งตามเวลา (1 นาที, 5 นาที, 1 ชั่วโมง) — **แย่ที่สุดสำหรับ ML** เพราะไม่ reflect information arrival
- **Tick Bars**: แบ่งตามจำนวน ticks — สะท้อน market activity ได้ดีกว่า
- **Volume Bars**: แบ่งตาม cumulative volume — สะท้อน information ได้ดี
- **Dollar Bars**: แบ่งตาม cumulative dollar value — **ดีที่สุด** เพราะสะท้อน information flow ได้ดีที่สุด

**Information-Driven Bars:**
- **Tick/Volume Imbalance Bars**: แบ่งเมื่อ signed volume ถึง threshold — สะท้อน informed trading
- **Tick/Volume/Dollar Runs Bars**: แบ่งเมื่อมี buying/selling run ถึงระดับหนึ่ง

#### 2.3 Multi-Product Series
- **ETF Trick**: ใช้ ETF เป็น proxy สำหรับ multi-asset portfolio — แก้ปัญหา rolling, corporate actions
- **PCA Weights**: ใช้ Principal Components Analysis สร้าง weights สำหรับ hedging
- **Single Future Roll**: วิธี roll futures contracts อย่างราบรื่น

#### 2.4 Feature Sampling
- **Event-Based Sampling**: ใช้ CUSUM filter หรือ event triggers แทน fixed sampling
- **Downsampling**: ลดขนาดข้อมูลสำหรับ computational efficiency

> **ข้อสำคัญสำหรับเรา**: Dollar bars + event-based sampling = data structure ที่ดีที่สุดสำหรับ ML trading

---

### บทที่ 3: Labeling

**ปัญหาหลัก**: วิธี label ข้อมูล finance แบบเดิม (fixed-time horizon) มีปัญหามาก

#### 3.1 Fixed-Time Horizon Method — ปัญหา
- กำหนด threshold แล้ว label UP/DOWN หลัง N bars — ไม่สนใจว่า price เคลื่อนไหวอย่างไรระหว่างทาง
- **ปัญหา**: ไม่ know side (BUY/SELL ไม่รู้), ไม่ know size, ไม่ account สำหรับ volatility

#### 3.2 Triple-Barrier Method ✅
- ใช้ **3 barriers**: profit-taking (TP), stop-loss (SL), time limit
- Label = ichever barrier ที่โดนก่อน — ให้ข้อมูลทั้ง **side** (BUY/SELL) และ **size** (magnitude)
- ใช้ daily volatility สำหรับกำหนด TP/SL ให้ dynamic

#### 3.3 Meta-Labeling ✅
- **แยก side selection จาก bet sizing**
- Primary model: ตัดสิน side (BUY/SELL)
- Secondary model (meta-label): ตัดสิน size (bet หรือ pass)
- **ผล**: เพิ่ม precision โดยไม่ต้องเปลี่ยน primary model — เหมาะกับ discretionary PM ที่อยากเพิ่ม ML layer

#### 3.4 Quantamental Way
- ใช้ meta-labeling เพื่อรวม human judgment (fundamental variables) เข้ากับ ML forecasts
- Human บอก side, ML บอก size

> **ข้อสำคัญสำหรับเรา**: Triple-barrier method + meta-labeling = framework ที่เราใช้ (BOCPD + regime detection ทำหน้าที่เป็น primary model)

---

### บทที่ 4: Sample Weights

**ปัญหาหลัก**: ข้อมูล finance มี overlap (labels ทับซ้อนกัน) — ทำให้ ML models เรียนรู้ผิด

#### 4.1 Overlapping Outcomes
- Triple-barrier labels สามารถ overlap กัน — trade หนึ่งอาจเป็น part ของอีกหลาย trades
- ถ้า treat ว่า IID (independent) → model overfit

#### 4.2 Average Uniqueness of a Label
- วัดว่า label หนึ่งถูก "share" กับ labels อื่นมากแค่ไหน
- **Uniqueness** = 1 / number of concurrent labels

#### 4.3 Sequential Bootstrap
- วิธี bootstrapping ที่ account สำหรับ uniqueness
- เลือก samples ที่มี overlap น้อยกว่า → ได้ weights ที่ถูกต้อง
- ใช้ indicator matrix ติดตาม

#### 4.4 Time Decay
- ให้ weight มากกว่ากับข้อมูลล่าสุด (newer observations มี weight มากกว่า)
- ใช้ exponential decay function

#### 4.5 Class Weights
- สำหรับ handling underrepresented labels
- ใน sklearn: `class_weight='balanced'`

> **ข้อสำคัญสำหรับเรา**: Sequential bootstrap + time decay = วิธีแก้ปัญหา overlap ที่我们在 backtest ใช้อยู่

---

### บทที่ 5: Fractionally Differentiated Features

**ปัญหาหลัก**: Time series finance ต้อง stationary สำหรับ ML — แต่ integer differentiation ( differencing 1 ครั้ง, 2 ครั้ง) ทำให้ memory หายหมด

#### 5.1 Stationarity vs Memory Dilemma
- **Integer diff**: ทำให้ series stationary แต่ memory หายเกือบหมด → predictive power หาย
- **No diff**: memory อยู่ครบแต่ non-stationary → model overfit
- **Fractional differentiation**: romise ที่ดีที่สุด — ทำให้ stationary ได้ โดยยังรักษา memory ไว้ได้มาก

#### 5.2 วิธี Fractional Differentiation
- ใช้ weights จาก binomial expansion แบบ fractional order `d` (0 < d < 1)
- **Expanding window**: ใช้ข้อมูลทั้งหมดตั้งแต่ต้น → ได้ weights ที่ดี แต่ computationally expensive
- **Fixed-width window fracdiff (FFD)**: ใช้ rolling window → practical มากกว่า

#### 5.3 Minimum Weight Cutoff
- ตัด weights ที่เล็กเกินไป → ลด computational cost
- ไม่ได้ส่งผลต่อ stationarity มากนัก (happens to achieve stationarity with maximum memory preservation)

> **ข้อสำคัญสำหรับเรา**: Fractional differentiation สำคัญมากสำหรับ stationarity — แต่ใน practice เราใช้ BOCPD (online method) ที่ไม่ต้อง diff ข้อมูล

---

## PART 2: MODELLING (บท 6-9)

---

### บทที่ 6: Ensemble Methods

**เป้าหมาย**: ลด variance, improve accuracy, จัดการ observation redundancy

#### 6.1 Bootstrap Aggregation (Bagging)
- **Variance Reduction**: Train model บน bootstrap samples หลายตัว → average predictions → variance ลดลงตาม OOB
- **Accuracy Improvement**: Bagging classifier มี accuracy สูงกว่า single classifier
- **Observation Redundancy**: ข้อมูล finance มี redundancy สูง — bagging ช่วยจัดการ

#### 6.2 Random Forest (RF)
- Bagging + random feature subsets ที่แต่ละ split
- ลด overfitting จาก correlation ระหว่าง trees
- **Feature importance**: MDI (Mean Decrease Impurity) — เร็วแต่มี bias

#### 6.3 Boosting
- Sequential learning — แต่ละ iteration แก้ misclassified samples
- **AdaBoost**: น้ำหนักสูงขึ้นสำหรับ misclassified samples
- **Bagging vs Boosting ใน Finance**: Bagging มักดีกว่าสำหรับ finance เพราะ data มี noise สูง — Boosting มีแนวโน้ม overfit กับ noisy data

#### 6.4 Bagging for Scalability
- ใช้ bagging เพื่อ parallelize computation — train trees บน different cores

> **ข้อสำคัญสำหรับเรา**: Random Forest + bagging เป็น ensemble ที่เราใช้ใน strategy selection

---

### บทที่ 7: Cross-Validation in Finance

**ปัญหาหลัก**: Standard K-Fold CV **ใช้ไม่ได้** ใน finance เพราะ data มี temporal dependency

#### 7.1 ทำไม K-Fold CV ถึงล้มเหลวใน Finance
- Data finance ไม่ IID — มี autocorrelation, regime changes
- K-Fold สุ่ม split → data leakage (train/test overlap ในเวลา)
- ผลลัพธ์: overfitting ที่ดูดีใน CV แต่ล้มเหลวใน live

#### 7.2 Purged K-Fold CV ✅
- **Purging**: ลบ training observations ที่ overlap กับ test set (的时间 overlap)
- **Embargo**: เพิ่ม buffer zone ระหว่าง training และ test → ป้องกัน leakage จาก autocorrelation
- **Implementation**: ใช้ sklearn base แต่ override split method

#### 7.3 Bugs in Sklearn's Cross-Validation
- sklearn มี bugs หลายจุดที่ทำให้ CV results ผิด → ต้องเช็คให้ดีก่อนใช้

> **ข้อสำคัญสำหรับเรา**: Purged K-Fold CV สำคัญมาก — เราใช้ walk-forward validation ที่คล้ายกัน (embargo + purging)

---

### บทที่ 8: Feature Importance

**ปัญหาหลัก**: วิธีวัด feature importance แบบเดิมมีปัญหา substitution effects

#### 8.1 Feature Importance with Substitution Effects
- **MDI (Mean Decrease Impurity)**: วัดจาก reduction in Gini impurity — เร็วแต่มี bias (ต่อ features ที่มีหลาย levels)
- **MDA (Mean Decrease Accuracy)**: วัดจาก reduction in accuracy เมื่อ shuffle feature — แต่ affected โดย correlated features (substitution effect)

#### 8.2 Feature Importance without Substitution Effects
- **SFI (Single Feature Importance)**: Train model บน feature เดียว → วัด accuracy — ไม่มี substitution effect
- **Orthogonal Features**: Transform features ให้ uncorrelated ก่อน → วัด importance ได้แม่นยำขึ้น

#### 8.3 Parallelized vs Stacked Feature Importance
- Parallelized: วัด feature importance ทีละตัว (ไม่ efficient)
- Stacked: ใช้ matrix algebra คำนวณพร้อมกัน (efficient มาก)

> **ข้อสำคัญสำหรับเรา**: Feature importance ช่วยเลือก indicators ที่สำคัญ — MDI/MDA สำหรับ screening, SFI สำหรับ validate

---

### บทที่ 9: Hyper-Parameter Tuning with Cross-Validation

#### 9.1 Grid Search Cross-Validation
- ลองทุก combination ของ hyperparameters — ละเอียดแต่ slow
- ใช้ purged K-Fold CV (ไม่ใช่ standard K-Fold)

#### 9.2 Randomized Search Cross-Validation
- สุ่ม parameter combinations — เร็วกว่ามักได้ผลใกล้เคียงกัน
- **Log-uniform distribution**: สำหรับ parameters ที่ต้องสุ่มบน log scale (เช่น learning rate, regularization)

#### 9.3 Scoring
- **Neg log loss**: เหมาะกับ classification — วัด probability calibration
- ไม่ควรใช้ accuracy เพียงอย่างเดียว

> **ข้อสำคัญสำหรับเรา**: Hyperparameter tuning ใช้ randomized search + purged CV — สำคัญสำหรับ strategy optimization

---

## PART 3: BACKTESTING (บท 10-16)

---

### บทที่ 10: Bet Sizing

**ปัญหาหลัก**: ตัดสินใจว่าจะ bet ใหญ่แค่ไหน (ไม่ใช่แค่ bet หรือไม่ bet)

#### 10.1 Strategy-Independent Bet Sizing
- **Budgeting approach**: กำหนด max budget แล้วแบ่งเท่าๆ กัน
- **Meta-labeling approach**: ใช้ predicted probability จาก meta-label model → bet size แปรผันกับ confidence

#### 10.2 Bet Sizing from Predicted Probabilities
- แปลง predicted probability เป็น bet size โดยใช้ inverse CDF
- ใช้ Mixture of Gaussians (EF3M) สำหรับ fitting

#### 10.3 Dynamic Bet Sizes and Limit Prices
- ปรับ bet size ตาม market conditions (liquidity, volatility)
- ใช้ limit orders สำหรับ execution

> **ข้อสำคัญสำหรับเรา**: Bet sizing สำคัญมาก — ไม่ใช่แค่ signal แต่ size กำหนด performance จริง

---

### บทที่ 11: The Dangers of Backtesting

**ข้อความสำคัญ**: "Backtesting Is Not a Research Tool"

#### 11.1 Flawless Backtest — Mission Impossible
- มี pitfalls มากมาย: look-ahead bias, survivorship bias, data snooping, transaction cost omission
- **แม้ backtest จะ flawless ก็มักผิด** เพราะเป็นเพียง one realization ของ stochastic process

#### 11.2 Strategy Selection
- ปัญหา: ลอง 100 strategies แล้วเลือกตัวที่ดีที่สุด → overfitting แน่นอน
- **Probability of Backtest Overfitting (PBO)**: วัดว่า backtest นี้ overfit มากแค่ไหน โดยพิจารณาจำนวน trials

#### 11.3 คำแนะนำ
1. อย่าใช้ backtest เป็น research tool — ใช้ feature importance แทน
2. อย่า optimize parameters บน same data ที่ backtest
3. ต้องรู้ว่า strategy ถูก distill มาจากกี่ trials

> **ข้อสำคัญสำหรับเรา**: ตรงกับ approach ที่เราใช้ — backtest สำหรับ validate, ไม่ใช่ research (feature importance + regime detection สำหรับ research)

---

### บทที่ 12: Backtesting through Cross-Validation

#### 12.1 Walk-Forward Method
- Train on historical data → test on next period → roll forward
- **Pitfalls**: ยังมี overfitting ถ้าเลือก window ผิด, ไม่ account สำหรับ multiple testing

#### 12.2 Combinatorial Purged Cross-Validation (CPCV) ✅
- **Combinatorial Splits**: สร้าง test sets จาก combinations ของ K folds — ได้ many more test scenarios
- **Backtest overfitting detection**: ถ้า in-sample performance สูงมากแต่ out-of-sample ต่ำ → overfitting
- ใช้ **Deflated Sharpe Ratio (DSR)**: Sharpe ratio ที่ปรับสำหรับ multiple testing

> **ข้อสำคัญสำหรับเรา**: CPCV + DSR = framework สำหรับ evaluate strategy quality

---

### บทที่ 13: Backtesting on Synthetic Data

**แนวคิด**: ใช้ข้อมูล synthetically generated ที่รู้ ground truth สำหรับ backtesting

#### 13.1 Optimal Trading Rule (OTR) Framework
- สร้าง synthetic data ที่มี known parameters (long-run equilibrium, mean reversion speed)
- ทดสอบ trading rules บน synthetic data → เปรียบเทียบ performance กับ known optimum
- ช่วย evaluate backtest quality

#### 13.2 กรณีต่างๆ
- **Zero long-run equilibrium**: ไม่มี edge จริง → backtest overfitting เห็นได้ชัด
- **Positive long-run equilibrium**: มี edge จริง → backtest quality ขึ้นกับ signal-to-noise ratio
- **Negative long-run equilibrium**: กลยุทธ์ไม่ดีจริง

> **ข้อสำคัญสำหรับเรา**: Synthetic data backtesting เป็นวิธี validate strategy quality ที่ powerful

---

### บทที่ 14: Backtest Statistics

#### 14.1 Performance Metrics
- **Time-Weighted Rate of Return (TWRR)**: Returns ที่ปรับสำหรับ cash flows
- **Drawdown (DD) และ Time under Water (TuW)**: วัด peak-to-trough loss

#### 14.2 Efficiency Measurements
- **Sharpe Ratio**: Standard metric — แต่มีปัญหาเรื่อง statistical significance
- **Probabilistic Sharpe Ratio (PSR)**: คำนวณ probability ว่า Sharpe ratio ที่สังเกตได้จริงมีค่า > threshold
- **Deflated Sharpe Ratio (DSR)**: ปรับ Sharpe ratio สำหรับ multiple testing (backtest overfitting)

#### 14.3 Implementation Shortfall
- ต้นทุนของการ delay execution — วัด slippage จริง

#### 14.4 Attribution
- วิเคราะห์ว่า performance มาจากส่วนไหน (side selection, sizing, timing)

> **ข้อสำคัญสำหรับเรา**: DSR สำคัญมาก — Sharpe ratio ที่ไม่ adjusted สำหรับ multiple testing มีค่า misleading

---

### บทที่ 15: Understanding Strategy Risk

#### 15.1 Symmetric Payouts
- ถ้า win/loss เท่ากัน — strategy risk ขึ้นกับ betting frequency + precision
- **Trade-off**: frequency สูง → precision ต่ำ, frequency ต่ำ → precision สูง

#### 15.2 Asymmetric Payouts
- Profit-taking vs stop-loss limits → payout distribution ไม่ symmetric
- วิเคราะห์ probability of strategy failure สำหรับ symmetric และ asymmetric payouts

#### 15.3 Probability of Strategy Failure
- Algorithm: คำนวณว่า strategy จะ lose money นานแค่ไหนก่อนที่จะ recover
- สำคัญสำหรับ risk management — ไม่ใช่แค่ Sharpe ratio

> **ข้อสำคัญสำหรับเรา**: Strategy risk ≠ portfolio risk — ต้องวิเคราะห์ทั้งสอง levels

---

### บทที่ 16: Machine Learning Asset Allocation

**ปัญหาหลัก**: Markowitz mean-variance optimization มีปัญหามาก (Markowitz's Curse)

#### 16.1 The Problem with Markowitz
- **Markowitz's Curse**: น้อย assets ที่มี expected return สูง + low correlation + low volatility — allocation ที่ได้มัก overfit
- Covariance matrix ไม่ stable (estimation error สูง)

#### 16.2 Hierarchical Risk Parity (HRP) ✅
- วิธี allocation ที่ใช้ **graph theory** แทน statistical optimization
- **Step 1 - Tree Clustering**: Cluster assets ตาม correlation distance → dendrogram
- **Step 2 - Quasi-Diagonalization**: Reorder correlation matrix ตาม dendrogram → block structure
- **Step 3 - Recursive Bisection**: แบ่ง portfolio เป็น sub-clusters → allocate inversely proportional กับ variance

#### 16.3 ข้อดีของ HRP
- **Robust**: ไม่ต้อง estimate inverse covariance matrix
- **Out-of-sample**: ทดสอบกับ Monte Carlo simulations → HRP ชนะ Markowitz และ IVP (Inverse-Variance Portfolio)
- **Spread of outcomes**: HRP มี variance ต่ำกว่า significantly

> **ข้อสำคัญสำหรับเรา**: HRP = asset allocation method ที่ดีกว่า Markowitz — ใช้ hierarchical relationship แทน statistical optimization

---

## PART 4: USEFUL FINANCIAL FEATURES (บท 17-19)

---

### บทที่ 17: Structural Breaks

**ปัญหาหลัก**: Finance มี regime changes — model ที่trained บน regime เก่าจะ fail เมื่อ regime เปลี่ยน

#### 17.1 CUSUM Tests
- **Brown-Durbin-Evans CUSUM**: ทดสอบ structural break โดยดู cumulative sum ของ recursive residuals
- **Chu-Stinchcombe-White CUSUM**: ทดสอบบน levels (ไม่ใช่ residuals)

#### 17.2 Explosiveness Tests
- **Chow-Type Dickey-Fuller**: ทดสอบว่า series มี explosive behavior (bubble) หรือไม่
- **Supremum Augmented Dickey-Fuller (SADF)**: ทดสอบ multiple structural breaks
  - **Conditional ADF**: ใช้ threshold สำหรับ regime detection
  - **Quantile ADF**: ทดสอบที่ quantiles ต่างๆ

#### 17.3 Sub- and Super-Martingale Tests
- ทดสอบว่า series เป็น martingale, sub-martingale (upward trend), หรือ super-martingale (downward trend)

> **ข้อสำคัญสำหรับเรา**: Structural break tests สำคัญมากสำหรับ regime detection — ตรงกับ BOCPD ที่เราใช้

---

### บทที่ 18: Entropy Features

**เป้าหมาย**: วัด "information content" ของ financial time series

#### 18.1 Shannon's Entropy
- H(X) = -Σ p(x) log p(x) — วัด average information content

#### 18.2 Estimators
- **Plug-in (MLE) Estimator**: คำนวณจาก observed frequencies — simple แต่ biased
- **Lempel-Ziv (LZ) Estimator**: วัด complexity — เร็วและ robust กับ finite samples

#### 18.3 Encoding Schemes
- **Binary Encoding**: UP/DOWN
- **Quantile Encoding**: แบ่งเป็น quantiles
- **Sigma Encoding**: แบ่งตาม standard deviations

#### 18.4 Financial Applications
- **Market Efficiency**: Entropy สูง → efficient market (harder to predict)
- **Maximum Entropy Generation**: ช่วงที่ entropy เพิ่ม = ช่วงที่ market กำลัง "เรียนรู้"
- **Portfolio Concentration**: ใช้ entropy วัด diversification
- **Market Microstructure**: วัด informed trading

> **ข้อสำคัญสำหรับเรา**: Entropy features ช่วย measure signal-to-noise ratio — สำคัญสำหรับ regime detection

---

### บทที่ 19: Microstructural Features

**เป้าหมาย**: ใช้ข้อมูลระดับ microstructure (tick-level) สำหรับ generating signals

#### 19.1 First Generation: Price Sequences
- **Tick Rule**: ระบุ buy/sell direction จาก price changes
- **Roll Model**: ประมาณ bid-ask spread จาก serial covariance ของ price changes
- **High-Low Volatility Estimator**: ประมาณ volatility จาก daily high-low range
- **Corwin and Schultz**: ประมาณ bid-ask spread จาก high-low prices

#### 19.2 Second Generation: Strategic Trade Models
- **Kyle's Lambda**: วัด price impact ต่อ informed trading — lambda สูง = market ไม่ liquidity
- **Amihud's Lambda**: วัด illiquidity ratio — |return| / dollar volume
- **Hasbrouck's Lambda**: วัด price impact จาก vector autoregression

#### 19.3 Third Generation: Sequential Trade Models
- **PIN (Probability of Informed Trading)**: ประมาณ probability ที่มี informed trader ใน market
- **VPIN (Volume-Synchronized PIN)**: ใช้ volume-synchronized approach — เร็วกว่า PIN มาก

#### 19.4 Additional Microstructural Features
- Distribution of order sizes
- Cancellation rates (limit orders vs market orders)
- TWAP execution algorithms
- Serial correlation of signed order flow

> **ข้อสำคัญสำหรับเรา**: Microstructural features ช่วย generate signals จาก order flow — สำคัญสำหรับ tick-level trading

---

## PART 5: HIGH-PERFORMANCE COMPUTING RECIPES (บท 20-22)

---

### บทที่ 20: Multiprocessing and Vectorization

**เป้าหมาย**: เร่ง computation สำหรับ ML ที่ต้อง process ข้อมูลจำนวนมาก

#### 20.1 Vectorization
- ใช้ numpy arrays แทน Python loops → เร็วขึ้น 10-100×
- Example: คำนวณ returns จาก price series

#### 20.2 Single-Thread vs Multithreading vs Multiprocessing
- **Single-thread**: ทำงานทีละ task
- **Multithreading**: หลาย tasks แต่ sharing memory (มี GIL limit ใน Python)
- **Multiprocessing**: หลาย processes แยก memory — **เหมาะสำหรับ CPU-bound tasks**

#### 20.3 Atoms and Molecules
- **Atom**: smallest unit of computation
- **Molecule**: group of atoms ที่ต้องทำพร้อมกัน
- จัด partitioning สำหรับ parallel execution

#### 20.4 Multiprocessing Engines
- Job preparation → Asynchronous calls → Callback unwrapping → Output reduction

> **ข้อสำคัญสำหรับเรา**: Multiprocessing สำคัญมากสำหรับ batch backtesting — เราใช้ multiprocessing อยู่แล้ว

---

### บทที่ 21: Brute Force and Quantum Computers

**เป้าหมาย**: แก้ combinatorial optimization problems ที่ classical computers แก้ไม่ได้

#### 21.1 Combinatorial Optimization
- ตัวอย่าง: หา optimal routing, portfolio optimization, feature selection
- Classical: exponential time complexity → ไม่ tractable สำหรับ large problems

#### 21.2 Quantum Computing Approach
- **Pigeonhole Partitions**: แบ่ง search space สำหรับ parallel exploration
- **Feasible Static Solutions**: หา initial solutions ที่ practicable
- **Dynamic Solutions**: ปรับ solutions แบบ step-by-step

#### 21.3 Random Matrices
- ใช้ random matrix theory สำหรับ initializing quantum optimization

> **ข้อสำคัญสำหรับเรา**: Quantum computing ยังไม่ practical สำหรับ trading ปัจจุบัน แต่เป็น direction ที่น่าสนใจ

---

### บทที่ 22: High-Performance Computational Intelligence and Forecasting Technologies
**(เขียนร่วมโดย Kesheng Wu และ Horst D. Simon — Lawrence Berkeley National Laboratory)**

**เป้าหมาย**: แสดงให้เห็นว่า National Laboratories ใช้ ML มานานและ successfully

#### 22.1 Flash Crash of 2010
- ตลาดหุ้น US ร่วง ~1000 points ในไม่กี่นาที — กลับมาภายใน 20 นาที
- Berkeley Lab ใช้ HPC tools สำหรับ analyze สาเหตุ

#### 22.2 HPC Hardware
- **CPUs**: multicore, hyper-threading
- **GPUs**: NVIDIA CUDA — massively parallel (thousands of cores)
- **FPGAs**: programmable hardware — low latency

#### 22.3 HPC Software
- **MPI (Message Passing Interface)**: standard สำหรับ distributed computing
- **HDF5 (Hierarchical Data Format 5)**: efficient data storage สำหรับ large datasets
- **In Situ Processing**: process data ขณะที่กำลังสร้าง (ไม่ write to disk แล้วค่อยอ่าน)
- **ADIOS (Adaptable I/O System)**: I/O framework สำหรับ scientific computing

#### 22.4 Use Cases
- **Supernova Hunting**: ใช้ ML ค้นหา supernova explosions จาก telescope data
- **Fusion Plasma**: วิเคราะห์ blob ใน fusion plasma สำหรับ nuclear fusion
- **Peak Electricity Usage**: ทำนาย peak demand
- **VPIN Calibration**: ปรับเทียบ VPIN indicator ด้วย HPC

> **ข้อสำคัญสำหรับเรา**: HPC infrastructure สำคัญมากสำหรับ scaling ML trading — GPU, multiprocessing, distributed computing ล้วนเป็นส่วนหนึ่งของ system ที่เรา build

---

## สรุปภาพรวมสำหรับ Trading System ที่เราสร้าง

### Key Takeaways จาก AFML ที่ตรงกับ project ปัจจุบัน:

1. **Data Structure**: Dollar bars + event-based sampling ดีกว่า time bars — ตรงกับ approach ที่เราใช้
2. **Triple-Barrier Method + Meta-Labeling**: Framework ที่เราใช้กับ BOCPD + regime detection
3. **Fractional Differentiation**: สำคัญสำหรับ stationarity แต่ BOCPD (online method) ทำหน้าที่เดียวกัน
4. **Purged K-Fold CV + Walk-Forward**: ตรงกับ validation ที่เราใช้ (embargo + purging)
5. **Feature Importance (MDI/MDA/SFI)**: ช่วยเลือก indicators ที่สำคัญ — ตรงกับ our feature selection approach
6. **HRP (Hierarchical Risk Parity)**: Asset allocation ที่ดีกว่า Markowitz — ยังไม่ได้ใช้แต่ควร implement
7. **Structural Breaks + Entropy**: ตรงกับ BOCPD regime detection ที่เราใช้
8. **Multiprocessing**: สำคัญมากสำหรับ batch backtesting — ตรงกับ architecture ที่เราใช้
9. **Backtest Overfitting**: DSR + CPCV = วิธี evaluate strategy quality ที่ถูกต้อง — ตรงกับ our statistical significance testing
10. **Meta-Strategy Paradigm**: โรงงานวิจัยแบบ systematic — ตรงกับ architecture ที่เรา build

### สิ่งที่ควร implement เพิ่ม:
1. **HRP สำหรับ portfolio allocation** — แทน mean-variance optimization
2. **Entropy features** สำหรับ measuring signal-to-noise ratio — เพิ่มเป็น regime indicator
3. **Microstructural features (VPIN)** สำหรับ liquidity detection — เพิ่มเป็น execution filter
4. **Deflated Sharpe Ratio** สำหรับ backtest evaluation — ปรับ Sharpe ratio สำหรับ multiple testing

---

## อ้างอิง
- López de Prado, M. (2018). *Advances in Financial Machine Learning*. Wiley.
- ISBN: 978-1-119-48208-6
- เว็บไซต์: www.QuantResearch.org
