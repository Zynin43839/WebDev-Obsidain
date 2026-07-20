# สรุปภาษาไทย: Algorithm Trading using Q-Learning and Recurrent Reinforcement Learning

> ผู้เขียน: Xin Du, Jinjian Zhai, Koupin Lv (Stanford University)
> ที่มา: Microsoft Word - report3.doc

---

## ภาพรวม

เอกสารนี้เป็นงานวิจัยจาก Stanford University ที่เปรียบเทียบประสิทธิภาพของอัลกอริทึม Reinforcement Learning สองวิธีในการซื้อขายอัตโนมัติ ได้แก่ Q-Learning (Value Iteration) และ Recurrent Reinforcement Learning หรือ RRL (Policy Iteration) ผลลัพธ์เชิงประสบการณ์ชี้ว่า RRL มีเสถียรภาพและประสิทธิภาพดีกว่า Q-Learning โดยเฉพาะเมื่อใช้งานกับข้อมูลที่มีสัญญาณรบกวน และ Derivative Sharpe Ratio เป็น Value Function ที่ให้ผลลัพธ์ดีที่สุดในการสะสมกำไร

---

## บทที่ 1: Introduction

### ปัญหาและวัตถุประสงค์

ในโลกแห่งความจริง การซื้อขายมุ่งเน้นไปที่การเพิ่มประสิทธิภาพให้กับเกณฑ์ที่นักลงทุนให้ความสนใจ ไม่ว่าจะเป็นกำไรสะสม (Cumulative Profit), ค่า Utility เชิงเศรษฐกิจ (Economic Utility) หรืออัตราผลตอบแทน (Rate of Return) งานวิจัยนี้ศึกษาประสิทธิภาพของ Q-Learning Algorithm โดยใช้ Value Function ที่แตกต่างกันสามแบบ:

1. **Internal Profit** - กำไรสะสมตรงๆ
2. **Sharpe Ratio** - อัตราผลตอบแทนที่ปรับความเสี่ยงแล้ว
3. **Derivative Sharpe Ratio** - อัตราการเปลี่ยนแปลงของ Sharpe Ratio

ผลลัพธ์เชิงประสบการณ์ชี้ว่า Derivative Sharpe Ratio มีประสิทธิภาพเหนือกว่าอีกสองตัวเลือก โดยสามารถสะสมกำไรได้มากกว่าในกระบวนการ Value Iteration

### ข้อจำกัดของ Q-Learning

เนื่องจากลักษณะเฉพาะของปัญหาทางการเงิน โดยเฉพาะเมื่อมีค่า Transaction Cost เข้ามาเกี่ยวข้อง ระบบการซื้อขายจำเป็นต้องเป็น Recurrent และต้องสามารถประเมินประสิทธิภาพระยะสั้นได้ทันที เพื่อการจัดสรรเงินลงทุนแบบค่อยเป็นค่อยไป งานวิจัยนี้เสนอ Recurrent Reinforcement Learning (RRL) ซึ่งเป็น Adaptive Algorithm ที่ให้ผลลัพธ์ดีกว่า Q-Learning ในแง่ของกำไรสะสม

### ข้อสังเกตสำคัญ

- ผลการซื้อขายของ RRL มีเสถียรภาพมากกว่า Q-Learning เมื่อเผชิญกับข้อมูลที่มีสัญญาณรบกวน (Noisy Data)
- Q-Learning Algorithm ค่อนข้างอ่อนไหวต่อการเลือก Value Function ซึ่งอาจเป็นเพราะคุณสมบัติแบบ Recursive ของการเพิ่มประสิทธิภาพเชิงพลวัต (Dynamic Optimization)
- RRL Algorithm มีความยืดหยุ่นมากกว่าในการเลือก Objective Function และประหยัดเวลาในการคำนวณ

---

## บทที่ 2: Portfolio and Trading System Setup

### โครงสร้าง Portfolio (Section 2.1)

งานวิจัยนี้สร้างบัญชีการลงทุนตามทฤษฎี CAPM (Capital Asset Pricing Model) ซึ่งเป็นทฤษฎีสำคัญที่สุดในวงการการเงินในช่วงทศวรรษ 1960 หลักการสำคัญของ CAPM คือ:

- สมมติว่ามี Risk-Free Asset (เช่น เงินสด หรือ T-Bill) ที่ไม่มีความสัมพันธ์กับสินทรัพย์อื่น
- มี Market Portfolio เพียงตัวเดียวที่สามารถบรรลุระดับความเสี่ยงต่ำสุดสำหรับระดับผลตอบแทนใดก็ได้
- Market Portfolio คือ Weighted Sum ของสินทรัพย์เสี่ยงทั้งหมดในตลาดการเงิน ซึ่งมีการกระจายความเสี่ยงอย่างสมบูรณ์

