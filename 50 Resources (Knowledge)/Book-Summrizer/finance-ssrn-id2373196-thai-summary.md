# สรุปภาษาไทย: Algorithmic Trading with Information and Dynamic Learning

> **Source:** SSRN-id2373196
> **Authors:** Álvaro Cartea, Sebastian Jaimungal, Damir Kinzebulatov
> **Year:** 2015
> **Pages:** 1-28

---

## ภาพรวมของงานวิจัย

งานวิจัยนี้ศึกษาว่า **Informed Trader (IT)** — ผู้มีข้อมูลเชิงลึกเกี่ยวกับทิศทางราคาในอนาคต — จะออกแบบ **algorithmic trading strategy** ที่เหมาะสมที่สุดได้อย่างไร โดยผสมผสานการ **เรียนรู้จาก market dynamics** ( Bayesian learning from price innovations) และการเลือกใช้ **limit orders vs market orders** อย่างชาญฉลาด

จุดเด่นของงานนี้คือเป็น **งานแรก** (ในขณะนั้น) ที่ใช้ **stochastic control** ในการแก้ปัญหา algorithmic trading ที่มี dynamic learning จากข้อมูลหลายสินทรัพย์พร้อมกัน

---

## 1. บทนำและ Motivation

- **ข้อมูลคือทรัพย์สินที่มีค่า** — ผู้ที่ได้เปรียบข้อมูลคือผู้ทำกำไร
- ตลาดยุคใหม่ใช้ computer power มหาศาลในการประมวลผลข้อมูลและตัดสินใจเทรด
- IT มี **prior belief** เกี่ยวกับการเปลี่ยนแปลงราคา และสามารถ **อัพเดท prior ได้ตลอดเวลา** จาก price innovations
- กลยุทธ์ของ IT ขึ้นอยู่กับ:
  - **Inventory ที่สะสม** (long/short position)
  - **เวลาที่เหลือ** ก่อนปิด position
  - **ความแม่นยำ** ของ price predictions

### ปัญหาคลาสสิก 2 ประการใน literature
1. **Optimal liquidation/acquisition** — จะขาย/ซื้อหุ้นอย่างไรให้ประหยัดที่สุด
2. **Market making** — จะทำตลาดอย่างไรให้ได้ spread

งานนี้แก้ **ปัญหาที่ 3 ใหม่**: **Directional trading with information learning**

---

## 2. แบบจำลองราคาหุ้น (Stock Price Dynamics)

### แบบจำลองหลัก: Randomized Brownian Bridge (rBb)

สมการราคา midprice ของสินทรัพย์:

$$S_t = S_0 + \sigma_i \beta_{tT}^i + \frac{t}{T} D_i$$

โดยที่:
- $S_t$ = midprice ณ เวลา t
- $D_i$ = random variable ที่ encode **prior belief** ของ IT เกี่ยวกับ midprice ณ เวลา T
- $\beta_{tT}$ = standard Brownian bridge (เชื่อม midprice ปัจจุบันกับ future midprice)
- $\sigma_i$ = volatility parameter

### ข้อดีของแบบจำลองนี้
- **D_i ไม่ใช่ Normal ทั่วไป** — IT สามารถใช้ prior ที่ซับซ้อนได้ (เช่น mixture of Gaussians)
- **Learning เกิดขึ้นตลอดเวลา** — เมื่อสังเกต price innovations, IT อัพเดท posterior belief ของ D_i
- **Drift term $A_i(t,S)$** ไม่ใช่ constant แต่เป็น **non-linear** ที่ capture **non-linear co-integration** ระหว่างสินทรัพย์

### SDE ของ midprice

$$dS_t = A_i(t,S) dt + \sigma_i dW_t^i$$

โดย drift $A_i(t,S)$ มี 2 ส่วน:
1. **Conditional expectation** $E[D_i | S_t = S]$ — ค่าที่คาดหวังของ D_i ณ ราคาปัจจุบัน
2. **Mean-reversion term** $-\frac{S_t - S_0}{T-t}$ — แรงดึงกลับสู่ prior

### ตัวอย่างการเรียนรู้
- ถ้า IT เชื่อว่าราคาจะขึ้น → drift เป็นบวก → สะสม long position
- ถ้า IT เชื่อว่าราคาจะลง → drift เป็นลบ → สะสม short position
- ยิ่ง **volatility สูง** ข้อมูลยิ่ง **noisy** → learning ยิ่งยาก → strategy ยิ่ง conservative

---

