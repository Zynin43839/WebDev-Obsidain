# สรุป Advances in Financial Machine Learning — ภาษาไทย

> **ผู้เขียน**: Marcos López de Prado
> **สรุปโดย**: AI Agent | 57 chunks, ~250 หน้า

---

## ภาพรวม

หนังสือเล่มนี้เป็นคู่มือเชิงปฏิบัติสำหรับการนำ Machine Learning (ML) มาใช้ในโลกการเงิน โดยผู้เขียนซึ่งเป็นผู้จัดการกองทุนหลายพันล้านดอลลาร์ ได้ออกแบบมาให้แก้ปัญหาเฉพาะทางที่ ML ทั่วไปไม่สามารถจัดการได้ เช่น overfitting, ข้อมูลที่ไม่เป็น IID, และ backtest overfitting

หนังสือแบ่งออกเป็น **5 ส่วนหลัก** ตามสายการผลิต (Production Chain) ของการวิจัยการลงทุน

---

## PART 1: Data Analysis — การวิเคราะห์ข้อมูล

### Chapter 1: Financial ML as a Distinct Subject

- **เหตุผลที่เขียนหนังสือ**: ต้องการเชื่อมช่องว่างระหว่าง theory เชิงวิชาการกับการใช้งานจริงในวงการ fund
- **Sisyphus Paradigm**: การให้ PhD 50 คน ทำงานคนละกลยุทธ์มักล้มเหลว เพราะแต่ละคนค้นหา signal ของตัวเองจน overfit
- **Meta-Strategy Paradigm**: ควรจัดทีมวิจัยเป็น assembly line — Data Curators → Feature Analysts → Strategists → Backtesters → Deployment → Portfolio Oversight
- **Financial ML ≠ Econometrics**: Econometrics ใช้ regression ที่ไม่เรียนรู้, ML เรียนรู้ pattern ในมิติสูงโดยไม่ต้องกำกับ ทิศทางของเศรษฐศาสตร์ยุคต่อไปคือ "Kepler's moment" ที่จะทิ้งสมมติฐานเชิงเส้น

### Chapter 2: Financial Data Structures

- **ประเภทข้อมูล 4 แบบ**: Fundamental (งบการเงิน), Market (ราคา/Volume), Analytics (sentiment), Alternative (ดาวเทียม/Social media)
- **Bar Types**: Time Bars (ไม่ดี — heteroscedasticity), Tick Bars (sample ตามจำนวน tick), Volume Bars (sample ตาม volume — ดีกว่า), Dollar Bars (sample ตามมูลค่า — ดีที่สุด เพราะ stable ต่อ corporate actions)
- **Information-Driven Bars**: Tick Imbalance Bars (TIB), Volume/Dollar Imbalance Bars (VIB/DIB), Runs Bars — sample บ่อยขึ้นเมื่อมี informed trading
- **ETF Trick**: แปลง multi-product spread ให้เป็น single cash-like series เพื่อป้องกัน negative prices และ weight-induced convergence
- **CUSUM Filter**: ใช้ cumulative sum ตรวจจับ regime change ได้ดีกว่า Bollinger Bands (ไม่ triggered ซ้ำที่ threshold)

### Chapter 3: Labeling

- **Fixed-Time Horizon**: วิธีดั้งเดิมที่มีปัญหา — threshold ตายตัวไม่เหมาะกับ volatility ที่เปลี่ยน
- **Triple-Barrier Method**: นวัตกรรมหลักของหนังสือ — ใช้ 3 barrier (profit-taking, stop-loss, vertical/expiry) ข้อดีคือ path-dependent, realistic (มี stop-loss เหมือนชีวิตจริง)
- **Meta-Labeling**: แยก side (long/short) กับ size (ขนาด position) ออกจากกัน — model หลักตัดสิน side, model รองตัดสิน size/probability ช่วยเพิ่ม F1-score, เหมาะกับ "quantamental" approach
- **Dynamic Thresholds**: ใช้ exponentially weighted std ของ daily returns ปรับ TP/SL ให้ adaptive กับ volatility

### Chapter 4: Sample Weights

