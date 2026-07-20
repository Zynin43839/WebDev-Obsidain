# Data Science Cheat Sheet - สรุปภาษาไทย

> **ต้นฉบับ:** Data Science Cheat Sheet
> **จำนวนหน้า:** ~10 หน้า (Cheat Sheet แบบหนาแน่น)
> **สรุปโดย:** Agent

---

## 1. Data Science คืออะไร? — ภาพรวมทั้งหมด

Data Science คือสาขาวิชาที่รวมสถิติ, การเขียนโปรแกรม, และ domain knowledge เพื่อดึงข้อมูลเชิงลึกจากข้อมูล (structured หรือ unstructured) สองคำถามหลักที่ Data Science ต้องตอบคือ:

1. **เราเรียนรู้อะไรจากข้อมูลนี้ได้บ้าง?**
2. **เราสามารถทำอะไรได้บ้างเมื่อพบสิ่งที่กำลังค้นหา?**

### ประเภทของข้อมูล

| ประเภท | คำอธิบาย | ตัวอย่าง |
|--------|----------|---------|
| **Structured** | มีโครงสร้างที่กำหนดไว้แล้ว | ตาราง, spreadsheets, relational databases |
| **Unstructured** | ไม่มีโครงสร้างตายตัว | ข้อความ, รูปภาพ, เสียง |
| **Quantitative** | เป็นตัวเลข | ส่วนสูง, น้ำหนัก |
| **Categorical** | จัดกลุ่มได้ | เชื้อชาติ, เพศ, สีผม |
| **Big Data** | ข้อมูลมหาศาล (3 Vs: Variety, Volume, Velocity) | ไม่สามารถเก็บในเครื่องเดียวได้ |

### รูปแบบและแหล่งข้อมูล

- **รูปแบบที่พบบ่อย:** CSV, XML, SQL, JSON, Protocol Buffers
- **แหล่งข้อมูล:** ข้อมูลของบริษัท/เอกชน, APIs, ภาครัฐ, งานวิจัย, Web Scraping

### ปัญหาหลักสองประเภทใน Data Science

1. **Classification:** การจัดหมวดหมู่ — เช่น spam/non-spok, กรุ๊ปเลือด (A, B, AB, O)
2. **Regression:** การทำนายค่าตัวเลข — เช่น รายได้, GDP ปีหน้า, ราคาหุ้น

---

## 2. Probability Theory — พื้นฐานสำคัญ

Probability theory เป็นกรอบการทำงานสำหรับการใช้เหตุผลเกี่ยวกับความน่าจะเป็นของเหตุการณ์

### ศัพท์สำคัญ

- **Experiment (การทดลอง):** กระบวนการที่ให้ผลลัพธ์หนึ่งในหลาย ๆ ผลลัพธ์ที่เป็นไปได้ เช่น การโยนลูกเต๋าหรือเหรียญ
- **Sample Space S (ชุดตัวอย่าง):** ชุดของผลลัพธ์ที่เป็นไปได้ทั้งหมด เช่น การโยนลูกเต๋า S = {1, 2, 3, 4, 5, 6}
- **Event E (เหตุการณ์):** ชุดย่อยของผลลัพธ์ เช่น เหตุการณ์ที่ทอยได้ 5 หรือผลรวมของ 2 ครั้งเท่ากับ 7
- **Random Variable (ตัวแปรสุ่ม):** ฟังก์ชันตัวเลขบนผลลัพธ์ของ probability space
- **Expected Value (ค่าคาดหวัง):** E(V) = sum(P(s) * V(s))

### ความสัมพันธ์ระหว่างเหตุการณ์

- **Independent Events:** P(A ∩ B) = P(A)P(B) — เหตุการณ์ A และ B เป็นอิสระต่อกัน
- **Conditional Probability:** P(A|B) = P(A,B) / P(B) — ความน่าจะเป็นของ A เมื่อรู้ว่า B เกิดขึ้น
- **Bayes Theorem:** P(A|B) = P(B|A)P(A) / P(B) — กลับลำดับเงื่อนไขได้
- **Joint Probability:** P(A,B) = P(B|A)P(A)
- **Marginal Probability:** P(A) — ความน่าจะเป็นโดยรวม

### Probability Distributions

- **PDF (Probability Density Function):** ให้ความน่าจะเป็นที่ตัวแปรสุ่มจะมีค่าเท่ากับ x
- **CDF (Cumulative Density Function):** ให้ความน่าจะเป็นที่ตัวแปรสุ่มน้อยกว่าหรือเท่ากับ x
- PDF และ CDF ของตัวแปรสุ่มเดียวกันมีข้อมูลเหมือนกันทุกประการ

---

## 3. Descriptive Statistics — สถิติเชิงพรรณา

สถิติเชิงพรรณาใช้สำหรับจับลักษณะของชุดข้อมูลหรือตัวอย่าง มีสองประเภทหลัก: centrality (ค่ากลาง) และ variability (การกระจาย)

### Centrality Measures (ค่ากลาง)

