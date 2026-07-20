# AI Algorithmic Trading Volume 2: Backtesting
> **Author:** Lopez de Prado

> เล่มที่ 2 ของชุด AI Algorithmic Trading เน้น Machine Learning สำหรับการพยากรณ์ผลตอบแทน (Chapters 11-15)

## ภาพรวม

Volume II ครอบคลุม Machine Learning สำหรับการเทรด: เริ่มจาก Supervised Learning สำหรับพยากรณ์ผลตอบแทน (Ch11) → Regularization และ Model Selection (Ch12) → Deep Learning (Ch13) → Regime Detection (Ch14) → Event-Driven Trading (Ch15) เป็นการวางรากฐานสำหรับการเทรดอัตโนมัติด้วย ML ภายใต้กรอบ Governance ที่เคร่งครัด

---

## Chapter 11: Supervised Learning สำหรับการพยากรณ์ผลตอบแทน

### 11.1 ตำแหน่งในหนังสือ
- เป็นจุดแรกที่อนุญาตให้ "fit a model" อย่างเป็นทางการ
- Chapters 01-10 เตรียมพื้นฐาน: avoiding look-ahead bias, risk management, backtesting
- ต้องเข้าใจว่า supervised learning นั่งอยู่ใน signal engine ไม่ใช่เหนือทั้งระบบ
- Prerequisites: time-series anatomy (Ch04), feature engineering (Ch05), backtesting (Ch06)

### 11.2 ทำไมการพยากรณ์ผลตอบแทนเป็น ML Problem ที่พิเศษ
- **Predictive stance**: เป้าหมายคือ prediction under constraints ไม่ใช่ explanation
- **Stylized facts ที่ทำลาย ML assumptions**:
  - Non-stationarity: ความสัมพันธ์ระหว่าง features และ returns เปลี่ยนแปลงตลอดเวลา
  - Heavy tails: เหตุการณ์รุนแรงเกิดบ่อยกว่า Gaussian distribution
  - Low signal-to-noise: component ที่คาดเดาได้เล็กมากเทียบกับ noise
- **Governance constraint**: Leakage เป็น failure mode หลัก — ข้อมูลจากอนาคตหลุดเข้าสู่ training

### 11.3 Problem Setup และ Notation
- ให้เวลาตัดสินใจ t, information set I_t, feature vector X_t ∈ R^d
- Target: y_{t+h} ที่ h ≥ 1 (forecast horizon)
- Dataset: {(X_t, y_{t+h})} ที่ features ต้องคำนวณได้จากข้อมูล ณ เวลา t เท่านั้น

### 11.4 Target Engineering และ Label Design
- **Return definitions**: Simple return, log return, net-of-carry return
- **Multi-horizon targets**: Aggregate returns หลาย period → ลด noise แต่สร้าง overlap
- **Excess returns / hedged targets**: ลบ benchmark return หรือ hedge ratio
- **Classification labels**: Direction (±1), binary (0/1), thresholded (±1/0 สำหรับ move ที่เกิน threshold τ)
- **Ranking targets**: Cross-sectional ranking — ใช้สำหรับ multi-asset strategies
- **Label sanity checks**: Distribution diagnostics, time stability, autocorrelation, alignment, economic plausibility, baseline predictability

### 11.5 Features: เชื่อมต่อ Chapter 05 กับ Predictive Modeling
- **Causality-first construction**: ต้องรู้ decision time, enforce one-sided computations, make alignment explicit
- **Canonical feature families**:
  - Return/volatility features (Ch04/Ch10): lagged returns, realized vol, vol-of-vol
  - Trend features (Ch07): MA deviation, multi-horizon momentum
  - Mean-reversion features (Ch08): z-scores, spread features, reversal proxies
  - Liquidity proxies: volume changes, range-based, dollar volume
- **Scaling**: Expanding-window vs rolling-window standardization — ต้อง fit เฉพาะ training data เท่านั้น
- **Feature stability diagnostics**: Rolling distribution, correlation drift, feature-label relationship drift

