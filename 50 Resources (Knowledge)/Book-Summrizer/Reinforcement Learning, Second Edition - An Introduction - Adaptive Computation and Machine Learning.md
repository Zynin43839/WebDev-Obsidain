# Reinforcement Learning, Second Edition
> **ผู้เขียน:** Richard S. Sutton, Andrew G. Barto

---

## ภาพรวมของหนังสือ

"Reinforcement Learning: An Introduction" ฉบับที่ 2 เป็นตำราหลักด้าน Reinforcement Learning (RL) ที่ครอบคลุมตั้งแต่พื้นฐานจนถึงขั้นสูง ผู้เขียนมุ่งให้เนื้อหาเข้าถึงได้สำหรับผู้อ่านจากทุกสาขาวิชา ทั้ง AI, วิทยาศาสตร์ข้อมูล, จิตวิทยา, และวิศวกรรมศาสตร์ ฉบับที่ 2 ขยายความเป็นสองเท่าจากฉบับแรก โดยแบ่งเนื้อหาเป็น 3 ส่วนใหญ่: Tabular Methods, Function Approximation, และ Applications/Psychology/Neuroscience

---

## Preface — คำนำ

### คำนำฉบับที่ 2
- เป้าหมายเดิม: อธิบายแนวคิดและอัลกอริทึมหลักของ RL ให้เข้าใจง่าย
- เน้น algo แบบ online learning ที่ใช้ได้จริง เพิ่มหัวข้อใหม่: UCB, Expected Sarsa, Double Learning, Tree-backup, Q(σ), RTDP, MCTS, Fourier Basis, LSTD, Kernel-based Methods, Gradient-TD, Emphatic-TD, True Online TD(λ), Policy Gradient
- เปลี่ยนระบบ notation: ตัวอักษรใหญ่ = random variable, ตัวเล็ก = ค่าจริง
- โครงสร้าง 3 ส่วน: (1) Tabular Methods (Ch.2-8), (2) Function Approximation (Ch.9-13), (3) Psychology/Neuroscience/Applications (Ch.14-17)

### คำนำฉบับที่ 1
- ผู้เขียนเริ่มทำงาน RL ตั้งแต่ปี 1979 ที่ UMass Amherst
- แรงบันดาลใจจาก A. Harry Klopf — "hedonistic learning system"
- RL ฟื้นฟูในยุค 1980s จาก 3 เส้นทาง: (1) trial-and-error, (2) optimal control/dynamic programming, (3) temporal-difference learning

---

## บทที่ 1: Introduction

### RL คืออะไร
RL คือการเรียนรู้ mapping จากสถานการณ์ไปสู่การกระทำเพื่อ maximize รางวัล ลักษณะสำคัญ: (1) trial-and-error search, (2) delayed reward ต่างจาก Supervised Learning (ไม่มี correct label), Unsupervised Learning (ไม่ได้ maximize reward), และ Evolutionary Methods (ไม่ได้ interact กับ environment)

### องค์ประกอบ 4 ข้อ
1. **Policy**: กฎที่บอกว่าจะทำอะไรในแต่ละสถานะ
2. **Reward Signal**: ตัวเลขที่ environment ส่งให้ agent — เป้าหมายหลัก
3. **Value Function**: คาดการณ์รางวัลสะสมในอนาคต — สำคัญกว่า reward ทันที
4. **Model of Environment**: ทำนายสถานะและรางวัลถัดไป — model-based vs. model-free

### ตัวอย่าง Tic-Tac-Toe
ใช้ value function + temporal-difference learning ต่างจาก minimax (สมมติคู่ต่อสู้สมบูรณ์แบบ) และ evolutionary method

### ประวัติศาสตร์ยุคแรก
- เส้นทาง 1: Thorndike's Law of Effect → Minsky's SNARCs → Michie's MENACE → Holland's Classifier Systems → Klopf
- เส้นทาง 2: Bellman's Dynamic Programming → Howard's Policy Iteration
- เส้นทาง 3: Samuel's Checkers → Klopf → Sutton's TD learning → **Watkins's Q-learning (1989)**