- **Arithmetic Mean (ค่าเฉลี่ย):** เหมาะกับการแจกแจงแบบสมมาตรที่ไม่มี outliers
- **Geometric Mean (ค่าเฉลี่ยแบบเรขาคณิต):** เหมาะกับการคำนวณค่าเฉลี่ยของอัตราส่วน จะน้อยกว่า arithmetic mean เสมอ
- **Median (ค่ามัธยฐาน):** ค่าตรงกลางของข้อมูล เหมาะกับการแจกแจงแบบ skew หรือข้อมูลที่มี outliers
- **Mode (ค่าฐานนิยม):** ค่าที่พบบ่อยที่สุดในชุดข้อมูล

### Variability Measures (การกระจาย)

- **Standard Deviation (ส่วนเบี่ยงเบนมาตรฐาน):** วัดค่าความแตกต่างระหว่างค่าแต่ละตัวกับค่าเฉลี่ย
- **Variance (ความแปรปรวน):** ค่ากำลังสองของ standard deviation

### การตีความ Variance

Variance เป็นส่วนหนึ่งของโลกโดยธรรมชาติ เป็นไปไม่ได้ที่จะได้ผลลัพธ์เหมือนกันทุกครั้งจากการสังเกตเหตุการณ์เดิมซ้ำ ๆ เนื่องจาก noise หรือ error แบบสุ่ม บางครั้ง variance อธิบายได้จาก sampling error หรือ measurement error แต่บางครั้งก็เกิดจากความผันผวนแบบสุ่มของจักรวาล

### Correlation Analysis

- **Correlation Coefficient r(X,Y):** ค่าตั้งแต่ -1 ถึง 1 (1 = correlation เชิงบวกเต็มที่, -1 = เชิงลบ, 0 = ไม่มีความสัมพันธ์)
- **Pearson Coefficient:** วัดความสัมพันธ์แบบ linear
- **Spearman Rank:** คำนวณจากอันดับ แสดงความสัมพันธ์แบบ monotonic
- **ข้อควรระวัง:** Correlation ไม่ได้หมายความว่า causation!

---

## 4. Data Cleaning — การทำความสะอาดข้อมูล

"Garbage in, garbage out" — ต้องมั่นใจว่าข้อมูลที่ใส่เข้าไปไม่ใช่ขยะ

### ความผิดพลาด vs Artifacts

1. **Errors:** ข้อมูลที่สูญหายระหว่างการได้มาและไม่สามารถกู้คืนได้ เช่น ไฟดับ, เซิร์ฟเวอร์ล่ม
2. **Artifacts:** ปัญหาเชิงระบบจากการทำความสะอาดข้อมูล — ตรวจพบและแก้ไขได้

### Data Compatibility

ปัญหาความเข้ากันได้เกิดขึ้นเมื่อรวม datasets เข้าด้วยกัน ต้องมั่นใจว่าเปรียบเทียบ "apples to apples" ไม่ใช่ "apples to oranges"

- **หน่วย:** metric vs imperial
- **ตัวเลข:** decimal vs integer
- **ชื่อ:** John Smith vs Smith, John
- **เวลา/วันที่:** UNIX vs UTC vs GMT
- **สกุลเงิน:** ประเภทสกุลเงิน, inflation-adjusted, dividends

### Data Imputation (การเติมข้อมูลที่ขาด)

- Drop บันทึกทั้งหมดที่ขาดข้อมูล
- Heuristic-Based: ทายจาก domain knowledge
- Mean Value: เติมด้วยค่าเฉลี่ย
- Random Value: เติมด้วยค่าสุ่ม
- Nearest Neighbor: เติมจากข้อมูลที่คล้ายคลึง
- Interpolation: ใช้ regression ทำนายค่าที่ขาด

### Outlier Detection

Outliers มักเกิดจากข้อผิดพลาดในการเก็บข้อมูล ควรทำการ "sanity check" เสมอ

### หลักการสำคัญ

**Always maintain both the raw data and the cleaned version(s).** ข้อมูลดิบควรเก็บไว้เหมือนเดิม การทำความสะอาดและการวิเคราะห์ทุกประเภทควรทำบนสำเนาของข้อมูลดิบ

---

## 5. Feature Engineering — การสร้าง Features

Feature engineering คือการใช้ domain knowledge เพื่อสร้าง features ที่ช่วยให้ ML algorithms ทำงานได้ดีขึ้น ถือเป็นหนึ่งในขั้นตอนที่สำคัญที่สุดในการสร้าง model ที่ดี ตามที่ Andrew Ng กล่าวไว้: "Coming up with features is difficult, time-consuming, requires expert knowledge. 'Applied machine learning' is basically feature engineering."

### Continuous Data

| เทคนิค | คำอธิบาย |
|--------|---------|
| **Raw Measures** | ข้อมูลที่ยังไม่ผ่านการ transform |
| **Rounding** | บางครั้งความแม่นยำคือ noise — ปัดเศษให้เป็นจำนวนเต็มหรือทศนิยม |
| **Scaling** | log, z-score, minmax scale |
| **Imputation** | เติมค่าที่ขาดด้วย mean, median, หรือ model output |
| **Binning** | แปลง numeric features เป็น categorical (เช่น ค่า 1-10 อยู่ในกลุ่ม A, 10-20 อยู่ในกลุ่ม B) |
| **Interactions** | ความสัมพันธ์ระหว่าง features — การลบ, บวก, คูณ, หรือ statistical test |
| **Statistical** | log/power transform (ช่วยแปลง distribution ที่ skew ให้เป็น normal มากขึ้น), Box-Cox |
| **Row Statistics** | จำนวน NaN, ค่า 0, ค่าลบ, max, min ฯลฯ |
| **Dimensionality Reduction** | ใช้ PCA, clustering, factor analysis |