## 3. กลยุทธ์การเทรดที่เหมาะสมที่สุด (Optimal Trading Strategy)

### โครงสร้างปัญหา

IT ต้องตัดสินใจ 2 อย่างพร้อมกัน:
1. **จะส่ง market order หรือ limit order?**
   - Market order = **guaranteed execution** แต่ **แพง** (จ่าย spread + fees)
   - Limit order = **ถูก/ฟรี** แต่ **ไม่แน่ใจว่าจะ fill** (ต้องรอ counterparty)

2. **จะซื้อหรือขาย?** (directional view)

### แบบจำลอง Portfolio
- IT เทรดใน **k สินทรัพย์** (k ≤ n) แต่ใช้ข้อมูลจาก **n สินทรัพย์** ทั้งหมด
- **Inventory constraint**: $q_i \leq q_t^i \leq \bar{q}_i$ (จำกัด position size)
- **Cash process** $X_t$ อัพเดทจาก:
  - Limit orders ที่ถูก fill (ได้ spread)
  - Market orders ที่ส่ง (จ่าย spread + fees)
- **Execution price**: 
  - Buy: $S_t + \frac{\Delta}{2} + \epsilon$ (จ่าย spread + fees)
  - Sell: $S_t - \frac{\Delta}{2} - \epsilon$

### Objective Function

$$H^\nu = E\left[ X_T + \sum_i \psi_i(q_T^i, S_T^i) - \phi_i \int_0^T (q_u^i)^2 du \right]$$

มี 3 ส่วน:
1. **Terminal cash** $X_T$
2. **Terminal liquidation cost** $\psi$ — ค่าใช้จ่ายในการปิด position สุดท้าย
3. **Running inventory penalty** $\phi$ — ลงโทษการถือ position มากเกินไป (risk aversion)

### QVI (Quasi-Variational Inequality)

ค่าฟังก์ชัน $H(t,X,S,q) = X + \sum q_i S_i + g(t,S,q)$ โดย $g$ แก้ QVI:

$$0 = \max \left\{ \text{HJB part} + \text{Limit order gains} + \text{Market order gains} \right\}$$

- **HJB part**: การเปลี่ยนแปลงจากราคาและการอัพเดท prior
- **Limit order gains**: เมื่อ limit order ถูก fill (ได้ spread $\frac{\Delta}{2}$)
- **Market order gains**: เมื่อส่ง market order (จ่าย $\frac{\Delta}{2} + \epsilon$)

### Boundary Conditions
- ตั้ง $|S^i| = S_0^i \pm \gamma\sigma\sqrt{T}$ โดย $\gamma = 5$
- เมื่อ $|S|$ ไกลมาก → $g$ เป็น linear → ไม่มี edge จาก information

---

## 4. พฤติกรรมและผลการดำเนินงาน

### 4.1 เปรียบเทียบ 3 ประเภท Trader

| ประเภท | Prior | Learning | ลักษณะเด่น |
|:------:|:-----:|:--------:|:----------|
| **IT** (Informed) | Mixture of Gaussians (pu=0.8, pd=0.2) | ✅ ได้ | สะสม long → unwind ช้า |
| **UL** (Uninformed Learner) | Mixture of Gaussians (pu=0.5, pd=0.5) | ✅ ได้ | ระหว่าง IT กับ UT |
| **UT** (Uninformed) | Normal(0, σ²T) | ❌ ไม่ได้ | Market maker 保守, mean-revert |

### พฤติกรรมการเทรดของ IT

**ช่วงต้นกลยุทธ์ (build-up)**:
- ส่ง **market buy orders** เพื่อสะสม long position (เพราะเชื่อว่าราคาจะขึ้น)
- Post **one-sided buy limit orders** เพื่อเก็บ spread

**ช่วงกลาง (inventory control)**:
- หยุดส่ง market orders
- เปลี่ยนเป็น **two-sided limit orders** เพื่อควบคุม inventory + เก็บ spread

**ช่วงท้าย (unwind)**:
- หยุด buy limit orders
- ใช้ **sell limit + market orders** เพื่อปิด position

### ความแตกต่างจาก Market Maker
- **IT/UL**: มี **asymmetry** ในการเทรด (buy มากกว่า sell ในช่วง build-up)
- **UT**: เทรด **symmetrically** ทั้ง 2 ข้าง เหมือน market maker (ไม่มี directional view)

### ผลการเปรียบเทียบ Performance

