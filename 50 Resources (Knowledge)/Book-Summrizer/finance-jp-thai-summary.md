# สรุปภาษาไทย: Idiosyncrasies and Challenges of Data-Driven Learning in Electronic Trading

> **ผู้แต่ง:** Vangelis Bacoyannis, Vacslav Glukhov, Tom Jin, Jonathan Kochems, Doo Re Song (J.P. Morgan)
> **ที่มา:** NIPS 2018 Workshop on Challenges and Opportunities for AI in Financial Services, Montreal, Canada (arXiv:1811.09549v2)

---

## ภาพรวม

บทความนี้เป็นงานวิจัยจาก J.P. Morgan ที่นำเสนอความท้าทายและลักษณะเฉพาะ (idiosyncrasies) ของการใช้ machine learning และ neural information processing ในวงการ electronic trading เชิงปริมาณ (quantitative finance) ผู้เขียนต้องการเชื่อมช่องว่างระหว่างงานวิชาการกับการใช้งานจริงในอุตสาหกรรม金融服务 โดยเน้นว่าปัญหาในโลกจริงมีความซับซ้อนกว่าสมมติฐานในห้องปฏิบัติการมาก

---

## บทที่ 1: บทนำ — บริบทของ Agency Electronic Trading

Electronic trading เป็นบริการที่โบรกเกอร์ให้บริการลูกค้า (เช่น กองทุนบำเหน็จบำนาญ, hedge fund) ในการ rebalance พอร์ตการลงทุนอย่างมีประสิทธิภาพ เงินออมที่เกิดจากการเทรดที่มีประสิทธิภาพจะถูกส่งต่อกลับไปยังผู้ได้รับผลประโยชน์สุดท้าย เช่น ครู, แพทย์, นักดับเพลิง ฯลฯ

**ประเด็นสำคัญ:**
- การ globalization ของ asset trading ประกอบกับ IT ที่รวดเร็วทำให้มนุษย์ไม่สามารถแข่งขันในการตัดสินใจระดับจุลภาคได้อีกต่อไป
- ปัจจุบัน micro-level trading decisions ในหุ้นและ futures ส่วนใหญ่ทำโดย algorithm ที่กำหนดว่าจะเทรดที่ไหน, ราคาเท่าไหร่, และจำนวนเท่าไหร่
- ลูกค้าส่ง instruction ที่มี constraint และ preference มากมาย เช่น currency neutrality, sector exposure, market impact control, urgency level
- Trading algorithm แบบเดิมเป็นการผสมผสานระหว่าง quantitative models กับ rules/heuristics จากประสบการณ์ human traders ซึ่งสะสมเป็น "feature creep" — code หลายหมื่นบรรทัดที่ยากต่อการดูแลรักษา
- อุตสาหกรรม金融服务มี regulation ที่เข้มงวด เช่น แนวคิด "best execution" ใน EMEA

**สิ่งที่บทความนี้ต้องการนำเสนอ:** ความท้าทายเชิงปฏิบัติ (practical challenges) ที่เกิดขึ้นจริงในการพัฒนา electronic trading algorithms โดยใช้ data-centric approaches, neural processing, และ machine learning — เพื่อเป็นแรงบันดาลใจให้กับนักวิจัยในแวดวงวิชาการ

---

## บทที่ 2: Three Cultures of Data-Centric Applications in Quantitative Finance

ผู้เขียนต่อยอดแนวคิดของ Peter Norvig (2011) โดยแบ่งวัฒนธรรมของ data-centric applications ใน quantitative finance ออกเป็น 3 ยุค

### 2.1 Data Modelling Culture (วัฒนธรรมการสร้างแบบจำลองข้อมูล)
- ความเชื่อที่ว่าธรรมชาติ (และตลาดการเงิน) สามารถอธิบายได้ด้วย "black box" ที่มี model ง่ายๆ อยู่ข้างในซึ่งสร้าง observational data ขึ้นมา
- หน้าที่ของ quantitative finance คือค้นหา functional approximation สำหรับ data generating process นี้ แล้วดึง parameter ออกมา
- **ความท้าทาย:** model ง่ายๆ ไม่สามารถจับ essential properties ของตลาดได้ทั้งหมด มักทำให้เกิด false sense of certainty และล้มเหลวรุนแรง

