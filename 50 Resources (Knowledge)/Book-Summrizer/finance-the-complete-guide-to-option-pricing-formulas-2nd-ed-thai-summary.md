# สรุปภาษาไทย — The Complete Guide to Option Pricing Formulas (2nd Edition)

> **ผู้แต่ง**: Espen Gaarder Haug
> **ฉบับ**: 2nd Edition
> **เนื้อหา**: 14 บท + Appendix — ครอบคลุมสูตร pricing options ทุกประเภท (vanilla, exotic, American, barrier, multi-asset, interest rate, commodity)
> **ความหนา**: ~550 หน้า, 227 chunks

---

## ภาพรวมของหนังสือ

หนังสือเล่มนี้คือ **"คัมภีร์สูตร pricing options"** ที่ครอบคลุมมากที่สุดเล่มหนึ่งในวงการ finance รวบรวมสูตร pricing ทุกรูปแบบตั้งแต่ Black-Scholes-Merton จนถึง exotic options ที่ซับซ้อนที่สุด แต่ละสูตรมาพร้อมตัวอย่างคำนวณจริง, code VBA, และตารางเปรียบเทียบค่า option จากหลาย model

Haugh ย้ำว่า: สูตร formula ที่ดีที่สุดคือ **"ง่ายที่สุดเท่าที่จะเป็นไปได้ แต่ไม่เรียบง่ายเกินไป"** (as simple as possible, but not simpler)

---

## บทที่ 1: Black-Scholes-Merton (BSM) — ฐานรากของทุกสูตร

### 1.1 สูตร BSM ดั้งเดิม (Black & Scholes 1973)

สมมติฐาน: ราคาหุ้น follow geometric Brownian motion (GBM)
```
dS = μS dt + σS dz
```
- **μ** = expected rate of return
- **σ** = volatility ของราคา
- **dz** = Wiener process

**สูตร European Call/Put:**
```
c = S·N(d₁) - X·e^(-rT)·N(d₂)
p = X·e^(-rT)·N(-d₂) - S·N(-d₁)

d₁ = [ln(S/X) + (r + σ²/2)T] / (σ√T)
d₂ = d₁ - σ√T
```

**ตัวอย่าง**: S=60, X=65, T=0.25, r=8%, σ=30% → Call = 2.1334

**สำคัญ**: ค่า Nobel Prize ของ Scholes & Merton (1997) ไม่ได้ให้เพราะสูตร — แต่ให้เพราะ **วิธี derivation** ผ่าน replicating portfolio argument และ continuous-time dynamic delta hedging

### 1.2 รูปแบบต่างๆ ของ BSM

| รูปแบบ | Cost-of-carry (b) | ใช้สำหรับ |
|---------|-------------------|-----------|
| Black-Scholes (1973) | b = r | หุ้นไม่จ่าย dividend |
| Merton (1973) | b = r - q | หุ้นจ่าย continuous dividend q |
| Black-76 (1976) | b = 0 | Futures options |
| Asay (1982) | b=0, r=0 | Margined futures options |
| Garman-Kohlhagen (1983) | b = r - r_f | Currency options |

**Generalized BSM formula:**
```
c = Se^((b-r)T)·N(d₁) - X·e^(-rT)·N(d₂)
p = X·e^(-rT)·N(-d₂) - Se^((b-r)T)·N(-d₁)
```

### 1.3 Parities และ Symmetries

**Put-Call Parity:**
```
c = p + Se^(-qT) - Xe^(-rT)   (หุ้นจ่าย dividend)
c = p + S - Xe^(-rT)           (หุ้นไม่จ่าย dividend)
c = p + (F-X)e^(-rT)           (futures)
c = p + Se^(-r_f·T) - Xe^(-rT) (currency)
```

**At-the-Money Forward Symmetry**: Put = Call เมื่อ S = Xe^(-bT) (ATM forward)

**Put-Call Symmetry (Bates 1991, Carr 1994):**
```
c(S, X) = Se^(bT) · p(S, (Se^(bT))²/X)
```
→ Call strike X = Put strike (Se^(bT))²/X ที่ให้ค่าเท่ากัน (ใช้ hedge barrier options)

**Put-Call Supersymmetry:**
```
c(S, X, T, r, b, σ) = -p(S, X, T, r, b, -σ)
```
→ ค่า Call = ค่า Put ที่ใช้ negative volatility → **ไม่ต้องเขียน code แยก Put/Call**