### Discrete Data

| เทคนิค | คำอธิบาย |
|--------|---------|
| **Encoding** | แปลงข้อมูล categorical เป็นตัวเลขเพราะ ML บาง algorithm ทำงานกับ categorical data ไม่ได้ |
| **Ordinal Values** | แปลงค่าที่แตกต่างเป็นเลขสุ่ม เช่น [r,g,b] → [1,2,3] |
| **One-Hot Encoding** | แปลงเป็น vector ที่มี 1 ตัวเดียว เช่น [r,g,b] → [[1,0,0],[0,1,0],[0,0,1]] |
| **Feature Hashing** | แปลง features ที่หลากหลายเป็น indices ใน vector หรือ matrix |
| **Embeddings** | แปลงวัตถุที่เป็น discrete (เช่น คำ) เป็น vectors |

---

## 6. Statistical Analysis — การวิเคราะห์ทางสถิติ

### Sampling From Distributions

- **Inverse Transform Sampling:** วาดตัวเลขสุ่ม p จาก [0,1] แล้วคำนวณ x ให้ CDF เท่ากับ p
- **Monte Carlo Sampling:** ใช้ในมิติสูง — กำหนด domain, สร้าง random inputs, คำนวณแบบ deterministic, วิเคราะห์ผลลัพธ์

### สถิติ Distribution คลาสสิก

| Distribution | ประเภท | ลักษณะเด่น | ตัวอย่างการใช้ |
|:------------|:------:|:----------:|:-------------|
| **Binomial** | Discrete | จำนวน "successes" ใน n ครั้งที่ลอง | โยนเหรียญ 10 ครั้ง ได้หัวกี่ครั้ง |
| **Normal/Gaussian** | Continuous | ระฆังคว่ำสมมาตร, 68-95-99.7 rule | ความสูงของคน, ค่า measurement error |
| **Poisson** | Discrete | จำนวนเหตุการณ์ในช่วงเวลา/พื้นที่ที่กำหนด | จำนวนลูกค้าที่เข้าร้านต่อชั่วโมง |
| **Power Law** | Discrete | หางยาวกว่า normal/poisson, วัดความเหลื่อมล้ำ | ความมั่งคั่ง, ความถี่ของคำ (Pareto 80/20) |

---

## 7. Statistical Modeling — การสร้างโมเดลทางสถิติ

Modeling คือการนำข้อมูลมาสร้างเป็นเครื่องมือที่สามารถพยากรณ์และทำนายได้

### ทำไมต้องประมาณค่า f(X)?

- **Prediction:** เมื่อได้ f(X) ที่ดีแล้ว สามารถใช้ทำนายข้อมูลใหม่ได้ — ปฏิบัติกับ f เป็น black box
- **Inference:** ต้องการเข้าใจความสัมพันธ์ระหว่าง X และ Y — ไม่สามารถปฏิบัติ f เป็น black box ได้

### Error Terms

- **Reducible Error:** ลดได้โดยใช้ statistical learning technique ที่เหมาะสม
- **Irreducible Error:** ลดไม่ได้ไม่ว่าจะประมาณ f ได้ดีแค่ไหน — เป็น upper bound เสมอ

### หลักการสำคัญ

- **Occam's Razor:** คำอธิบายที่ง่ายที่สุดคือคำอธิบายที่ดีที่สุด ถ้า model สองตัวทำนายได้ดีเท่ากัน เลือกตัวที่ง่ายกว่า
- **Bias-Variance Trade-Off:** model ที่ bias ต่ำจะ variance สูง และกลับกัน — เป้าหมายคือให้ทั้งคู่ต่ำ
  - **Bias:** ข้อผิดพลาดจากสมมติฐานที่ไม่ถูกต้อง → underfitting
  - **Variance:** ข้อผิดพลาดจากความไวต่อความผันผวนในข้อมูล → overfitting
- **No Free Lunch Theorem:** ไม่มี ML algorithm  nào ดีกว่าตัวอื่นในทุกปัญหา — ต้องลองหลาย model

---

## 8. Modeling Philosophies — ปรัชญาการสร้างโมเดล

### Think Probabilistically

Forecast แบบ probability (มี mean + standard deviation) มีความหมายมากกว่า statement แบบตายตัว ควรรายงานเป็น probability distributions

### Incorporate New Information

ใช้ live models ที่อัพเดทข้อมูลใหม่เรื่อย ๆ ใช้ Bayesian reasoning คำนวณว่า probability เปลี่ยนอย่างไรเมื่อมีหลักฐานใหม่

### Look For Consensus Forecast

ใช้หลายแหล่งหลักฐานที่แตกต่างกัน เทคนิคเช่น boosting และ bagging ใช้ weak classifiers จำนวนมากเพื่อสร้าง strong classifier

### ประเภทของ Model