### 2.2 Machine Learning Culture (วัฒนธรรม machine learning)
- ใช้agnostic approach กับคำถามที่ว่าธรรมชาติเรียบง่ายหรือไม่
- โลกการเงินดู "Darwinian มากกว่า Newtonian" — วิวัฒนาการตลอดเวลา, process ที่สังเกตได้เป็น emerging behaviors มากกว่า data generating machines
- ใช้ complex และ opaque functions ในการสร้างแบบจำลอง แต่ไม่ได้อ้างว่า functions เหล่านั้นเปิดเผย nature ของ processes ที่อยู่เบื้องหลัง
- **ความท้าทาย:** model ที่ซับซ้อนก็เสี่ยงต่อความล้มเหลวเช่นกัน — ความเสี่ยงเพิ่มขึ้นตาม complexity

### 2.3 Algorithmic Decision-Making Culture (วัฒนธรรมการตัดสินใจเชิง algorithm)
- ข้ามขั้นตอนการเรียนรู้ "โลกทำงานอย่างไร" ไปเลย แล้วฝึก electronic agents ให้แยกแยะ good decisions จาก bad decisions โดยตรง
- ความท้าทายคือ ability ในการทำความเข้าใจและอธิบายการตัดสินใจของ agent, ทำให้ sense จาก policy ของมัน, และมั่นใจว่า agent จะทำงานได้ถูกต้องในทุก environment
- Agent เรียนรู้ว่าการกระทำบางอย่างไม่ดีเพราะนำไปสู่ผลลัพธ์เชิงลบ (malum in se) แต่ยังต้องฉีด values, rules, constraints ที่ห้าม agent ทำสิ่งที่เราถือว่าเป็นสิ่งต้องห้าม (malum prohibitum) ซึ่ง agent ไม่สามารถเรียนรู้ได้จาก environment และ history เพียงอย่างเดียว

---

## บทที่ 3: Low to High Dimensionality and Back Again

### 3.1 High Level Decision-Making — การตัดสินใจระดับสูง

หลักการพื้นฐาน: ทุก order มี optimal execution rate หรือ execution schedule — คือความเร็วที่ order ควรจะถูก execute ในตลาด

**Two limiting cases:**
1. **Execute ทันที:** เป็นไปได้ถ้าลูกค้าไม่สนเรื่องต้นทุน แต่จะกระทบราคาตลาดอย่างรุนแรง — ไม่สมเหตุสมผล
2. **Execute ช้ามาก (ช้าตลอดกาล):** ไม่กดดันตลาดเลย แต่ราคามีเวลาเคลื่อนที่สวนทาง order — market risk สูงมาก — ก็ไม่สมเหตุสมผลเช่นกัน

**บทสรุป:** ต้องมี optimal rate ที่ balance ระหว่าง market impact (เทรดเร็วเกินไป → กระทบราคา) กับ market risk (เทรดช้าเกินไป → ราคาเคลื่อนที่สวนทาง) efficiency rate ถูกกำหนดโดย tolerance ของลูกค้าต่อ market impact และ appetite for risk

**ความจริงที่สำคัญ:** "There are no solutions, only trade-offs." — ไม่มีทางออกที่สมบูรณ์แบบ มีแต่ trade-off เท่านั้น

### 3.2 Low Level Decision-Making — การตัดสินใจระดับต่ำ

เมื่อกำหนด optimal schedule แล้ว ขั้นตอนถัดไปคือ implementation — agent ต้อง blend กับ market ที่เหลือ (เป็น outlier = เปิดเผยความตั้งใจ = ถูก penalize)

**Dimensionality explosion:**
- Limit order book มี variable dimension และ high dimension — แต่ละ price level เป็น queue ของ orders จาก participants ต่างๆ ที่มีขนาดต่างกัน อาจยาวหรือว่างก็ได้
- Order book เปลี่ยนแปลงตลอดเวลาเมื่อ trade เกิดขึ้น, orders ถูกส่งและถอน
- ทุก market state ที่สังเกตได้สามารถ evolve ไปเป็น market states อื่นๆ ได้อย่างไม่จำกัด