**BSM on Variance Form**: เขียนใหม่โดยใช้ variance V=σ² แทน volatility → ใช้ hedging variance swaps

### 1.4 ก่อน BSM — โมเดลโบราณ

| ผู้สร้าง | ปี | สมมติฐาน | ข้อจำกัด |
|----------|-----|----------|----------|
| **Bachelier** | 1900 | Normal distribution (arithmetic BM) | ราคาหุ้นติดลบได้ |
| **Sprenkle** | 1964 | Lognormal + drift | risk aversion parameter กำกวม |
| **Boness** | 1964 | Lognormal + expected return | ไม่ risk-neutral |
| **Samuelson** | 1965 | GBM + positive drift | w > p (option return > stock return) |

### 1.5 วิธี Derivation ที่ Robust กว่า — Derman-Taleb Method

- **ไม่ใช้** continuous-time delta hedging (fragile ในทางปฏิบัติ)
- **ไม่ใช้** CAPM
- ใช้ **pure arbitrage argument**: forward price = arbitrage-free (ไม่ขึ้นกับ drift) แล้วใช้ put-call parity → ได้ risk-neutral valuation
- **Robust กับ volatility smile** — เพราะเป็น relative value arbitrage ไม่ใช่ strong-form replication

**สำคัญ**: ในทางปฏิบัติ trader ใช้ option formulas โดย hedge risk ด้วย option อื่นๆ ก่อน แล้วค่อย dynamic delta hedging ทีหลัง → ต้องคำนึง **supply and demand ของ options** ด้วย ไม่ใช่แค่ underlying process

### 1.6 Black-Scholes PDE

```
∂c/∂t + (1/2)σ²S²·∂²c/∂S² + b·S·∂c/∂S = r·c
```
- Boundary condition: c = max(S-X, 0) ที่ expiration
- แก้ PDE ได้ closed-form → ได้ BSM formula
- หรือแก้ numerical (Chapter 7)

---

## บทที่ 2: BSM Greeks — ความอ่อนไหวของราคา option

Greeks คือ partial derivatives ของ option price ต่อ parameter ต่างๆ

### 2.1 Delta Greeks

| Greek | สัญลักษณ์ | ความหมาย |
|-------|-----------|----------|
| **Delta** | Δ | ความอ่อนไหวต่อราคา underlying |
| **Elasticity (Lambda)** | Λ | % change ของ option ต่อ % change ของ underlying |
| **DdeltaDvol (Vanna)** | ∂²c/∂S∂σ | Delta เปลี่ยนตาม volatility |
| **Charm (DdeltaDtime)** | ∂Δ/∂T | Delta เปลี่ยนตามเวลา (delta decay) |

**Delta Formula:**
```
Δ_call = e^((b-r)T) · N(d₁)    > 0
Δ_put  = e^((b-r)T) · [N(d₁)-1] < 0
```

**Delta อาจ > 1 ได้** ถ้า b > r (เช่น commodity options ที่ cost-of-carry สูง) → delta เรียกว่า "spot delta"

**Delta Mirror Strikes** (ใช้ทำ delta-neutral strategies):
```
X_call = S²·e^((2b+σ²)T) / X_put
```

**Strike from Delta** (OTC market quote เป็น delta ไม่ใช่ strike):
```
X_call = S · exp[-N⁻¹(Δ_call·e^((r-b)T))·σ√T + (b+σ²/2)T]
```

**Futures Delta:**
```
Δ_futures = Δ_spot · e^(-bT)
```

### 2.2 Gamma Greeks

| Greek | ความหมาย |
|-------|----------|
| **Gamma** | ∂²c/∂S² — convexity ของ option |
| **DgammaDvol (Zomma)** | Gamma เปลี่ยนตาม volatility |
| **Speed** | ∂³c/∂S³ — rate of change ของ gamma |
| **Colour** | ∂Γ/∂T — gamma decay ตามเวลา |

**Gamma Formula:**
```
Γ = e^((b-r)T) · n(d₁) / (S·σ·√T)
```

**Gamma คือ measure ของ convexity** — option price เปลี่ยน non-linearly ตาม underlying

### 2.3 Vega Greeks

| Greek | ความหมาย |
|-------|----------|
| **Vega** | ∂c/∂σ — ความอ่อนไหวต่อ volatility |
| **Vomma (Volga)** | ∂²c/∂σ² — convexity ของ vega |
| **Ultima** | ∂³c/∂σ³ |
| **Vega Bleed** | ∂²c/∂σ∂T — vega decay ตามเวลา |