- **ปัญหา Overlapping Outcomes**: Labels ที่ overlap กันทำให้ไม่ IID → bootstrap ได้ sample ที่ redundant
- **Uniqueness**: คำนวณ average uniqueness ของแต่ละ label จาก indicator matrix แล้วใช้เป็น sample weight
- **Sequential Bootstrap**: ดึงตัวอย่างทีละตัว โดยลด probability ของ observations ที่ overlap กับที่ดึงไปแล้ว — ได้ sample ที่ใกล้ IID กว่า standard bootstrap
- **Return Attribution**: Weight ตาม absolute return ที่ attribut ได้จริง (ไม่ใช่ weight เท่ากันหมด)
- **Time Decay**: ให้ weight น้อยลงสำหรับข้อมูลเก่า โดย decay ตาม cumulative uniqueness (ไม่ใช่ chronological)

### Chapter 5: Fractionally Differentiated Features

- **Dilemma**: Returns ไม่มี memory (over-differentiated), Prices มี memory แต่ไม่ stationary
- **Fractional Differentiation (FFD)**: ใช้ weights จาก binomial expansion แบบ fractional order d ∈ (0,1) — ทำให้ series stationary โดยยังคง memory ได้มากที่สุด
- **Key Insight**: สำหรับ E-mini S&P 500, d = 0.35 ผ่าน ADF test (stationary) แต่ correlation กับ original price ยัง 0.995 เทียบกับ returns (d=1) correlation แค่ 0.03
- **Fixed-Width Window**: ตัด weight ที่เล็กกว่า threshold → ป้องกัน negative drift จาก expanding window

---

## PART 2: Modelling — แบบจำลอง

### Chapter 6: Ensemble Methods

- **Bagging (Bootstrap Aggregation)**: ลด variance โดยสร้าง N models จาก bootstrap samples แล้ว average predictions — variance ลดลงเมื่อ average correlation ρ̄ ต่ำ
- **Random Forest**: เหมือน bagging แต่เพิ่ม random subspace ที่ node split → decorrelate trees มากขึ้น
- **Boosting (AdaBoost)**: Sequential, ลดทั้ง bias และ variance แต่เสี่ยง overfitting กว่า bagging — ใน finance, bagging มักดีกว่าเพราะ overfitting เป็นปัญหาหลัก
- **Bagging for Scalability**: ใช้ bagging กับ classifier ที่ช้า เช่น SVM โดย early-stop แต่ละ estimator → parallel ได้

### Chapter 7: Cross-Validation in Finance

- **K-Fold CV ล้มเหลวใน Finance**: Leakage จาก serial correlation + overlapping labels → inflates accuracy
- **Purged K-Fold CV**: ลบ training observations ที่ label overlap กับ test set (purging) + ลบ observations หลัง test set (embargo ≈ 1% of T)
- **Sklearn bugs**: scoring functions ไม่รู้จัก classes_, cross_val_score ไม่ pass weights → ต้องใช้ cvScore function ของผู้เขียนเอง

### Chapter 8: Feature Importance

- **"Backtesting is not a research tool. Feature importance is."** — Marcos' First Law
- **MDI (Mean Decrease Impurity)**: เร็ว, in-sample, เฉพาะ tree-based — มี bias จาก substitution effects (features ที่คล้ายกัน importance ถูกแบ่ง)
- **MDA (Mean Decrease Accuracy)**: Slow, out-of-sample, permutation-based — ใช้กับ classifier ใดก็ได้ แต่ก็มี substitution effects
- **SFI (Single Feature Importance)**: OOS per-feature — ไม่มี substitution effects แต่ไม่จับ joint effects
- **Orthogonal Features (PCA)**: ทำ PCA ก่อนแล้ววิเคราะห์ feature importance → ลด substitution effects + ใช้ eigenvalue ranking เป็น confirmatory evidence

### Chapter 9: Hyper-Parameter Tuning

- **GridSearchCV + PurgedKFold**: ใช้ purged CV แทน standard k-fold
- **RandomizedSearchCV**: ดีกว่าเมื่อ parameter space ใหญ่ — ควบคุม computational budget ได้
- **Log-Uniform Distribution**: สำหรับ parameters เช่น C, gamma ที่ไม่ respond linearly → ควร sample จาก log-uniform ไม่ใช่ uniform
- **Scoring**: ใช้ neg_log_loss แทน accuracy เพราะ profit/loss ขึ้นกับ probability (confidence), ไม่ใช่แค่ correct/incorrect

---

## PART 3: Backtesting — การทดสอบย้อนหลัง

### Chapter 10: Bet Sizing