### 11.6 Baseline Models ที่ต้องเอาชนะ
- **Null predictors**: Predict zero, predict mean, random sign
- **Linear regression (OLS)**: Walk-forward OLS — re-fit ทุก fold, interpret coefficients ว่า directional association
- **Logistic regression**: สำหรับ directional labels — output probability สำหรับ position sizing
- **Simple tree-based baselines**: Decision trees สำหรับ nonlinear interactions — ใช้ conservative settings

### 11.7 Loss Functions ที่สอดคล้องกับการเทรด
- **Statistical losses**: MSE, MAE, log-loss — สำหรับ training
- **Decision-oriented objectives**: Forecast → position mapping — P&L ขึ้นอยู่กับ correlation ไม่ใช่ error
- **Asymmetric losses**: Weight positive/negative errors ต่างกัน — tail risk
- **Calibration vs discrimination**: Calibration สำคัญสำหรับ position sizing, discrimination สำหรับ ranking

### 11.8 Evaluation Under Time: Walk-Forward Research Protocol
- **One-shot split**: ง่ายแต่ fragile — ขึ้นอยู่กับ boundary เดียว
- **Walk-forward evaluation**: Expanding-window หรือ rolling-window — produce time series of out-of-sample performance
- **Forecast quality metrics**: IC (correlation), directional accuracy, MSE/MAE
- **Strategy metrics**: Sharpe ratio, drawdowns, hit-rate, turnover
- **Statistical hygiene**: Multiple testing, p-hacking warnings

### 11.9 From Forecast to Trade: Minimal Signal-to-Position Mapping
- **Thresholding**: Trade only when |forecast| ≥ τ — ลด turnover
- **Risk targeting wrapper**: position = κ/σ × signal — ควบคุม volatility
- **Position limits**: Clipping, exposure caps, fail-safe rules
- **Minimal backtest handshake**: Decision schedule → forecast → position → realized return

### 11.10 Failure Modes และ Diagnostic Playbook
- **Leakage triage**: 3 invariants — strict time ordering, causal preprocessing, embargo/purging
- **Non-stationarity**: Rolling performance decay, retraining cadence
- **Overfitting symptoms**: Instability across folds, sensitivity to design changes
- **Feature instability**: Distribution drift, correlation drift, regime sensitivity

### 11.11 Governance-Native Checklist
- **Determinism**: Seeds, configs, run manifests
- **Data lineage**: Fingerprints, hashes, timing proofs
- **Causality gates**: No-look-ahead assertions as hard stops
- **Model cards**: Assumptions, limits, monitoring plan
- **Minimum reproducible artifact bundle**: Data fingerprints, feature manifests, split protocols, evaluation reports

---

## Chapter 12: Regularization, Hyperparameters, Model Selection

### 12.1 ตำแหน่งในหนังสือ
- จุดเปลี่ยนจาก "fit a model" เป็น "run a disciplined research program"
- Prerequisites: Ch11 forecasting discipline, causality gates, baseline philosophy

### 12.2 Regularization จาก First Principles
- **Hypothesis space restriction**: จำกัด set ของ functions ที่ model สามารถ represent ได้
- **Channels**: Explicit penalties (Ridge/Lasso/Elastic Net) + Implicit constraints (early stopping, dropout, feature reduction)
- **Stability under correlated predictors**: Features มัก correlate → unconstrained estimation unstable
- **Penalized risk minimization**: L(θ; D) + λΩ(θ) — λ เป็น governance knob ไม่ใช่ nuisance constant
- **Bias-variance as operational diagnostic**: Training-window sensitivity, split-boundary sensitivity, feature-set perturbation

### 12.3 Canonical Penalties
- **Ridge (L2)**: Shrink coefficients, smooth exposure, handles multicollinearity
- **Lasso (L1)**: Sparsity — ผลัก coefficients ไปที่ 0, selection instability ภายใต้ time variation
- **Elastic Net**: ผสม L1+L2 — ลด instability ของ Lasso เมื่อ correlated
- **Structured penalties**: Block/group penalties สำหรับ feature families

