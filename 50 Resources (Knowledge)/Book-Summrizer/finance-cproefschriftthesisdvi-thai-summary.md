# สรุปวิทยานิพนธ์: ผลการดำเนินงานของกฎการซื้อขายทางเทคนิคในตลาดการเงิน

> **ผู้เขียน:** P.W. van Foreest (Tinbergen Institute, Erasmus Universiteit Rotterdam / Universiteit van Amsterdam / Vrije Universiteit Amsterdam)
> **ประเภท:** วิทยานิพนธ์ปริญญาเอก (Proefschrift), 322 หน้า, 6 บท

---

## ภาพรวม

วิทยานิพนธ์นี้เป็นการศึกษาเชิงประจักษ์และทฤษฎีอย่างกว้างขวางเกี่ยวกับประสิทธิผลของกฎการซื้อขายทางเทคนิค (technical trading rules) แบบ trend-following ที่มีต่อตลาดการเงินหลายแห่ง ผู้เขียนทดสอบสมมติฐาน EMH (Efficient Markets Hypothesis) รูปแบบ weak-form โดยใช้กฎการซื้อขายหลายพันรายการ ครอบคลุมสินค้าโภคภัณฑ์ หุ้นรายตัว ดัชนีหุ้น และอัตราแลกเปลี่ยนทั่วโลก

---

## บทที่ 1: ภาพรวมและทบทวนวรรณกรรม

### 1.1 การวิเคราะห์พื้นฐาน vs การวิเคราะห์ทางเทคนิค

- **การวิเคราะห์พื้นฐาน (Fundamental Analysis):** มีรากฐานจาก firm-foundation theory ของ John B. Williams ได้รับความนิยมจาก Benjamin Graham และ Warren Buffett วิเคราะห์ปัจจัยเศรษฐกิจมหภาค อุตสาหกรรม และรายบริษัท เพื่อคำนวณมูลค่าพื้นฐานของสินทรัพย์
- **การวิเคราะห์ทางเทคนิค (Technical Analysis):** ศึกษาความเคลื่อนไหวของราคาในอดีตเพื่อทำนายอนาคต เกิดจาก Dow Theory (Charles Dow, 1890s) ได้รับการพัฒนาต่อโดย Hamilton, Rhea, Edwards, Magee, Wilder และ Murphy
- **ข้อวิจารณ์:** นักวิชาการกล่าวหาว่าเป็น "voodoo finance" (Malkiel) แต่ในทางปฏิบัติ ผลสำรวจ Bank of England (1988) พบว่ากว่า 90% ของผู้ค้าสกุลเงินใช้การวิเคราะห์ทางเทคนิค
- **ข้อได้เปรียบทางเทคนิค:** ใช้งานง่าย ถูก ทดสอบได้ง่ายกว่า fundamental analysis ต้องการแค่ข้อมูลราคา ปริมาณ และเงินปันผล

### 1.2 ทบทวนวรรณกรรม: EMH กับการซื้อขายทางเทคนิค

- **研究成果 ยุคแรก (1930s-1960s):** Cowles (1933) ไม่พบพลังทำนายของนักวิเคราะห์; Working, Kendall, Roberts (1934-1959) พบว่าราคาสุ่ม (random walk); Fama (1965, 1970) สรุปว่ามีหลักฐานสนับสนุน EMH อย่างกว้างขวาง
- **Alexander (1961):** ใช้ filter rules กับ DJIA พบว่าราคามีแนวโน้ม (trends persist) แต่ค่าคอมมิชชันทำให้กำไรหมด
- **Brock, Lakonishok & LeBaron (1992):** จุดเปลี่ยนสำคัญ — ทดสอบกฎ 26 รายการกับ DJIA (1897-1986) พบผลกำไรสำคัญ ใช้ bootstrap technique ยืนยันว่าไม่สามารถอธิบายด้วย random walk หรือ AR/GARCH
- **LeBaron (2000):** ผลของ Brock ลดลงหลังปี 1986 — กำไรหายไปในตลาดหุ้นและอัตราแลกเปลี่ยนช่วงทศวรรษ 1990
- **การแก้ data snooping:** White (2000) Reality Check และ Hansen (2001) SPA test — Sullivan et al. (1999) พบว่าผลของ Brock ทน data snooping ได้ในช่วง 1897-1986 แต่หายไปหลัง 1986

