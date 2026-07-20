# สรุปภาษาไทย: A Practical Guide to Quantitative Finance Interviews
**ผู้เขียน:** Xinfeng Zhou

---

## สารบัญ
1. [Chapter 2: Brain Teasers — ปัญหาเชิงตรรกะและคณิตศาสตร์](#ch2)
2. [Chapter 3: Calculus & Linear Algebra — แคลคูลัสและพีชคณิตเชิงเส้น](#ch3)
3. [Chapter 4: Probability Theory — ทฤษฎีความน่าจะเป็น](#ch4)
4. [Chapter 5: Stochastic Processes & Stochastic Calculus — กระบวนการสุ่มและแคลคูลัสสุ่ม](#ch5)
5. [Chapter 6: Finance — การเงินและการกำหนดราคาอนุพันธ์](#ch6)
6. [Chapter 7: Algorithms & Numerical Methods — อัลกอริทึมและวิธีเชิงตัวเลข](#ch7)

---

<a name="ch2"></a>
## Chapter 2: Brain Teasers — ปัญหาเชิงตรรกะและคณิตศาสตร์

### 2.1 Truth-Teller, Guards, and the Door
ปัญหาคลาสสิกเกี่ยวกับผู้พูดจริง (truth-teller) และผู้โกหก (liar) ที่ต้องค้นหาประตูที่ถูกต้องโดยถามคำถามเพียงคำถามเดียว

**วิธีแก้:** ใช้ modular arithmetic — กำหนดให้ประตูแต่ละบานมีหมายเลข 0, 1, 2 แล้วถามว่า "ถ้าถามว่าประตูหมายเลข 0 ใช่ไหม คุณจะตอบว่าใช่หรือไม่?" เพื่อให้ได้คำตอบที่เชื่อถือได้แม้จะไม่รู้ว่าใครพูดจริงใครโกหก

**หลักการสำคัญ:** การใช้ double negation (การปฏิเสธซ้ำ) ทำให้เราสามารถ "กรอง" ความเท็จออกจากคำตอบได้

### 2.2 Ants on a Square / Counterfeit Coins
**มดบนสี่เหลี่ยม:** มด 4 ตัวเดินบนสี่เหลี่ยมด้านเท่า แต่ละตัวเดินไปหามดตัวถัดไปด้วยความเร็วคงที่ คำถามคือมดจะเดินชนกันหรือไม่

**วิธีแก้:** ใช้ symmetry — เนื่องจากมดทุกตัวมีความเร็วเท่ากันและเดินในทิศทางเดียวกัน (วนรอบสี่เหลี่ยม) ตำแหน่งสัมพัทธ์ของมดทุกตัวจะคงที่ตลอดเวลา ดังนั้นมดจะไม่มีวันเดินชนกัน

**เหรียญปลอม (Counterfeit Coins):** ปัญหาการชั่งเหรียญปลอม 1 เหรียญจากหลายเหรียญโดยใช้ตาชั่งน้อยครั้งที่สุด — ใช้ binary representation ของหมายเลขเหรียญเพื่อกำหนดว่าเหรียญไหนต้องชั่งด้านซ้ายหรือขวา

### 2.3 Modular Arithmetic — Prisoner Problem
**นักโทษหมุนวงล้อ:** นักโทษ 100 คนต้องแก้ปัญหา modular arithmetic เพื่อให้รอดชีวิต — ใช้หลักการ "invariant" คือค่าคงที่ที่ไม่เปลี่ยนแปลงระหว่างการทำ operation

### 2.4 Division by 9 / Chameleon Colors
**หารด้วย 9:** กฎการหารด้วย 9 — ถ้าผลรวมของหลักหารด้วย 9 ได้เศษเท่าไหร่ ตัวเลขนั้นหารด้วย 9 ก็ได้เศษเท่านั้น

**กิ้งก่าเปลี่ยนสี:** ปัญหาเกี่ยวกับ parity (ความคู่คี่) — กิ้งก่าจะเปลี่ยนสีเมื่ออยู่ติดกับกิ้งก่าอีกตัวหนึ่ง คำถามคือกิ้งก่าจะเปลี่ยนสีได้ทุกสีหรือไม่ คำตอบคือไม่ได้ — จำนวนกิ้งก่าแต่ละสีจะมี parity คงที่

### 2.5 Math Induction — Coin Split Problem
**การแบ่งเหรียญ:** ใช้ mathematical induction พิสูจน์ว่าสามารถแบ่งเหรียญ $n$ เหรียญออกเป็น 2 กองที่ผลรวมเท่ากันได้เสมอ (ถ้า $n$ เป็นคู่)

### 2.6 Race Track Problem
ปัญหาการแข่งรถบนสนามแข่ง — ใช้ dynamic programming หรือ probability เพื่อหา optimal strategy

### 2.7 Irrational Number / Rainbow Hats
**ตัวเลขอตรรกยะ:** การพิสูจน์ว่า $\sqrt{2}$ เป็นจำนวนอตรรกยะ — ใช้ proof by contradiction

**หมวกสายรุ้ง:** ปัญหาตรรกะเกี่ยวกับหมวกสี — ใช้ pigeonhole principle

---

<a name="ch3"></a>
## Chapter 3: Calculus & Linear Algebra — แคลคูลัสและพีชคณิตเชิงเส้น

### 3.1 Limits (ขีดจำกัด)
**นิยาม ε-δ:** ฟังก์ชัน $f(x)$ มีขีดจำกัด $L$ เมื่อ $x \to a$ ก็ต่อเมื่อ สำหรับทุก $\varepsilon > 0$ จะมี $\delta > 0$ ที่ทำให้ $|f(x) - L| < \varepsilon$ เมื่อ $0 < |x - a| < \delta$

**วิธีหาขีดจำกัด:** ใช้ algebraic manipulation (แยกตัวประกอบ, rationalize, ใช้ trigonometric identity) หรือใช้ L'Hôpital's Rule

### 3.2 Derivatives (อนุพันธ์)
**นิยาม:** $f'(x) = \lim_{h \to 0} \frac{f(x+h) - f(x)}{h}$

**กฎสำคัญ:**
- **Chain Rule:** $\frac{d}{dx}[f(g(x))] = f'(g(x)) \cdot g'(x)$
- **Product Rule:** $\frac{d}{dx}[f(x)g(x)] = f'(x)g(x) + f(x)g'(x)$
- **Quotient Rule:** $\frac{d}{dx}\left[\frac{f(x)}{g(x)}\right] = \frac{f'(x)g(x) - f(x)g'(x)}{[g(x)]^2}$

**Higher-order derivatives:** $f''(x)$ คืออนุพันธ์ที่สอง ใช้บอกความโค้ง (concavity) ของฟังก์ชัน

### 3.3 L'Hôpital's Rule
เมื่อเจอ indeterminate form $\frac{0}{0}$ หรือ $\frac{\infty}{\infty}$ สามารถใช้:

$$\lim_{x \to a} \frac{f(x)}{g(x)} = \lim_{x \to a} \frac{f'(x)}{g'(x)}$$

ถ้ายังเป็น indeterminate form ก็ใช้ซ้ำได้เรื่อยๆ

**ตัวอย่าง:** $\lim_{x \to 0} \frac{\sin x}{x} = \lim_{x \to 0} \frac{\cos x}{1} = 1$

### 3.4 Integration (ปริพันธ์)
**นิยาม Riemann Integral:** $\int_a^b f(x)dx = \lim_{n \to \infty} \sum_{i=1}^n f(x_i) \Delta x$

**เทคนิคสำคัญ:**
- **Integration by Parts:** $\int u \, dv = uv - \int v \, du$
- **Substitution:** เปลี่ยนตัวแปรเพื่อให้ integral ง่ายขึ้น
- **Partial Fractions:** แยกตัวเศษส่วน

### 3.5 Applications of Integration
- **พื้นที่:** $A = \int_a^b |f(x) - g(x)| dx$
- **ปริมาตร (Disk Method):** $V = \pi \int_a^b [f(x)]^2 dx$
- **Expected Value ของตัวแปรสุ่ม:** $E[X] = \int_{-\infty}^{\infty} x f(x) dx$
- **Arc Length:** $L = \int_a^b \sqrt{1 + [f'(x)]^2} dx$

### 3.6 Taylor's Series
**Taylor Expansion รอบจุด $a$:**

$$f(x) = \sum_{n=0}^{\infty} \frac{f^{(n)}(a)}{n!}(x-a)^n$$

**Maclaurin Series (รอบจุด 0):**
- $e^x = 1 + x + \frac{x^2}{2!} + \frac{x^3}{3!} + \cdots$
- $\sin x = x - \frac{x^3}{3!} + \frac{x^5}{5!} - \cdots$
- $\cos x = 1 - \frac{x^2}{2!} + \frac{x^4}{4!} - \cdots$
- $\ln(1+x) = x - \frac{x^2}{2} + \frac{x^3}{3} - \cdots$

**Lagrange Remainder:** $R_n(x) = \frac{f^{(n+1)}(c)}{(n+1)!}(x-a)^{n+1}$ สำหรับ $c$ ระหว่าง $a$ กับ $x$

### 3.7 Ordinary Differential Equations (ODEs)
**Separable ODE:** $\frac{dy}{dx} = g(x)h(y)$ → แยกตัว $x$ กับ $y$ แล้ว integrate ทั้งข้าง

**Linear First-Order ODE:** $\frac{dy}{dx} + P(x)y = Q(x)$
- ใช้ integrating factor: $\mu(x) = e^{\int P(x)dx}$
- คำตอบ: $y = \frac{1}{\mu(x)}\left[\int \mu(x)Q(x)dx + C\right]$

**Homogeneous Linear ODE (Constant Coefficients):**
- สมการรูป: $ay'' + by' + cy = 0$
- ใช้ characteristic equation: $ar^2 + br + c = 0$
- ถ้า $r_1 \neq r_2$: $y = C_1e^{r_1x} + C_2e^{r_2x}$
- ถ้า $r_1 = r_2$: $y = (C_1 + C_2x)e^{rx}$
- ถ้าเป็น complex: $y = e^{\alpha x}(C_1\cos\beta x + C_2\sin\beta x)$

### 3.8 Linear Algebra — Vectors and Matrices
**Determinant:**
- $2 \times 2$: $\det\begin{pmatrix} a & b \\ c & d \end{pmatrix} = ad - bc$
- $3 \times 3$: ใช้ cofactor expansion

**Inverse Matrix:** $A^{-1} = \frac{1}{\det(A)} \text{adj}(A)$

**Matrix Multiplication:** $(AB)_{ij} = \sum_k A_{ik}B_{kj}$ — ไม่ commutative ($AB \neq BA$ โดยทั่วไป)

### 3.9 Eigenvalues and Eigenvectors
**นิยาม:** $Av = \lambda v$ สำหรับ eigenvector $v \neq 0$ และ eigenvalue $\lambda$

**วิหา eigenvalues:** แก้ $\det(A - \lambda I) = 0$ (characteristic polynomial)

**Properties:**
- $\sum \lambda_i = \text{tr}(A)$ (ผลรวมของ trace)
- $\prod \lambda_i = \det(A)$ (ผลคูณของ determinant)
- $A$ เป็น diagonalizable ก็ต่อเมื่อมี eigenvectors เพียงพอ ($n$ ตัวสำหรับ $n \times n$ matrix)

### 3.10 Positive Definite Matrices
**นิยาม:** เมตริกซ์ $A$ เป็น positive definite ก็ต่อเมื่อ $x^TAx > 0$ สำหรับทุก $x \neq 0$

**Symmetric Positive Definite (SPD):**
- eigenvalues ทั้งหมดเป็นบวก
- มี unique Cholesky decomposition: $A = LL^T$
- ใช้ใน Mean-Variance Optimization ของ Modern Portfolio Theory

### 3.11 Singular Value Decomposition (SVD)
$A = U\Sigma V^T$ ที่:
- $U$: orthogonal matrix (eigenvectors ของ $AA^T$)
- $\Sigma$: diagonal matrix (singular values)
- $V$: orthogonal matrix (eigenvectors ของ $A^TA$)

**Applications:** Least squares, pseudoinverse, data compression, principal component analysis (PCA)

### 3.12 Linear Regression (OLS — Ordinary Least Squares)
**โมเดล:** $Y = X\beta + \varepsilon$

**Normal Equation:** $\hat{\beta} = (X^TX)^{-1}X^TY$

**Properties ของ OLS estimators:**
- Unbiased: $E[\hat{\beta}] = \beta$
- Gauss-Markov Theorem: OLS เป็น BLUE (Best Linear Unbiased Estimator) เมื่อ $\varepsilon$ มี constant variance และไม่มี autocorrelation

---

<a name="ch4"></a>
## Chapter 4: Probability Theory — ทฤษฎีความน่าจะเป็น

### 4.1 Basic Probability Definitions
**นิยามคลาสสิก:** $P(A) = \frac{|A|}{|\Omega|}$ ( equiprobable outcomes)

**Axioms of Probability (Kolmogorov):**
1. $P(A) \ge 0$
2. $P(\Omega) = 1$
3. Countable additivity: $P(\cup A_i) = \sum P(A_i)$ ถ้า $A_i$ เป็น disjoint

**Inclusion-Exclusion:** $P(A \cup B) = P(A) + P(B) - P(A \cap B)$

### 4.2 Combinatorial Analysis
**Permutations:** $P(n, r) = \frac{n!}{(n-r)!}$

**Combinations:** $\binom{n}{r} = \frac{n!}{r!(n-r)!}$

**Multinomial Coefficient:** $\binom{n}{n_1, n_2, \ldots, n_k} = \frac{n!}{n_1!n_2!\cdots n_k!}$

**Poker Hands ตัวอย่าง:**
- Royal Flush: $\frac{4}{\binom{52}{5}} = \frac{4}{2598960}$
- Full House: $\frac{13 \times \binom{4}{3} \times 12 \times \binom{4}{2}}{\binom{52}{5}} = \frac{3744}{2598960}$

### 4.3 Conditional Probability
**นิยาม:** $P(A|B) = \frac{P(A \cap B)}{P(B)}$

**Independence:** $A$ และ $B$ เป็นอิสระก็ต่อเมื่อ $P(A \cap B) = P(A)P(B)$

### 4.4 Bayes' Theorem
$$P(A|B) = \frac{P(B|A)P(A)}{P(B)}$$

**ตัวอย่าง (Medical Test):**
- โรคมี incidence 1% ($P(D) = 0.01$)
- ตรวจเจอ 90% ($P(+|D) = 0.9$)
- False positive 9% ($P(+|\neg D) = 0.09$)
- $P(D|+) = \frac{0.9 \times 0.01}{0.9 \times 0.01 + 0.09 \times 0.99} = \frac{0.009}{0.009 + 0.0891} \approx 9.1\%$

### 4.5 Discrete Distributions

**Bernoulli:** $X \in \{0, 1\}$, $P(X=1) = p$

**Binomial:** $P(X = k) = \binom{n}{k} p^k (1-p)^{n-k}$
- $E[X] = np$, $\text{Var}(X) = np(1-p)$

**Poisson:** $P(X = k) = \frac{\lambda^k e^{-\lambda}}{k!}$
- ใช้แทน Binomial เมื่อ $n$ ใหญ่และ $p$ เล็ก
- $E[X] = \lambda$, $\text{Var}(X) = \lambda$

**Geometric:** $P(X = k) = p(1-p)^{k-1}$ (จำนวน trial จนสำเร็จครั้งแรก)
- $E[X] = \frac{1}{p}$, $\text{Var}(X) = \frac{1-p}{p^2}$

**Negative Binomial:** จำนวน trial จนสำเร็จ $r$ ครั้ง

### 4.6 Continuous Distributions

**Uniform:** $f(x) = \frac{1}{b-a}$ สำหรับ $x \in [a, b]$

**Exponential:** $f(x) = \lambda e^{-\lambda x}$ (memoryless property)
- ใช้จำลอง inter-arrival time
- $E[X] = \frac{1}{\lambda}$, $\text{Var}(X) = \frac{1}{\lambda^2}$

**Normal (Gaussian):** $f(x) = \frac{1}{\sigma\sqrt{2\pi}} e^{-\frac{(x-\mu)^2}{2\sigma^2}}$
- **Standard Normal:** $\mu = 0$, $\sigma = 1$
- **68-95-99.7 Rule:** 68% อยู่ใน $\mu \pm \sigma$, 95% อยู่ใน $\mu \pm 2\sigma$, 99.7% อยู่ใน $\mu \pm 3\sigma$
- **Central Limit Theorem:** ผลรวมของ random variables จำนวนมากจะเข้าใกล้ Normal distribution แม้ว่าจะไม่ได้มาจาก Normal distribution เดิม

### 4.7 Expected Value, Variance, Covariance

**Expected Value:**
- Discrete: $E[X] = \sum x_i p_i$
- Continuous: $E[X] = \int_{-\infty}^{\infty} x f(x) dx$
- Properties: $E[aX + b] = aE[X] + b$, $E[XY] = E[X]E[Y]$ ถ้า independent

**Variance:** $\text{Var}(X) = E[(X - E[X])^2] = E[X^2] - (E[X])^2$
- $\text{Var}(aX + b) = a^2\text{Var}(X)$

**Covariance:** $\text{Cov}(X, Y) = E[(X-\mu_X)(Y-\mu_Y)] = E[XY] - E[X]E[Y]$
- $\text{Cov}(X, X) = \text{Var}(X)$
- $\text{Correlation}(X, Y) = \frac{\text{Cov}(X, Y)}{\sigma_X \sigma_Y} \in [-1, 1]$

**Portfolio Variance (2 assets):** $\sigma_p^2 = w_1^2\sigma_1^2 + w_2^2\sigma_2^2 + 2w_1w_2\rho_{12}\sigma_1\sigma_2$

### 4.8 Order Statistics
**Order Statistics:** เรียงค่า $X_{(1)} \le X_{(2)} \le \cdots \le X_{(n)}$ จากตัวอย่าง $n$ ตัว

- $X_{(1)}$ = minimum, $X_{(n)}$ = maximum
- **Median:** ค่าตรงกลาง (ถ้า $n$ คู่ ใช้ค่าเฉลี่ยของ 2 ค่ากลาง)

**Expected Value ของ Order Statistics:**
- สำหรับ Uniform[0,1]: $E[X_{(k)}] = \frac{k}{n+1}$
- สำหรับ Exponential: $E[X_{(k)}] = \frac{1}{\lambda}\sum_{i=1}^k \frac{1}{n-i+1}$

### 4.9 Joint Distribution and Independence
**Joint PDF:** $f(x, y) = \frac{\partial^2 F(x, y)}{\partial x \partial y}$

**Marginal Distribution:** $f_X(x) = \int_{-\infty}^{\infty} f(x, y) dy$

**Independent:** $f(x, y) = f_X(x) f_Y(y)$

### 4.10 Common Joint Distributions
- **Multivariate Normal:** ใช้ covariance matrix $\Sigma$
- **Multinomial:** ทดลอง $n$ ครั้ง มี $k$ ผลลัพธ์
- **Poisson Process:** arrival times เป็น Exponential

### 4.11 Markov Chains
**นิยาม:** กระบวนการที่ state ถัดไปขึ้นอยู่กับ state ปัจจุบันเท่านั้น (memoryless property)

**Transition Matrix:** $P_{ij} = P(X_{n+1} = j | X_n = i)$

**Stationary Distribution:** $\pi = \pi P$ (ค่าคงตัวเมื่อ time $\to \infty$)

**Classification of States:**
- **Transient:** กลับมาไม่ได้
- **Recurrent:** กลับมาได้เสมอ
- **Absorbing:** state ที่ไม่ออกจากตัวเอง
- **Ergodic:** recurrent + aperiodic → มี stationary distribution

**Gambler's Ruin Problem:**
- นักพนันมีเงิน $i$ เหรียญ เดิมพันทีละ 1 เหรียญจนกว่าจะหมดหรือถึงเป้าหมาย $N$
- ความน่าจะเป็นที่จะได้ $N$: $P = \frac{1 - (q/p)^i}{1 - (q/p)^N}$ (เมื่อ $p \neq q$)
- ถ้า $p = q = 0.5$: $P = \frac{i}{N}$

---

<a name="ch5"></a>
## Chapter 5: Stochastic Processes & Stochastic Calculus

### 5.1 Dynamic Programming
**หลักการ Bellman:** ปัญหาที่มี optimal substructure สามารถแก้ได้โดยแบ่งเป็น subproblems เล็กลง

**Optimal Stopping:** หาจุดที่ควรหยุดเพื่อ maximize expected reward

**Application ใน Finance:** American option pricing — ต้องตัดสินใจว่าจะ exercise เมื่อไหร่

### 5.2 Brownian Motion (Wiener Process)
**นิยาม:** $W(t)$ เป็น Brownian Motion ก็ต่อเมื่อ:
1. $W(0) = 0$
2. Increments เป็น independent: $W(t) - W(s)$ เป็นอิสระจากค่าก่อนหน้า
3. Stationary increments: $W(t) - W(s) \sim N(0, t-s)$
4. Continuous sample paths (แต่ไม่ differentiable ที่ไหนเลย)

**Properties:**
- $E[W(t)] = 0$, $\text{Var}(W(t)) = t$
- $E[W(t)^2] = t$ (quadratic variation = $t$)
- **Reflection Principle:** $P(\max_{s \le t} W(s) > a) = 2P(W(t) > a)$

### 5.3 Stopping Time and First Passage Time
**Stopping Time:** เวลา $\tau$ ที่ 결정โดยข้อมูลจนถึงเวลานั้น (ไม่ใช่ข้อมูลในอนาคต)

**First Passage Time (First Hitting Time):**
- $\tau_a = \inf\{t > 0 : W(t) = a\}$
- **Distribution:** $\tau_a \sim \text{Inverse Gaussian}$ — density: $f(t) = \frac{a}{\sqrt{2\pi t^3}} e^{-a^2/(2t)}$
- $E[\tau_a] = \infty$ (ค่าเฉลี่ยเป็นอนันต์!)
- $P(\tau_a < t) = 2(1 - \Phi(a/\sqrt{t}))$ สำหรับ Standard Brownian Motion

**APPLICATION สำคัญใน Trading:**
- ใช้หา probability ที่ราคาจะชน barrier
- ใช้ในการกำหนด stop-loss level
- First passage time distribution บอกความเสี่ยงที่ราคาจะออกจาก trading range

### 5.4 Itô's Lemma
**Itô's Formula:** ถ้า $X(t)$ เป็น Itô process ที่ $dX = \mu dt + \sigma dW$ และ $f$ เป็น $C^2$ function:

$$df(X) = \left(\mu \frac{\partial f}{\partial x} + \frac{1}{2}\sigma^2 \frac{\partial^2 f}{\partial x^2}\right)dt + \sigma \frac{\partial f}{\partial x} dW$$

**ข้อแตกต่างจาก calculus ปกติ:** มี term $\frac{1}{2}\sigma^2 \frac{\partial^2 f}{\partial x^2}$ เพิ่มขึ้นมา (Itô correction)

**ตัวอย่าง (Geometric Brownian Motion):**
ถ้า $dS = \mu S dt + \sigma S dW$ ต้องการหา $d(\ln S)$:

ใช้ Itô กับ $f(S) = \ln S$:
$$d(\ln S) = \left(\mu - \frac{\sigma^2}{2}\right)dt + \sigma dW$$

ผลลัพธ์: $\ln S_T = \ln S_0 + (\mu - \frac{\sigma^2}{2})T + \sigma W(T)$

ดังนั้น $S_T = S_0 \exp\left[(\mu - \frac{\sigma^2}{2})T + \sigma W(T)\right]$

**APPLICATION ใน Trading:**
- Itô's lemma เป็นพื้นฐานของ Black-Scholes-Merton model
- ใช้แปลง GBM เป็น lognormal distribution
- ใช้หา drift ที่ถูกต้องของราคาหุ้น (risk-neutral drift = $r - \frac{\sigma^2}{2}$)

---

<a name="ch6"></a>
## Chapter 6: Finance — การเงินและการกำหนดราคาอนุพันธ์

### 6.1 Option Basics
**Call Option:** สิทธิ์ซื้อ (right to buy) — profit = $\max(S_T - K, 0) - C$
**Put Option:** สิทธิ์ขาย (right to sell) — profit = $\max(K - S_T, 0) - P$

**Put-Call Parity (European Options):**
$$C - P = S - Ke^{-rT}$$

**Applications:**
- หา price ของ Call จาก Put หรือ vice versa
- ถ้า Put-Call Parity ไม่ถูก → arbitrage opportunity
- **Cash-Secured Put:** Short put + deposit $Ke^{-rT}$ = Long call equivalent

### 6.2 Black-Scholes-Merton (BSM) Model
**สมการ Black-Scholes PDE:**
$$\frac{\partial V}{\partial t} + \frac{1}{2}\sigma^2 S^2 \frac{\partial^2 V}{\partial S^2} + rS\frac{\partial V}{\partial S} - rV = 0$$

**Black-Scholes Formula (European Call):**
$$C = S_0 N(d_1) - Ke^{-rT} N(d_2)$$

ที่:
- $d_1 = \frac{\ln(S_0/K) + (r + \sigma^2/2)T}{\sigma\sqrt{T}}$
- $d_2 = d_1 - \sigma\sqrt{T}$
- $N(\cdot)$ = cumulative standard normal distribution

**Key Assumptions:**
1. ไม่มี arbitrage
2. ไม่มี transaction costs
3. Risk-free rate คงที่
4. Volatility คงที่
5. หุ้นไม่จ่าย dividend (สำหรับuropean)
6. สามารถ short sell ได้

### 6.3 The Greeks ( Sensitivity Measures)

**Delta ($\Delta$):** $\frac{\partial C}{\partial S} = N(d_1)$ — ความไวต่อราคาหุ้น
- Call: $\Delta \in (0, 1)$, Put: $\Delta \in (-1, 0)$
- ใช้ hedging: ต้อง short $\Delta$ หุ้นต่อ 1 option

**Gamma ($\Gamma$):** $\frac{\partial^2 C}{\partial S^2} = \frac{N'(d_1)}{S\sigma\sqrt{T}}$ — ความเร็วของ Delta
- สูงสุดเมื่อ ATM (at-the-money)
- ใช้บอก convexity ของ position

**Vega ($\nu$):** $\frac{\partial C}{\partial \sigma} = S\sqrt{T}N'(d_1)$ — ความไวต่อ volatility
- สูงสุดเมื่อ ATM
- ใช้ volatility trading

**Theta ($\Theta$):** $\frac{\partial C}{\partial t}$ — time decay (ความเสียหายต่อวัน)
- Theta มักจะเป็นลบสำหรับ long options
- สูงสุดเมื่อ ATM และใกล้ expiry

**Rho ($\rho$):** $\frac{\partial C}{\partial r} = KTe^{-rT}N(d_2)$ — ความไวต่อ interest rate

### 6.4 Option Portfolios and Exotic Options

**Warrants vs Options:** Warrants ออกโดยบริษัท (เพิ่ม dilution), Options ออกโดย exchange

**Exotic Options:**
- **Binary Option:** ได้固定 amount ถ้าเข้าเงื่อนไข ไม่ก็ไม่ได้อะไรเลย
- **Exchange Option:** แลกหุ้น A ตัวหนึ่งเป็นหุ้น B — ใช้ Margrabe's formula
- **Barrier Option:** เปลี่ยนสถานะเมื่อราคาแตะ barrier — ใช้ static replication
- **Chooser Option:** เลือกภายหลังว่าจะเป็น Call หรือ Put
- **Asian Option:** payoff ขึ้นอยู่กับค่าเฉลี่ยของราคา — ลด volatility ของ payoff

**Spread Option:** Option ที่ payoff ขึ้นอยู่กับผลต่างของ 2 หุ้น
- ใช้ Margrabe's formula: $C = S_1 N(d_1) - S_2 N(d_2)$
- $d_1 = \frac{\ln(S_1/S_2) + (\sigma^2/2)T}{\sigma\sqrt{T}}$

### 6.5 Portfolio Optimization (Mean-Variance)

**Modern Portfolio Theory (Markowitz):**

**Efficient Frontier:** เส้นโค้งของ portfolios ที่ให้ return สูงสุดสำหรับแต่ละระดับความเสี่ยง

**Optimal Weights (Closed Form):**
$$w^* = \frac{1}{\lambda}\Sigma^{-1}(\mu - rf \cdot \mathbf{1})$$

ที่:
- $\Sigma$ = covariance matrix
- $\mu$ = expected returns vector
- $\lambda$ = risk aversion coefficient

**Two-Fund Theorem:** ทุก efficient portfolio เป็น combination ของ risk-free asset และ portfolio บน Capital Market Line (CML)

**CAPM (Capital Asset Pricing Model):**
$$E[R_i] = rf + \beta_i (E[R_m] - rf)$$
ที่ $\beta_i = \frac{\text{Cov}(R_i, R_m)}{\text{Var}(R_m)}$

**Sharpe Ratio:** $\frac{E[R_p] - rf}{\sigma_p}$ — return ต่อหน่วยความเสี่ยง

### 6.6 Value at Risk (VaR)
**นิยาม:** VaR คือ loss สูงสุดที่จะเกิดขึ้นใน confidence level $\alpha$ (เช่น 95% หรือ 99%)

**Mathematical:** $\alpha = \int_{-\infty}^{-VaR} f(x) dx$ — VaR เป็น negative percentile ของการกระจายผลตอบแทน

**ข้อจำกัดของ VaR:**
1. **ไม่ sub-additive:** $VaR(A+B) \le VaR(A) + VaR(B)$ ไม่จำเป็นต้องเป็นจริง → ขัดกับหลักการ diversification
2. **ไม่บอก tail risk:**  VaR 95% ไม่บอกว่า损失จะมากแค่ไหนถ้าเกิดเหตุการณ์ร้ายแรง (top 5%)
3. **ไม่ใช่ coherent risk measure:** ใช้ Conditional VaR (CVaR) หรือ Expected Shortfall แทน

**Backtesting VaR:**
- Kupiec's Test (POF Test): ทดสอบว่า breach rate ตรงกับ confidence level หรือไม่
- Traffic Light Approach ของ Basel: Green (< 4%), Yellow (4-5%), Red (> 5%)

---

<a name="ch7"></a>
## Chapter 7: Algorithms & Numerical Methods

### 7.1 Algorithms

**Asymptotic Notations:**
- $O(g(n))$: upper bound — แย่ที่สุด
- $\Omega(g(n))$: lower bound — ดีที่สุด
- $\Theta(g(n))$: tight bound — ทั้งบนและล่าง

**Master Theorem:** สำหรับ recurrence $T(n) = aT(n/b) + f(n)$
- ถ้า $f(n) = O(n^{\log_b a - \varepsilon})$: $T(n) = \Theta(n^{\log_b a})$
- ถ้า $f(n) = \Theta(n^{\log_b a})$: $T(n) = \Theta(n^{\log_b a} \log n)$
- ถ้า $f(n) = \Omega(n^{\log_b a + \varepsilon})$: $T(n) = \Theta(f(n))$

### 7.2 Number Swap (Without Temporary Variable)
**วิธีที่ 1 (Addition/Subtraction):**
```c
i = i + j;  // เก็บผลรวม
j = i - j;  // j = ค่าเดิมของ i
i = i - j;  // i = ค่าเดิมของ j
```

**วิธีที่ 2 (XOR):**
```c
i = i ^ j;
j = j ^ i;  // j = ค่าเดิมของ i
i = i ^ j;  // i = ค่าเดิมของ j
```
- XOR ปลอดภัยกว่า (ไม่มี overflow risk)

### 7.3 Unique Elements (Sorted Array)
- ใช้ two-pointer technique
- ถ้า $a[i] \neq a[i-1]$ → ใส่ในผลลัพธ์
- Complexity: $O(n)$

### 7.4 Horner's Algorithm ( Polynomial Evaluation)
**Naive:** $O(n^2)$ (คำนวณ $x^k$ ทีละตัว)

**Horner's:** $O(n)$ — เขียนใหม่เป็น nested form:
$$y = ((\cdots((A_nx + A_{n-1})x + A_{n-2})x + \cdots + A_1)x + A_0$$

### 7.5 Moving Average (Sliding Window)
**Naive:** $O(mn)$

**Optimized:** $O(m)$ — ใช้ sliding window:
- เก็บ sum ปัจจุบัน
- ลบตัวเก่า บวกตัวใหม่ → ได้ sum ใหม่
- หารด้วย $n$ → moving average ใหม่

### 7.6 Sorting Algorithms

**Insertion Sort:**
- $W(n) = \Theta(n^2)$, $A(n) = \Theta(n^2)$
- ดีสำหรับ nearly sorted data

**Merge Sort:**
- Divide-and-conquer: แบ่งเป็น 2 ส่วน เรียงแต่ละส่วน แล้ว merge
- $T(n) = 2T(n/2) + \Theta(n) = \Theta(n \log n)$
- Stable sort, consistent performance

**Quick Sort:**
- เลือ pivot แล้ว partition
- $W(n) = \Theta(n^2)$ (worst case: already sorted)
- $A(n) = \Theta(n \log n)$
- In practice เร็วกว่า merge sort (cache-friendly)

### 7.7 Random Permutation (Knuth Shuffle)
**Algorithm:**
```c
for (i = n-1; i > 0; i--) {
    j = Random(0, i);  // random index 0 ถึง i
    swap(A[i], A[j]);
}
```
- Complexity: $\Theta(n)$
- ทุก permutation มีความน่าจะเป็นเท่ากัน $1/n!$

** reservoir sampling (เลือก $k$ จาก stream ที่ไม่รู้ความยาว):**
- เก็บ $k$ ตัวแรก
- สำหรับตัวที่ $i$ (i > k): แทนที่ด้วยความน่าจะเป็น $k/i$

### 7.8 Search Algorithms

**Binary Search (ใน sorted array):**
- $O(\log n)$ — ตัดครึ่งทุก step
- Master Theorem: $T(n) = T(n/2) + O(1) = O(\log n)$

**Find Min & Max ด้วย 3n/2 Comparisons:**
- จับคู่ elements เปรียบเทียบ → ใส่ใน 2 กลุ่ม (เล็ก/ใหญ่)
- หา min ในกลุ่มเล็ก, max ในกลุ่มใหญ่
- ใช้ $n/2 + (n/2-1) + (n/2-1) = 3n/2 - 2$ comparisons

**Sorted Matrix Search ($n \times n$):**
- เริ่มจากมุมบนขวา: ถ้าเจอ → หยุด, ถ้าเล็กกว่า → เลื่อนลง, ถ้าใหญ่กว่า → เลื่อนซ้าย
- Complexity: $O(n)$ (ไม่ใช่ $O(n^2)$)

### 7.9 Fibonacci Numbers
**Naive Recursive:** $T(n) = T(n-1) + T(n-2) + 1$ → $O(\phi^n)$ ที่ $\phi = \frac{1+\sqrt{5}}{2}$ (golden ratio) — ช้ามาก

**Bottom-Up:** $O(n)$ — คำนวณจาก $F_0$ ขึ้นมา

**Recursive Squaring (Matrix Exponentiation):** $O(\log n)$
$$\begin{bmatrix} F_{n+1} & F_n \\ F_n & F_{n-1} \end{bmatrix} = \begin{bmatrix} 1 & 1 \\ 1 & 0 \end{bmatrix}^n$$

### 7.10 Maximum Contiguous Subarray (Kadane's Algorithm)
**ปัญหา:** หา subarray ต่อเนื่องที่มีผลรวมมากที่สุด

**Naive:** $O(n^2)$

**Kadane's Algorithm:** $O(n)$
- ใช้ prefix sum: $V(i,j) = T(j) - T(i-1)$ ที่ $T(i) = \sum_{k=1}^i A[k]$
- ติดตาม $T_{min}$ (minimum prefix sum จนถึงตอนนี้) → $V_{max} = T(j) - T_{min}$

**APPLICATION ใน Trading:**
- ใช้หา maximum run-up (กำไรสูงสุดติดต่อกัน)
- ใช้หา maximum drawdown (ขาดทุนสูงสุดติดต่อกัน)
- ทุก trading system ใช้ algorithm นี้ในการวิเคราะห์ performance

### 7.11 Power of 2 / Bitwise Operations
**Power of 2 Check:** `x & (x-1) == 0`
- ตัวเลขที่เป็น power of 2 มี bit เพียง 1 ตัวที่เป็น 1
- `x-1` จะกลับ bit ทั้งหมด → AND กันได้ 0

**Multiply by 7:** `(x << 3) - x`
- `x << 3` = $x \times 8$
- $8x - x = 7x$

### 7.12 Probability Simulation
**Simulate probability $p$ ด้วย fair coin:**
- เขียน $p$ เป็น binary: $p = 0.p_1p_2p_3\ldots$
- Toss coin: head = 1, tail = 0
- เปรียบเทียบทีละ bit: ถ้า $s_i < p_i$ → ชนะ, ถ้า $s_i > p_i$ → แพ้
- ถ้าเท่ากัน → toss ต่อ

### 7.13 Poisonous Wine Problem (Binary Encoding)
**ปัญหา:** 1000 ขวดไวน์ มี 1 ขวดมีพิษ ใช้หนู 10 ตัวทดสอบ within 20 ชม. (พิษออกฤทธิ์ 18 ชม.)

**วิธีแก้:** ใช้ binary representation
- กำกับขวดแต่ละขวดเป็น 10-bit binary (0000000001 ถึง 1111101000)
- หนูตัวที่ $k$ กินไวน์จากทุกขวดที่ bit ที่ $k$ เป็น 1
- หลัง 18 ชม.: หนูตาย = 1, หนูรอด = 0 → อ่าน binary number ออกมา → ขวดที่มีพิษ

** APPLICATION:** Binary encoding ใช้ใน data compression, error detection (checksum), hash functions

### 7.14 Monte Carlo Simulation
**หลักการ:** ใช้ random numbers จำลองสถานการณ์จำนวนมาก → ประมาณค่า expected value

**European Call Option Pricing:**
1. จำลอง price path: $S_{t+1} = S_t \exp\left[(r - \frac{\sigma^2}{2})\Delta t + \sigma\sqrt{\Delta t}\epsilon\right]$
2. คำนวณ payoff: $\max(S_T - K, 0)$
3. Discount: $C = e^{-rT} \frac{1}{M}\sum_{k=1}^M \max(S_{T,k} - K, 0)$

**Random Number Generation:**
- **Inverse Transform Method:** $X = F^{-1}(U)$ ที่ $U \sim \text{Uniform}(0,1)$
- **Rejection Method:** ใช้ helper distribution $g(x)$ เมื่อ $F^{-1}$ หา analytical ไม่ได้

**Variance Reduction Techniques:**
1. **Antithetic Variable:** ใช้ $+\epsilon$ และ $-\epsilon$ คู่กัน → ลด variance
2. **Moment Matching:** rescale samples ให้ตรงกับ population moments
3. **Control Variate:** ใช้ derivative ที่มี analytical solution มาแก้ estimation error
4. **Importance Sampling:** เปลี่ยน distribution เพื่อลด variance (เช่น deep OTM options)
5. **Low-Discrepancy Sequence:** ใช้ deterministic sequence แทน random → convergence $O(1/M)$

**Estimate Delta & Gamma ด้วย Monte Carlo:**
- $\Delta \approx \frac{f(S+\delta S) - f(S-\delta S)}{2\delta S}$
- $\Gamma \approx \frac{f(S+\delta S) - 2f(S) + f(S-\delta S)}{(\delta S)^2}$

### 7.15 Finite Difference Methods
**หลักการ:** แปลง PDE (Black-Scholes) เป็น system ของ algebraic equations โดย discretize time และ price

**Explicit Method:**
$$u_j^{n+1} = \alpha u_{j-1}^n + (1-2\alpha)u_j^n + \alpha u_{j+1}^n$$
- $\alpha = \Delta\tau / (\Delta x)^2$
- เสถียรเมื่อ $\alpha < 1/2$

**Implicit Method:**
- ใช้ backward difference → เสถียรเสมอ
- ต้องแก้ system of equations (invert matrix)

**Crank-Nicolson Method:**
- Average ของ explicit และ implicit → second-order accurate
- ใช้ใน practice มากที่สุด

**Stability Condition:** Explicit method ต้องการ $\Delta t < \frac{(\Delta x)^2}{2}$ — ถ้า $\Delta x$ เล็กเกินไป (space steps มากเกินไป) → ไม่เสถียร

---

## สรุปสำหรับ Algo Trading

### หัวข้อสำคัญที่เชื่อมโยงกับ Trading

| Topic | Application ใน Trading |
|-------|----------------------|
| **Brownian Motion** | พื้นฐานของราคาหุ้น (GBM) |
| **Itô's Lemma** | Black-Scholes PDE, lognormal pricing |
| **Monte Carlo** | Option pricing, VaR estimation |
| **Finite Difference** | American option pricing |
| **Kadane's Algorithm** | Maximum drawdown analysis |
| **Bayes' Theorem** | Signal updating, regime detection |
| **Markov Chains** | Regime switching models |
| **Portfolio Optimization** | Mean-Variance, risk parity |
| **VaR & CVaR** | Risk management |
| **The Greeks** | Delta hedging, volatility trading |
| **Put-Call Parity** | Arbitrage detection |
| **Sorting Algorithms** | Order book management |
| **Binary Encoding** | Data compression, checksum |
| **Master Theorem** | Algorithm complexity analysis |
| **Recursive Squaring** | Fast matrix exponentiation (HMM) |

### Formula สำคัญสำหรับ Quant Interview

| Formula | ใช้ทำอะไร |
|---------|----------|
| $C = SN(d_1) - Ke^{-rT}N(d_2)$ | Black-Scholes Call |
| $P = Ke^{-rT}N(-d_2) - SN(-d_1)$ | Black-Scholes Put |
| $\Delta = N(d_1)$ | Delta ของ Call |
| $\text{Var}(X) = E[X^2] - (E[X])^2$ | Variance shortcut |
| $\text{Cov}(X,Y) = E[XY] - E[X]E[Y]$ | Covariance shortcut |
| $w^* = \frac{1}{\lambda}\Sigma^{-1}(\mu - rf)$ | Optimal portfolio weights |
| $\alpha = \Delta\tau / (\Delta x)^2$ | Finite difference stability |
| $P = \frac{1-(q/p)^i}{1-(q/p)^N}$ | Gambler's Ruin probability |
| $F = Se^{(r+u-y)t}$ | Forward/Futures pricing |

---

*สรุปจากหนังสือ: "A Practical Guide to Quantitative Finance Interviews" โดย Xinfeng Zhou*
*Covered: Brain Teasers, Calculus, Linear Algebra, Probability, Stochastic Processes, Finance, Algorithms*