**IT เหนือกว่า UL และ UT** ทั้ง:
- **Mean P&L สูงกว่า** (กำไรเฉลี่ยมากกว่า)
- **Std Dev ต่ำกว่า** (ความเสี่ยงน้อยกว่า)
- **Second-order stochastic dominance** — IT ดีกว่า UL/UT ทุกมิติ

**ปัจจัยที่มีผลต่อ performance**:
- **Inventory bound ยิ่งกว้าง** → กำไรยิ่งมาก (เพราะ IT ใช้ leverage ได้มากขึ้น)
- **Running penalty $\phi$ ยิ่งสูง** → กำไรยิ่งน้อย (เพราะ strategy ยิ่ง conservative)
- **Volatility ยิ่งสูง** → กำไรยิ่งน้อย (because learning ยิ่งยาก)

### 4.2 เรียนรู้จาก 2 สินทรัพย์, เทรด 1 สินทรัพย์

**Scenario**: 2 สินทรัพย์ co-move (Gaussian mixture joint distribution)

| เรียนรู้จาก | เทรดใน | ผลลัพธ์ |
|:-----------:|:------:|:-------|
| 1 asset (high vol) | High vol | กำไรน้อย (learning ยาก) |
| 2 assets | High vol | **กำไรมากขึ้น** (learn from low vol asset ช่วย) |
| 1 asset (low vol) | Low vol | กำไรปานกลาง |
| 2 assets | Low vol | กำไรเพิ่มเล็กน้อย |

**ข้อค้นพบสำคัญ**: การเรียนรู้จาก **low-volatility asset ที่ co-move** ช่วยเพิ่มกำไรใน high-volatility asset ได้อย่างมีนัยสำคัญ

---

## 5. การปรับกลยุทธ์สำหรับ Adverse Selection

### ปัญหา Adverse Selection
- เมื่อ market order arrive → ราคาถูก push ไปในทิศทางนั้น
- ถ้า IT post limit order buy → ราคาอาจ **ลง** ทันที (เพราะ informed trader อื่น sell ใส่)
- เรียก price impact นี้ว่า **adverse selection cost**

### แบบจำลองที่แก้ไข

เพิ่ม **price impact term**:

$$dS_t = A(t,S)dt + \sigma dW_t + \eta(dN_t^+ - dN_t^-)$$

โดย $\eta$ = price impact constant

### ผลที่เกิดขึ้นกับกลยุทธ์

| ก่อนแก้ (ไม่มี adverse selection) | หลังแก้ (มี adverse selection) |
|:--------------------------------:|:---------------------------:|
| Post limit orders **asymmetrically** (buy มากกว่า) | Post limit orders **symmetrically** (ทั้ง 2 ข้างเท่ากัน) |
| เริ่ม directional ทันที | **รอ** ให้ price push ก่อนแล้วค่อย directional |

**คำอธิบาย**: Adverse selection ทำให้ IT **ระมัดระวังมากขึ้น** — ไม่ post limit order เอียงข้างใดข้างหนึ่งจนกว่าจะมี confirmation จาก price movement

---

## 6. ผลลัพธ์เชิงคณิตศาสตร์ (Convergence Results)

### จุดประสงค์
พิสูจน์ว่า **numerical scheme** ที่ใช้แก้ QVI **converge** จริงสู่ viscosity solution

### Finite Difference Scheme
- ใช้ **finite difference** บน grid ของ $(t, S)$
-  discretize QVI (24) ด้วย forward difference ในเวลา, central difference ในราคา
- Parameter $\kappa > 0$ เป็น robustness parameter (ให้ $\kappa \downarrow 0$)

### ผลลัพธ์หลัก

**Proposition 3 (Comparison Principle)**:
- ถ้า $u \leq v$ (subsolution ≤ supersolution) → comparison ยังใช้ได้กับระบบ PDE

**Proposition 4 (Discrete Convergence)**:
- Finite difference scheme 满足 conditions (C1)-(C4):
  - **(C1)**: Monotonicity ของ discrete operator
  - **(C2)**: Continuity
  - **(C3)**: Consistency
  - **(C4)**: Existence ของ discrete solution

**ข้อสรุป**: จาก Briani et al. (2012) — discrete solution $u^{\epsilon,h}$ converge สู่ **unique viscosity solution** $u$ ของ QVI

### ข้อจำกัด
- เนื่องจาก inventory เป็น **discrete** (ไม่ใช่ continuous) → ไม่สามารถใช้ classical convergence results ตรงๆ ได้
- ต้อง **construct proof จาก first principles** โดยใช้ extension ของ Barles & Soganidis (1991)

---