- **Strategy-Independent**: ใช้ Gaussian mixture กับ concurrent bets แล้ว derive size จาก CDF
- **Budget Approach**: จำกัด position ไม่ให้เกิน max concurrent bets
- **Meta-Labeling Bet Size**: แปลง probability จาก classifier เป็น size ด้วย z-test กับ null hypothesis p = 1/|X|
- **Dynamic Bet Size + Limit Price**: ใช้ sigmoid function m = x / √(ω + x²) ปรับ position ตาม price divergence → limit price อยู่ระหว่าง current price กับ forecast

### Chapter 11: The Dangers of Backtesting

- **7 Sins of Quantitative Investing** (Deutsche Bank): Survivorship bias, Look-ahead bias, Storytelling, Data mining, Transaction costs, Outliers, Shorting costs
- **Backtesting ≠ Research Tool**: Feature importance คือเครื่องมือวิจัย, backtest ใช้ discard bad models
- **Backtest Overfitting**: ยิ่งเชี่ยวชาญ backtest มากเท่าไหร่ ยิ่งเจอ false discovery มากเท่านั้น เพราะ multiple testing

### Chapter 12: Backtesting through Cross-Validation

- **Walk-Forward (WF)**: มี historical interpretation ชัดเจน แต่ทดสอบแค่ 1 scenario (history), overfit ง่าย
- **CV Method**: ทดสอบ stress scenarios แต่ไม่มี historical interpretation, leakage อาจเกิดถ้าไม่ purged ดี
- **Combinatorial Purged CV (CPCV)**: สร้าง multiple backtest paths จาก N groups × k testing — ได้ distribution ของ Sharpe ratio แทน single Sharpe ratio → ลด false discovery

### Chapter 13: Backtesting on Synthetic Data

- **Approach**: ใช้ Ornstein-Uhlenbeck process สร้าง synthetic data จาก parameters ที่ estimate จาก history → ทดสอบ trading rules บน 100,000 synthetic paths
- **Key Finding**: Half-life (τ) สำคัญมาก — สั้น = optimal PT/SL ชัดเจน, ยาว = 逼近 random walk = ไม่มี optimal rule = overfitting exercise
- **Zero Equilibrium (Market Maker)**: Optimal คือ small PT + large SL (hold inventory จนเล็กน้อย profit)
- **Positive Equilibrium (Position Taker)**: Optimal คือ PT ใกล้ forecast, SL ใหญ่พอ

### Chapter 14: Backtest Statistics