Portfolio ของงานวิจัยนี้ประกอบด้วยส่วนผสมของ Riskless Asset (เงินสด) และ Risky Market Portfolio โดยมีการปรับสัดส่วน (Rebalance) อยู่ตลอดเวลา นักลงทุนสามารถเปิดสถานะ Long หรือ Short ของสินทรัพย์แต่ละตัว ซึ่งราคาสินทรัพย์มีเครื่องหมายว่า $z_i^t$ สำหรับสินทรัพย์ i ณ เวลา t

Decision Function ถูกนิยามว่า $F^{t-1}(\theta; I_t)$ ซึ่ง $\theta_t$ คือ Weight ที่เรียนรู้ได้ระหว่างสินทรัพย์เสี่ยงและไม่เสี่ยง และ $I_t$ คือข้อมูล Market Information ที่มีอยู่ ณ เวลานั้น

### กำไรและความมั่งคั่ง (Section 2.2)

ในกรณีที่ใช้ Market Portfolio เป็นสินทรัพย์เสี่ยง งานวิจัยใช้ Multiplicative Profits ในการคำนวณความมั่งคั่ง เนื่องจากสินทรัพย์แต่ละตัวมีการผันผวนตลอดเวลา

สูตรความมั่งคั่งสะสม $W_T$ ณ เวลา T คือ:

$$W_T = W_0 \cdot \prod_{t=1}^{T} (1 + F^{t-1} \cdot r_t^f + F^{t-1} \cdot (r_t^i - r_t^f) - |F^t - F^{t-1}| \cdot \delta)$$

โดยที่:
- $W_0$ คือความมั่งคั่งเริ่มต้น
- $r_t^f$ คือ Risk-Free Rate
- $F^{t-1}$ คือสัดส่วน Portfolio
- $\delta$ คือ Transaction Cost
- $r_t^i$ คือผลตอบแทนของสินทรัพย์เสี่ยง

### Performance Criterion (Section 2.3)

งานวิจัยใช้สองเกณฑ์หลักในการประเมินผล:

**Utility Function**: ใช้เพื่อแสดงความพึงพอใจของนักลงทุน ขึ้นอยู่กับค่า $\gamma$:
- $\gamma = 1$: นักลงทุนเป็นกลางต่อความเสี่ยง (Risk Neutral) - สนใจแค่กำไรสุทธิ
- $\gamma > 1$: นักลงทุนชอบเสี่ยง (Risk Seeking)
- $\gamma < 1$: นักลงทุนรังเกียจความเสี่ยง (Risk Averse) - ซึ่งเป็นสมมติฐานของ CAPM

งานวิจัยนี้ใช้ $\gamma = 0.5$ ในการจำลอง

**Sharpe Ratio**: เป็นเกณฑ์ที่ยอมรับกันอย่างกว้างขวางในอุตสาหกรรมการเงิน โดยคำนวณจาก Mean Return หารด้วย Standard Deviation ของผลตอบแทน

---

## บทที่ 3: Learning Theories

### Reinforcement Learning (RL)

Reinforcement Learning เป็นวิธีการปรับค่า Parameter ของระบบเพื่อเพิ่มผลตอบแทนที่คาดหวัง (Expected Payoff หรือ Reward) กระบวนการนี้สำเร็จผ่านการทดลองและข้อผิดพลาด (Trial and Error) ของสภาพแวดล้อมและกลยุทธ์การกระทำ ต่างจาก Supervised Learning ที่มีข้อมูลตัดสินใจให้แล้ว RL จะได้รับสัญญาณจากสภาพแวดล้อมว่าการกระทำปัจจุบันดีหรือไม่

RL Algorithm สามารถจำแนกเป็นสองประเภท:
1. **Policy Search** - ค้นหาและปรับปรุงนโยบายโดยตรง
2. **Value Search** - ค้นหาค่า Value Function ที่เหมาะสม

ในช่วง 20 ปีที่ผ่านมา Value Search Methods เช่น Temporal Difference Learning (TD-Learning) หรือ Q-learning เป็นหัวข้อหลักในสาขา แต่ Value Function Approach มีข้อจำกัดหลายประการ

### Q-Learning (Section 3.1)

Value Function $V_\pi(x)$ เป็นการประมาณค่าของผลตอบแทน Discounted ที่จะได้รับจากสถานะเริ่มต้น x จุดประสงค์คือปรับปรุง Policy $\pi$ ในแต่ละรอบของการ Iteration เพื่อเพิ่ม Value Function ให้สูงสุด ซึ่งต้อง满足 Bellman's Equation:

$$V_\pi(x) = \sum_{y,a} \pi(x,a) \cdot p_a(x,y) \cdot \{D(x,y,a) + r \cdot V_\pi(y)\}$$