**Vega-Gamma Relationship:**
```
Vega = Γ · σ · S² · T
```
→ ถ้ารู้ gamma แล้ว คำนวณ vega ได้ทันที

**Global Max Vega** เกิดขึ้นที่ T = 1/(2r) เมื่อ S = X

### 2.4 Theta Greeks

| Greek | ความหมาย |
|-------|----------|
| **Theta call** | ∂c/∂T — time decay |
| **Theta put** | ∂p/∂T — time decay |
| **Driftless theta** | Theta โดยไม่มี drift term |

Theta มักจะ **ติดลบ** (=time decay) แต่ deep ITM put อาจมี theta เป็นบวกได้

### 2.5 Rho Greeks

| Greek | ความหมาย |
|-------|----------|
| **Rho call** | ∂c/∂r — ความอ่อนไหวต่อ interest rate |
| **Rho put** | ∂p/∂r |
| **Phi (Rho-2)** | ∂c/∂b — ความอ่อนไหวต่อ cost-of-carry |

### 2.6 Strike Greeks

- **Strike Delta** = ∂c/∂X — ความอ่อนไหวต่อ strike price
- **Risk Neutral Density** = ∂²c/∂X² = n(d₂)·e^(-rT) / (X·σ·√T)

### 2.7 Numerical Greeks

ใช้ finite difference method:
- **Delta** = [c(S+ΔS) - c(S-ΔS)] / (2ΔS) — central difference
- **Gamma** = [c(S+ΔS) - 2c(S) + c(S-ΔS)] / (ΔS²)
- **Vega** = [c(σ+0.01) - c(σ-0.01)] / 2

⚠️ **Closed-form approximation อาจแม่นสำหรับ price แต่ไม่แม่นสำหรับ Greeks** โดยเฉพาะ higher-order Greeks (speed, color) → ใช้ numerical Greeks จะดีกว่า

---

## บทที่ 3: Analytical Formulas สำหรับ American Options

### 3.1 Barone-Adesi and Whaley (BAW) Approximation

- **วิธี**: แยก American option value = European value + early exercise premium
- **ใช้**: Newton-Raphson iteration หา critical stock price S* ที่ optimal exercise
- **ข้อจำกัด**: แม่นพอสำหรับใช้งานจริง แต่ไม่ใช่ exact solution

**BAW American Call Approximation (2002 improved version)** ใช้ time-splitting technique (split time into t₁ = τ(√5-1)/2) → แม่นกว่า BAW ดั้งเดิม

### 3.2 Bjerksund-Stensland (2002) Approximation

- ใช้ **two-layer exercise boundary** (S₁, S₂)
- แม่นกว่า BAW สำหรับ many cases
- **เป็นที่นิยมมากใน practice** — เป็น standard สำหรับ American option pricing

### 3.3 Put-Call Transformation สำหรับ American Options

ถ้ามีสูตร American Call อยู่แล้ว → คำนวณ American Put ได้ทันที:
```
P(S, X, T, r, b, σ) = C(X, S, T, r-b, -b, σ)
```
→ **สลับ S กับ X, เปลี่ยน sign ของ b**

### 3.4 American Perpetual Options

Options ที่ไม่มีวันหมดอายุ → มี **closed-form solution** เพราะ time to maturity ไม่เปลี่ยน:

**Perpetual Call:**
```
c = (X/(γ₁-1)) · (S*/X)^γ₁
γ₁ = (1/2 - b/σ²) + √((b/σ² - 1/2)² + 2r/σ²)
```
ถ้า b ≥ r → ไม่ exercise call ก่อนหมดอายุ

**Perpetual Put:**
```
p = (X/(1-γ₂)) · (S**/X)^γ₂
```

---

## บทที่ 4: Exotic Options — Single Asset

### 4.1 Variable Purchase Options (VPO)

- ซื้อหุ้นได้จำนวนหนึ่ง (ไม่ใช่固定จำนวน) ที่ราคา X
- จำนวนหุ้น N = ST(1-D)/X (D = discount, 0<D<1)
- มี cap (U) และ floor (L)
- **มี closed-form solution** ด้วย cumulative normal distribution

### 4.2 Chooser Options

- ผู้ถือเลือกได้ภายหลังว่าจะเป็น Call หรือ Put
- **Simple chooser**: เลือกได้ครั้งเดียว
- **Complex chooser**: เลือกได้หลายครั้ง, strike ต่างกัน