### 1.3 โครงสร้างวิทยานิพนธ์

- **บทที่ 2:** กฎ 5,350 รายการ ทดสอบกับสัญญาล่วงหน้าโกโก้ (LIFFE vs CSCE)
- **บทที่ 3:** กฎ 787 รายการ กับ DJIA และหุ้น 34 ตัว
- **บทที่ 4:** กฎ 787 รายการ กับ AEX-index และหุ้น 50 ตัว
- **บทที่ 5:** กฎ 787 รายการ กับดัชนีหุ้นหลัก 50 ประเทศ
- **บทที่ 6:** แบบจำลองทฤษฎี heterogeneous agents (fundamentalists vs MA traders)

---

## บทที่ 2: ผลสำเร็จและความล้มเหลวของกฎการซื้อขายทางเทคนิคในตลาดสัญญาล่วงหน้าโกโก้

### แรงจูงใจ
ผู้เขียนร่วมมือกับ Guido Veenstra จาก Unicom International B.V. บริษัทค้าโกโก้ชั้นนำของเนเธอร์แลนด์ คำถามสำคัญ: "สัญญาล่วงหน้าโกโก้สามารถทำนายได้ด้วยการวิเคราะห์ทางเทคนิคหรือไม่?"

### ข้อมูลและวิธีวิจัย
- **ข้อมูล:** ราคา settlement ของสัญญาล่วงหน้าโกโก้ 160 สัญญา จาก CSCE (นิวยอร์ก) และ LIFFE (ลอนดอน) มกราคม 1982 - มิถุนายน 1997
- **อัตราแลกเปลี่ยน:** Sterling-Dollar (WM/Reuters) และอัตราดอกเบี้ย Certificate of Deposit ของ UK และ US
- **กฎการซื้อขาย:** 5,350 รายการ — ประกอบด้วย Moving Average (MA), Trading Range Break-out (TRB), Filter Rules (FR) แต่ละแบบมี variant ต่างๆ (band filter, time delay, fixed holding period, stop-loss)
- **การสร้าง time series ต่อเนื่อง:** ใช้การต่อ returns (ไม่ใช่ราคา) ที่จุด rollover เพื่อหลีกเลี่ยง price jumps จาก time premium

### ผลลัพธ์หลัก

| ตลาด | สัดส่วนกฎที่มีกำไร (หลังค่าใช้จ่าย) | สัดส่วนมีนัยสำคัญทางสถิติ |
|:-----:|:------:|:------:|
| **LIFFE โกโก้ (£)** | **58.3%** | 26.6% (Buy-Sell > 0) |
| **CSCE โกโก้ ($)** | 12.2% | แทบไม่มี |
| **GBP/USD** | มีกำไรทางสถิติ | มีนัยสำคัญ |

### การวิเคราะห์ Bootstrap

ทดสอบ null model 4 แบบ:
1. **Random walk with drift:** ผล LIFFE ดีกว่าสุ่มอย่างมีนัยสำคัญ
2. **AR(16):** มี autocorrelation ที่ lag 16 แต่ bootstrap ยังยืนยันผล
3. **Exponential GARCH:** ยังยืนยันผล
4. **Structural break in trend:** อธิบายผล LIFFE ได้ — ช่วง 1983-1987 มี structural break ที่สอดคล้องกับแนวโน้มค่าเงินปอนด์

### คำอธิบายความสำเร็จ vs ความล้มเหลว