---

## บทที่ 2: Multi-armed Bandits

### ปัญหาพื้นฐาน
k-armed Bandit: เลือกจาก k ตัวเลือกซ้ำๆ เพื่อ maximize รางวัลรวม ความท้าทาย: **Exploration vs. Exploitation**

### อัลกอริทึมหลัก
1. **ε-greedy**: เลือก greedy ด้วย 1-ε, สุ่มด้วย ε
2. **UCB**: เลือก action ที่ Q(a) + c√(ln(t)/N(a)) สูงสุด — ให้ priority กับ action ที่ยังไม่ลองมาก
3. **Gradient Bandit**: เรียนรู้ action preference H(a) ผ่าน softmax — stochastic gradient ascent
4. **Optimistic Initialization**: ตั้งค่าเริ่มต้นสูงมากเพื่อ explore โดยธรรมชาติ

UCB ทำได้ดีที่สุด, ε-greedy ดีกว่า greedy

---

## บทที่ 3: Finite Markov Decision Processes

### กรอบ MDP
Agent เลือก action ณ สถานะ → ได้ reward และสถานะใหม่ Markov Property: สถานะปัจจุบันเพียงพอต่อการทำนายอนาคต

### Return และ Value Functions
- **Discounted Return**: G_t = R_{t+1} + γR_{t+2} + γ²R_{t+3} + ...
- **v_π(s)**: รางวัลสะสมที่คาดหวังจากสถานะ s ตาม policy π
- **q_π(s,a)**: รางวัลสะสมที่คาดหวังเมื่อทำ action a แล้วทำตาม π
- **Bellman Equation**: เชื่อมโยงค่าสถานะกับค่าสถานะถัดไป
- **Optimal Policy**: v*(s) = max_π v_π(s) — q*(s,a) ทำให้เลือก action ที่ดีที่สุดได้ทันที

---

## บทที่ 4: Dynamic Programming