### 4.3 Forward Starting Options

- Strike ถูก fix ในอนาคต (ไม่ใช่ตอนซื้อ)
- **Ratchet option (cliquet)**: series ของ forward-starting options, strike = fixed × ราคา ณ วัน exercise ก่อนหน้า
- **Reset strike option**: strike reset เป็นราคาปัจจุบัน ถ้า underlying ไปถึงระดับหนึ่ง

### 4.4 Barrier Options (สำคัญมาก!)

Barrier options มี 4 ประเภทหลัก:

| Type | Knock-in | Knock-out |
|------|----------|-----------|
| **Down-and-In** (cdi) | มีชีวิตเมื่อราคาต่ำกว่า H | — |
| **Down-and-Out** (cdo) | — | ตายเมื่อราคาต่ำกว่า H |
| **Up-and-In** (cui) | มีชีวิตเมื่อราคาสูงกว่า H | — |
| **Up-and-Out** (cuo) | — | ตายเมื่อราคาสูงกว่า H |

**In-Out Parity:**
```
Vanilla = Knock-in + Knock-out
```
→ Barrier option price คำนวณได้จาก vanilla minus barrier counterpart

**Standard Barrier Formulas** (Merton, 1973) — มี closed-form solution สำหรับ European barrier options

**American Barrier Options:**
- Down-and-In Call (H < X): `Cdi = (H/S)^(2b/σ²) · C(S*(H²/S²))`
- ใช้ reflection principle (Haug 2001)
- **In-Out parity ไม่ holds สำหรับ American barrier options** (ต่างจาก European)

### 4.5 Lookback Options

- Payoff ขึ้นอยู่กับ **สูงสุด/ต่ำสุด** ของราคาตลอดอายุ option
- **Fixed strike lookback**: max/min price เป็น payoff
- **Floating strike**: strike = max/min price → payoff = S_T - min หรือ max - S_T

### 4.6 Asian Options

- Payoff ขึ้นอยู่กับ **average price** ของ underlying
- **Fixed strike Asian**: max(Average - X, 0) หรือ max(X - Average, 0)
- **Floating strike Asian**: max(S_T - Average, 0) หรือ max(Average - S_T, 0)
- **ถูกกว่า vanilla options** เพราะ payoff มี variance ต่ำกว่า

### 4.7 Exchange Options

- Payoff = max(S₁ - S₂, 0) — option สลับสินทรัพย์ 2 ตัว
- **Margrabe (1978) formula** — เป็น BSM formula ที่ underlying = ratio S₁/S₂

---

## บทที่ 5: Exotic Options — Two Assets

### 5.1 Relative Outperformance Options

- Call: max(S₁/S₂ - X, 0) — สินทรัพย์ A ชนะ B มากแค่ไหน
- Put: max(X - S₁/S₂, 0)
- **ใช้ Black-76 formula** กับ forward price และ volatility ของ ratio

### 5.2 Product Options

- Payoff = max(S₁·S₂ - X, 0)
- Volatility ของ product = √(σ₁² + σ₂² + 2ρσ₁σ₂)

### 5.3 Two-Asset Correlation Options

- Call: จ่าย max(S₂ - X₂, 0) ถ้า S₁ > X₁, ไม่งั้นจ่าย 0
- Put: จ่าย max(X₂ - S₂, 0) ถ้า S₁ < X₁, ไม่งั้นจ่าย 0
- **ใช้ bivariate normal distribution** M(a, b; ρ)

### 5.4 Spread Options

- Payoff = max(S₁ - S₂ - X, 0)
- **Kirk's approximation** เป็นที่นิยม — เขียนเป็น Black-76 formula ที่มี adjustment

### 5.5 Best-of / Worst-of Options

- **Best-of**: จ่าย max(S₁, S₂, ..., Sₙ) - X
- **Worst-of**: จ่าย min(S₁, S₂, ..., Sₙ) - X
- **Basket options**: payoff ขึ้นอยู่กับ weighted average ของหลายสินทรัพย์

---

## บทที่ 6: BSM Adjustments and Alternatives

### 6.1 Delayed Settlement

BSM ดั้งเดิมสมมติ immediate settlement → ปรับเป็น:
- **t₁** = เวลาจนกว่าจะจ่ายค่า option
- **T₂** = เวลาจนกว่าจะ settle payoff (ถ้า in-the-money)
- สำคัญมากเมื่อ short-term rate สูงกว่า long-term rate