**>Action space ที่ซับซ้อน:**
- Agent ต้องตัดสินใจว่าจะวาง order ที่ราคาเท่าไหร่, จำนวนเท่าไหร่, จะมีกี่ orders พร้อมกัน
- การวาง orders ที่ราคาห่างจาก market price (passive orders) จะอยู่ใน book จนกว่าราคาจะถึงจุดนั้น — action space เป็น dynamic และ complex
- อาจมี multiple trading venues และ order types ให้เลือก

**เปรียบเทียบกับเกมกระดาน:**
- Chess ประมาณ 40 steps, Go ประมาณ 200 steps
- Electronic trading algorithm ที่ reconsider options ทุก 1 วินาที = **3,600 steps ต่อชั่วโมง**
- การกระทำหนึ่ง (action) ของ electronic trading ไม่ใช่แค่เลือก piece ชิ้นเดียว แต่เป็น **collection ของ child orders** — หลาย orders พร้อมกันที่มี characteristics ต่างกัน (passive buy + aggressive buy = 1 action)
- Action space ขยายตัวแบบ exponential

**Non-local optimality:**
- ไม่ชัดเจนว่าจะ define efficiency ของ action แต่ละอันอย่างไร
- Trade ที่ดีหรือไม่ดีไม่ทราบจนกว่าจะ execute แล้ว (และอาจไม่ทราบแม้กระทั่งหลัง execute)
- Local optimality ไม่จำเป็นต้องเป็น global optimality — trade ที่ดูแย่ตอนนี้อาจกลายเป็นเทรดที่ดีมากเมื่อสิ้นวัน
- Objective ที่เป็นไปได้: agent ต้อง blend กับ market ได้ดี, ใช้ reward function ที่ measure best execution price เทียบกับ VWAP

### 3.3 Prior Work — งานที่มีอยู่ก่อนแล้ว

สรุปผลงานที่เกี่ยวข้อง:
- **Akbarzadeh et al. [2018]**: Online learning สำหรับ limit order book trading execution แต่จำกัดแค่ market orders
- **Nevmyvaka et al. [2006]**: Reinforcement learning สำหรับ optimized trade execution แต่ action space จำกัดแค่ single order (new orders cancel old ones)
- **Zhang et al. [2018]**: Summarize order book เป็น vector 40 มิติ (10 price levels ทั้ง bid/ask) ใช้ทำนาย market movement
- **Doering et al. [2017]**: ออกแบบ 4 matrices ที่ครอบคลุม order book, trades, new orders, order cancellations — แต่ dimensionality เพิ่ม 4 เท่าและข้อมูลsparse มาก

**ทิศทางวิจัยในอนาคต:** ต้องทำ dimensionality reduction ที่มีประสิทธิภาพ, สร้าง fixed-dimensional representation ของ variable dimension data, และต้องไม่จำกัด action space เท่ากับ human traders (มี child orders ที่ fixed number ใน fixed prices)

### 3.4 A Nano-Description of Our Approach — แนวทางของ J.P. Morgan

- กำลังใช้ **reinforcement learning-based limit order placement engine เจเนอเรชันที่ 2**
- ใช้ **hierarchical learning และ multi-agent training** ที่ leverage domain knowledge
- Train **local policies** (เช่น aggressive orders vs passive orders) บน local short-term objectives ที่มี rewards, step, time horizon ต่างกัน
- Local policies ถูก combine, และ longer-term policies เรียนรู้วิธี combine local policies
- **Inverse reinforcement learning** เป็นแนวทางที่มีอนาคตมาก — ใช้ประวัติ rollout ของ human และ algo policies ในตลาดเพื่อสร้าง local rewards

---

## บทที่ 4: Beyond Policy Learning in Development of AI for Electronic Trading

### 4.1 Policy Learning Algorithms
- Objective หลักของ RL คือ maximize aggregated rewards ที่ approximates business objective จริง
- Policy learning algorithms ที่ optimize parametrized action policy เป็น focus หลักของงานวิจัย RL
- มีการนำ算法มาใช้จริงใน electronic trading แล้ว (Akbarzadeh et al. 2018, Nevmyvaka et al. 2006)
- แต่ยังมี aspects อื่นของ RL ที่อยู่นอกเหนือ capability ของ policy learning algorithms

### 4.2 Hierarchical Decision Making — การตัดสินใจแบบลำดับชั้น