ผู้เขียนให้คำอธิบายที่สำคัญมาก:

> "ความสัมพันธ์ระหว่างประสิทธิผลของ technical trading กับลักษณะ trend และ volatility ของราคา"

| ตลาด | Trend | Volatility | ผล technical trading |
|:-----:|:-----:|:----------:|:----:|
| CSCE โกโก้ | อ่อน | สูง | ไม่มีผล (volatility กลบ trend) |
| GBP/USD | อ่อน | ต่ำ | มีสถิติ แต่ไม่มีเศรษฐกิจ (กำไรไม่คุ้มค่าใช้จ่าย) |
| **LIFFE โกโก้** | **แข็ง** | **สูง** | **มีทั้งสถิติและเศรษฐกิจ** ✅ |

ปัจจัยเสริม: อัตราแลกเปลี่ยนปอนด์-ดอลลาร์ส่งผล LIFFE (ปอนด์) แต่ไม่ส่งผล CSCE (ดอลลาร์) — ช่วง 1983-1987 ค่าเงินปอนด์แข็ง/อ่อนสอดคล้องกับแนวโน้มโกโก้ LIFFE ทำให้ trends แข็งแรงขึ้น

---

## บทที่ 3: ผลการดำเนินงานของกฎการซื้อขายทางเทคนิคในหุ้นที่จดทะเบียนใน DJIA

### ข้อมูลและวิธีวิจัย
- **ข้อมูล:** DJIA และหุ้น 34 ตัวที่เคยอยู่ใน DJIA (มกราคม 1973 - มิถุนายน 2001)
- **กฎการซื้อขาย:** 787 รายการ (MA 425 + TRB 190 + FR 172)
- **เกณฑ์การเลือกกลยุทธ์:** (1) Mean return criterion, (2) Sharpe ratio criterion
- **ค่าใช้จ่าย:** 0%, 0.10%, 0.25%, 0.50%, 0.75%, 1% ต่อการซื้อขาย
- **แบ่งยุค:** 1973-1986 (ก่อนวิกฤต) vs 1987-2001 (หลังวิกฤต)

### ผลลัพธ์หลัก

**In-sample (ไม่แก้ data snooping):**
- ทุกช่วงเวลาและทุกข้อมูล สามารถหา rules ที่เอาชนะ buy-and-hold ได้แม้มีค่าใช้จ่าย
- CAPM: ไม่มีค่าใช้จ่าย → α บวกอย่างมีนัยสำคัญสำหรับหุ้นเกือบทั้งหมด; ค่าใช้จ่ายเพิ่ม → α ลดลง
- **β < 1 สำหรับหุ้นส่วนใหญ่** → กลยุทธ์มีความเสี่ยงน้อยกว่า buy-and-hold

**Data snooping (White RC + Hansen SPA):**
- **Mean return criterion:** ค่าใช้จ่าย ≥ 0.25% → ไม่มีหุ้นตัวไหนผ่าน test
- **Sharpe ratio criterion:** ผลดีกว่าเล็กน้อย แต่ยังไม่ชัดเจน
- **สรุป:** ไม่พบหลักฐานว่า trend-following rules สามารถทำนายทิศทางราคา DJIA ได้อย่างมีนัยสำคัญหลังแก้ data snooping

**Out-of-sample (recursive optimizing):**
- ไม่พบกำไรที่มีนัยสำคัญทางเศรษฐกิจและสถิติ

---

## บทที่ 4: ผลการดำเนินงานของกฎการซื้อขายทางเทคนิคในหุ้นที่จดทะเบียนใน AEX-index

### ข้อมูล
- AEX-index และหุ้น 50 ตัว (มกราคม 1983 - พฤษภาคม 2002)
- สกุลเงิน: Dutch Guilder
- กฎการซื้อขาย: 787 รายการ (เหมือนบทที่ 3)

### ผลลัพธ์หลัก — แตกต่างจาก DJIA อย่างชัดเจน