### 6.2 Trading Day Volatility (French 1984)

- Volatility สูงกว่าในวันเทรด (vs วันหยุด)
- ปรับ T เป็น " trading days / trading days per year" แทน calendar days

### 6.3 Discrete Hedging (Leland 1985)

- BSM สมมติ continuous hedging → 但在 practice hedging เป็น discrete
- **Transaction costs** ทำให้ optimal hedging frequency มี finite
- ค่า hedging ขึ้นอยู่กับ bid-ask spread

### 6.4 CEV Model (Cox & Ross 1976)

```
dS = μS dt + σS^β dz
```
- β < 1 → volatility สูงขึ้นเมื่อราคาต่ำ (skew ธรรมชาติ)
- β = 1 → BSM (lognormal)
- **ใช้ Bessel function** ในการคำนวณ

### 6.5 Jump-Diffusion (Merton 1976, Bates 1991)

```
dS = (μ - λk)S dt + σS dz + J·dN
```
- J = size of jump (lognormal)
- N = Poisson process (λ = expected jumps per year)
- **จับ downside gaps** ที่ BSM ไม่สามารถจับได้

### 6.6 Stochastic Volatility (Hull & White 1987, 1988)

- σ เปลี่ยนแปลงตาม stochastic process
- **Taylor series expansion** → approximation ของ option value
- จับ volatility smile/skew

### 6.7 SABR Model

- Stochastic Alpha, Beta, Rho
- **เป็นที่นิยมมากใน interest rate derivatives**
- จับ volatility surface ได้ดี

### 6.8 Variance/Volatility Swaps

- **Variance swap**: swap variance จริง vs variance strike
- **Volatility swap**: swap volatility จริง vs vol strike
- Hedging ด้วย strip ของ vanilla options

---

## บทที่ 7: Trees and Finite Difference Methods

### 7.1 Binomial Tree (Cox-Ross-Rubinstein)

**พารามิเตอร์ CRR:**
```
u = e^(σ√Δt)    (up factor)
d = 1/u           (down factor)
p = (e^(bΔt) - d) / (u - d)  (risk-neutral probability)
```

**uropean Option Pricing:**
```
c = e^(-rT) · Σ [C(n,i) · p^i · (1-p)^(n-i) · max(Su^i·d^(n-i) - X, 0)]
```

**ข้อดี:**
- คำนวณ American options ได้ (เปรียบเทียบ early exercise value กับ continuation value)
- เข้าใจง่าย, implement ง่าย

**ข้อเสีย:**
- ช้าถ้า n (time steps) มาก
- Barrier options ต้องปรับให้ barrier fall ตรง nodes

### 7.2 Trinomial Tree

- 3 ทางเลือกต่อ step: up, stay, down
- **flexible กว่า binomial** — barrier options จัดการง่ายกว่า
- แต่ computational cost สูงกว่า

### 7.3 Implied Trees (Implied Binomial / Trinomial)

- **Calibrate จาก market prices** ของ vanilla options
- จับ volatility smile ได้
- ใช้ Arrow-Debreu prices

### 7.4 Finite Difference Methods

| Method | สูตร | ความเสถียร |
|--------|------|-----------|
| **Explicit FD** | คำนวณ value ปัจจุบันจาก future nodes โดยตรง | เสถียรถ้า Δt พอเล็ก |
| **Implicit FD** | แก้ system of equations backward | เสถียรกว่า explicit |
| **Crank-Nicholson** | เฉลี่ย explicit + implicit | แม่นกว่า, ใช้กัน widest |

### 7.5 Stochastic Volatility Trees

- Trees ที่ volatility เปลี่ยนแปลงได้
- ใช้ calibration กับ market prices

---

## บทที่ 8: Monte Carlo Simulation

### 8.1 Standard Monte Carlo

**ขั้นตอน:**
1. จำลอง path ของราคา S ด้วย GBM
2. คำนวณ payoff แต่ละ path
3. Discount เฉลี่ย → ได้ option value

**Formula:**
```
c = e^(-rT) · (1/n) · Σ max[S·e^((b-σ²/2)T + σ√T·εᵢ) - X, 0]
```

**ข้อดี:** ยืดหยุ่นมาก — ใช้ได้กับ exotic options ทุกประเภท
**ข้อเสีย:** ช้า (accuracy ∝ √n, ต้องเพิ่ม simulation 4 เท่าเพื่อเพิ่ม accuracy 2 เท่า)