**ปัญหาหลักของ AI ใน electronic trading:**
- Time horizon ยาวมาก — client orders ใช้เวลาหลายนาทีถึงหลายชั่วโมง (บางทีหลายวัน) แต่ agent ต้องตัดสินใจทุกๆ ไม่กี่วินาทีหรือเร็วกว่า
- ความถี่ในการ sampling ต่ำเกินไปที่จะ integrate ข้อมูล market dynamics ทั้งหมด
- Decision-making ของ agent มี time-inhomogeneity — ไม่ได้ขับเคลื่อนด้วย clock แต่ respond กับผลของการกระทำของตัวเองและการเปลี่ยนแปลงของ environment

**Temporal abstraction เป็นปัญหาหลัก:**
- Frame skipping (ตัดสินใจทุกๆ หลาย time steps) อาจไม่สามารถใช้ได้ที่นี่
- **Semi-MDP (sMDP)** เป็น framework สำคัญสำหรับ discover temporal abstractive behavior ของ RL agents
- แต่ single policy สำหรับตัดสินใจว่าเมื่อไหรจะทำอะไรยัง sample-inefficient
- วิธีแก้: ใช้ sMDP ร่วมกับ **Hierarchical RL (HRL)** — decision model ประกอบด้วย layers ของ policies ที่มี decision frequency ต่างกัน (meta-policy → primitive policies)

**สถาปัตยกรรมที่ใช้:**
- ได้แรงบันดาลใจจาก **Kulkarni's rule-based deep HRL** [2016] — ใช้ rules จาก domain knowledge สร้าง meta-policy
- ยังมี end-to-end (rule-free) hierarchical RL ที่ temporal abstraction ของ meta-policy emerge จาก behavior/goal clustering
- **ปัญหาหลักที่ยังไม่แก้ได้:** agent's interpretation ของ sub-goals และ intrinsic rewards, collapse ของ temporal abstraction เมื่อ converge, sample efficiency ใน exploration-heavy environments, และ deep hierarchy

### 4.3 Algorithmic, Regulatory and Computational Challenges

- สิ่งแวดล้อมของ electronic trading agents มี complexity สูง, วิวัฒนาการตลอดเวลา, เปลี่ยนแปลงเร็ว
- complexity ที่เพิ่มขึ้นอาจ improve decision-making แต่อาจกระทบ computational performance และทำให้ deployment ไม่สามารถทำได้
- **Regulatory constraint สำคัญ:** trading algorithms ต้อง produce behaviors ที่ predictable, controllable, explainable — ต้องรักษา "orderly market conditions" และ operator ต้องอธิบายได้ว่า actions ให้ผลลัพธ์ที่ดีที่สุดสำหรับลูกค้าอย่างไร
- **Hierarchical approach ช่วยเรื่องนี้:** แยก groups ของการตัดสินใจที่ต้องการ sampling frequency และ granularity ต่างกัน → จัดการ complexity, เข้าใจ action ของ agent ได้ดีขึ้น

---

## บทที่ 5: Hierarchical Reinforcement Learning Scheme

### 5.1 Search-Based Optimization of Meta-Policy on Simulation-Heavy Learning Task

**ความท้าทายในการ train RL agent:**
- Episodic rollouts แต่ละตัว parallelize ไม่ได้เพราะมี feedback loop ระหว่าง agent กับ environment
- Gradient-based training มีปัญหา memory-heavy reservoir ของ experience pairs ที่ redundant และ noisy
- Good behaviors ถูกลืมระหว่าง training เว้นแต่ learning algorithm จะเป็น off-policy อย่างมาก
- Gradient optimization ที่ involve moving objective ไม่ guarantee สำเร็จ

**วิธีแก้ที่ใช้จริง:** Gradient-free optimization ด้วย parameter search algorithms — ยังเป็น practical choice ที่ดี
- ใช้ hyper-parameter optimization techniques ฝึก parametrized agent กับ episodic utility
- ปรับปรุง execution performance โดยไม่ต้อง design rewards ใหม่
- ใช้ parallelism ร่วมกับ early-stopping ของ uninteresting paths
- หวังจะพัฒนาต่อด้วย Bayesian approach สำหรับ early-stopping

### 5.2 Scalable Deep Reinforcement Learning for Low-Level Decision Processes