- **General Characteristics**: Time range, AUM, Capacity, Leverage, Bet frequency, Holding period, Turnover, Correlation to underlying
- **Performance**: PnL, Hit ratio, Time-Weighted Rate of Return (TWRR)
- **Runs**: HHI concentration (returns + time), Drawdown (DD), Time under Water (TuW)
- **Sharpe Ratio**: SR = μ/σ — แต่มี estimation error เสมอ
- **Probabilistic SR (PSR)**: ปรับ SR ด้วย skewness, kurtosis, track record length → PSR > 0.95 คือ significant
- **Deflated SR (DSR)**: ปรับ SR สำหรับ multiple testing — ยิ่ง trial มาก ยิ่งต้อง deflate มาก → **"Every backtest result must be reported in conjunction with all the trials involved"** (Marcos' Third Law)
- **Classification Scores**: Accuracy, Precision, Recall, F1-score (harmonic mean), Negative log-loss

---

## PART 4: Useful Financial Features — Feature ทางการเงินที่มีประโยชน์

### Chapter 17: Structural Breaks

- **CUSUM Tests**: Brown-Durbin-Evans (recursive residuals), Chu-Stinchcombe-White (levels)
- **Explosiveness Tests**: Chow-Type Dickey-Fuller, Supremum ADF (SADF) — ตรวจ bubble/regime change
- **Sub- and Super-Martingale Tests**: ทดสอบ hypothesized price behavior

### Chapter 18: Entropy Features

- **Shannon Entropy**: วัด information content ของ time series
- **Lempel-Ziv Estimators**: Compression-based — ยิ่ง compress ได้ดี ยิ่งมี structure/predictability
- **Encoding Schemes**: Binary, Quantile, Sigma — เปลี่ยน price series เป็น symbol sequence
- **Applications**: Market efficiency (entropy สูง = efficient), Portfolio concentration, Market microstructure

### Chapter 19: Microstructural Features

- **1st Gen (Price-based)**: Tick rule, Roll model (bid-ask spread proxy), High-low estimator, Corwin-Schultz
- **2nd Gen (Strategic Trade)**: Kyle's Lambda, Amihud's Lambda, Hasbrouck's Lambda — วัด price impact
- **3rd Gen (Sequential Trade)**: PIN (Probability of Information-based Trading), VPIN (Volume-synchronized PIN) — วัด informed trading
- **Additional**: Order size distribution, Cancellation rates, TWAP execution, Options implied info

---

## PART 5: High-Performance Computing Recipes

### Chapter 20: Multiprocessing and Vectorization

- **Vectorization**: ใช้ NumPy operations แทน Python loops
- **Multiprocessing**: แบ่งงานเป็น atoms/molecules → ทำ parallel ผ่าน multiprocessing pools
- **Pickle/Unpickle**: Serialize objects สำหรับ inter-process communication

### Chapter 21: Brute Force and Quantum Computers

- **Combinatorial Optimization**: ใช้ integer optimization + pigeonhole partitions แก้ allocation problems
- **Quantum Computing**: สำรวจวิธีใช้ quantum computers สำหรับ intractable financial problems

### Chapter 22: HPC Computational Intelligence (by Kesheng Wu & Horst Simon)

- **Background**: US National Labs (Berkeley Lab) ใช้ ML มานานก่อน Silicon Valley
- **Use Cases**: Supernova hunting, Fusion plasma analysis, Electricity peak prediction, Flash Crash 2010 analysis, VPIN calibration
- **Key Hardware**: GPU (NVIDIA), MPI, HDF5, In Situ Processing

---

## ประเด็นสำคัญสำหรับการนำไปใช้จริง

1. **Fractional Differentiation** แก้ปัญหา stationarity vs memory ได้ดีกว่า returns — ลอง d ∈ [0.3, 0.5] กับ price data ของคุณ
2. **Purged K-Fold CV** จำเป็นมากสำหรับ financial time series — standard k-fold จะ leakage เสมอ
3. **Feature Importance > Backtesting** สำหรับการวิจัย — อย่า backtest ซ้ำๆ จน overfit
4. **Meta-Labeling** เป็น bridge ที่ดีระหว่าง discretionary และ systematic — เพิ่ม ML layer บน model ที่มีอยู่
5. **DSR (Deflated Sharpe Ratio)** ต้องรายงานทุกครั้ง — Sharpe ratio ที่ไม่ได้ deflate แทบไม่มีความหมาย
6. **Sequential Bootstrap** + **Sample Weights** แก้ปัญหา non-IID observations ได้จริง
7. **Backtest on Synthetic Data** หลีกเลี่ยง overfitting ได้มากกว่า backtest บน history อย่างเดียว

---

## ศัพท์เทคนิคที่คงไว้ (ไม่แปล)

| ศัพท์ | ความหมายโดยย่อ |
|--------|----------------|
| ML (Machine Learning) | การเรียนรู้จากข้อมูล |
| Overfitting | แบบจำลองจดจำ noise แทน pattern จริง |
| Cross-Validation | การแบ่งข้อมูล train/test เพื่อทดสอบ generalization |
| Feature Importance | วัดความสำคัญของแต่ละ feature |
| Ensemble Methods | รวมหลาย models เพื่อผลลัพธ์ที่ดีกว่า |
| Meta-Labeling | แยก side/size — model รองตัดสิน size จาก model หลัก |
| Fractional Differentiation | การถอดลักษณะแบบเศษส่วน — รักษา memory มากกว่า returns |
| Triple-Barrier Method | ใช้ 3 barrier (PT/SL/expiry) ติดป้าย labels |
| Sharpe Ratio | ผลตอบแทนต่อความเสี่ยง |
| Purged K-Fold CV | k-fold CV ที่ลบ overlapping observations ออกแล้ว |
| CPCV | Combinatorial Purged Cross-Validation — สร้าง multiple backtest paths |
| DSR (Deflated Sharpe Ratio) | Sharpe ratio ที่ปรับสำหรับ multiple testing |
| PBO (Probability of Backtest Overfitting) | ความน่าจะเป็นที่ backtest overfit |
| Bet Sizing | การกำหนดขนาด position จาก ML predictions |
| HPC (High-Performance Computing) | การคำนวณประสิทธิภาพสูง |