### 8.2 Antithetic Monte Carlo

- สำหรับแต่ละ random number ε ใช้คู่ (-ε, +ε)
- **ลด variance** อย่างมีนัยสำคัญ
- Accuracy ดีกว่า standard MC สำหรับจำนวน simulation เท่ากัน

### 8.3 Importance Sampling (IQMC)

- เปลี่ยน probability distribution → มุ่ง simulation ไปที่ regions ที่ important
- **เร็วกว่า standard MC มาก**

### 8.4 Quasi-Random Monte Carlo

- ใช้ quasi-random sequences (Sobol, Halton) แทน pseudo-random
- **เร็วกว่า standard MC มาก** — accuracy ดีกว่าต่อจำนวน simulation

### 8.5 American Monte Carlo

- **Longstaff-Schwartz method** — ใช้ least-squares regression หา continuation value
- ช้ามากแต่ยืดหยุ่น

### 8.6 Greeks ใน Monte Carlo

- **Finite difference**: รัน MC สองครั้ง (S+ΔS, S-ΔS) → delta
- **Curran's method**: นับ path ที่ ITM → delta จาก sum of S_T
- **Pathwise derivatives** และ **Likelihood ratio method** → alternative approaches

---

## บทที่ 9: Options on Stocks with Discrete Dividends

### 9.1 European Options

**Escrowed Dividend Model:**
```
S_adj = S - D₁e^(-rt₁) - D₂e^(-rt₂) - ...
```
→ แทนที่ S ด้วย S_adj ใน BSM formula

⚠️ **ข้อจำกัด**: 低估ราคา call options เมื่อ dividend จ่ายใกล้ expiration

**Volatility Adjustments:**
- **Simple (Chriss 1997)**: σ_adj = σ·(S/(S-ΣPV(div)))
- **Haug-Haug (1998)**: ปรับ volatility แบบ piecewise — แม่นกว่า

### 9.2 American Options — Binomial Tree with Discrete Dividends

**วิธี (Standard):**
1. สร้าง binomial tree ปกติ
2. ที่ nodes ที่ dividend จ่าย → หาราคาหุ้นด้วย (1-D) ทุก node
3. backward induction → American value

**Roll-Geske-Whaley (RGW) model:**
- ใช้ compound option approach
- **มีจุดอ่อน**: escrowed dividend approximation ทำให้ mispricing → arbitrage opportunities ได้

**Important**: Binomial tree กับ discrete dividend yield → ใช้ `DiscreteDividendYield` function ที่ book ให้มา

---

## บทที่ 10: Commodity and Energy Options

### 10.1 Black-76 Model (เป็นที่นิยมมากที่สุด)

```
c = e^(-rT) [F·N(d₁) - X·N(d₂)]
```
- ใช้กับ futures options บน commodity
- **เป็น standard สำหรับ commodity options** ใน market

### 10.2 Energy Swaption

- Options บน commodity swaps
- **Adjusted Black-76** สำหรับ options on swaps

### 10.3 Miltersen-Schwartz (1998)

- Three-factor model: convenience yield + forward interest rates + spot price
- ใช้กับ commodity options ที่มี convenience yield

### 10.4 Seasonality Adjustment

- Commodity มี seasonality (น้ำมัน, ก๊าซ)
- **Pilipovie (1997)**: seasonal adjustment ที่ใช้กับ Monte Carlo

---

## บทที่ 11: Interest Rate Derivatives

### 11.1 FRAs (Forward Rate Agreements)

**FRA Rate:**
```
FRA = [(1 + r₂·T₂/Basis) / (1 + r₁·T₁/Basis) - 1] · Basis / (T₂-T₁)
```

**Convexity Adjustment (Money Market Futures):**
- Eurodollar futures มี linear payoff (ไม่มี convexity)
- FRA มี convexity payoff
- **Convexity bias** = difference ระหว่าง futures rate กับ FRA rate
- ใช้ Hull-White model ในการคำนวณ

### 11.2 Cap and Floor (Black-76)

- **Cap**: series ของ call options บน interest rate
- **Floor**: series ของ put options บน interest rate
- **Formula**: Black-76 สำหรับ caplets/floorlets

### 11.3 Swaptions

- Options บน interest rate swaps
- **Payer swaption**: สิทธิ์เข้า swap ในอัตรา fixed (จ่าย fixed, รับ floating)
- **Receiver swaption**: สิทธิ์เข้า swap ในอัตรารับ fixed
- **Modified Black-76 swaption model**