**ปัญหาที่ต้องแก้:**
- Partially observable environment
- Time horizons ที่ไม่ commensurable กันระหว่าง market dynamics, agent's observations, และ business objective
- State space ที่กว้างใหญ่
- Delayed และ staggered rewards

**Agent เปลี่ยน environment ของตัวเอง** — ต้อง train ใน simulated environments ที่ reproduce บาง property ของตลาดจริง แต่ยัง reproduce ไม่ได้ทั้งหมด (โดยเฉพาะ market's response ต่อ agent's actions)

**สถาปัตยกรรมและเครื่องมือ:**
- **Gorila architecture** [Nair et al. 2015]: แสดงว่า DQN สามารถ scale ได้
- **IMPALA** [Espeholt et al. 2018]: Scale A3C สำหรับ distributed deep RL
- **Open source RL frameworks:** OpenAI Baselines, ELF, Horizon, Dopamine, TRFL, Ray RLlib — ทำให้ state-of-the-art RL algorithms accessible กว้างขึ้น แต่ยังไม่ "production-ready" เท่า Deep Learning libraries (TensorFlow, PyTorch)

**Ray RLlib** เป็น framework ที่ทีมใช้:
- สร้างมาตั้งแต่ต้นสำหรับ distributed reinforcement learning
- ใช้ actor model programming pattern ที่พิสูจน์แล้วว่ามีประสิทธิภาพสำหรับ distributed computing
- Fault-tolerant — รองรับ RL experiments ที่มัก interrupted by faults
- Resource-aware scheduler — ให้ user กำหนด resource requirements (CPU, GPU) ได้

---

## บทที่ 6: Uncertainty of Outcomes and Insufficiency of Classical RL Theory

**ประเด็นหลัก:** ใน RL application ส่วนใหญ่ rewards ถูกสมมติว่า deterministic แต่ electronic trading agents ทำงานในสิ่งแวดล้อมที่ **uncertainty of outcomes เป็น built-in feature** ไม่ใช่ noise

**ทัศนคติที่แตกต่าง:**
- Data modelling culture / ML culture: มองว่า uncertainty เป็น "noise" ที่ซ้อนอยู่บน data-generating process ที่ซ่อนอยู่
- Algorithmic culture: "uncertainty ไม่ใช่ noise, มันคือวิธีการทำงานทั้งหมด" — ไม่สามารถ aggregate away ได้เพราะมันมีความสำคัญเชิงปฏิบัติ

**Multidimensional rewards:**
- ค่าของ outcomes ในการ electronic trading มีหลายมิติและหลายมิติไม่ commensurable กัน
- ต้องมี methodology ที่ robust สำหรับ incorporate hierarchy ของ soft constraints และ prohibited actions
- Standard RL ที่ agents เรียนรู้ actions ที่นำไปสู่ scalar-valued outcome ดีกว่าค่าเฉลี่ยไม่เพียงพอ — ต้องคำนึงถึง tails ของ distribution ด้วย

**Certainty Equivalent RL (CERL):**
- แก้ปัญหาโดยใช้ **utility functions** เพื่อ value multidimensional และ uncertain outcomes
- Agent เรียนรู้ good actions ใน **certainty equivalent sense** — uncertain outcomes ถูกจัดอันดับโดย taking expectation ของ utility function เหนือ future distribution
- **สูตร CE:** CE(π(ai|si)) = U^(-1) E[U(r_{i+1}(π(ai|si))) + max_{π} CE(π(a_{i+1}|s_{i+1}))]

**ผลลัพธ์ที่น่าสนใจ:**
- การใช้ utility functions ทำให้ agent มี "character" — มี risk preferences และ constraints ที่มาจาก overarching business objectives
- ถ้าลูกค้า risk-averse → uncertainty ที่เพิ่มขึ้นจะลด CE reward
- **Discount factor γ** เกิดขึ้นอย่างเป็นธรรมชาติใน CERL — ไม่ต้องตั้งเป็น exogenous parameter เหมือน classical RL แต่เป็นผลจาก distribution ที่กว้างขึ้นเมื่อมองไปไกลขึ้นในอนาคต (risk ที่เพิ่มขึ้น)

---