**In-sample:**
- ทุกหุ้นหา strategy เอาชนะ buy-and-hold ได้
- CAPM: แม้มีค่าใช้จ่าย 1% ยังมีหุ้น ~50% ที่ α > 0 อย่างมีนัยสำคัญ
- **β < 1 อย่างชัดเจน** สำหรับหุ้นส่วนใหญ่ → ลดความเสี่ยงจริง

**Data snooping:**
- **Mean return criterion:** ค่าใช้จ่าย ≥ 0.10% → ไม่ผ่าน RC/SPA test
- **Sharpe ratio criterion:** ✅ ประมาณ 1/3 ของหุ้นผ่าน SPA test แม้มีค่าใช้จ่าย 1% — **พบว่ามี predictive ability จริงสำหรับหุ้นกลุ่มนี้**

**Out-of-sample:**
- ค่าใช้จ่าย 0.10%: มากกว่า 40% ของหุ้นมี α > 0 อย่างมีนัยสำคัญ
- ค่าใช้จ่าย 0.50%: ลดลงมาก แทบไม่เหลือ

### ข้อสรุปสำคัญ
หุ้นในตลาดหลักทรัพย์อัมสเตอร์ดัมมีผล的技术 trading ที่ดีกว่าหุ้นใน DJIA อย่างมีนัยสำคัญ — หลังแก้ data snooping, ค่าใช้จ่าย และความเสี่ยงแล้ว ยังพบ predictive ability สำหรับหุ้นบางตัว

---

## บทที่ 5: ผลการดำเนินงานของกฎการซื้อขายทางเทคนิคในดัชนีหุ้นหลักของ 50 ประเทศ

### ข้อมูล
- ดัชนีหุ้นหลัก 50 ประเทศ + MSCI World Index (มกราคม 1981 - มิถุนายน 2002)
- สองกรณี: (1) ผู้ค้าท้องถิ่น (สกุลเงินท้องถิ่น) (2) ผู้ค้าสหรัฐฯ (แปลงเป็น USD)

### ผลลัพธ์หลัก

**In-sample:**
- ทุกดัชนีมี strategy ที่เอาชนะ buy-and-hold ได้ (ทั้ง mean return และ Sharpe ratio criteria)
- CAPM: แม้มีค่าใช้จ่าย 1% ยังมีดัชนีมากกว่าครึ่งที่ α > 0 อย่างมีนัยสำคัญ
- **β < 1 สำหรับดัชนีส่วนใหญ่** — กลยุทธ์มีความเสี่ยงต่ำกว่า buy-and-hold

**Data snooping:**
- **Mean return criterion:** ค่าใช้จ่าย ≥ 0.25% → ไม่ผ่าน RC/SPA test
- **Sharpe ratio criterion:** ✅ SPA test ยัง reject null สำหรับ ~1/4 ของดัชนี (ส่วนใหญ่เอเชีย) แม้มีค่าใช้จ่าย 1%

**Out-of-sample (recursive optimizing):**
- กำไร out-of-sample พบมากในตลาดเอเชีย ละตินอเมริกา ตะวันออกกลาง และรัสเซีย
- **ไม่พบกำไรในสหรัฐฯ ญี่ปุ่น และยุโรปตะวันตกส่วนใหญ่**
- หลัง CAPM: มีกำไร risk-corrected out-of-sample สำหรับค่าใช้จ่าย ≤ 0.25% ในตลาดเอเชีย ละตินอเมริกา ตะวันออกกลาง และรัสเซีย

### ข้อสรุปสำคัญ
ตลาดเกิดใหม่มีผล的技术 trading ที่ดีกว่าตลาดพัฒนาแล้ว — เหตุผลเป็นไปได้ว่า market inefficiency ยังมีอยู่มากกว่า

---

## บทที่ 6: แบบจำลองทฤษฎี Heterogeneous Agents — Fundamentalists กับ Moving Average Traders