### 11.4 Bond Options

- **Black-76 for bonds**: ใช้ forward price ของ bond
- **Schaefer-Schwartz (1987)**: ปรับ volatility ตาม bond duration

### 11.5 Short Rate Models

| Model | ลักษณะ | ข้อดี |
|-------|--------|------|
| **Vasicek (1977)** | Mean reversion, normal rate | Simple, analytical |
| **Ho-Lee (1986)** | Normal rate, arbitrage-free | Calibrate กับ yield curve |
| **Hull-White (1990)** | Mean reversion + time-dependent | Flexible, arbitrage-free |
| **Black-Derman-Toy (1990)** | Lognormal rate, arbitrage-free | Fit กับ cap vol |

### 11.6 Rendleman-Barter (1980)

- Lognormal interest rate → **ไม่ arbitrage-free** (ข้อเสีย)

---

## บทที่ 12: Volatility and Correlation

### 12.1 Historical Volatility

**Close-to-Close Method:**
```
σ = StDev(ln(Close_t / Close_{t-1})) · √(N_per_year)
```

**High-Low Method (Parkinson 1980):**
```
σ = √(1/(4N·ln2) · Σ ln(High/Low)²)
```
- **มีประสิทธิภาพสูงกว่า close-to-close** มาก (ต้องการ observation น้อยกว่า)
- แต่สมมติ continuous trading

**High-Low-Close Method (Beckers 1983):**
- ใช้ทั้ง High, Low, Close → แม่นกว่า High-Low

**Rogers-Satchell (1991) & Kunitomo (1992):**
- Extended high-low methods → แม่นกว่าเดิม

### 12.2 Exponentially Weighted Moving Average (EWMA)

```
σ²_t = λ·σ²_{t-1} + (1-λ)·u²_{t-1}
```
- λ ปกติ = 0.94 (RiskMetrics)
- **ให้น้ำหนักกับข้อมูลล่าสุด** มากกว่า

### 12.3 GARCH(1,1)

```
σ²_t = ω + α·u²_{t-1} + β·σ²_{t-1}
```
- **Mean-reverting** (EWMA ไม่ mean-revert)
- Long-run variance = ω/(1-α-β)
- ใช้สำหรับ volatility forecasting

### 12.4 Implied Volatility

- **Back out volatility** จาก市场价格 ของ options
- Newton-Raphson iteration หา σ ที่ BSM price = market price
- **Volatility smile/skew**: implied vol ต่างกันสำหรับ strike ต่างกัน

### 12.5 Correlation

- **Historical correlation**: Pearson correlation ของ returns
- **Implied correlation**: back out จาก multi-asset options (basket, rainbow)
- **EWMA correlation**: exponentially weighted

---

## บทที่ 13: Distributions

### 13.1 Cumulative Normal Distribution N(x)

** Hastings Approximation (high accuracy):**
```
N(x) = 1 - n(x)·(a₁y + a₂y² + ... + a₇y⁷) / (1 + b₁y + ... + b₈y⁸)
```
- y = |x|
- 精度: double precision accuracy
- **เป็น foundation ของ BSM ทุกสูตร**

### 13.2 Standard Normal Density n(x)

```
n(x) = (1/√2π) · e^(-x²/2)
```

### 13.3 Bivariate Normal Distribution M(a, b; ρ)

- ใช้สำหรับ **two-asset options** (correlation options, best-of, worst-of)
- **Numerical integration** (Kronrod method) สำหรับคำนวณ

### 13.4 Inverse Cumulative Normal (Moro 1995)

- ใช้หา strike จาก delta → **สำคัญมากสำหรับ FX options quoting**
- Rational approximation ที่แม่นกว่า polynomial

---

## บทที่ 14: Some Useful Formulas

### 14.1 Interpolation

| Method | ใช้สำหรับ |
|--------|----------|
| **Linear** | Yield curve interpolation (ง่ายที่สุด) |
| **Log-linear** | Discount factor interpolation |
| **Exponential** | Discount factor interpolation |
| **Cubic (Lagrange)** | Smooth interpolation 4 points |
| **Cubic Spline** | Smooth interpolation ทุก points |
| **2D Interpolation** | Volatility surface (strike × maturity) |

### 14.2 Interest Rate Formulas