| คู่เปรียบเทียบ | ประเภท A | ประเภท B |
|:---------------|:---------|:---------|
| **Parametric vs Nonparametric** | สมมติรูปร่างของ f ก่อน แล้ว fit model (ง่ายแต่ถ้าสมมติผิดจะแย่) | ไม่สมมติอะไร -fit ได้หลากหลายshapes (เสี่ยง overfitting) |
| **Supervised vs Unsupervised** | มี output ที่รู้จัก supervision | ไม่มี output — หาความสัมพันธ์ระหว่าง variables |
| **Blackbox vs Descriptive** | ตัดสินใจแต่ไม่รู้ว่าเกิดอะไรข้างใน (เช่น deep learning) | ให้ insight ว่าทำไมถึงตัดสินใจแบบนั้น (เช่น linear regression) |
| **First-Principle vs Data-Driven** | สร้างจากความเชื่อ先前และ domain knowledge | สร้างจาก correlations ที่สังเกตเห็น |
| **Deterministic vs Stochastic** | ให้ "prediction" เดียว (yes/no) | ให้ probability distributions |
| **Flat vs Hierarchical** | แก้ปัญหาบนระดับเดียว | แก้ปัญหาหลาย subproblems ที่ซ้อนกัน |

---

## 9. Model Evaluation — การประเมินโมเดล

วิธีที่ดีที่สุดในการประเมิน model คือ **out-of-sample predictions** — ข้อมูลที่ model ไม่เคยเห็น

### Classification Metrics

| Metric | สูตร | คำอธิบาย |
|--------|------|---------|
| **Accuracy** | (TP+TN)/(TP+TN+FN+FP) | สัดส่วนการทำนายที่ถูกต้อง — หลอกได้เมื่อ class sizes ต่างกันมาก |
| **Precision** | TP/(TP+FP) | เมื่อทำนาย positive บ่อยแค่ไหนที่ถูกต้อง |
| **Recall** | TP/(TP+FN) | จาก positive ทั้งหมด ทำนายถูกบ่อยแค่ไหน |
| **F-Score** | 2 * (precision*recall)/(precision+recall) | ค่าเดียวที่รวม precision และ recall |
| **ROC/AUC** | Plot TPR vs FPR | AUC=1 คือดีที่สุด, AUC=0.5 คือสุ่มเดา |

### Regression Metrics