โดยที่:
- $\pi(x,a)$ คือความน่าจะเป็นในการกระทำ a จากสถานะ x
- $p_a(x,y)$ คือ Transition Probability จากสถานะ x ไป y
- $D(x,y,a)$ คือ Intermediate Reward
- $r$ คือ Discount Factor

ใน Q-Learning จำเป็นต้อง Discretize States ออกเป็น {rise, fall} และ Action Set เป็น {buy, sell} ทำให้เกิด Transition Probability Matrix ขนาด 2x2 ที่คำนวณจากอัตราการเปลี่ยนแปลงราคาและน้ำหนัก Portfolio

**ข้อจำกัดของ Q-Learning**:
- **Bellman's Curse of Dimensionality** - ปัญหาจาก Discrete States และ Action Spaces ที่มีจำนวนมาก
- **Markov Decision Processes ไม่收敛** - เมื่อขยายไปใช้ Functional Approximation
- **ไม่เสถียร** - แม้จะมีสัญญาณรบกวนเล็กน้อยในข้อมูลก็ทำให้เลือก Optimal Policy ได้ไม่ดี

### Recurrent Reinforcement Learning - RRL (Section 3.2)

RRL Algorithm จะทำให้ Trading System $F_t(\theta)$ มีค่าเป็น Real Value (ไม่ต้อง Discretize) และจะหาค่า Parameter $\theta$ ที่เหมาะสมที่สุดเพื่อเพิ่ม Value Criterion $U_T$ ให้สูงสุด

**Gradient Ascent แบบ Batch**:

$$\frac{dU_T}{d\theta} = \sum_{t=1}^{T} \frac{dU_t}{dR_t} \cdot \frac{dR_t}{dF_t} \cdot \frac{dF_t}{d\theta}$$

โดยที่:
- $\frac{dU_t}{dR_t}$ คืออัตราการเปลี่ยนแปลงของ Utility ต่อ Returns
- $\frac{dR_t}{dF_t}$ คืออัตราการเปลี่ยนแปลงของ Returns ต่อ Portfolio Weight
- $\frac{dF_t}{d\theta}$ คือ Total Derivative ที่ขึ้นอยู่กับเส้นทาง (Path Dependent)

การอัพเดท Parameter ทำแบบ Online Stochastic Optimization:

$$\Delta\theta = \rho \cdot \frac{dU_t}{d\theta}$$

โดยใช้ Back-Propagation Through Time (BPTT) ในการประมาณ Total Derivative ซึ่งเป็นวิธีที่คำนวณได้มีประสิทธิภาพกว่า Value Iteration

---

## บทที่ 4: Results and Discussions

### Trading Signal (Section 4.1)

Trading Signal คือการตัดสินใจซื้อหรือขายของนักลงทุน ซึ่งถูกกำหนดย้อนกลับจากการเพิ่ม Value Function ให้สูงสุดในแต่ละช่วงเวลา:
- Buy = +1 (เพิ่มสัดส่วนสินทรัพย์เสี่ยง, เพิ่ม $\theta$)
- Sell = -1 (ลดสัดส่วนสินทรัพย์เสี่ยง, ลด $\theta$)

ผลลัพธ์การจำลองแสดงให้เห็นว่า Weight ของสินทรัพย์เสี่ยงและไม่เสี่ยงมีการปรับเปลี่ยนตาม Trading Signal ที่เกิดจากการแก้ปัญหาแบบ Maximization

### Q-Learning กับ Value Function ต่างๆ (Section 4.2)

ผลลัพธ์การจำลองแสดงให้เห็นว่า Cumulative Profit แตกต่างกันอย่างมีนัยสำคัญเมื่อใช้ Value Function ที่ต่างกัน:

- **Internal Profit**: ไม่เสถียร ผลลัพธ์แปรผันมากเมื่อข้อมูลมีสัญญาณรบกวน
- **Sharpe Ratio**: ให้ผลลัพธ์ดีกว่า Internal Profit แต่ยังมีความไม่เสถียร
- **Derivative Sharpe Ratio**: ให้ผลลัพธ์ดีที่สุด สามารถสะสมกำไรได้มากกว่าและเสถียรกว่า

Derivative Sharpe Ratio มีความคล้ายคลึงกับ Marginal Utility ในแง่ของความเต็มใจที่จะรับความเสี่ยงต่อการเพิ่มขึ้นหนึ่งหน่วยของ Sharpe Ratio Value Function นี้รวมถึงความแปรปรวนของข้อมูลและค่า Risk Aversion ของนักลงทุน ทำให้เหมาะสมกับการจำลองแบบ Probabilistic

### RRL กับ Optimization Functions ต่างๆ (Section 4.3)

RRL Algorithm ใช้ Stochastic Gradient Ascent ในการเพิ่ม Value Criterion $U_T$ โดยการปรับ Weight Parameter $\theta$

