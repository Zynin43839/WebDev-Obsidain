# บทสรุปภาษาไทย: Machine Learning for Stock Selection

**ต้นฉบับ:** Microsoft Word - Machine Learning for Stock Selection.docx (SSRN, https://ssrn.com/abstract=3330946)
**เนื้อหา:** 35 หน้า, ครอบคลุมการประยุกต์ใช้ Machine Learning สำหรับการคัดเลือกหุ้น (Stock Selection) โดยเน้นหลักการ Feature Engineering และ Forecast Combinations เพื่อลด Overfitting

---

## สารบัญ

1. [บทนำ — ทำไม Machine Learning จึงเหมาะกับ Stock Selection](#1-บทนำ)
2. [Perils of Overfitting — ปัญหาใหญ่ของ ML ในงาน Finance](#2-perils-of-overfitting)
3. [Forecast Combinations — การรวมพยากรณ์จากหลายแหล่ง](#3-forecast-combinations)
4. [Feature Engineering — หัวใจสำคัญของระบบ](#4-feature-engineering)
5. [Big Data กับ Machine Learning](#5-big-data)
6. [Case Study — ตัวอย่างเชิงปฏิบัติ](#6-case-study)
7. [ผลลัพธ์ — ML vs. Traditional Methods](#7-ผลลัพธ์)
8. [Feature Importance — ปัจจัยสำคัญที่สุด](#8-feature-importance)
9. [Time-Varying Exposures — ความสัมพันธ์ที่เปลี่ยนแปลงตลอดเวลา](#9-time-varying-exposures)
10. [Conclusion — บทสรุปและข้อคิด](#10-conclusion)
11. [Glossary — ศัพท์เทคนิคที่ใช้](#11-glossary)
12. [Relevance สำหรับ Algorithmic Trading](#12-relevance-สำหรับ-algorithmic-trading)

---

## 1. บทนำ

งานวิจัยนี้นำเสนอกรอบการทำงาน (Framework) สำหรับการนำ Machine Learning Algorithms (MLAs) มาใช้คัดเลือกหุ้น โดยมีจุดประสงค์หลัก 2 ประการ:

1. **Discuss Feature Engineering** — อธิบายปัญหาที่นักปฏิบัติงาน (Practitioners) ต้องเผชิญเมื่อนำ ML มาใช้เลือกหุ้น
2. **Demonstrate Forecast Combinations** — แสดงให้เห็นถึงประโยชน์ของการรวมพยากรณ์จากหลาย Algorithm และ Training Windows

### ทำไม ML จึงเหนือกว่า Linear Regression?

- **Uncover Complex Patterns** — ML สามารถค้นหาความสัมพันธ์ที่ซับซ้อนแบบ Non-linear ซึ่งการวิเคราะห์แบบ Linear ไม่สามารถตรวจจับได้
- **Multi-collinearity Robustness** — ML มีประสิทธิภาพดีกว่า OLS Regression เมื่อมีตัวแปรต้น (Predictors) ที่มีความสัมพันธ์กันสูง (Multi-collinearity)
- **No Structure Assumption** — ML ไม่ได้กำหนดโครงสร้างความสัมพันธ์ล่วงหน้า จึงสามารถค้นหา Pattern ที่ซ่อนอยู่ได้มากกว่า

### งานวิจัยที่เกี่ยวข้อง

- **Wang & Luo (2012)**: ใช้ AdaBoost ทำนาย Equity Returns
- **Batres-Estrada (2015), Takeuchi & Lee (2013)**: Deep Learning กับ Financial Time Series
- **Heaton, Polson & Witte (2016)**: Deep Learning กับ Smart Indexing
- **Gu, Kelly & Xiu (2018)**: ตรวจสอบ efficacy ของ ML techniques ใน Asset Pricing — พบว่า Non-linear estimators มี accuracy ดีกว่า OLS อย่างมีนัยสำคัญ
- **Miller et al. (2013, 2015)**: Classification Trees ดีกว่า Linear Regression ในการทำนาย Factor Returns และการ Combine Linear + Non-linear models ให้ผลดีขึ้นอีก

### ความแตกต่างจากงานวิจัยอื่น

ผู้เขียนเลือกที่จะเน้น **Cross-section of Returns** โดยไม่รวม Macro Variables (เช่น Equity Risk Premium) โดยอ้างว่า:
- ลด Noise
- ลดความเสี่ยง Overfitting
- ใช้เฉพาะ Individual Equity Characteristics (194 ปัจจัย)

---

## 2. Perils of Overfitting

Overfitting คือปัญหาที่ใหญ่ที่สุดของ ML ในงาน Finance เนื่องจาก:

- **Low Signal-to-Noise Ratio** — ความสัมพันธ์ระหว่าง Factor กับ Return มี Noise สูงมาก เปรียบเทียบกับ Image Recognition ที่มี Error Rate < 1% แต่ Stock Return Forecasting มี Noise มากกว่าหลายเท่า
- **High Dimensionality** — มี Factor จำนวนมากที่อาจเป็นตัวทำนาย ( Predictor) ทำให้มิติของปัญหาสูง
- **Pattern ที่พบในข้อมูล Past 未必 generalize ไปยังข้อมูลใหม่**

### ตัวอย่าง Overfitting (Figure 1)

เมื่อใช้ Gradient Boosted Regression Tree กับข้อมูล Simulated:
- **In-sample error** ลดลงเรื่อยๆ และแทบเป็น 0 หลังจาก 400 boosting iterations
- **Out-of-sample error** ลดลงช่วงแรก แต่ **เพิ่มขึ้น** หลังจาก ~50 iterations — จุดนี้คือ Overfitting เริ่มต้น

ข้อคิดสำคัญ: การ评估 ผลการพยากรณ์บน Training Set ทำให้ผลลัพธ์ดูดีเกินจริง

### วิธีแก้ 2 วิธีหลัก

1. **Forecast Combinations** — รวมพยากรณ์จากหลาย Algorithm และ Training Windows
2. **Feature Engineering** — ออกแบบ Input ให้มี Signal-to-Noise Ratio สูงขึ้นก่อนเข้าสู่ Algorithm

---

## 3. Forecast Combinations

### หลักการ

- ถ้าหลาย Algorithm ที่ Different ได้ Training ด้วย Data ต่างกัน ต่างก็พบ Pattern เดียวกัน → มั่นใจได้ว่าพยากรณ์นั้น Robust ไม่ใช่ Overfit
- **Clemen (1989)**: "Combining multiple forecasts leads to increased forecast accuracy... in many cases one can make dramatic performance improvements by simply averaging the forecasts"
- **Makridakis & Hibon (2000)**: Forecast combinations มัก outperform แม้แต่ constituent forecast ที่ดีที่สุด

### 4 Dimensions of Diversity

1. **Different Algorithm Classes** — _combine ผลจากหลาย algorithm_ (เช่น Random Forest + Neural Network + SVM) เพราะแต่ละ algorithm จับ Relationship คนละแบบ
2. **Different Training Windows** — ใช้ข้อมูลคนละช่วงเวลา (temporal, seasonal, conditional) — ช่วยลด Variance และเพิ่ม Risk-adjusted Returns
3. **Different Factor Libraries** — แบ่ง Factor Library เป็นกลุ่มย่อย เพื่อให้ Algorithm สำรวจ Pattern ที่หลากหลายขึ้น
4. **Different Horizons** — ปัจจัยต่างกันสำคัญในช่วงเวลาต่างกัน (Fundamental สำคัญใน Long-term, Technical สำคัญใน Short-term)

### Ensemble Methods

| ประเภท | กลไก | ข้อดี | ข้อเสีย |
|--------|------|-------|--------|
| **Bagging** (Bootstrap Aggregating) | -fit estimator บน Random Subsets แบบ Independent, รวมด้วย Equal Weighting | ลด Overfitting, Run แบบ Parallel ได้ | ไม่ได้เน้น Learner ที่ดีกว่า |
| **Boosting** | Sequential fitting, ให้น้ำหนักมากกับ Misclassified Observations | จัดการ Bias ได้ดี | ต้อง Tune Parameters ระวัง, Run Sequential ช้ากว่า |
| **Dropout** (Neural Networks) | Drop Elements of Layers ระหว่าง Training | คล้าย Model Averaging, ลด Overfitting | เป็น Technique เฉพาะ Neural Networks |

### ข้อแนะนำในการเลือก Algorithm

- ใช้ Algorithm ที่มี Track Record ดีกับ Noisy Data
- ใช้ Software Library ที่มีการ Test แล้ว (เช่น Scikit-learn, XGBoost, TensorFlow)
- **Parameterize อย่างระมัดระวัง** — จำกัดความสามารถของ Algorithm ในการ Over-fit
- **ห้าม Optimize In-sample** — ใช้ Cross-validation สำหรับ Financial Data (แนะนำ Chapter 7 ใน de Prado, 2018)

---

## 4. Feature Engineering

Feature Engineering คือจุดที่ Domain Knowledge ไหลเข้าสู่กระบวนการ เป็นวิธีที่มีประสิทธิภาพที่สุดในการ Overcome Overfitting เพราะเพิ่ม Signal-to-Noise Ratio ก่อนเข้าสู่ Algorithm

### 4.1 What Are We Forecasting?

- **แนะนำให้ Forecast Discrete Variables** (Outperformer vs. Underperformer) แทนการ Predict Returns ตรงๆ เพราะมี Noise น้อยกว่า
- ใช้ 2 ประเภท (Binary Classification) ดีกว่า 3 ประเภท เพราะแต่ละ Category ที่เพิ่มจะเพิ่มความเสี่ยง Overfitting
- **Risk-adjusted Returns** ควรใช้เป็น Basis ในการจัด Category (ใช้ CAPM, Carhart 4-factor, Fama-French 5-factor, หรือ MSCI-Barra Model)

### 4.2 นิยาม Categories

- Rank Stocks เป็น Outperformer/Underperformer สำหรับแต่ละวันใน Training Set
- ควรจัดกลุ่ม **Within Sectors/Industries** เพื่อลด Noise
- Factor ก็ Percentile Rank ภายใน Region/Industry Bucket เช่นกัน

### 4.3 Forecast Horizon

- **Short Horizon** → Low Capacity, High Turnover Strategy
- **Long Horizon** → High Capacity, Low Turnover Strategy
- Horizon สั้น = มี Training Period มากขึ้น = ช่วย uncover subtle patterns ในข้อมูล Noise
- สำหรับ Stock Selection แนะนำ Daily ถึง Quarterly

### 4.4 Which Algorithms?

- Wikipedia ระบุ ML Methods มากกว่า 100 วิธี และเพิ่มขึ้นเรื่อยๆ
- ไม่สามารถรู้ Relationship ระหว่าง Returns กับ Features ล่วงหน้าได้
- **การ Combine หลาย Algorithm = Insurance against Misspecification**
- เน้น Algorithm ที่มีการใช้สำเร็จกับ Noisy Data แล้ว

### 4.5 Training Windows

| ประเภท | ข้อมูลที่ใช้ | สถานการณ์ที่เหมาะ |
|--------|-------------|-------------------|
| **Recent** | 12 เดือนล่าสุด | ต้องการความต่อเนื่องของสภาวะตลาดปัจจุบัน |
| **Seasonal** | เดือนเดียวกันย้อนหลัง 10 ปี | ต้องการจับ Seasonality |
| **Hedge** | Bottom Half of Performance จาก Recent + Seasonal | ต้องการ Hedge ความเสี่ยงจาก 2 ชุดแรก |

- ชุดข้อมูลเดียวกันอาจอยู่ใน Training Set หลายชุด (เช่น เดือนเดียวกันปีที่แล้วจะอยู่ใน Recent ด้วย)
- ควร Train แยกตาม Region (US, Japan, Europe, Asia ex Japan)
- ไม่ควรแบ่งข้อมูลย่อยเกินไป (เช่น US Tech vs. Japan Auto) เพราะเพิ่มความเสี่ยง Overfitting

### 4.6 Which Factors?

- เลือก Factor ที่เกี่ยวข้องกับ:
  - **Economic Success** (Fundamental Factors) — สำหรับ Long-term
  - **Supply and Demand** (Technical Factors) — สำหรับ Short-term
- ML จัดการ Collinear Data ได้ดี จึงสามารถใส่ Factor ที่คล้ายกันได้ แต่ไม่ควรใส่太多的จน Runtime นาน
- **Industry Neutralization** — Force Factor ให้เป็น Industry-neutral ช่วยลด Variance โดยไม่ลด Average Factor Returns

---

## 5. Big Data

ผู้เขียนระบุชัดเจนว่า:
- ML เป็นที่รู้จักในการ Extract Signal จาก Big Data (เช่น Sentiment จาก Text, Social Media)
- แต่บทความนี้ **ไม่ได้เน้น Big Data** แต่เน้นว่า ML สามารถใช้กับ **Quant Signals ที่เป็นที่รู้จักอยู่แล้ว** ได้ดีกว่า Traditional Techniques

---

## 6. Case Study — ตัวอย่างเชิงปฏิบัติ

### 6.1 Data

| รายละเอียด | ข้อมูล |
|------------|--------|
| กลุ่มหลักทรัพย์ | Small, Medium, Large Cap Stocks |
| จำนวนหุ้นเฉลี่ย | 5,907 หุ้น/เดือน |
| ตลาด | 22  Developed Markets |
| Factor Library | 194 Factors (from IHS Markit) |
| ช่วงเวลา | 1994-2016 (Walk-forward เริ่ม 2004) |
| Forecast Horizon | Monthly |
| ความถี่ข้อมูล | Monthly |
| Lag ระหว่าง Model Estimation กับ Trading | 2 วัน |

### 6.2 Factor Library ประกอบด้วย

| กลุ่ม Factor | จำนวน |
|-------------|-------|
| Deep Value | 21 |
| Relative Value | 18 |
| Earnings Quality | 10 |
| Earnings Momentum | 26 |
| Historical Growth | 25 |
| Liquidity | 35 |
| Management Quality & Profitability | 29 |
| Technical (Price-based) | 29 |
| **รวม** | **194** |

### 6.3 Feature Engineering Workflow

1. **สร้าง 3 Training Sets** สำหรับแต่ละ Region (US, Japan, Europe, Asia ex Japan):
   - Recent: ข้อมูล 12 เดือนล่าสุด
   - Seasonal: ข้อมูลเดือนเดียวกันย้อนหลัง 10 ปี
   - Hedge: Bottom Half of Performance จาก Recent + Seasonal

2. **Percentile Rank Factors** ภายใน Region/Industry Bucket

3. **Risk-adjust Excess Returns** = Excess Return / 100-day Volatility

4. **Classify Winners vs. Losers** = Split ครึ่งหนึ่งภายใน Region/Industry Bucket

### 6.4 Algorithms ที่ใช้ (4 ประเภท x 3 Training Windows = 12 Models)

| Algorithm | รายละเอียด | Library |
|-----------|-----------|---------|
| **AdaBoost + Bagging** | Decision Stumps (max depth=1), 50 boosting iterations, learning rate=1, Bagging 20 random forecasts | Scikit-learn |
| **Gradient Boosted Trees (XGBoost)** | Learning rate=0.05, 300 boosting iterations, max depth=3 | XGBoost |
| **Neural Network (MLP)** | 4 layers (มี Bottleneck), Dropout 20% หลัง Layer 1, activation='tanh' | TensorFlow |
| **SVM + Bagging** | RBF Kernel, Bagging 20 models, ใช้ Decision Function แทน Probability (เร็วกว่า) | Scikit-learn |

**หมายเหตุ:**
- AdaBoost และ SVM ช้าและ Parallelize ยาก → ใช้เป็น Base Learner ใน Bagging เพื่อให้ Run แบบ Parallel ได้
- Parameters เลือกจากช่วงก่อนปี 2004 (ไม่ Optimize In-sample)
- tanh activation ดีกว่า ReLU ในการ Test กับ Training Set นี้

### 6.5 Composite Signal

1. สำหรับแต่ละ Model (12 Models) → ได้ Probability of Outperformance สำหรับแต่ละหุ้นและเดือน
2. Percentile Rank 12 Probabilities ภายใน Region/Industry Bucket
3. **เฉลี่ย** → Composite ML Signal

### 6.6 Benchmarks เปรียบเทียบ

1. **OLS Benchmark** — Linear Regression ที่ใช้ Factor ทุกตัวที่อยู่ใน MLA ทั้งหมด, Train 12 เดือนล่าสุด, Lag 1 วัน
2. **Top-10 Benchmark** — หา 10 Factor ที่มี Sharpe Ratio สูงสุดถึงจุดเวลาแต่ละจุด, Equal-weight 评分

---

## 7. ผลลัพธ์

### 7.1 Portfolio Construction

- แบ่งหุ้นเป็น Deciles ตาม Composite Signal
- Decile Spread = Return ของ Decile สูงสุด - ต่ำสุด (Long/Short Portfolio)
- 2 วิธี weighting:
  - **1/n** — Equal-weight หุ้นในแต่ละ Decile
  - **1/vol** — Scale Position กลับกันกับ 100-day Standard Deviation

### 7.2 Forecast Combination Benefit

ผลลัพธ์สำคัญ:
- **Composite Forecast ดีกว่าทุก Sub-model** ทั้งในแง่ Decile Spread และ Rank IC
- Neural Network และ Hedge Training Set มักมี IC สูงสุดใน Sub-model
- **Benefit จาก Training Window Diversity** ชัดเจนมาก

### 7.3 Risk Factor Analysis (Table 3)

**Panel A — Correlation กับ Market Factor:**
- US Equal-Weight: Correlation สูงเชิงลบ (-0.57 กับ Mkt-RF) → มัก Long Low-Beta, Short High-Beta
- US Risk-Weight (1/vol): Correlation ต่ำกว่า (-0.20) — ลด Market Exposure ได้

**Panel B — Performance (Gross of Transaction Costs):**

| พอร์ตฟอลิโอ | Excess Return (%) | Alpha 4-factor (%) |
|------------|:-----------------:|:------------------:|
| US Equal-Weight | 1.60 | 1.84 |
| **US Risk-Weight** | **1.95** | **2.01** |
| ROW Equal-Weight | 1.10 | 1.17 |
| ROW Risk-Weight | 1.50 | 1.65 |
| Top-10 Benchmark | 1.23 | 1.17 |
| OLS Benchmark | 1.10 | 1.03 |

**Panel C — หัก Transaction Costs (30 bps round-trip):**
- Alphas ยังคงมีนัยสำคัญสูง

**ข้อสังเกตสำคัญ:**
- ML Portfolios Load เชิงลบบน HML (Value) และ SMB (Small Size) แต่ไม่มีนัยสำคัญ
- ผลลัพธ์เชิงบวกไม่ได้มาจาก Common Risk Factors
- Alpha มีนัยสำคัญมากกว่า Excess Return → Four-factor Model อธิบาย Returns ของ ML Strategy ไม่ได้
- **ML Composite ดีกว่า HFRI EMN Index** (0.24%/เดือน) อย่างมีนัยสำคัญ (>1%/เดือน หลังหัก Transaction Cost)

### 7.4 Fama-MacBeth Regressions (Table 4)

- ML Composite Forecast มีความสัมพันธ์กับ Returns อย่างมีนัยสำคัญแม้ควบคุม Factor ยอดนิยมหลายตัว
- Factor ควบคุมหลายตัวยังมีนัยสำคัญเล็กน้อย — อาจเพราะ Algorithm พิจารณาเฉพาะ Point-in-time Information

### 7.5 Long vs. Short Analysis

- **Long Side (Top Decile)**: มี Excess Return สูงกว่า Short Side — สมเหตุสมผลเพราะหุ้น outperform Treasury Bills
- **4-factor Alpha**: มีนัยสำคัญทั้ง Long และ Short ทั้งใน US และ ROW
- **Decile Spread Alpha ดีกว่า Sum ของ Long + Short** เพราะ Feature Engineering ช่วย Industry-neutralize และ Volatility-adjust ทำให้ Industry Risk และ Idiosyncratic Volatility ลดลง

---

## 8. Feature Importance

วิเคราะห์จาก Gradient Boosted Classification Tree ในช่วง 10 ปีสุดท้ายของ US Sample:

| อันดับ | Feature | คำอธิบาย |
|-------|---------|---------|
| 1 | **IndRelRtn5d** | Industry-relative return 5 วันล่าสุด |
| 2 | **IndRelRtn4w** | Industry-relative return 4 สัปดาห์ล่าสุด |
| 3 | **Sigma60D** | Standard Error ของ Residuals จาก Regression 60 วัน |
| 4 | **SIP** | Shares Sold Short เป็น % ของ Shares Outstanding |
| 5 | **VolDiff_PC** | ความแตกต่างระหว่าง Implied Volatility ของ Put และ Call ATM |
| 6 | **DR_1MStd** | 1-Month Return Volatility ล่าสุด |
| 7 | **StockRating** | Consensus Analyst Rating |
| 8 | **SalesAccel4Q** | Slope ของ Year-over-Year Sales Growth |
| 9 | **ChgOPM** | Operating Profit Margin ล่าสุด - 4 quarters ago |
| 10 | **CEROE** | Cash Flow from Operations / Book Value of Common Equity |

**เปรียบเทียบกับ Gu, Kelly & Xiu (2018):**
- ตรงกัน: Price-trend factors, Volatility, Liquidity สำคัญ
- ต่างกัน: SIP (Short Interest), VolDiff_PC (Put-Call Vol Spread), Financial Statement-based Features ก็อยู่ใน Top 10

---

## 9. Time-Varying Exposures

การวิเคราะห์ Cross-Sectional Correlation ระหว่าง ML Composite กับ Factor ของ Carhart Model:

- **Average Exposure ไม่ใช่ภาพรวมทั้งหมด** — มี Time Variation ชัดเจนในทุก Factor
- **Momentum**: Correlation เป็นลบใน 3 ช่วง (ต้น Sample, 2009-2011, 2013-2015) — ตรงกับช่วง Momentum Crash ของ Daniel & Moskowitz (2016)
- **Size และ Beta**: Average เป็นลบ แต่ Variations สูง และบางช่วงเป็นบวก

**ข้อคิดสำคัญ:**
- Factor Exposures ของ ML Strategy มีความผันผวนมากกว่า Linear Factor Models อย่างมีนัยสำคัญ
- บ่งชี้ว่า **Factor Timing** และ **Non-linear Effects** มีส่วนในผลลัพธ์เชิงบวก

---

## 10. Conclusion

### ประเด็นหลัก

1. **Overfitting คือปัญหาใหญ่ที่สุด** — Low Signal-to-Noise Ratio ในงาน Finance ทำให้ ML ง่ายต่อการ Overfit
2. **Feature Engineering ช่วยได้มาก** — เพิ่ม Signal-to-Noise Ratio ก่อนเข้า Algorithm
3. **Forecast Combinations ลด Noise** — รวมพยากรณ์จากหลาย Algorithm และ Training Windows
4. **MLA ยังไม่สามารถ Replace Human Experts** — ต้องใช้ Domain Knowledge อย่างมาก

### ผลลัพธ์สำคัญ

- ML Algorithms สามารถใช้ Firm Characteristics มากมายทำนาย Stock Returns โดยไม่ Overfit
- Feature Engineering + Forecast Combinations → ผลลัพธ์ดีกว่า OLS และ Top-10 Factor Benchmark อย่างมีนัยสำคัญ
- ผลลัพธ์ Robust สำหรับ Risk Adjustments หลายแบบ
- ใช้ได้ทั้ง US และ Developed Market Regions อื่นๆ
- Factor Exposures มี Time Variation สูง → Non-linear Effects และ Factor Timing มีส่วนในผลลัพธ์

---

## 11. Glossary — ศัพท์เทคนิค

| ศัพท์ | คำจำกัดความ |
|------|-------------|
| **Activation Function** | ฟังก์ชันที่กำหนด Output ของ Node ใน Neural Network — Non-linear functions (ReLU, tanh, sigmoid) ทำให้ NN เรียนรู้ Pattern ที่ซับซ้อน |
| **Neural Networks / Deep Learning** | Algorithm ที่เลียนแบบสมองมนุษย์ — จัดเรียง Units เป็น Layers และเชื่อมต่อกัน สามารถเรียนรู้ Relationship ที่ซับซ้อน แต่ง่ายต่อการ Overfit |
| **Bagging** | Bootstrap Aggregating — รวม Forecast จาก Base Algorithms ที่ Training บน Random Subsets แบบ Equal Weighting — ช่วยลด Overfitting |
| **Bottleneck Layer** | Layer ที่มี Neurons น้อยกว่า Layer บนและล่าง — บังคับให้ Network ลด Dimensionality ของ Features |
| **Boosting** | รวม Forecast จาก Base Algorithms โดยให้น้ำหนักมากกับ Model ที่สำเร็จ — เรียนรู้จาก Data ได้มีประสิทธิภาพกว่า Bagging แต่เสี่ยง Overfit มากกว่า |
| **CART** | Classification and Regression Tree — พื้นฐานของ ML Algorithms หลายตัว สามารถตรวจจับ Hierarchical Relationships (Non-linear) |
| **Dropout** | เทคนิคใน Neural Network ที่ Drop Elements ออกขณะ Training — คล้าย Model Averaging ช่วยลด Overfitting |
| **Gradient Boosted Trees** | ใช้ Decision Trees เป็น Base Learners, Trees ถัดไป Training บน Residuals — Learning Rate และ Boosting Iterations เป็น Parameters หลัก |
| **MLP** | Multi-Layer Perceptron — Feed-forward Neural Network ที่มี 3 ชั้นขึ้นไป |
| **Random Forest** | รวม CART Models หลายตัวด้วย Averaging — ลด Overfitting ของ Individual Trees |
| **RBF Kernel** | Radial Basis Function Kernel — ใช้ใน SVM เหมาะกับปัญหา Non-linear |
| **SVM** | Support Vector Machine — Classifier ที่มีประสิทธิภาพใน High Dimensional Space ใช้ได้ทั้ง Linear และ Non-linear Kernels |

---

## 12. Relevance สำหรับ Algorithmic Trading

### บทเรียนสำคัญสำหรับการสร้าง Trading System

1. **Feature Engineering สำคัญกว่า Algorithm Selection** — วิธีการสร้าง Features และการปรับข้อมูลมีผลมากกว่าการเลือก Algorithm ที่ "ดีที่สุด"

2. **Forecast Combinations = Ensemble Strategy** — การรวมพยากรณ์จากหลาย Model ลดความเสี่ยง Overfitting ได้อย่างมีนัยสำคัญ เปรียบเทียบกับ:
   - Multi-Strategy Trading System (SMC Range + Breakout Retest + OB+FVG)
   - Multi-Timeframe Confirmation (H1 structure + M15 entry timing)

3. **Risk-adjusted Returns เป็น Basis ที่ดีกว่า Raw Returns** — การใช้ Volatility-adjusted Returns ช่วยลด Noise และทำให้ Algorithm เรียนรู้ได้มีประสิทธิภาพขึ้น

4. **Industry Neutralization ลด Variance** — การ Neutralize Factor ในระดับ Industry/Region ช่วยให้ Signal สะอาดขึ้น เปรียบเทียบกับ Regime-based Filtering ใน trading system ปัจจุบัน

5. **Time-Varying Exposures สำคัญ** — Factor ที่มีความสัมพันธ์กับ Returns ไม่คงที่ตลอดเวลา → ต้องมี Mechanism สำหรับ Adapting (เช่น Regime Detection, Walk-forward Optimization)

6. **Overfitting Prevention** — ใช้ Multiple Training Windows, Multiple Algorithms, Cross-validation สำหรับ Financial Data

7. **Transaction Cost Analysis สำคัญ** — ML Strategy ที่ดูดีอาจถูกกินกำไรด้วย Transaction Costs → ต้อง Validate หลังหัก Costs เสมอ

### การประยุกต์กับ Trading System ปัจจุบัน

| Concept จากหนังสือ | การประยุกต์ในระบบปัจจุบัน |
|---------------------|--------------------------|
| Forecast Combinations | Multi-Strategy (SMC Range + Breakout Retest + OB+FVG) |
| Feature Engineering | Regime Detection, BOCPD Gate, ATR-based SL/TP |
| Training Window Diversity | Walk-forward with different lookback periods |
| Risk-adjusted Returns | Position sizing ด้วย ATR-based Volatility |
| Industry Neutralization | Regime-based Strategy Routing (Ranging → SMC Range, Trending → Breakout Retest) |
| Time-Varying Exposures | Regime Detector 8 สถานะ + Dynamic Strategy Selection |

---

## บรรณานุกรม (_selected_)

- Alberg, J. & Lipton, Z. (2017). Improving Factor-Based Quantitative Investing by Forecasting Company Fundamentals.
- Asness, C.S. (2016). The Siren Song of Factor Timing.
- Carhart, M. (1997). On Persistence in Mutual Fund Performance.
- Clemen, R.T. (1989). Combining forecasts: A review and annotated bibliography.
- Daniel, K. & Moskowitz, T.J. (2016). Momentum crashes.
- de Prado, M.L. (2018). Advances in Financial Machine Learning.
- Fama, E.F. & French, K.R. (1992). The cross-section of expected stock returns.
- Fama, E.F. & French, K.R. (2017). International tests of a five-factor asset pricing model.
- Gu, S., Kelly, B.T. & Xiu, D. (2018). Empirical Asset Pricing via Machine Learning.
- Heaton, J.B., Polson, N.G. & Witte, J.H. (2016). Deep Learning in Finance.
- Makridakis, S. & Hibon, M. (2000). The M3-Competition.
- Miller, K.L. et al. (2013, 2015). Factor Timing Models.
- Schapire, R. (1990). The Strength of Weak Learnability.
- Srivastava, N. & Hinton, G. (2014). Dropout: A simple way to prevent neural networks from overfitting.
- Timmermann, A. (2006). Forecast combinations.