- **FV of Annuity**: FV = C[(1+r)^n - 1]/r
- **NPV of Annuity**: NPV = C·[1 - (1+r)^(-n)]/r
- **Continuous Compounding**: r_c = m·ln(1 + r_m/m)
- **Bond Price**: P = Σ CF_t·e^(-r_t·T_t)

### 14.3 Risk Metrics

| Metric | Formula | ใช้สำหรับ |
|--------|---------|----------|
| **Sharpe Ratio** | (r_p - r_f) / σ_p | Risk-adjusted return |
| **Sortino Ratio** | (r_p - r_f) / σ_downside | Downside risk only |
| **VaR** | Portfolio loss ที่ confidence level | Risk limit |
| **CVaR (ES)** | Expected loss beyond VaR | Tail risk |
| **Jensen's Alpha** | r_p - [r_f + β(r_m - r_f)] | Portfolio alpha |

### 14.4 Basic Math Reference

- **Greek Alphabet** (ใช้เป็น notation สำหรับ Greeks ทุกตัว)
- **Natural Logarithm identities**
- **Differentiation rules**

---

## ความสำคัญสำหรับ Algorithmic Trading

### 1. Pricing Models ที่ใช้ในระบบเทรด

| โมเดล | ใช้ในระบบเทรดอย่างไร |
|--------|----------------------|
| **BSM** | คำนวณ theoretical fair value → เปรียบเทียบกับ market price → หา edge |
| **Greeks** | Position sizing, hedging, risk management |
| **Implied Volatility** | Volatility regime detection, vol trading strategies |
| **Binomial Tree** | American option early exercise decisions |
| **Monte Carlo** | Exotic option pricing, scenario analysis |

### 2. Greeks สำหรับ Risk Management

- **Delta hedging**: maintain delta-neutral portfolio
- **Gamma scalping**: profit from gamma when underlying moves
- **Vega exposure**: volatility trading, hedging vega risk
- **Theta decay**: understanding time decay for selling strategies

### 3. Barrier Options สำหรับ Structured Products

- **Knock-in/knock-out** options ใช้ทำ structured products มากมาย
- เข้าใจ in-out parity → hedge ได้
- Barrier options ถูกกว่า vanilla → capital efficient

### 4. Volatility Models สำหรับ Vol Trading

- **EWMA/GARCH** → forecast volatility
- **Implied vs Realized volatility** → volatility spread trading
- **SABR model** → interest rate vol trading

### 5. Monte Carlo สำหรับ Exotic Strategies

- ใช้ MC simulation ทดสอบ trading strategies ที่ซับซ้อน
- **Antithetic variates** → reduce simulation noise
- **Path-dependent strategies** (Asian, lookback) ต้องใช้ MC

### 6. Numerical Methods สำหรับ Real-Time Pricing

- **Finite difference** → fast pricing สำหรับ barrier/American options
- **Implied trees** → calibrate กับ market prices → consistent pricing

---

## สรุปสูตรสำคัญที่ต้องจำ

| สูตร | ใช้เมื่อ |
|------|---------|
| BSM (Generalized) | European options ทุกประเภท |
| Put-Call Parity | ตรวจสอบ arbitrage, hedge |
| Bjerksund-Stensland | American options (fast approximation) |
| Black-76 | Futures/commodity options |
| Garman-Kohlhagen | Currency options |
| Barrier formulas | Knock-in/knock-out options |
| CRR Binomial Tree | American options (accurate) |
| Monte Carlo | Exotic options (flexible) |
| EWMA/GARCH | Volatility forecasting |
| Crank-Nicholson FD | Barrier options, American options |

---

## สรุป

หนังสือเล่มนี้คือ **reference ที่ครอบคลุมที่สุด** สำหรับ option pricing formulas ทุกประเภท:

1. **BSM** เป็น foundation — ทุกสูตรอื่น derivation จาก BSM หรือ extensions
2. **Greeks** สำคัญเท่ากับ pricing formula — ไม่เข้าใจ Greeks = ไม่เข้าใจ risk
3. **Exotic options** มีสูตร analytical หลายตัว — ไม่จำเป็นต้องใช้ MC เสมอไป
4. **Numerical methods** (trees, FD, MC) ยืดหยุ่นที่สุด — ใช้ได้กับทุก type
5. **Volatility** เป็น parameter ที่สำคัญที่สุด — ถ้า volatility ผิด = pricing ผิดทั้งหมด
6. **实践**: เลือกสูตรที่ "ง่ายที่สุดที่ยังแม่นพอ" — อย่าใช้ complex model โดยไม่จำเป็น