ต้องการ model ที่สมบูรณ์ (รู้ p(s',r|s,a))

### อัลกอริทึม
1. **Iterative Policy Evaluation**: ประมาณค่า v_π ผ่าน iteration
2. **Policy Improvement**: สร้าง policy ใหม่ที่ greedy กับ value function
3. **Policy Iteration**: วนซ้ำ evaluation → improvement จน optimal
4. **Value Iteration**: รวม evaluation + improvement ใน sweep เดียว
5. **Asynchronous DP**: อัพเดท state ใดก็ได้ตามลำดับ

### Generalized Policy Iteration (GPI)
สองกระบวนการ — policy evaluation + policy improvement — โต้ตอบกันจนบรรลุ optimality

---

## บทที่ 5: Monte Carlo Methods

ไม่ต้องรู้ model — เรียนรู้จาก experience เท่านั้น

### MC Prediction
- **First-visit MC**: เฉลี่ย return หลัง visit ครั้งแรก
- **Every-visit MC**: เฉลี่ย return ทุกครั้งที่ visit

### MC Control
- **MC ES**: ใช้ Exploring Starts assumption
- **Without Exploring Starts**: ใช้ ε-soft หรือ ε-greedy

### Off-policy MC
- **Importance Sampling**: แปลง return จาก behavior policy b ให้เข้ากับ target policy π
- Weighted Importance Sampling ใช้ใน practice (variance ต่ำกว่า)

---

## บทที่ 6: Temporal-Difference Learning — หัวใจของ RL

### TD(0)
V(S_t) ← V(S_t) + α[R_{t+1} + γV(S_{t+1}) - V(S_t)]
ผสม MC (เรียนรู้จาก experience) และ DP (bootstrap) converge เร็วกว่า MC

### SARSA (On-policy TD Control)
อัพเดท q(S_t, A_t) ด้วย ε-greedy policy

### Q-Learning (Off-policy TD Control)
**Q(S_t, A_t) ← Q(S_t, A_t) + α[R_{t+1} + γ max_a Q(S_{t+1}, a) - Q(S_t, A_t)]**
เรียนรู้ optimal policy โดยตรง — เป็นหนึ่งใน algo สำคัญที่สุดของ RL

### Expected Sarsa
ใช้ expected value แทน max → ลด variance

---

## บทที่ 7: n-step Bootstrapping

- **n-step return**: เชื่อมช่องว่างระหว่าง MC (n=∞) และ TD(0) (n=1)
- **n-step Sarsa**, **n-step Tree Backup**, **Q(σ)**: ผสมวิธีต่างๆ
- Forward view vs. Backward view (eligibility traces)

---

## บทที่ 8: Planning and Learning with Tabular Methods

### Dyna Architecture
Agent เรียนรู้จาก (1) experience จริง และ (2) model (simulated experience)
- **Dyna-Q**: TD update จาก experience จริง → สร้าง experience จาก model → TD update อีก M รอบ
- Model-based: เรียนรู้ model แล้วใช้ planning
- Model-free: เรียนรู้จาก experience เท่านั้น

### MCTS
Monte Carlo Tree Search: ใช้ simulation-based planning สำหรับ game playing — สร้าง tree ด้วย UCB

---

## บทที่ 9: On-policy Prediction with Approximation

### Function Approximation
เมื่อ state space ใหญ่ → ใช้ parameterized function v̂(s,w) ≈ v_π(s) แทนตาราง

### วิธีทำ Feature
- **Linear**: v̂(s,w) = w^T x(s) + Gradient Descent (Semi-gradient)
- **Polynomial Features**, **Fourier Basis**, **Coarse Coding**, **Tile Coding**
- Linear semi-gradient TD(0) converge สู่ TD fixed point — ไม่จำเป็นต้อง converge สู่ v*

### Deep RL
Neural Networks เรียนรู้ features อัตโนมัติ → Deep Learning + RL = Deep RL

---

## บทที่ 10: On-policy Control with Approximation

### Sarsa with Function Approximation
Linear Sarsa(λ) + eligibility traces + Mountain Car task

### Average Reward
สำหรับ continuing tasks: differential return r̄(π) = average reward

### Policy Gradient Methods (เบื้องต้น)
- เรียนรู้ parameterized policy π(a|s,θ) ตรงๆ
- **REINFORCE**: θ ← θ + α G_t ∇ln π(A_t|S_t,θ)

---

## บทที่ 11: Off-policy Methods with Approximation

### Deadly Triad
(1) off-policy learning + (2) function approximation + (3) bootstrapping = มักไม่ stable

### Gradient TD Methods
- **GTD**: ใช้ two time-scale learning — เรียนรู้ w ช้าและ v เร็ว
- **Emphatic-TD**: แก้ bias ของ off-policy ด้วย emphasis weight
- **Double Learning**: แก้ overestimation bias ของ Q-learning

---

## บทที่ 12: Eligibility Traces

### หลักการ
z_t = γλz_{t-1} + ∇v̂(S_t,w) — ติดตามว่าสถานะไหน "สมควรได้รับ credit" จาก reward ล่าสุด

### วิธีใช้
- **TD(λ)**: ผสม n-step return กับ decay rate λ
- **SARSA(λ)**, **Watkins's Q(λ)**, **True Online TD(λ)**
- ค่ากลาง λ (0.1-0.9) มักดีที่สุด

---

## บทที่ 13: Policy Gradient Methods

### REINFORCE
Policy Gradient Theorem: ∇J(θ) = E_π[∇ln π(A_t|S_t,θ) G_t]
มี variance สูง → ช้า

### Actor-Critic Methods
- **Actor**: policy π(a|s,θ)
- **Critic**: value function v̂(s,w)
- ใช้ TD error δ_t แทน full return → ลด variance

### Advantage Function
A_π(s,a) = q_π(s,a) - v_π(s) — measure ว่า action ดีกว่าค่าเฉลี่ยแค่ไหน

### A3C
Asynchronous Advantage Actor-Critic: ใช้ multiple parallel actors — State-of-the-art

---

## บทที่ 14: Psychology

### ความเชื่อมโยง
- **Law of Effect**: การกระทำที่ตามด้วย satisfaction จะถูกเชื่อมกับสถานะนั้นมากขึ้น
- **Classical Conditioning**: TD learning อธิบาย blocking, overshadowing, extinction
- **Pavlovian vs. Instrumental Conditioning**: RL เชื่อมทั้งสอง — instrumental = action selection, Pavlovian = state value

---

## บทที่ 15: Neuroscience

### Dopamine = TD Learning Signal
- **Dopamine neurons** ใน VTA ส่งสัญญาณ reward prediction error (RPE) — ตรงกับ TD error (δ)
- Schultz et al. (1997): dopamine fire เมื่อ reward ไม่คาดคิด (δ > 0), ไม่ fire เมื่อคาดไว้แล้ว (δ = 0), ลด firing เมื่อ reward หายไป (δ < 0)

### โครงสร้างสมอง
- **Basal Ganglia**: striatum = actor, ventral striatum = reward processing
- **Hippocampus**: episodic memory, model-based RL → replay experience คล้าย Dyna
- **Orbitofrontal Cortex**: value representation
- **Anterior Cingulate Cortex**: exploration/exploitation trade-off

---

## บทที่ 16: Applications and Case Studies

### TD-Gammon
Tesauro (1992/1995): neural network + TD(λ) เรียนรู้เล่น backgammon เทียบเท่าแชมป์โลก

### Watson's Jeopardy!
IBM Watson ใช้ RL สำหรับ betting strategy

### AlphaGo / AlphaGo Zero / AlphaZero
- **AlphaGo** (2016): MCTS + policy/value networks + RL — ชนะ Lee Sedol 4-1
- **AlphaGo Zero** (2017): เรียนรู้จาก scratch โดยไม่มี human data — เก่งกว่าทุกเวอร์ชัน
- **AlphaZero** (2017): ใช้ arch เดียวกันกับ chess, shogi — เรียนรู้จาก game rules เท่านั้น

### DQN (Atari Games)
Mnih et al. (2015): CNN เล่น Atari จาก raw pixels — ชนะ human ใน 29/49 เกม
เทคนิค: experience replay, target network, frame stacking

---

## บทที่ 17: Frontiers

### หัวข้อที่ยังเปิดกว้าง
1. **General value functions**: เรียนรู้ multiple objectives พร้อมกัน
2. **Continuing tasks**: optimize average reward
3. **Dangerous situations**: RL ต้องระวังเมื่อ exploration อาจสร้างความเสียหาย
4. **Temporal abstraction**: options/hierarchical RL
5. **Knowledge-based RL**: สร้าง knowledge representations จาก RL experience
6. **Intra-option learning**: เรียนรู้ value ของ subgoals/options
7. **Attention and meta-learning**: ให้ agent เรียนรู้ว่าจะ "ใส่ใจ" กับอะไร
8. **Societal impacts**: RL มีผลกระทบต่อสังคม — ต้องรับผิดชอบ

---

## สรุปภาพรวม

หนังสือเล่มนี้วางรากฐาน RL ตั้งแต่ระดับพื้นฐาน (bandit, MDP) ถึงขั้นสูง (policy gradient, deep RL, neuroscience) แกนหลักคือ **Value Functions + Temporal-Difference Learning + Policy Gradient** ซึ่งเป็น pillar ที่ระบบ RL ทุกวันนี้ต่อยอดมา ผู้อ่านจะได้รับทั้งทฤษฎี (Bellman equations, convergence proofs) และ practice (algorithm pseudocode, experiments) ที่ใช้ได้จริงในการวิจัยและงานประยุกต์