### 12.4 Hyperparameters เป็นส่วนหนึ่งของ Model
- **Hyperparameters** = ทุก choice ที่ researcher เลือกซึ่งส่งผลต่อ forecasts
- 4 layers: feature layer, model-fitting layer, decision layer, evaluation layer
- **Search spaces**: Mixed, discrete, conditional, constrained — ไม่ใช่ continuous box
- **Sensitivity and fragility maps**: Map _REGION_ ที่ performance stable, ไม่ใช่ find best point
- **Tuning budgets = credibility budgets**: ลองมาก = ต้อง skeptical มากขึ้น, maintain trial ledger

### 12.5 Validation Under Time
- **Random CV ปกติ invalid**: Leaks future info ผ่าน overlapping labels, rolling features, preprocessing leakage
- **Holdout + walk-forward**: Simulate deployment — train on past, test on future
- **Purging**: ลบ training observations ที่ label window overlap กับ test period
- **Embargo**: เพิ่ม buffer zone ระหว่าง training และ test
- **Metric choice ต้อง match decision**: MSE สำหรับ regression, IC สำหรับ ranking, directional accuracy สำหรับ direction

### 12.6 Model Selection Procedures
- **Single split**: Minimal defensible protocol — train/val/test chronological, tune on val only, evaluate on test once
- **Time-series CV**: Blocked CV หรือ Rolling CV — produce stability picture across folds
- **Nested selection**: Outer loop = OOS evaluation, Inner loop = hyperparameter tuning — protect evaluation from tuning bias
- **Early stopping**: Model-agnostic regularization — stop training เมื่อ validation หยุด improve

### 12.7 Selection Bias
- **Multiple comparisons**: ลองมากพอ = มี winner จาก luck
- **Hidden degrees of freedom**: Preprocessing, feature engineering, target definition, validation protocol
- **Stability selection mindset**: เลือก configuration ที่ consistent ไม่ใช่ best average
- **Minimum disclosure**: จำนวน trials, degrees of freedom, split protocol, selection rule, sensitivity

---

## Chapter 13: Neural Nets และ Deep Learning ในการเทรด

### 13.1 ตำแหน่งในหนังสือ
- นั่งหลัง Ch11 (supervised forecasting) และ Ch12 (model selection governance)
- Deep learning = function class ที่ขยาย — magnifies ทุก mistake ที่ practitioner ทำได้
- Objective: Enlarge hypothesis space + tighten experimental rules

### 13.2 ทำไม Neural Nets ในการเทรด และทำไมยาก
- **What deep learning adds**:
  - Nonlinear function approximation ภายใต้ experimental discipline
  - Representation learning — ลด brittle feature engineering
  - Capacity สำหรับ heterogeneous predictors
  - Multi-objective forecasting (ranking, calibration)
  - Model ensembling สำหรับ variance reduction
- **What deep learning ไม่ได้ fix**: Noise, drift, selection bias — ไม่สร้าง signal ที่ไม่มี

### 13.3 Neural Networks เป็น Composed Functions
- **MLP**: f(x) = ϕ(Wx + b) — composed affine transforms + nonlinearities
- **Universal approximation**: Can approximate any continuous function — แต่ warning label ใน finance
- **Activations**: Sigmoid (saturation), tanh (zero-centered), ReLU (non-saturating, sparse)
- **Loss functions**: MSE/Huber (regression), BCE (classification), pairwise ranking loss
- **Capacity control**: Parameter counting, weight decay, early stopping, dropout

### 13.4 Training Dynamics
- **Optimization**: SGD/Adam — step size η (learning rate)
- **Backpropagation**: Chain rule bookkeeping — exploding/vanishing gradients
- **Normalization**: Batch norm, layer norm — stabilization
- **Initialization**: Xavier, He — ป้องกัน vanishing/exploding gradients

### 13.5 Architectures สำหรับ Market Data
- **MLP**: Baseline — flatten windowed input, learn nonlinear combinations
- **1D CNN**: Local pattern extraction — shared filters, multi-scale features
- **RNN/LSTM/GRU**: Sequential dependence — latent market state, gating mechanisms
- **Transformers**: Attention — non-local dependencies, แต่ leakage trap ถ้าไม่มี causal masking
- **Architecture selection**: Start simple → escalate only when justified