## 7. บทสรุปและข้อค้นพบสำคัญ

### ข้อค้นพบหลัก 5 ประการ

1. **Learning จาก price dynamics ช่วยเพิ่มกำไร** — แต่ขึ้นอยู่กับ quality ของ information

2. **Volatility เป็นตัวแปรสำคัญ**:
   - Volatility ต่ำ → learning ง่าย → strategy aggressive (directional)
   - Volatility สูง → learning ยาก → strategy conservative (market making)
   - Volatility สูงมาก → learning หมดสภาพ → strategy เหมือน market maker

3. **Learning จาก asset อื่นที่ co-move ช่วยได้มาก** — โดยเฉพาะกับ volatile asset

4. **Adverse selection ทำให้ strategy ระมัดระวังขึ้น** — ไม่ post limit order เอียงข้าง

5. **Numerical scheme converge จริง** — พิสูจน์แล้วด้วย viscosity solution theory

### ผลลัพธ์เชิง Performance
- **IT outperforms UL และ UT** ทั้ง mean return และ risk (second-order stochastic dominance)
- Profitability มาจาก **2 แหล่ง**:
  1. **Capital appreciation** (กำไรจากราคาที่เปลี่ยน)
  2. **Spread earning** (กำไรจากการ round-trip trades)
- ยิ่ง **inventory bound กว้าง** ยิ่งกำไรมาก (แต่มี limit จาก assumption ว่าไม่ move market)

---

## 8. ความเชื่อมโยงกับ Algorithmic Trading ยุคปัจจุบัน

### สำหรับระบบ SMC-BOCPD ของเรา

| แนวคิดจากงานวิจัย | การประยุกต์ใช้ |
|:-------------------|:-------------|
| **Informed Trader learning** | Regime detection (BOCPD) ทำหน้าที่เหมือน IT ที่ update belief |
| **Limit vs Market order** | ปัจจุบันใช้ market order เท่านั้น — ควรเพิ่ม limit order logic |
| **Inventory constraint** | มี position sizing แล้ว แต่ไม่มี running penalty |
| **Volatility-Adaptive** | ATR-based SL/TP ทำหน้าที่คล้ายกัน — volatility สูง → strategy 保守 |
| **Multi-asset learning** | BOCPD regime จาก 1 symbol ไม่ได้ cross-asset learning |
| **Adverse selection** | Spread + slippage ปัจจุบันไม่ได้ model อย่างเป็นทางการ |

### ข้อจำกัดของงานวิจัยนี้
- สมมติ **zero interest rate** (ไม่คิด carry cost)
- สมมติ **constant spread** (spread จริง time-varying)
- สมมติ **strategy small enough** ไม่ move market (ไม่适用于 institutional size)
- ไม่มี **transaction cost แบบ realistic** (spread + commission + slippage จริงซับซ้อนกว่า)

---

## 9. สรุปสูตรและสมการสำคัญ

| สมการ | ความหมาย |
|:-----:|:--------|
| $S_t = S_0 + \sigma \beta_{tT} + \frac{t}{T}D$ | Randomized Brownian Bridge price model |
| $dS_t = A(t,S)dt + \sigma dW_t$ | SDE ที่ IT เทรด (drift จาก learning) |
| $A(t,S) = E[D|S_t=S] - \frac{S_t - S_0}{T-t}$ | Drift = posterior expectation − mean-reversion |
| $H = X + \sum q_i S_i + g(t,S,q)$ | Value function decomposition |
| $0 = \max\{HJB + LO + MO\}$ | QVI สำหรับ g |
| $dS_t = ... + \eta(dN^+ - dN^-)$ | Price impact model (adverse selection) |

---

## 10. คำถามสำคัญสำหรับการศึกษาต่อ

1. **ถ้า volatility สูงมากจริง strategy จะเป็นอย่างไร?** — คำตอบ: กลายเป็น pure market maker
2. **IT ควรเทรดกี่สินทรัพย์พร้อมกัน?** — คำตอบ: ยิ่งมากยิ่งดี (cross-asset learning)
3. **Running inventory penalty ควรตั้งเท่าไร?** — ขึ้นกับ risk aversion; สูงมาก → conservative เกินไป
4. **Adverse selection ทำให้ strategy เปลี่ยนอย่างไร?** — ทำให้ symmetric limit orders มากขึ้น
5. **Numerical scheme นี้ converge เร็วแค่ไหน?** — ขึ้นกับ grid density (ε, h) — trade-off ระหว่าง accuracy กับ computation time
