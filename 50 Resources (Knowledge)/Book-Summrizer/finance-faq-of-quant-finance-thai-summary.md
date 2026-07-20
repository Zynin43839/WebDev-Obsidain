# สรุป: Frequently Asked Questions in Quantitative Finance (FAQ of Quant Finance)

**ผูเขียน:** Paul Wilmott  
**ปีที่ตีพิมพ์:** 2007 (second edition)  
**แนว:** หนังสือรวบรวม FAQ ด้าน Quantitative Finance — คลอบคลุมตั้งแต่พื้นฐานไปจนถึงหัวข้อขั้นสูง ในรูปแบบถาม-ตอบ พร้อม Timeline ประวัติศาสตร์, 10 วิธีพิสูจน์ Black-Scholes, รายการแบบจำลองและสมการ, ตาราง Greeks, ตัวอย่างสัญญา Exotic, Probability Distributions, Brainteasers สำหรับสัมภาษณ์งาน, และคำแนะนำการหางานสาย Quant

---

## สารบัญ

1. [บทที่ 1: Quantitative Finance Timeline](#บทที่-1-quantitative-finance-timeline)
2. [บทที่ 2: FAQs (คำถามที่พบบ่อย 50 ข้อ)](#บทที่-2-faqs)
3. [บทที่ 3: The Most Popular Probability Distributions](#บทที่-3-the-most-popular-probability-distributions)
4. [บทที่ 4: Ten Different Ways to Derive Black-Scholes](#บทที่-4-ten-different-ways-to-derive-black-scholes)
5. [บทที่ 5: Models and Equations](#บทที่-5-models-and-equations)
6. [บทที่ 6: Black-Scholes Formulae and the Greeks](#บทที่-6-black-scholes-formulae-and-the-greeks)
7. [บทที่ 7: Common Contracts](#บทที่-7-common-contracts)
8. [บทที่ 8: Popular Quant Books](#บทที่-8-popular-quant-books)
9. [บทที่ 9: Popular Search Words on Wilmott.com](#บทที่-9-popular-search-words-on-wilmottcom)
10. [บทที่ 10: Brainteasers](#บทที่-10-brainteasers)
11. [บทที่ 11: Paul & Dominic's Guide to Getting a Quant Job](#บทที่-11-paul--dominics-guide-to-getting-a-quant-job)

---

## บทที่ 1: Quantitative Finance Timeline

Timeline ประวัติศาสตร์การเงินเชิงปริมาณ ตั้งแต่ปี 1827 จนถึง 2002:

**1827-1923: รากฐานทางคณิตศาสตร์**
- **Brown (1827):** Robert Brown ค้นพบการเคลื่อนที่แบบสุ่มของอนุภาคในของเหลว (Brownian motion) — ต่อมากลายเป็นพื้นฐานของแบบจำลองราคาสินทรัพย์
- **Bachelier (1900):** Louis Bachelier เป็นคนแรกที่สร้างแบบจำลองทางคณิตศาสตร์ของ Brownian motion และนำไปใช้ pricing options เป็นคนแรกของโลก แต่วิชาการในยุคนั้นไม่ให้การยอมรับ
- **Einstein (1905):** ให้รากฐานทางฟิสิกส์สำหรับ Brownian motion
- **Richardson (1911):** บุกเบิก finite-difference method สำหรับแก้สมการ diffusion — ต่อมาใช้ใน option pricing
- **Wiener (1923):** พัฒนาทฤษฎี rigorous สำหรับ Brownian motion (Wiener process) — กลายเป็นหัวใจของแบบจำลองทางการเงิน

**1950s-1970s: ยุคปฏิวัติ**
- **Samuelson (1950s):** นำ Bachelier กลับมา, วางรากฐาน option pricing ผ่าน real expectations
- **Ito (1951):** Ito's lemma — ความสัมพันธ์ระหว่าง stochastic differential equation ของตัวแปรต้นกับฟังก์ชันของตัวแปรนั้น เป็นเครื่องมือสำคัญที่สุดใน derivatives pricing
- **Markowitz (1952):** Modern Portfolio Theory (MPT) — แนวคิด diversification, efficient frontier, การ optimize ความเสี่ยงกับผลตอบแทน ได้รับรางวัลโนเบล
- **Sharpe, Lintner, Mossin (1963):** Capital Asset Pricing Model (CAPM) — แบ่งความเสี่ยงเป็น specific risk (diversifiable) และ systematic risk (undiversifiable)
- **Fama (1966):** Efficient Markets Hypothesis — ราคาหุ้นสะท้อนข้อมูลที่มีอยู่ทั้งหมด, ไม่มีใครได้เปรียบ
- **Sobol', Faure, Halton, et al. (1960s):** พัฒนา quasi-random number / low-discrepancy sequence theory — ใช้ในการคำนวณ integrals หลายมิติ
- **Thorp (1968):** ใช้ Black-Scholes formula ก่อนใคร เป็นคนแรกที่สร้าง hedge fund จาก quantitative finance
- **Black, Scholes, Merton (1973):** Black-Scholes equation — PDE สำหรับ option pricing dS = mu S dt + sigma S dX นำไปสู่สมการ `dV/dt + 0.5 sigma^2 S^2 d^2V/dS^2 + rS dV/dS - rV = 0` Scholes และ Merton ได้รับโนเบล (Black เสียชีวิตก่อน)

**1977-1990s: การขยายตัว**
- **Merton (1974):** Structural approach to credit risk — มูลค่าบริษัทเป็น call option บนสินทรัพย์, หนี้ = strike price
- **Boyle (1977):** Monte Carlo method สำหรับ option pricing — สุ่ม path ราคาสินทรัพย์ ดู average payoff
- **Vasicek (1977):** แบบจำลอง interest rate แรก — short-term rate เป็น random walk
- **Cox, Ross, Rubinstein (1979):** Binomial model — ทำให้ option pricing เข้าถึงได้สำหรับ MBA โดยใช้แค่ +, -, x, /
- **Harrison, Kreps, Pliska (1979-1981):** เชื่อมโยง option pricing กับ martingale theory และ advanced probability
- **Ho and Lee (1986):** Yield curve fitting / calibration — ทำให้แบบจำลองตรงกับราคาพันธบัตรในตลาด
- **Heath, Jarrow, Morton (1992):** HJM model — modeled การเคลื่อนที่สุ่มของ yield curve ทั้งเส้น

**1994-2002: นวัตกรรมล่าสุด**
- **Dupire, Rubinstein, Derman & Kani (1994):** Local volatility model — สร้าง volatility surface ที่สอดคล้องกับ market prices (แม้จะมีข้อถกเถียงทางวิทยาศาสตร์)
- **Avellaneda & Paras (1996):** Uncertain volatility model — nonlinear option pricing, ผลรวมของ portfolio ติดลบ ไม่เท่ากับผลรวมทีละตัว มาพร้อม static hedging
- **Brace, Gatarek, Musiela (1997):** BGM/LIBOR Market Model — interest rate model ที่ใช้ discrete set of rates (ที่ซื้อขายได้จริง) แทน continuous forward rates
- **Li (2000):** Copula approach สำหรับ CDO pricing — รวม default models ของบริษัทเดี่ยวๆ เป็น joint default model
- **Hagan, Kumar, Lesniewski, Woodward (2002):** SABR model (stochastic alpha, beta, rho) — asymptotic approximation ที่แม่นยำสำหรับ pricing

---

## บทที่ 2: FAQs

บทนี้คือหัวใจของหนังสือ — 50 คำถาม-ตอบ คลอบคลุมหัวข้อสำคัญทั้งหมดใน quantitative finance:

### หมวดคณิตศาสตร์ที่ใช้ใน Quantitative Finance (FAQ 1)
- **Probabilistic approach:** สมมติว่าราคาสินทรัพย์เป็น random, ใช้ stochastic calculus
- **Deterministic approach:** สมมติว่าแบบจำลองทำนายอนาคตได้ (chaos theory, dynamical systems) — ไม่ประสบความสำเร็จในโลกการเงิน
- **Discrete/Continuous:** discrete = binomial model, continuous = Black-Scholes PDE
- **Tools:** simulations, finite-difference, approximations, asymptotic analysis, Green's functions

### Arbitrage และ Put-Call Parity (FAQ 2-3)
- **Arbitrage:** การทำกำไรไร้ความเสี่ยง — พื้นฐานของ classical finance theory
- **Static vs Dynamic arbitrage:** static ไม่ต้อง rebalance, dynamic ต้องปรับพอร์ตตาม market states
- **Model-independent vs Model-dependent:** put-call parity = model-independent (ไม่ต้องใช้แบบจำลองราคาหุ้น)
- **Put-Call Parity:** `C - P = S - K e^{-r(T-t)}` — ถ้าถูกละเมิด = arbitrage opportunity

### Central Limit Theorem (FAQ 4)
- ผลรวมของ random numbers จำนวนมากจะแจกแจงแบบ normal (Gaussian)
- ข้อแม้: ตัวเลขต้อง i.i.d., มี finite mean และ variance
- ถ้า variance เป็นอนันต์ จะเข้า stable Levy distribution แทน
- การคูณ random numbers → lognormal distribution (สำคัญสำหรับราคาหุ้น)

### Risk Definition (FAQ 5-8)
- **Market risk, Credit risk, Model risk, Operational risk, Legal risk**
- **Knightian distinction:** Risk = รู้ probability distribution, Uncertainty = ไม่รู้
- **Value at Risk (VaR):** การขาดทุนสูงสุดที่ confidence level ใดๆ (e.g., 95%, 99%) — นิยมใช้แต่มีข้อเสียคือไม่บอกขนาด loss ที่เกิน VaR และไม่ใช่ coherent risk measure
- **CrashMetrics:** stress-testing สำหรับ extreme market moves — ใช้ crash coefficient (kappa) แทน correlation
- **Coherent Risk Measure:** ต้องมี 4 คุณสมบัติ — sub-additivity, monotonicity, positive homogeneity, translation invariance — VaR แบบดั้งเดิมไม่ผ่าน sub-additivity

### Modern Portfolio Theory และ CAPM (FAQ 9-10)
- **MPT (Markowitz):** 2N + N(N-1)/2 parameters (expected return, std dev, correlation)
- **Efficient Frontier:** เส้นที่ให้ expected return สูงสุดสำหรับแต่ละระดับความเสี่ยง
- **Capital Market Line:** เมื่อเพิ่ม risk-free asset, efficient frontier กลายเป็นเส้นสัมผัส
- **CAPM (Sharpe):** `Ri = alpha_i + beta_i * Rm + epsilon_i` — แบ่งเป็น systematic risk (beta) และ specific risk (epsilon) — specific risk diversifiable ได้

### Arbitrage Pricing Theory (FAQ 11)
- **APT (Ross):** แทน returns ด้วย linear combination ของ multiple random factors (macroeconomic หรือ statistical)
- แตกต่างจาก CAPM: APT ใช้ approximate arbitrage argument, ไม่ใช่ equilibrium
- ตัวอย่าง factors: index level, GDP growth, interest rate, default spread, exchange rate

### Maximum Likelihood Estimation (FAQ 12)
- MLE = เลือก parameters ที่ maximize probability ของ outcomes ที่เกิดขึ้นจริง
- ตัวอย่าง: ประมาณ volatility จาก historical returns โดยหา sigma ที่ maximize joint probability

### Cointegration (FAQ 13)
- Stationarity = time series มี mean, std dev, autocorrelation คงที่
- Cointegration = linear combination ของ non-stationary series ให้ผลลัพธ์เป็น stationary
- ใช้ใน pairs trading, index tracking (15 stocks cointegrated with S&P500)
- Granger causality: ตัวแปรหนึ่งนำก่อน อีกตัวแปรตามหลัง

### Kelly Criterion (FAQ 14)
- Maximize expected growth of wealth โดยการเดิมพัน fraction `f = (mu)/(sigma^2)`
- ตัวอย่าง: coin ที่มี bias → bet `f = 2p - 1`
- Kelly fraction อาจทำให้ portfolio ผันผวนสูง แนะนำ half-Kelly

### Hedging (FAQ 15)
- **Model-independent:** put-call parity, spot-forward parity
- **Model-dependent:** Black-Scholes delta hedging
- **Delta hedging:** perfect elimination ของ risk ถ้า hedge ต่อเนื่อง
- **Gamma hedging:** hedge second-order effects — ลดความถี่ในการ rebalance
- **Vega hedging:** hedge ความเสี่ยงจาก volatility
- **Static hedging:** ใช้ liquid traded contracts ลด model risk
- **Superhedging:** ใน incomplete markets — สร้าง portfolio ที่มี positive payoff เสมอ
- **Margin hedging:** ป้องกัน margin call
- **Platinum (Crash) hedging:** CrashMetrics — robust hedge สำหรับ extreme markets

### Marking to Market (FAQ 16)
- Mark-to-market = valuation ที่ราคาตลาดปัจจุบัน (regulatory requirement)
- Mark-to-model = valuation จากแบบจำลอง (สำหรับ OTC contracts)
- Impact: hedging ต้องเลือกว่าจะใช้ implied volatility หรือ forecast volatility

### Efficient Markets Hypothesis (FAQ 17)
- ราคาสะท้อนข้อมูลทั้งหมด → ไม่มีใครได้เปรียบ
- 3 รูปแบบ: weak, semi-strong, strong

### Performance Measures (FAQ 18)
- Sharpe ratio, Sortino ratio (ใช้ semi-variance), Information ratio

### Utility Function (FAQ 19)
- Jensen's Inequality: สำหรับ convex function, E[f(X)] > f(E[X])
- อธิบายว่า convexity มี value ใน derivatives อย่างไร

### Brownian Motion และ Ito's Lemma (FAQ 20-22)
- **Brownian Motion:** dX ~ N(0, dt)
- **Ito's Lemma:** `dF = (dF/dX)dX + 0.5(d^2F/dX^2)dt`
- **Risk-neutral valuation:** เปลี่ยน drift จาก mu เป็น r (Girsanov's theorem)

### Greeks (FAQ 25-26)
- **Delta:** sensitivity ต่อ underlying
- **Gamma:** second-order sensitivity
- **Theta:** time decay
- **Vega:** sensitivity ต่อ volatility
- **Rho:** sensitivity ต่อ interest rate
- Closed-form solutions = faster calibration, more intuition

### Numerical Methods (FAQ 27-30)
- **Monte Carlo:** ดีสำหรับ high-dimensional problems, path-dependent options
- **Finite-difference:** ดีสำหรับ low-dimensional, American options, embedded decisions
- **Binomial tree:** intuitive, ใช้สำหรับ American options

### Jump-Diffusion, Complete/Incomplete Markets (FAQ 31-32)
- Jump-diffusion: เพิ่ม Poisson process สำหรับ jumps → incomplete market
- Complete market = ทุก derivative สามารถ replicated ได้ (=> unique price)

### Volatility, Smile, GARCH (FAQ 33-35)
- **Volatility smile:** implied volatility ไม่คงที่ เปลี่ยนตาม strike (skew/flat smile)
- **GARCH:** time-varying volatility model — conditional variance ตอบสนองต่อ past returns

### Dispersion Trading, Bootstrapping, LMM (FAQ 37-39)
- **Dispersion trading:** long/short vol บน single stocks vs index
- **Bootstrapping:** สร้าง discount curve จาก instrument prices
- **LIBOR Market Model:** discrete forward rate model

### คำถามที่ 40-50
- **Value of a contract:** risk-neutral expectation
- **Calibration:** ปรับ parameters ให้ model ตรงกับ market prices
- **Market price of risk:** compensation per unit risk
- **Equilibrium vs No-arbitrage approach:** CAPM = equilibrium, Black-Scholes = no-arbitrage
- **Normal distribution assumption:** financial returns มี fat tails — lognormal distribution ดีกว่า
- **Black-Scholes robustness:** ใช้งานได้ดีในหลายกรณีแต่ volatility ไม่ constant
- **Copulas:** รวม marginal distributions เป็น multivariate distribution — ใช้ใน credit derivatives
- **Asymptotic analysis:** approximate solution จาก large/small parameters (e.g., close to expiry)
- **Free-boundary problem:** American option optimal exercise boundary
- **Low discrepancy numbers:** quasi-Monte Carlo — convergence rate O(1/N) แทน O(1/sqrt(N))

---

## บทที่ 3: The Most Popular Probability Distributions

รายการ Probability Distributions ที่พบบ่อยใน quantitative finance พร้อม PDF, mean, variance:

- **Chi-square:** hedging error จาก discrete hedging
- **Gumbel / Weibull:** extreme value distributions — maximum ของ large sample
- **Student's t:** small-sample normal, fat-tail equity returns (มี finite moments แค่เมื่อ c > n)
- **Pareto:** power-law distribution สำหรับ distribution of wealth
- **Uniform, Inverse Normal, Gamma, Logistic, Laplace, Cauchy, Beta, Exponential**
- **Levy distribution:** stable distribution — sum of Levy = Levy (same alpha, beta) — tail decay |x|^(-1-alpha) — normally infinite variance

---

## บทที่ 4: Ten Different Ways to Derive Black-Scholes

10 วิธีพิสูจน์ Black-Scholes equation/formula (อ้างอิงจาก Andreason, Jensen, Poulsen 1998):

ผู้เขียนเน้นวิธีที่ 10 — "Black-Scholes for accountants" — ใช้คณิตศาสตร์น้อยที่สุด intuititive:
- Portfolio = long option + short stock
- 3 สาเหตุที่มูลค่าเปลี่ยน: (1) theta (time decay) (2) interest on bank account (3) stock move
- Stock move profit = `0.5 * sigma^2 * S^2 * Gamma * delta_t`
- No-arbitrage => `theta + 0.5*sigma^2*S^2*Gamma + r(S*Delta - V) = 0`
- ข้อดี: ไม่ต้องใช้ Itô calculus, ใช้แค่ variance ของ returns (ไม่ต้อง Normal)
- ข้อเสีย: hedging error ถ้า hedge แบบ discrete

---

## บทที่ 5: Models and Equations

รวมสมการสำคัญของแบบจำลองต่างๆ:

**Equity/FX/Commodities:**
- Lognormal random walk: `dS = mu*S*dt + sigma*S*dX`
- Black-Scholes PDE: `dV/dt + 0.5*sigma^2*S^2*d^2V/dS^2 + (r-D)*S*dV/dS - rV = 0`
- Probabilistic interpretation: `V = e^{-integral r dt} * E^Q[Payoff(ST)]`
- Formula for European options (time-dependent parameters): ใช้ root-mean-square average สำหรับ sigma

**Multi-dimensional:**
- Formula for d assets: correlation matrix, integral over d dimensions

**Stochastic Volatility:**
- `dS = mu*S*dt + sigma*S*dX1`
- `dsigma = (p - lambda*q)dt + q*dX2`
- PDE มี cross-derivative term `rho*sigma*S*q * d^2V/dS dsigma`

**Interest Rate Models:**
- **Vasicek:** short-rate model: `dr = (eta - gamma*r)dt + sqrt(alpha*r + beta)*dX` (กรณี general)
- **Bond pricing equation:** PDE คล้าย Black-Scholes
- **HJM:** modeled whole yield curve evolution (forward rates)
- **BGM/LIBOR Market Model:** discrete set of traded rates, drift determined by no-arbitrage

**Credit Risk:**
- **Merton's structural approach:** firm value = call option on assets
- **Reduced form / intensity-based:** default เป็น Poisson process (random arrival)
- **Credit spread:** ใช้ default probability + recovery rate

---

## บทที่ 6: Black-Scholes Formulae and the Greeks

ตาราง Greeks สำหรับ Call, Put, Binary Call, Binary Put:

- **N(x):** cumulative normal distribution
- **d1, d2:** `d1 = (ln(S/K) + (r-D+0.5*sigma^2)(T-t)) / (sigma*sqrt(T-t))`, `d2 = d1 - sigma*sqrt(T-t)`

**Call:**
- Value: `S*e^{-D(T-t)}*N(d1) - K*e^{-r(T-t)}*N(d2)`
- Delta: `e^{-D(T-t)}*N(d1)`
- Gamma: `e^{-D(T-t)}*N'(d1) / (sigma*S*sqrt(T-t))`
- Vega: `S*sqrt(T-t)*e^{-D(T-t)}*N'(d1)`

**Put:**
- Value: `-S*e^{-D(T-t)}*N(-d1) + K*e^{-r(T-t)}*N(-d2)`
- Delta: `e^{-D(T-t)}*(N(d1)-1)`

**คำเตือนจากผู้เขียน:** Greeks ที่เป็น partial derivative เทียบกับ parameters (sigma, r, D) ที่สมมติว่า constant อาจทำให้เข้าใจผิด โดยเฉพาะ binary options ที่ vega เปลี่ยนเครื่องหมาย

---

## บทที่ 7: Common Contracts

### 6 Features ของ Exotic Contracts
1. **Time dependence:** special dates ต้อง align discretization
2. **Cashflows:** jump conditions สำหรับ discrete cashflows
3. **Path dependence:** Strong (ต้องเพิ่ม state variable เช่น Asian options) vs Weak (ไม่ต้องเพิ่ม เช่น Barrier)
4. **Dimensionality:** 2D = vanilla, 3D = strong path-dependent หรือ second asset
5. **Order:** First-order option = option on asset, Second-order = option on option (compound)
6. **Embedded decisions:** American exercise → ต้องหา optimal strategy (finite-difference > Monte Carlo)

### รายการสัญญา
- **Accrual / Range note:** สะสม payout ตามวันที่ underlying อยู่ใน range
- **American option:** exercise เมื่อไหร่ก็ได้ — ต้องใช้ finite-difference
- **Asian option:** payoff ขึ้นกับ average — ราคาถูกกว่า vanilla เพราะ volatility ต่ำกว่า
- **Barrier option:** เกิด/ดับ เมื่อ price ถึง level ที่กำหนด
- **Basket option:** payoff ขึ้นกับหลาย underlyings
- **CDO (Collateralized Debt Obligation):** pool of debt → securitized tranches — ใช้ copula
- **CDS (Credit Default Swap):** insurance ต่อ credit event
- **CMS (Constant Maturity Swap):** convexity adjustment จำเป็น
- **Convertible bond:** bond + embedded option to convert to stock
- **Dividend swap:** สลับ dividend flow
- **Equity swap:** สลับ equity return กับ fixed/floating
- **FX option:** Quanto option (currency-protected) → correlation term
- **Swaption:** option to enter a swap
- **Total Return Swap (TRS):** transfer all risks (credit + market)
- **Variance/Volatility swap:** hedge volatility — variance swap ง่ายกว่า (one-over-strike-squared rule)

---

## บทที่ 8: Popular Quant Books

12 หนังสือยอดนิยมจาก wilmott.com bookshop (2001-2006):
- *Paul Wilmott Introduces Quantitative Finance* (introductory)
- *Paul Wilmott On Quantitative Finance 2ed* (research-level, 3 volumes)
- *Advanced Modelling in Finance Using Excel and VBA* (Jackson & Staunton)
- *Option Valuation under Stochastic Volatility* (Alan Lewis)
- *The Concepts and Practice of Mathematical Finance* (Mark Joshi)
- และอื่นๆ

---

## บทที่ 9: Popular Search Words on Wilmott.com

คำค้นหายอดนิยมบน wilmott.com พร้อมคำอธิบายสั้นๆ เช่น Arbitrage, Asian option, Barrier option, Base correlation, Basket, Bermudan swaption, Calibration, Callable, Cap, CDO, CDS, CFA, CMS, Convertible, Convexity, Copula, Correlation, CQF, Default probability, Delta, Digital, Dispersion, Duration, และอื่นๆ

---

## บทที่ 10: Brainteasers

ปริศนาที่ใช้ในการสัมภาษณ์งานสาย Quant:

- **Russian roulette:** spin vs not spin — ความน่าจะเป็นในการรอด
- **Matching birthdays:** ต้องมีกี่คนถึง > 50% ที่มีวันเกิดซ้ำ (23 คน)
- **Biased coins:** ความน่าจะเป็นของ odd number of heads
- **Two heads in a row:** expected waiting time = 6 tosses
- **Balls in bag (Bayesian):** conditional probability หลังจากเห็นตัวอย่าง
- **Airforce One:** probability คนสุดท้ายได้ที่นั่งตัวเอง (50%)
- **Hit-and-run taxi:** Bayes' theorem — 85% green + witness 80% correct
- และอื่นๆ (Minimum/maximum correlation, Sum of uniforms, etc.)

---

## บทที่ 11: Paul & Dominic's Guide to Getting a Quant Job

คำแนะนำสำหรับคนหางาน Quant:

1. **อยู่อย่างโดดเด่น (Stand out):** ต้องแสดงให้เห็นว่าฉลาด, ทำงานกับคนอื่นได้, ส่งงานได้จริง, จัดการเวลาได้, commitment
2. **เข้าใจกระบวนการ:** > 80% of interviews wasted on people you don't hire — ทำให้ชีวิต interviewer ง่าย
3. **CV คืออาวุธ:** ต้อง set agenda สำหรับคำถามที่อยากให้ถาม — ใส่เฉพาะจุดที่ถนัด
4. **Keyword filter:** แสดงทักษะที่ต้องการให้ชัดเจน (HR อาจไม่มีพื้นฐาน finance)
5. **ติดต่อได้เสมอ:** ใช้ Gmail, ตรวจ email สม่ำเสมอ
6. **Proofread:** ให้ native speaker ตรวจ CV — การสื่อสารสำคัญมากในสายงานนี้

---

## สรุปท้ายเล่ม

"FAQ of Quant Finance" โดย Paul Wilmott เป็นหนังสืออ้างอิงที่เหมาะสำหรับทั้งผู้เริ่มต้นและผู้มีประสบการณ์ในสาย quantitative finance เนื้อหาคลอบคลุมตั้งแต่แนวคิดพื้นฐานอย่าง arbitrage, put-call parity, MPT, CAPM ไปจนถึงหัวข้อขั้นสูงอย่าง stochastic volatility, copulas, credit derivatives, และ exotic contracts

จุดเด่นของหนังสือคือรูปแบบ FAQ ที่ช่วยให้ผู้อ่านสามารถค้นหาคำตอบสำหรับคำถามเฉพาะได้อย่างรวดเร็ว พร้อมด้วย Timeline ประวัติศาสตร์ที่แสดงวิวัฒนาการของสายงาน และ 10 วิธีพิสูจน์ Black-Scholes ที่ให้มุมมองที่หลากหลาย

สำหรับสาย algorithmic trading, เนื้อหาที่เกี่ยวข้องมากที่สุดได้แก่: cointegration (pairs trading foundation), Kelly criterion (position sizing), VaR/CrashMetrics (risk management), GARCH (volatility modeling), และ brainteasers ที่ฝึก probabilistic thinking — เหมาะสำหรับการนำไปประยุกต์ใช้กับระบบเทรดอัตโนมัติ