## บทที่ 7: Conclusion — คำถามที่ยังเปิดอยู่

ผู้เขียนสรุปว่ายังมีคำถามมากมายที่ยังไม่มีคำตอบ และหวังว่าบทความนี้จะเพิ่มมุมมองใหม่:

1. **Multidimensional rewards:** มี rigorous way ที่จะ account ไหม?
2. **Uncertain duration:** จะ incorporate แนวคิด processes ที่มีระยะเวลาไม่แน่นอนเข้ากับ MDP paradigm อย่างไร?
3. **Uncertain outcomes/rewards:** จะแก้ไขอย่างไร?
4. **Simulated training environments:** จะสร้าง realistic training environments สำหรับ agents ที่ทำงานในตลาดอย่างไร? วิธีที่เป็นไปได้คือสร้าง artificial environment ขนาดเต็มที่ reproduce ตลาดเป็น emergent phenomena จาก activities ของ heterogeneous agents หลายตัว
5. **Local vs global rewards:** จะ combine conflicting/complementary local และ global rewards ได้อย่างไร?
6. **Multi-time-scale agents:** นอกจากการใช้ domain knowledge แล้ว มี rigorous way ที่จะ design agents ที่ทำงานบนหลาย time scales ไหม?
7. **Scalability:** ควร train many agents สำหรับ environments ที่คล้ายกันแต่ต่างกัน หรือ one agent สำหรับทุก environment? Agents ที่ train ต่าง environment จะ benefit จาก skills ของกันและกันได้อย่างไร?
8. **Bellman's equation:** ใช้ได้แค่กับ processes ที่ global reward เป็น sequential aggregate ของ local rewards — มี approach ที่ general กว่านี้ไหม?
9. **Explainability:** จะมี balanced approach ที่ allow RL agents แก้ปัญหาซับซ้อนขึ้น แต่ยัง preserve ability ในการเข้าใจและอธิบายพฤติกรรมของ agent?

---

## ความเชื่อมโยงกับ Algorithmic Trading

### สำหรับระบบเทรดอัตโนมัติ:

1. **Trade-off ไม่ใช่ solution:** ทุกกลยุทธ์มี trade-off ระหว่าง market impact กับ market risk — ไม่มี sacred grail ที่สมบูรณ์แบบ

2. **Dimensionality reduction สำคัญมาก:** Limit order book มี variable dimension สูงมาก ต้องหา ways ที่จะ represent market state ใน fixed dimension โดยไม่สูญเสียข้อมูลสำคัญ — ตรงกับปัญหาที่เราเจอในการสร้าง feature vectors

3. **Hierarchical decision making ตรงกับ Multi-Timeframe analysis:** การใช้ H1 สำหรับ zone identification + M15 สำหรับ entry timing เป็น hierarchical approach แบบเดียวกัน — high-level policy (regime detection) → low-level policy (entry/exit)

4. **Uncertainty ไม่ใช่ noise:** Market uncertainty ไม่ใช่สิ่งที่ต้องกำจัด แต่เป็น fundamental property — การเข้าใจว่า reward มี distribution (ไม่ใช่ scalar fixed) จะช่วยให้ design risk management ที่ดีขึ้น

5. **CERL สำหรับ Risk Management:** แนวคิด certainty equivalent + utility function ตรงกับ position sizing ที่ risk-adjusted — ไม่ใช่แค่ maximize returns แต่ maximize utility ของ returns distribution

6. **Inverse RL สำหรับ Strategy Learning:** ใช้ประวัติการเทรดที่ผ่านมา (backtest results, human decisions) เพื่อเรียนรู้ reward function ที่ซ่อนอยู่ — ตรงกับ idea ของการเรียนรู้จาก Trading in the Zone principles

7. **Simulated environments สำคัญ:** การสร้าง realistic simulation สำหรับ training agents ยังเป็น open problem — ตรงกับปัญหาที่เราเจอในการสร้าง backtest ที่ realistic (gap protection, execution physics)

8. **Explainability เป็น regulatory requirement:** ไม่ใช่แค่ nice-to-have — ในบางภูมิภาค trading algorithms ต้อง explain ได้ว่าทำไมถึงทำแบบนั้น — เชื่อมกับ EA ที่ต้องมี logic ที่โปร่งใสและ debug ได้