### แรงจูงใจ
คำถามสำคัญจากบทที่ 2-5: ทำไม technical trading ถึงได้ผลในบางตลาดและไม่ได้ผลในตลาดอื่น? แบบจำลองนี้พัฒนาต่อจาก Brock & Hommes (1998) เพื่ออธิบายการอยู่รอดร่วมกันของ trader สองกลุ่ม

### แบบจำลอง

**สมมติฐานพื้นฐาน:**
- ตลาดมี N agents ที่เลือกจากกลุ่มความเชื่อ 2 แบบ: (1) Fundamentalists ทำนายว่าราคาจะกลับสู่มูลค่าพื้นฐาน, (2) Moving Average Traders ใช้ EMA cross-over เป็นสัญญาณ
- _fraction ของ traders ที่เลือกแต่ละ belief เปลี่ยนตามผลการดำเนินงานที่ผ่านมา (adaptive learning via discrete choice model)
- **Constant Relative Risk Aversion (CRRA)** — ต่างจาก Brock-Hommes ที่ใช้ CARA

**Demand function ของ MA traders:**
- $y^{MA}_t = f(x_{t-1}) = 2\gamma \frac{x_{t-1}}{1 + x_{t-1}^2}$ ซึ่ง $x_{t-1} = \frac{P_{t-1} - MA_{t-1}}{\lambda MA_{t-2}}$
- ซื้อเมื่อ $P > MA$, ขายเมื่อ $P < MA$
- Position น้อยลงเมื่อราคาห่างจาก MA มาก (เหมือน reality)

### ผลวิเคราะห์เสถียรภาพ (Stability Analysis)

- **Steady state:** $P^* = P^{fund} = D/r_f$ (มูลค่าพื้นฐาน), MA = P, demand ทั้งสองกลุ่มเป็น 0
- **Hopf bifurcation:** steady state อาจไม่เสถียรเมื่อ intensity of choice (β) สูงพอ — ราคาเบี่ยงเบนจากมูลค่าพื้นฐาน
- **ผลสำคัญ:** fundamentalists ไม่สามารถขับ MA traders ออกจากตลาดได้ แม้ค่าใช้จ่ายเป็น 0

### ผลการจำลอง (Numerical Simulations)

**ไดนามิกของราคา:**
- ราคาเคลื่อนที่ช้าออกจากมูลค่าพื้นฐาน (ช้า) แล้วกลับมาเร็ว
- Fundamentalists เปลี่ยนทิศทางแนวโน้ม แต่ chartists ดันราคาสกลับมูลค่าพื้นฐาน
- **ปริมาณการซื้อขาย (volume) สอดคล้องกับแนวโน้ม** — volume เพิ่มตามแนวโน้มหลัก, ลดลงเมื่อเปลี่ยนทิศทาง

**ลักษณะทางสถิติของผลตอบแทน:**
- Autocorrelation ของผลตอบแทน ≈ 0 ✅
- Fat tails (excess kurtosis) ✅
- Volatility clustering — ไม่พบในแบบจำลองพื้นฐาน ❌ (ต้องใช้ dynamic noise)

**ผลสำคัญสำหรับนักเทรด:**
- แม้ fundamentalists จะมีข้อมูลที่ "ถูกต้อง" แต่ไม่สามารถขับ technical traders ออกได้
- ทั้งสองกลุ่ม coexist ตลอดกาล โดยความสำคัญสัมพัทธ์เปลี่ยนตามเวลา
- ตลาดเกิด oscillation ระหว่างช่วงที่ราคาใกล้ fundamental (fundamentalists ชนะ) กับช่วง trend-following (chartists ชนะ)

---

## ข้อสรุปรวมของวิทยานิพนธ์

### 1. Technical Trading มีผลจริงแต่มีเงื่อนไข