- **Absolute Error:** y' - y
- **Squared Error:** (y' - y)^2
- **MSE:** ค่าเฉลี่ยของ squared error
- **RMSD:** รากที่สองของ MSE
- **Absolute Error Distribution:** ควรสมมาตร, center ที่ 0, รูประฆัง, มี extreme outliers น้อย

### Resampling Methods

- **Training/Validation/Test split:** แยกข้อมูลเป็น 3 ส่วน ใช้ train, ใช้ validate parameters, ใช้ test (สุดท้าย)
- **LOOCV (Leave-One-Out CV):** validation set มีแค่ 1 observation ทำซ้ำ n-1 ครั้ง
- **k-Fold CV:** แบ่งข้อมูลเป็น k กลุ่ม ใช้แต่ละกลุ่มเป็น validation ทีละกลุ่ม
- **Bootstrapping:** random sampling with replacement — ช่วย quantifying uncertainty

### การเพิ่มข้อมูลขนาดเล็ก

- สร้าง Negative Examples
- สร้าง Synthetic Data (เพิ่ม noise ให้ข้อมูลจริง)

---

## 10. Linear Regression — การถดถอยเชิงเส้น

ความสัมพันธ์ระหว่าง input X และ output Y: Y = beta_0 + beta_1*X_1 + ... + beta_p*X_p + epsilon

### วิธีหา Best Fit

1. **Matrix Form (Normal Equation):** w = (X^T * X)^-1 * X^T * Y — เร็วสำหรับ matrix เล็ก แต่ inverse คณิตศาสตร์มีราคาแพง
2. **Gradient Descent:** เริ่มจากจุดสุ่ม แล้วก้าวลงตาม negative gradient จน converge — เรียนรู้จาก learning rate alpha
3. **Stochastic Gradient Descent (SGD):** ใช้ mini-batch แทนทั้ง dataset — เร็วกว่าและอาจ converge เร็วกว่า

### การปรับปรุง Linear Regression

- **Subset/Feature Selection:** เลือกเฉพาะ predictors ที่เกี่ยวข้อง — Best, Forward, Backward Selection
- **Shrinkage/Regularization:** ลด coefficients เข้าใกล้ 0
  - **Lasso (L1):** min RSS + lambda * sum(|beta_j|) — บังคับ coefficients ให้เท่ากับ 0 ได้
  - **Ridge (L2):** min RSS + lambda * sum(beta_j^2) — ลด coefficients แต่ไม่เป็น 0
  - lambda คือ tuning parameter — ยิ่งใหญ่ ยิ่งน้อย flexibility, น้อย bias แต่ variance สูง
- **Dimension Reduction:** โปรเจกต์ predictors ลง M มิติ (M < p) โดยใช้ PCA
- **Miscellaneous:** ลบ outliers, feature scaling, ลบ multicollinearity

### การประเมินความแม่นยำ

- **RSE (Residual Standard Error):** ยิ่งน้อยยิ่งดี
- **R-squared:** สัดส่วนของ variance ที่อธิบายได้ — ยิ่งสูงยิ่งดี (0-1)
- **Standard Error + p-value:** ใช้ทดสอบสมมติฐาน — p-value เล็ก (ต่ำกว่า 0.05 หรือ 0.01) บ่งชี้ว่ามีความสัมพันธ์ระหว่าง X และ Y

---

## 11. Logistic Regression — การถดถอยลอจิสติก

ใช้สำหรับ **classification** ที่ output เป็น categorical ไม่ใช่ตัวเลข ทำนายความน่าจะเป็นว่า Y จะอยู่ในหมวดหมู่ไหน

### หลักการทำงาน

1. -fit ข้อมูลเข้ากับ linear regression model ก่อน
2. ผ่าน logistic function (S-shaped curve) → ได้ค่าระหว่าง 0 ถึง 1 เสมอ
3. ถ้า probability > threshold (เช่น 0.5) → ทำนาย Yes

### การหา Best Coefficients

**Maximum Likelihood:** หา beta_0...beta_p ให้ probability ทำนายของแต่ละ observation ใกล้ 1 ถ้าอยู่ใน class ที่ถูก และใกล้ 0 ถ้าไม่อยู่

### ปัญหาที่อาจพบ

- **Imbalanced Classes:** จำนวน class ต่างกันมาก → แก้ด้วยการลบ observation จาก class ใหญ่, replicate ข้อมูลจาก class เล็ก, หรือชั่งน้ำหนัก training examples
- **Multi-Class Classification:** ยิ่งมี class มาก ยิ่งยาก — logistic regression ทำได้แต่ LDA อาจเหมาะกว่า

---

## 12. Distance/Network Methods — วิธีวัดระยะทางและเครือข่าย

การตีความตัวอย่างเป็นจุดใน space ช่วยให้หา natural groupings หรือ clusters ในข้อมูลได้

### Distance Metrics

| Metric | สูตร | ลักษณะ |
|--------|------|--------|
| **Minkowski** | (sum(|a_i - b_i|^k))^(1/k) | k ควบคุมการให้น้ำหนัก — ค่า k ใหญ่ให้ความสำคัญกับค่าต่างที่มากที่สุด |
| **Manhattan (k=1)** | sum(|a_i - b_i|) | ระยะทางแบบ city block |
| **Euclidean (k=2)** | sqrt(sum((a_i - b_i)^2)) | ระยะทางเส้นตรง |
| **Cosine Similarity** | (a . b) / (|a|*|b|) | ความคล้ายคลึงระหว่าง vectors — ยิ่งสูงยิ่งคล้าย |
| **KL Divergence** | sum(a_i * log2(a_i/b_i)) | วัดความแตกต่างระหว่าง probability distributions |
| **Jensen-Shannon** | 0.5*KL(A||M) + 0.5*KL(M||B) | metric ที่ถูกต้องสำหรับ distance ระหว่าง probability distributions |

**ข้อควรระวัง:** Weighted Minkowski ไม่แนะนำ — ควร normalize ข้อมูลด้วย Z-scores ก่อนคำนวณ distance

---

## 13. K-Nearest Neighbors (KNN) — เพื่อนบ้านที่ใกล้ที่สุด

### อัลกอริทึม

1. คำนวณ distance D(a,b) จากจุด b ไปยังจุดทุกจุด
2. เลือก k จุดที่ใกล้ที่สุดและ labels ของมัน
3. -output class ที่มี labels บ่อยที่สุดใน k จุดนั้น

ค่า k ที่ optimal หาได้จาก cross validation

### การเพิ่มความเร็ว (เมื่อข้อมูลถึงล้านหรือพันล้านจุด)

- **Voronoi Diagrams:** แบ่ง plane เป็น regions ตาม distance
- **Grid Indexes:** แบ่ง space เป็น d-dimensional boxes
- **Locality Sensitive Hashing (LSH):** ไม่หา nearest neighbor ที่แน่นอน — ใช้ hash function จัดกลุ่มจุดที่ใกล้กัน

---

## 14. Clustering — การจัดกลุ่มข้อมูล

Clustering คือการจัดกลุ่มจุดตามความคล้ายคลึง — เป็นหนึ่งในสิ่งแรกที่ควรทำกับ dataset ใหม่

### K-Means Clustering

1. เลือก K, สุ่ม assign ค่า 1-K ให้แต่ละ observation
2. วนซ้ำจนกว่า assignments จะไม่เปลี่ยน:
   - คำนวณ centroid ของแต่ละ cluster (ค่าเฉลี่ยของ features ใน cluster นั้น)
   - Assign แต่ละ observation ไปยัง cluster ที่ centroid ใกล้ที่สุด

**ข้อควรระวัง:** ผลลัพธ์ขึ้นอยู่กับ initial random assignments — ควรรันจากหลาย initialization แล้วเลือกดีที่สุด ใช้ MSE ตัดสินว่า clustering ไหนดีกว่า

### Hierarchical Clustering

- ไม่ต้องกำหนด K ล่วงหน้า
- สร้าง dendrogram — จุดที่ fuse ที่ด้านล่างมีความคล้ายคลึงกัน, จุดที่ fuse ที่ด้านบนมีความแตกต่างกัน
- **Linkage types:** Complete (max dissimilarity), Single (min), Average, Centroid

### ขั้นตอน Hierarchical Clustering

1. เริ่มจาก n observations และ pairwise dissimilarities ทั้งหมด — แต่ละ observation เป็น cluster ของตัวเอง
2. วนจาก i = n, n-1, ...2:
   - หาคู่ cluster ที่ least dissimilar (most similar)
   - Fuse สอง cluster นั้นเข้าด้วยกัน
   - Assign แต่ละ observation ไปยัง cluster ที่ centroid ใกล้ที่สุด

---

## 15. Machine Learning — Part I: Naive Bayes, Decision Trees, Ensembles

### Naive Bayes

- ใช้ Bayes' theorem กับสมมติฐาน "naive" ว่า features ทุกคู่เป็นอิสระต่กัน
- คำนวณ P(C_i|X) สำหรับแต่ละ class แล้วเลือก class ที่มีค่าสูงสุด
- P(C_i): prior probability
- P(X|C_i): conditional probability ของ input X เมื่อรู้ class
- **ข้อดี:** เรียบง่าย, เร็ว, ใช้ได้ดีกับข้อมูลขนาดใหญ่

### Decision Trees

- Binary branching structure ใช้ classify input vector X
- แต่ละ node มี feature comparison (เช่น x_i > 42?)
- **ข้อดี:** Non-linearity, รองรับ categorical variables, interpret ง่าย, ใช้กับ regression ได้
- **ข้อเสีย:** เสี่ยง overfitting, ไม่ stable กับ noise, variance สูง, bias ต่ำ
- แทบไม่ใช้ decision tree ตัวเดียว — ใช้ ensembling, bagging, boosting รวมหลายตัว

### Ensemble Learning

- กลยุทธ์รวม classifiers/model หลายตัวเป็น model เดียว — "wisdom of crowds"
- **Bagging:** ใช้ B bootstrapped subsamples สร้าง B trees, แต่ละตัว train บน subsample ต่างกัน
- **Random Forests:** ต่อยอด bagging — ทุกครั้งที่ split, เลือก random subset ของ predictors (m ≈ sqrt(p)) แทนทั้งหมด → decorrelate trees
- **Boosting:** ปรับปรุง model ในส่วนที่ทำได้ไม่ดีโดยใช้ข้อมูลจาก classifiers ที่สร้างก่อนหน้า — slow learner
  - Tuning parameters: B (จำนวน classifiers), lambda (learning rate), d (interaction depth)

---

## 16. Machine Learning — Part II: SVM, PCA

### Support Vector Machines (SVM)

- สร้าง hyperplane ที่แยกจุดระหว่าง two classes
- ใช้ maximal margin hyperplane — hyperplane ที่ distance จาก training observations มากที่สุด (เรียกว่า margin)
- จุดที่ตกอยู่ฝั่งหนึ่งของ hyperplane ถูก classify เป็น -1 อีกฝั่งเป็น +1

### Principal Component Analysis (PCA)

- สรุปชุดของ correlated variables ด้วย variables ที่น้อยกว่าที่รวมกันแล้วอธิบาย variability ส่วนใหญ่ได้
- เป็น unsupervised approach ใช้สำหรับ dimensionality reduction, feature extraction, data visualization
- Variables หลังทำ PCA เป็น independent กัน
- **สำคัญ:** ต้อง scale variables ก่อนทำ PCA

---

## 17. Deep Learning — ภาพรวม

Deep learning เป็น subset ของ machine learning ที่ใช้ Neural Networks (NN) — เลียนแบบสมองมนุษย์ โครงสร้างเรียงเป็น layers โดย output ของ layer ก่อนเป็น input ของ layer ถัดไป → features ที่สูงขึ้นเรื่อย ๆ

### Neural Networks เป็น Universal Approximators

ไม่ว่าฟังก์ชันจะซับซ้อนแค่ไหน ก็มี NN ที่สามารถ approximate ได้ เพิ่ม complexity ด้วยการเพิ่ม hidden layers และ neurons

### สถาปัตยกรรมยอดนิยม

| สถาปัตยกรรม | การใช้งาน |
|:----------:|:---------|
| **Linear Classifier** | รวม input features กับ weights + biases เพื่อ predict output |
| **DNN (Deep Neural Net)** | มี hidden layers หลายชั้น + activation functions สำหรับ non-linearity |
| **CNN (Convolutional NN)** | convolutional + pooling + dense layers — เหมาะกับ image classification |
| **Transfer Learning** | ใช้ model ที่ train มาแล้วเป็น starting point + เพิ่ม layers สำหรับ use case เฉพาะ |
| **RNN (Recurrent NN)** | จัดการ sequence inputs ที่มี "memory" — LSTMs เป็น version ขั้นสูง ใช้กับ NLP |
| **GAN (Generative Adversarial NN)** | model หนึ่งสร้าง fake samples อีก model แยก real vs fake |
| **Wide and Deep** | รวม linear classifier กับ deep neural net — "wide" = memorize, "deep" = understand high-level features |

### TensorFlow — ข้อมูลเบื้องต้น

TensorFlow คือ open-source library สำหรับ numerical computation โดยใช้ data flow graphs

- **Tensors:** edges ใน graph, multidimensional data arrays — ค่าหน่วยกลางของข้อมูลใน TF
- **Variables:** ค่าที่ persist และเปลี่ยนแปลงได้ระหว่าง training (parameters ของ model)
- **Placeholders:** input nodes ที่ assign ครั้งเดียวไม่เปลี่ยน — รับ Tensor ที่ป้อนเข้ามา runtime

### Deep Learning Terminology สำคัญ

| คำศัพท์ | คำอธิบาย |
|--------|---------|
| **Neuron** | node ใน NN ที่รับ multiple inputs แล้ว output ค่าเดียวโดยใช้ activation function |
| **Weights** | edges ใน NN — เป้าหมายของ training คือหา weights ที่ optimal; weight=0 หมายถึง feature นั้นไม่มีผล |
| **Activation Functions** | ฟังก์ชันทางคณิตศาสตร์ที่เพิ่ม non-linearity เช่น ReLU, tanh |
| **Sigmoid Function** | แปลงค่าติดลบมาก → ใกล้ 0, ค่าบวกมาก → ใกล้ 1, ค่า 0 → 0.5 — เหมาะกับการทำนาย probability |
| **Gradient Descent / Backpropagation** | อัลกอริทึม optimization หลัก — Backpropagation คือ gradient descent สำหรับ NN |
| **Optimizer** | operation ที่เปลี่ยน weights และ biases เพื่อลด loss เช่น Adagrad หรือ Adam |
| **Learning Rate** | ความเร็วที่ optimizer เปลี่ยน weights — สูงเร็วแต่เสี่ยง diverge, ต่ำช้าแต่เสถียร |
| **Converge** | อัลกอริทึมที่ converge จะถึง optimal answer ในที่สุด |
| **Embeddings** | แปลง discrete objects (เช่น คำ) เป็น vectors ของ real numbers |
| **Convolutional Layer** | convolutional operations หลายตัว แต่ละตัวทำงานกับ different slice ของ input matrix |
| **Dropout** | regularization — สุ่มลบ units บางส่วนออกชั่วคราวระหว่าง training |
| **Early Stopping** | หยุด training ก่อนครบจำนวนที่กำหนดเพื่อหลีกเลี่ยง overfitting |
| **Pooling** | ลดขนาด matrix จาก convolutional layer — โดยใช้ค่า max หรือค่าเฉลี่ย |

---

## 18. Big Data — Hadoop Ecosystem

เมื่อข้อมูลไม่สามารถ fit ในเครื่องเดียวได้ ต้องใช้ distributed computing — cluster ของ servers ที่ต้องประสานงาน: partition data, coordinate computing tasks, handle fault tolerance, allocate capacity

### Hadoop Components หลัก

1. **HDFS (Hadoop Distributed File System):** istributed file system ที่ให้ high-throughput access โดยแบ่ง data ข้ามหลาย machines
2. **YARN (Yet Another Resource Negotiator):** จัดการ job scheduling และ cluster resources — resource manager (master node) + node manager (ทุก node)
3. **MapReduce:** parallel programming paradigm สำหรับ processing ข้อมูลมหาศาล
   - **Map:** operation ที่ทำงาน parallel บนส่วนย่อยของ dataset → output key-value pair
   - **Reduce:** operation รวมผลลัพธ์จาก Map

### Hadoop Ecosystem Tools

| เครื่องมือ | หน้าที่ |
|:----------:|:--------|
| **Hive** | Data warehouse ที่ใช้ SQL-like queries (HiveQL) กับ HDFS — abstract away MapReduce |
| **Pig** | scripting language (Pig Latin) สำหรับ data transformation — performs ETL into data warehouse |
| **Spark** | fast, distributed data processing — in-memory approach, รองรับ SQL, streaming, ML, graph processing ใช้ RDDs (Resilient Distributed Datasets) |
| **HBase** | NoSQL, column-oriented database บน HDFS — เหมาะกับ sparse datasets |
| **Flink/Kafka** | Stream processing — ใช้ tumbling/sliding/session windows จัดกลุ่ม streaming data |
| **Beam** | programming model สำหรับ data processing pipelines — สร้าง DAG แล้ว execute โดย distributed processing back-end |
| **Oozie** | workflow scheduler สำหรับ Hadoop jobs |
| **Sqoop** | transfer data จาก relational databases (MySQL) ไป HDFS |

---

## 19. SQL — Structured Query Language

SQL คือภาษาแบบ declarative ใช้เข้าถึงและจัดการข้อมูลใน databases (ปกติเป็น RDBMS)

### Basic Queries

```sql
-- เลือก columns
SELECT col1, col3 FROM table1;

-- กรอง rows
WHERE col4 = 1 AND col5 = 2;

-- จัดกลุ่ม
GROUP BY ...

-- กรองผลลัพธ์ที่จัดกลุ่มแล้ว
HAVING count(*) > 1;

-- เรียงลำดับ
ORDER BY col2;
```

### Useful Keywords

- **DISTINCT:** คืนผลลัพธ์ที่ไม่ซ้ำกัน
- **BETWEEN a AND b:** จำกัดช่วง (ตัวเลข, ข้อความ, หรือวันที่)
- **LIKE:** pattern search ใน column
- **IN (a, b, c):** ตรวจสอบว่าค่าอยู่ในชุดที่กำหนด

### Data Modification

- **UPDATE:** UPDATE table1 SET col1 = 1 WHERE col2 = 2
- **INSERT:** INSERT INTO table1 (col1, col3) VALUES (val1, val3)
- **INSERT from query:** INSERT INTO table1 (col1, col3) SELECT col, col2 FROM table2

### JOIN

ใช้ JOIN clause เพื่อรวม rows จาก two or more tables บน column ที่เกี่ยวข้องกัน

---

## 20. Python Data Structures — โครงสร้างข้อมูลใน Python

### Basic Structures

| Structure | ลักษณะ | ตัวอย่าง |
|-----------|--------|---------|
| **Lists (Arrays)** | ordered sequence, mutable | `l = [42, 3.14, "hello", "world"]` |
| **Tuples** | เหมือน lists แต่ immutable | `t = (42, 3.14, "hello", "world")` |
| **Dictionaries** | hash tables, key-value pairs, unsorted | `d = {"life": 42, "pi": 3.14}` |
| **Sets** | unordered, unique elements, mutable | `s = set([42, 3.14, "hello"])` |
| **Frozensets** | immutable version ของ sets | |

### Collections Module

- **deque:** double-ended queue — รองรับ append, appendLeft, pop, rotate
- **Counter:** dict subclass — เก็บ elements เป็น keys และ counts เป็น values

### heapq Module (Priority Queue)

- Binary tree ที่ parent node มีค่า >= children (max-heap)
- รองรับ push, pop, pushpop, heapify, replace
- ลำดับสำคัญ — ใช้สำหรับ scheduling และ optimization problems

---

## 21. ML Terminology สรุป

| คำศัพท์ | คำอธิบาย |
|--------|---------|
| **Features** | ข้อมูล/variables ที่ model ใช้ |
| **Feature Engineering** | แปลง input features ให้เป็นประโยชน์มากขึ้น |
| **Train/Eval/Test** | ข้อมูล 3 ชุด: train = optimize, eval = assess ระหว่าง train, test = final evaluation |
| **Classification/Regression** | Classification = ทำนาย category, Regression = ทำนาย number |
| **Overfitting** | model ทำงานดีกับ training data แต่แย่กับ test data — แก้ด้วย dropout, early stopping, ลด nodes/layers |
| **Bias/Variance** | variance สูง → overfitting, bias สูง → model แย่ |
| **Regularization** | เทคนิคลด overfitting — เพิ่ม weights ใน loss function, dropout |
| **Ensemble Learning** | train multiple models ต่าง parameters สำหรับปัญหาเดียวกัน |
| **A/B Testing** | เปรียบเทียบ techniques 2+ ตัวเพื่อดูว่าตัวไหน perform ดีกว่าอย่างมีนัยสำคัญ |
| **Baseline Model** | model ง่าย/heuristic ที่ใช้เป็นจุดเปรียบเทียบ |
| **Dynamic Model** | train online, อัพเดทต่อเนื่อง |
| **Static Model** | train offline |
| **Normalization** | แปลงค่าจริงเป็น standard range (ปกติ -1 ถึง +1) |
| **i.i.d.** | data drawn จาก distribution ที่ไม่เปลี่ยน, แต่ละค่าที่วาดไม่ขึ้นกับค่าก่อนหน้า — ในอุดมคติแต่หายากในโลกจริง |
| **Hyperparameters** | "knobs" ที่ปรับระหว่าง training runs |
| **Generalization** | ความสามารถของ model ในการทำนายถูกบนข้อมูลใหม่ |
| **Cross-Entropy** | วัดความแตกต่างระหว่าง probability distributions สองตัว |

---

## 22. สรุป — การประยุกต์ใช้กับ Algorithmic Trading

Data Science Cheat Sheet เล่มนี้ครอบคลุมเครื่องมือและแนวคิดทั้งหมดที่จำเป็นสำหรับ algorithmic trading:

1. **Probability Theory** — พื้นฐานสำหรับ risk assessment, position sizing, และ Bayesian updating ของ market signals
2. **Statistics** — descriptive statistics สำหรับ market data analysis, correlation analysis สำหรับ finding relationships ระหว่าง instruments
3. **Feature Engineering** — สำคัญมากสำหรับ trading: scaling prices, creating technical indicators (binning, interactions), encoding market regimes
4. **Machine Learning** — classification (buy/sell/hold signals), regression (price prediction), ensemble methods (combining multiple strategies), SVM (finding optimal decision boundaries)
5. **Deep Learning** — neural networks สำหรับ time series prediction, CNN สำหรับ pattern recognition ใน price charts, LSTM สำหรับ sequence modeling
6. **SQL** — query historical trade data, backtesting results, performance metrics
7. **Big Data** — processing large volumes of tick data, real-time streaming สำหรับ live trading

---

*สรุปจาก Data Science Cheat Sheet — 4 chunks, ~10 หน้า, ครอบคลุม Data Science fundamentals ทั้งหมดตั้งแต่ Probability ถึง Deep Learning*