### 13.6 Representing Market Information
- **Returns/log-returns as primary inputs**: More stationary than prices
- **Windowed tensors**: Shape L×p (L = window, p = features) — ต้อง maintain time ordering
- **Scaling**: Fit on training only, apply forward — causality gate
- **Label design**: Horizon, overlapping labels, decision time vs label time

### 13.7 Time-Aware Validation
- **Walk-forward evaluation**: Same as Ch12 — chronological splits
- **Embargo logic**: Gap between training and test สำหรับ overlapping labels
- **Nested tuning discipline**: Outer loop = OOS, Inner loop = hyperparameter tuning
- **Metrics**: MSE, sign accuracy, IC, rank IC, calibration
- **Sanity checks**: Random labels, permuted features, leakage canary, seed robustness

### 13.8 Regularization สำหรับ Neural Nets
- **Weight decay (L2)**: Penalize large weights — smoother functions
- **Dropout**: Random deactivation — implicit ensemble
- **Early stopping**: Stop before overfitting
- **Data augmentation**: สร้าง training examples ที่หลากหลายขึ้น

### 13.10 Governance-Native Checklist
- **Deterministic runs**: Fixed seeds, logged configs
- **Data fingerprints**: Input validation
- **Split manifests**: Time-aware splits documented
- **Normalization proofs**: Fit on training only
- **Causality assertions**: No-look-ahead gates
- **Training traces**: Loss curves, validation metrics per fold
- **Model registries**: Reproducible inference

---

## Chapter 14: Regime Detection และ HMMs

### 14.1 ตำแหน่งในหนังสือ
- Extended จาก prediction เป็น structure — non-stationarity เป็น latent state model
- Prerequisites: Ch04 (time-series anatomy), Ch11-13 (supervised learning + deep learning)

### 14.2 ทำไม "Regimes" มีอยู่ใน Trading Systems
- Markets เปลี่ยนแปลงสภาพ — trending ↔ ranging ↔ volatile ↔ calm
- Single global model ไม่ work สำหรับ non-stationary data
- Regime model ช่วยให้ conditional forecasting/risk management

### 14.3 Formal Definition ของ Regime Model
- Latent states {s_t} ที่ observe ไม่ได้ แต่ส่งผลต่อ returns distribution
- Observation model: p(y_t | s_t) — returns distribution ขึ้นอยู่กับ regime
- Transition model: p(s_t | s_{t-1}) — regime changes ตาม Markov chain

### 14.4 Hidden Markov Models (HMMs)
- **Model specification**:
  - Transition matrix A: p(s_t = j | s_{t-1} = i)
  - Emission parameters: p(y_t | s_t = k) — มัก Gaussian mixture
- **Number of states**: เลือกผ่าน BIC, AIC, cross-validation

### 14.5 Inference I: Forward Algorithm
- Filtering: p(s_t | y_{1:t}) — probability ของ current state given all observations
- Prediction: p(s_{t+1} | y_{1:t}) — forward prediction
- Forward algorithm: Recursive computation ที่ numerical stability ดี

### 14.6 Inference II: Backward Algorithm และ Smoothing
- Backward: p(y_{t+1:T} | s_t) — backward probabilities
- Smoothing: p(s_t | y_{1:T}) — probability ของ state ณ เวลา t โดยใช้ข้อมูลทั้งหมด
- Forward-backward algorithm: คำนวณ smoothed probabilities

### 14.7 Learning HMM Parameters
- Baum-Welch (EM for HMMs): Iterative parameter estimation
- Maximization step: ปรับ transition/emission parameters
- Likelihood: p(y_{1:T} | θ) — local optimum, initialization-sensitive

### 14.8 เลือก Number of Regimes
- BIC, AIC — penalize complexity
- Cross-validation — walk-forward regime selection
- Domain knowledge — 2-4 states มัก sufficient (trending, ranging, volatile, crisis)

### 14.9 Feature Design สำหรับ Regime Detection
- Returns, volatility, volume, correlations
- Regime-dependent features: Features ที่ behave differently ในแต่ละ regime