ผลลัพธ์จากทุกบทชี้ว่า:
- มีกฎ trend-following ที่สามารถเอาชนะ buy-and-hold ได้ ทั้งแบบ in-sample และ out-of-sample
- กำไรสามารถทนค่าใช้จ่ายการซื้อขายได้ในระดับหนึ่ง (≤ 0.25-0.50% ต่อรายการ)
- กำไรไม่ได้เป็นแค่ reward สำหรับความเสี่ยง (CAPM α > 0 + β < 1 สำหรับข้อมูลส่วนใหญ่)

### 2. Data snooping เป็นภัยคุกคามร้ายแรง

- หลังแก้ data snooping (White RC, Hansen SPA) ผลกำไรลดลงอย่างมาก
- Hansen SPA test ดีกว่า White RC เพราะไม่ bias จาก poor models
- Sharpe ratio criterion ให้ผลดีกว่า mean return criterion หลังแก้ data snooping

### 3. ตลาดเกิดใหม่ > ตลาดพัฒนาแล้ว

- Technical trading มีผลดีที่สุดในตลาดเอเชีย ละตินอเมริกา ตะวันออกกลาง และรัสเซีย
- ตลาดสหรัฐฯ ญี่ปุ่น และยุโรปตะวันตกส่วนใหญ่: ผลกำไรหายไปหลังแก้ data snooping และค่าใช้จ่าย
- คำอธิบาย: ตลาดเกิดใหม่มี inefficiency มากกว่า (ข้อมูลน้อยกว่า, liquidity ต่ำกว่า, arbitrage ยากกว่า)

### 4. ความสัมพันธ์ระหว่าง Trend-Volatility-Autocorrelation

จากการศึกษาโกโก้ (LIFFE vs CSCE):
- **Trend แข็ง + Volatility สูง** = technical trading ได้ผลทั้งสถิติและเศรษฐกิจ ✅
- **Trend อ่อน + Volatility ต่ำ** = มีสถิติแต่ไม่มีเศรษฐกิจ ❌
- **Trend อ่อน + Volatility สูง** = ไม่มีผลทั้งสอง ❌

### 5. แบบจำลองทฤษฎีสนับสนุนผลเชิงประจักษ์

- Fundamentalists กับ technical analysts อยู่ร่วมกันตลอดกาล
- การ interact ของทั้งสองกลุ่มสร้าง cycle ของราคาที่เบี่ยงเบนจากมูลค่าพื้นฐาน
- นี่เป็นกลไกที่อธิบายว่าทำไม trends จึงเกิดขึ้นและ persists ได้ในตลาดจริง

---

## ความเชื่อมโยงกับ Trading Agent System ปัจจุบัน

วิทยานิพนธ์นี้มีความเชื่อมโยงโดยตรงกับระบบ Automated Trade Agent Hub:

1. **SMC (Smart Money Concepts) strategies** ที่ใช้ในระบบ — มีรากฐานเดียวกันกับ technical trading rules ในวิทยานิพนธ์นี้ (OB detection, MSS, trend-following)
2. **Regime detection** — วิทยานิพนธ์พบว่า market regime (trend strength, volatility) กำหนดว่า technical trading จะได้ผลหรือไม่ ตรงกับ regime_detector.py ในระบบ
3. **Transaction cost sensitivity** — วิทยานิพนธ์ยืนยันว่าค่าใช้จ่ายคือตัวแปรสำคัญที่สุดตัวหนึ่ง ตรงกับ cost_validation ที่ทำใน Session 4.23-4.24
4. **Per-symbol strategy selection** — วิทยานิพนธ์พบว่าผลกำไรต่างกันมากในแต่ละตลาด/หุ้น ตรงกับ strategy_selector.py ที่เลือกกลยุทธ์ตาม symbol
5. **Out-of-sample validation** — วิทยานิพนธ์ใช้ recursive optimizing/testing ซึ่งเป็นแนวทางเดียวกับ forward testing daemon