ผลลัพธ์ชี้ว่า:
- RRL มีเสถียรภาพดีกว่า Q-Learning เมื่อเผชิญกับข้อมูลที่ผันผวน
- ทั้ง Sharpe Ratio และ Derivative Sharpe Ratio ให้ผลลัพธ์ที่เสถียรและมีกำไรภายใต้สถานการณ์ราคาที่แตกต่างกัน
- Policy Iteration ของ RRL คำนวณได้มีประสิทธิภาพกว่า Value Iteration ของ Q-Learning

### ผลของการใช้ Backward Steps จำนวนต่างๆ (Section 4.4)

ในการปฏิบัติจริง แทนที่จะใช้ Derivative Terms ทั้งหมดจนถึงเวลา T งานวิจัยทดสอบโดยใช้จำนวน Backward Time Steps ที่แตกต่างกัน (2, 5, 10, 20 และ 40 ชั่วโมง) กับ S&P 500 Index ที่ Normalize แล้ว

ผลลัพธ์:
- RRL Algorithm มีการเพิ่มขึ้นอย่างสม่ำเสมอของความมั่งคั่งในทุกกรณี
- ยิ่งมีข้อมูลย้อนหลังมากขึ้น Cumulative Profit ก็เติบโตในอัตราที่สูงขึ้น
- เมื่อ Backward Step มากกว่า 10 แล้ว การเพิ่มข้อมูลย้อนหลังเพิ่มเติมไม่ได้ทำให้ประสิทธิภาพดีขึ้นอย่างมีนัยสำคัญ (ผลตอบแทนลดน้อยลง)

---

## บทที่ 5: Conclusion

### สรุปผลสำคัญ

1. **Value Function Selection สำคัญมาก** - โดยเฉพาะเมื่อใช้กับข้อมูลที่มีสัญญาณรบกวน Value Function ที่มีข้อมูลเกี่ยวกับ Noise (ความแปรปรวน) และ Dynamic Properties (Marginal Utility) จะให้ผลลัพธ์ดีกว่า Value Function แบบ Stationary และ Fixed

2. **RRL เหนือกว่า Q-Learning** - ในแง่ของความเสถียรและการคำนวณ RRL ดีกว่า Q-Learning:
   - ไม่ต้อง Discretize States (ลดปัญหา Curse of Dimensionality)
   - เสถียรกว่าเมื่อมีสัญญาณรบกวนในข้อมูล
   - ยืดหยุ่นกว่าในการเลือก Objective Function
   - ใช้ Stochastic Batch Gradient Ascent ได้มีประสิทธิภาพ

3. **Backward Information มีประโยชน์แต่มีขีดจำกัด** - การเพิ่มข้อมูลย้อนหลังช่วยกระบวนการเรียนรู้ แต่ผลตอบแทนจะลดน้อยลงเมื่อข้อมูลย้อนหลังมีมากพอ (เกิน 10 ช่วงเวลา)

### ความเชื่อมโยงกับ Algorithmic Trading สมัยใหม่

งานวิจัยนี้มีความสำคัญต่อสาขา Algorithmic Trading ในหลายประเด็น:

- **การเลือก Objective Function**: Derivative Sharpe Ratio ที่รวมทั้งความแปรปรวนและ Marginal Utility เข้าด้วยกัน เป็นแนวทางที่ควรใช้ในการออกแบบ Trading System สมัยใหม่
- **Policy Gradient Methods**: RRL เป็นต้นแบบของ Policy Gradient Methods ที่ใช้กันอย่างแพร่หลายใน RL สำหรับการเทรดในปัจจุบัน
- **Online Learning**: ความสามารถในการอัพเดท Parameter แบบ Online (ไม่ต้อง Train ใหม่ทั้งหมด) เป็นคุณสมบัติสำคัญสำหรับ Trading System ที่ต้องปรับตัวตามตลาดที่เปลี่ยนแปลง
- **Transaction Cost Integration**: การรวม Transaction Cost เข้าไปใน Value Function ตั้งแต่ต้น เป็นแนวทางที่ถูกต้องสำหรับระบบเทรดจริง

---

## อ้างอิง

งานวิจัยอ้างอิงผลงานสำคัญในสาขา Reinforcement Learning และ Algorithmic Trading 30 เรื่อง ซึ่งรวมถึง:
- Sutton & Barto (1998) - Reinforcement Learning textbook มาตรฐาน
- Watkins & Dayan (1992) - Q-Learning paper ต้นฉบับ
- Moody & Wu (1996) - Optimization of Trading Systems
- Tsitsiklis & Van Roy (1999, 2001) - Markov Decision Processes for Pricing
- Wierstra et al. (2009) - Recurrent Policy Gradients