### 14.10 From Regime Beliefs สู่ Trading Decisions
- Regime-conditional signals: เลือก strategy ตาม current regime
- Regime-dependent position sizing: Higher confidence → larger positions
- Regime transition trades: ปรับพอร์ตเมื่อ regime เปลี่ยน

### 14.11 Evaluation
- In-sample fit: Log-likelihood, BIC
- Out-of-sample: Walk-forward regime prediction accuracy
- Trading performance: Sharpe, drawdowns เทียบกับ no-regime model

---

## Chapter 15: Event-Driven และ News/Sentiment Trading

### 15.1 ตำแหน่งในหนังสือ
- Information arrives irregularly, market reacts quickly
- Spurious correlation risk สูง — text data noisy, publication delays

### 15.2 ทำไม Events และ Text ต่างจาก Prices
- **Prices**: Continuous, numeric, regular intervals
- **Events/Text**: Discrete, unstructured, irregular timing, publication delays
- **Challenge**: Align event timing กับ trading decisions

### 15.3 Formal Framework สำหรับ Event-Driven Trading
- Event time vs calendar time
- Event windows: Look-back, look-forward, asymmetric windows
- Signal construction: แปลง events เป็น trading signals

### 15.4 Taxonomy ของ Events
- Earnings, macro announcements, corporate actions, news headlines
- Scheduled vs unscheduled events
- High-frequency vs low-frequency events

### 15.5 Data Sources และ Hidden Biases
- News feeds, social media, earnings transcripts
- **Biases**: Survivorship, selection, timing, source quality
- Publication delay: Time between event occurrence และ availability

### 15.6 Building an Event Log
- Event timestamping: When did it happen? When was it known?
- Event classification: Positive/negative/neutral sentiment
- Event deduplication: Multiple sources, related events

### 15.7 From Text to Numbers: Feature Engineering
- Bag-of-words, TF-IDF
- Sentiment analysis: lexicon-based, ML-based
- Named entity recognition: Which company/asset?
- Topic modeling: What topics are being discussed?

### 15.8 Event Study Mechanics
- Abnormal returns around events
- Cumulative abnormal returns (CAR)
- Cross-sectional event studies

### 15.9 Backtesting Event-Driven Signals
- Event-timing discipline: Signal time vs trade time
- Look-ahead prevention: Publication delay awareness
- Transaction cost sensitivity: Event signals อาจ concentrate ใน illiquid periods

### 15.11 Failure Modes
- **Sneak peek**: Using information ที่ published หลัง trading decision
- **Temporal misalignment**: Event timestamp ≠ availability timestamp
- **Survivorship bias**: Only surviving companies' events
- **Overfitting to events**: Event categories too specific

---

## สรุป Volume II

| Chapter | Topic | Key Contribution |
|---------|-------|-----------------|
| 11 | Supervised Learning | Causal prediction framework, target engineering, baselines, walk-forward evaluation |
| 12 | Regularization & Selection | Hyperparameter governance, time-aware validation, selection bias prevention |
| 13 | Deep Learning | Neural architectures for market data, training discipline, causality gates |
| 14 | Regime Detection | HMMs, latent state inference, regime-conditional strategies |
| 15 | Event-Driven Trading | Text/sentiment pipelines, event timing, publication delay handling |

### หลักการสำคัญที่贯穿 ทุก Chapter
1. **Causal timing**: Features ณ เวลา t ต้องคำนวณได้จากข้อมูล ≤ t เท่านั้น
2. **Walk-forward evaluation**: ทดสอบภายใต้ time-ordered splits เสมอ
3. **Governance artifacts**: Data fingerprints, feature manifests, run manifests, split protocols
4. **Leakage prevention**: Causality gates เป็น hard stops — ถ้า leak = run invalid
5. **Stability over peak performance**: เลือก model ที่ consistent across folds ไม่ใช่ highest average metric
6. **Baseline discipline**: Model ต้อง beat strong baselines under same protocol

---

✅ เสร็จแล้ว: AI_Algo_Trading_Vol2_Backtesting.md