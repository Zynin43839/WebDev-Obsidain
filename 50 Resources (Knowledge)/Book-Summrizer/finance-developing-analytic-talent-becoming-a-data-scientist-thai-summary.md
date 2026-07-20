# สรุปหนังสือ: Developing Analytic Talent — Becoming a Data Scientist

> ผู้เขียน: Vincent Granville | 8 บท, ~300 หน้า
> สรุปจาก EN chunks ทั้งหมด 255 ชิ้น

---

## ภาพรวม

หนังสือเล่มนี้เป็นคู่มือสำหรับผู้ที่ต้องการประกอบอาชีพ Data Scientist ครอบคลุมทั้ง "Data Science คืออะไร" "Big Data ต่างจากข้อมูลปกติอย่างไร" "จะเป็น Data Scientist ได้อย่างไร" เทคนิคงานฝีมือ (Craftsmanship) กรณีศึกษาจริง วิธีเริ่มต้นอาชีพ และทรัพยากรอ้างอิง ผู้เขียน Vincent Granville มีประสบการณ์ 20+ ปีในวงการ และเป็นผู้ก่อตั้ง Data Science Central

---

## บทที่ 1: Data Science คืออะไร?

### จริง vs ปลอม (Real vs Fake Data Science)
- **Data Science ปลอม** = การเอาสถิติเก่าหรือ R programming มาเปลี่ยนชื่อเป็น "Data Science" โดยไม่มีการจัดการ Big Data จริง เช่น คอร์สเรียนที่สอน logistic regression กับข้อมูล 10,000 แถว แล้วอ้างว่าเป็น data science
- **Data Science จริง** = การผสมผสานระหว่าง วิทยาศาสตร์คอมพิวเตอร์ + สถิติ + ความเชี่ยวชาญเชิงธุรกิจ + การสื่อสาร และต้องมีทักษะจริงในการจัดการข้อมูลขนาดใหญ่ (terabytes)

### Data Scientist ต่างจากอาชีพอื่นอย่างไร?
| อาชีพ | ความแตกต่างหลัก |
|--------|-----------------|
| **Data Engineer** | Engineer ทำ ETL (Extract/Transform/Load) — เน้นโครงสร้างข้อมูล ส่วน Data Scientist ทำ DAD (Discover/Access/Distill) — เน้นสกัดคุณค่าจากข้อมูล |
| **Statistician** | นักสถิติเน้นโมเดลและทฤษฎี Data Scientist เน้น implementation อัตโนมัติ (ระบบ bid อัตโนมัติ, fraud detection, real-time pricing) |
| **Business Analyst** | BA เน้น dashboard, รายงาน, ROI assessment DS ช่วย automate งาน BA และเพิ่มขนาดข้อมูลที่วิเคราะห์ได้ 100 เท่า |

### ทักษะที่ Data Scientist จำเป็นต้องมี
- Business acumen (ความเข้าใจธุรกิจ)
- ความสามารถในการจัดการ big data จริง (50 ล้าน rows ในไม่กี่ชั่วโมง)
- การรู้สึกข้อมูล (sense the data) + ความไม่เชื่อในโมเดล (distrust of models)
- คณิตศาสตร์พื้นฐาน: Algebra, Calculus ขั้นพื้นฐาน, Statistics & Probability
- เครื่องมือ: R, Python/Perl, Excel, SQL, Visualization, UNIX, Web crawlers

### 13 กรณีศึกษาจริง (ตัวอย่างเด่น)

1. **DUI Arrests ลดหลังขายเหล้าเสรี** — ต้องคิดให้รอบคอบว่าเป็นเพราะกฎหมายจริง หรือเพราะปัจจัยอื่น (จำนวนตำรวจ, ราคาเหล้า, ระยะทางขับรถ) — บทเรียน: อย่าด่วนสรุป causal relationship

2. **Data Science vs Intuition** — ดาวยูbinary star system: 15% ของดาวดูเหมือนเป็นคู่ แต่ probability บอกว่า expected = 14.5% — แต่จริงๆ 80%+ อยู่ใน binary system แสดงว่ามีกลไกบังคับให้จับคู่ — บทเรียน: อย่าพึ่ง intuition อย่างเดียว

3. **Data Glitch** — ข้อมูลเสียหายจากการ import/export, format ไม่ตรงกัน, comma ใน field value — Wells Fargo, eBay เคยเจอปัญหานี้

4. **Regression ในพื้นที่ผิดปกติ** — reverse-engineering สูตร Coca-Cola: ตัวแปรเป็น binary (มี/ไม่มีส่วนผสม) ต้อง regression บน simplex domain ไม่ใช่ linear regression ปกติ

5. **Analytics vs Seduction** — การเพิ่ม conversion ต้องผสม analytics + seduction (ศิลปะการโน้มน้าว) — การใช้คำ hype ซ้ำๆ จะได้ผลแค่ช่วงแรก แล้วจะสูญเสีย subscriber

6. **Hidden Data** — ข้อมูลที่หายไป (censored data, missing data) หรือข้อมูลที่ไม่รู้ว่ามีอยู่ — Target ไม่รู้ว่าควรขาย garden items ในแคลิฟอร์เนียเดือนกุมภาพันธ์

7. **Gasoline Lead vs Crime** — correlation ≠ causation — อย่าอ้าง causal โดยไม่พิสูจน์

8. **Boeing Dreamliner** — ปัญหา battery เกิดจาก experimental design ไม่ดีใน QA

9. **NLP 7 ประโยคยาก** — "She threw up her dinner" vs "threw up her hands" — context สำคัญ

10. **Data Scientists กับอาหาร** — Supermarket ใช้ analytics ตัดสินใจว่าขายอะไร ราคาเท่าไหร่ จัดร้านอย่างไร

11. **Amazon Relevancy** — ปรับ search/relevancy engine ให้รวม price เป็นปัจจัยหลัก ปรับ optimal book price ตาม user profile

12. **Fake Facebook Likes** — วิธีตรวจจับ: ดู ratio ของ comment vs likes (5,000 likes แต่ 20 comments = fake)

13. **Analytics for Restaurants** — วิเคราะห์ layout โต๊ะ, pricing, inventory, menu optimization

### ประวัติ Data Science & แนวโน้ม
- **1988**: AI → **1995**: Web Analytics → **2003**: Business Analytics → **2012**: Data Science
- สถิติจะกลับมา (renaissance) ในรูปแบบที่ applied มากขึ้น, model-free, robust
- Google, Yahoo!, Facebook เป็นผู้บุกเบิก Big Data
- แนวโน้ม: In-memory analytics, MapReduce/Hadoop, NoSQL, Python, AaaS (Analytics as a Service)

---

## บทที่ 2: Big Data ต่างจากข้อมูลปกติอย่างไร

### คำสาปของ Big Data (Curse of Big Data)
- เมื่อมี k ตัวแปร (เช่น หุ้น 10,000 ตัว) และ n สังเกตการณ์น้อย (<200) — ความสัมพันธ์แบบ cross-correlation จะมี false positive สูงมาก
- ตัวอย่าง: ถ้าคำนวณ correlation ของหุ้น 10,000 ตัว = 50 ล้านคู่ — จะเจอ correlation > 0.80 โดยบังเอิญหลายคู่แน่นอน

### เมื่อ Data Flows เร็วเกินไป
- ข้อมูล velocity (เร็ว), variety (หลายรูปแบบ), volume (มาก) — สถิติดั้งเดิมใช้ไม่ได้กับ big data
- ต้อง sampling, binning, หรือ data reduction แทนที่จะประมวลผลทั้งหมด

### MapReduce ทำอะไรไม่ได้
- การ compute cross-correlation ทุกคู่จาก k ตัวแปร ไม่สามารถ split แล้ว parallel ได้ เพราะ cross-cluster computations ทำให้ MapReduce ไร้ประโยชน์
- **ทางออก**: Sampling (เลือกบางคู่มาคำนวณ), Binning, Classical data reduction

### Clustering & Taxonomy สำหรับ Big Data
- อัลกอริทึม clustering ปกติ O(n² log n) ช้าเกินไปสำหรับ big data
- ต้องใช้อัลกอริทึมที่เร็วกว่า เช่น O(n) หรือ O(n/log n)
- ใช้ graph theory + hash table ในการจัดกลุ่ม keywords 10 ล้านคำ

### Excel กับ 100 ล้าน Rows
- Excel มีประโยชน์ในการ design/test model แบบ real-time
- แต่มีข้อจำกัดเรื่อง in-memory processing — ต้องใช้ add-on หรือเครื่องมืออื่นช่วย

### The Eight Worst Predictive Modeling Techniques
- ใช้ cross-validation ที่ดี: split training set เป็นหลาย subset, ใช้ data ที่ใหม่กว่าใน control set, ตรวจสอบ confidence interval ของ error

### การแต่งงานระหว่าง Computer Science + Statistics + Domain Expertise
- ต้องผสมผสานทั้ง 3 ด้าน — อย่าใช้ nuclear weapon ยิงแมลง (over-engineering โดยไม่ดู lift จริง)

---

## บทที่ 3: จะเป็น Data Scientist ได้อย่างไร

### ประเภทของ Data Scientist
| ประเภท | ลักษณะ |
|--------|--------|
| **Fake Data Scientist** | เอาสถิติเก่ามาเปลี่ยนชื่อ |
| **Self-Made Data Scientist** | เรียนรู้ด้วยตัว, ไม่มี degree ตรง |
| **Amateur Data Scientist** | มีเครื่องมือดี, ชนะ Kaggle contests, ทำงาน freelance |
| **Extreme Data Scientist** | เชี่ยวชาญหลายด้าน, สร้าง foundation ของ data science เอง |

### Horizontal vs Vertical Data Scientist
- **Horizontal**: มีความรู้กว้างในทุกด้าน (data engineering + statistics + ML + business)
- **Vertical**: เชี่ยวชาญลึกใน domain เดียว (เช่น healthcare, finance)

### โปรแกรมฝึกอบรม
- **มหาวิทยาลัย**: Stanford, NC State (MSA), Carnegie Mellon, Harvard, MIT — หลักสูตร涵盖: Data intro → Storage → Hadoop → NoSQL → Statistics → ML → NLP → Social Network Analysis
- **องค์กร/สมาคม**: INFORMS, TDWI, Data Science Central, statistics.com
- **ฟรี**: Coursera, Data Science Central apprenticeship

### เส้นทางอาชีพ
- **The Independent Consultant**: เริ่มจาก lead generation, สร้าง mailing list, lean startup, ไม่ต้องใช้ VC money
- **The Entrepreneur**: เริ่มต้น data science startup — lean approach, outsource, ใช้ Ning.com ราคาถูก, open-source patent (เปิดเผย IP เพื่อป้องกันการถูกขโมย)

### เรื่องจริงจากผู้เขียน
- สร้าง Data Science Central บน Ning.com ราคา <$200/เดือน
- Lead generation มีค่า $40/lead, ต้นทุน $10/lead
- Newsletter optimization: สร้าง segment Gmail แยก, ลบ subscriber ที่ไม่เปิดอ่าน, ใช้ SPF records

---

## บทที่ 4: Data Science Craftsmanship Part I

### เมตริกใหม่ๆ
- **Compound metrics** = เมตริกที่มาจาก base metrics เหมือน molecule จาก atom
- **KPIs** (Key Performance Indicators) แตกต่างจาก **leading indicators** (ใช้ทำนายแนวโน้ม)
- ตัวอย่าง: open rate, click rate, churn rate — แม้ไม่ใช่ KPI ก็ต้อง track เพราะช่วยเข้าใจความแปรปรวน

### Analytics สำหรับ Digital Marketing
- Metrics สำหรับ fraud detection, click fraud, email marketing
- วัด lift (ROI) จากแคมเปญต่างๆ

### Visualization
- กราฟที่ดี: เข้าใจได้ใน 30 วินาที, เชื่อมโยงกับ KPI, แสดงโอกาสหรือปัญหา
- กราฟที่แย่: เยิ้ยยัด, rescale data, ซ่อนข้อมูลสำคัญ, เปรียบเทียบ metrics ที่ไม่เทียบกันได้
- **Data Videos**: ใช้ R สร้าง scatter plot 200 ภาพ → ต่อเป็น video แสดง evolving patterns

### Statistical Modeling Without Models
- **Three Classes of Metrics**: Centrality (แนวโน้มส่วนกลาง), Volatility (ความผันผวน), Bumpiness (ความกระเพื่อม)
- Bumpiness coefficient = วัดว่า data สั่นไหวแค่ไหน — ใช้แยก growth/decline/stable/volatile periods

### Clustering สำหรับ Big Data
- อัลกอริทึมเร็ว O(n) สำหรับ clustering ข้อมูลมหาศาล
- ใช้ graph theory (node = keyword, edge = ความสัมพันธ์) + hash table

### Correlation สำหรับ Big Data
- Spearman's rank correlation เป็นจุดเริ่มต้น — แต่ q(n) = O(n³) โตเร็วเกินไป
- **Family ใหม่ของ Rank Correlations** (t-correlation): ปรับ parameter c ให้เหมาะกับ data

### Computational Complexity
- ต้องเข้าใจ O-notation พื้นฐาน — ไม่ต้องเรียนลึกแต่ต้องรู้ว่า algorithm ไหนเร็ว/ช้า
- Internet Topology Mapping: ใช้ clustering สร้าง keyword taxonomy ของ internet

---

## บทที่ 5: Data Science Craftsmanship Part II

### Data Dictionary
- ตารางที่มี label, value, frequency count — ใช้ explore ข้อมูลเบื้องต้น
- ช่วย detect outliers, sparseness, data glitches
- สร้างด้วย hash table: วนรอบข้อมูล, นับ name-value pairs ทุก dimension

### Hidden Decision Trees
- Decision tree ที่ไม่ต้อง optimize — ใช้ rule-based approach ในการ categorize
- เร็วกว่า traditional decision tree, ง่ายต่อการ implement

### Model-Free Confidence Intervals
- **Analyticbridge First Theorem**: สร้าง confidence interval โดยไม่ต้องมี statistical model
- ใช้ Monte Carlo simulation — เหมาะสำหรับ non-statisticians

### 4 วิธีแก้ปัญหาเดียวกัน
| วิธี | กลุ่มเป้าหมาย |
|------|--------------|
| Intuitive Approach | Business analysts ที่มี intuition ดี |
| Monte Carlo Simulations | Software engineers |
| Statistical Modeling | Statisticians |
| Big Data Approach | Computer scientists |

### Causation vs Correlation
- Correlation ≠ Causation — แต่ใน data mining บางครั้งไม่ต้องแยกก็ได้ (.pattern detection แบบ black box)
- วิธีตรวจ causation: Granger causality test, experimental design

### Life Cycle of Data Science Projects
- วงจร: Define problem → Data collection → Exploratory analysis → Model building → Validation → Deployment → Monitoring

### Predictive Modeling Mistakes
- Logistic regression ใช้ไม่ได้เสมอไป — ต้องพิจารณา interactions between variables
- First order vs second order approximation

### Experimental Design (DOE)
- สำคัญมากสำหรับ A/B testing, clinical trials
- ตัวอย่าง: วัด metrics สำหรับผู้ป่วย alcoholism (time between drinking, duration, intensity) → segment เป็น 8 กลุ่ม → customize treatment

### Analytics as a Service & APIs
- สร้าง API ให้คนอื่นเรียกใช้ analytics ได้ (เช่น keyword correlation API)
- ใช้ HTTP requests + XML parameters

### Hadoop/MapReduce & Synthetic Variance
- สูตร variance ปกติใน Hadoop ไม่เสถียร (numerical instability) — variance ติดลบได้
- ต้องใช้ synthetic variance ที่ทนทานต่อ outliers

---

## บทที่ 6: กรณีศึกษา (Case Studies)

### ตลาดหุ้น
- **Pattern to Boost Return by 500%**: หุ้นที่เป็น top performer วันนี้ มักจะบวกต่อวันถัดไป — return 20% ใน 30 วัน vs S&P 500 ที่ 4.5%
- **Black-Scholes Option Pricing**: ใช้ Brownian motion แต่สมมติฐาน (variance คงที่, drift เป็น linear) มักไม่จริงในตลาดจริง
- **Discriminate Analysis**: จำแนกหุ้นเป็น bad/neutral/good → เลือกกลยุทธ์ trading ที่เหมาะสม
- **Lorenz Curve**: ถ้า 90% ของกำไรมาจาก 5% ของเทรด → กระจุกตัวเกินไป ควรหลีกเลี่ยง

### Encryption & Steganography
- Steganography = ซ่อนข้อมูลในรูปภาพ
- Solid email encryption, Captcha hack

### Fraud Detection
- **Click fraud**: วิเคราะห์ continuous click score (ไม่ใช่แค่ binary fraud/non-fraud)
- **Automated feature selection**: ใช้ combinatorial optimization หา features ที่ดีที่สุด
- **Association rules**: ตรวจจับ collusion และ botnets
- **Extreme value theory**: ตรวจจับ patterns หายาก

### Digital Analytics
- **Email marketing**: Boost performance 300% ด้วย better segmentation
- **Keyword advertising**: Optimize ใน 7 วัน
- **Twitter hashtags**: วัด ROI จาก hashtags
- **Google Search**: แก้ 3 ปัญหา — outdated results, duplicate content, personalization bias

### Miscellaneous
- **Sales forecasts**: โมเดลง่ายๆ ดีกว่าโมเดลซับซ้อน (Occam's razor)
- **Healthcare fraud detection**
- **Attribution modeling**: ว่าวงจร conversion มาจากช่องทางไหน

---

## บทที่ 7: เริ่มต้นอาชีพ Data Science

### Job Interview Questions (90+ คำถาม)
- **Experience questions**: ชุดข้อมูลใหญ่สุดที่เคยจัดการ, success stories, data modeling, dashboard creation
- **Technical questions**: Lift, KPI, robustness, collaborative filtering, MapReduce, cosine distance, hash table collisions, Naive Bayes, star schema
- **General questions**: ชอบ data scientist คนไหน, หนังสือ/conference ล่าสุดที่อ่าน/ไป, blog post ล่าสุด

### ทดสอบตัวเอง: Visual & Analytic Thinking
- ฝึกจับ patterns ด้วยตาเปล่า
- ระบุ anomalies ใน time series
- เข้าใจ misleading time series และ random walks

### จาก Statistician สู่ Data Scientist
- Statisticians มี skill ที่ data scientists ขาด: DOE, sampling, R
- แต่ต้องเพิ่ม: business acumen, Python, MapReduce, NoSQL, API, Hadoop
- ทั้งสองอาชีพกำลังเบลอเข้าหากัน

### ทักษะ Data Science ยอดนิยม (จาก LinkedIn)
- Skill ที่ endorse สูงสุด: Analytics, Machine Learning, Data Mining, Statistics, Big Data, Python, R, SQL, NLP
- 400+ job titles ที่เกี่ยวข้องกับ data science
- เงินเดือนสูงสุดใน: San Francisco, New York City, Seattle

---

## บทที่ 8: ทรัพยากร Data Science

### หนังสือแนะนำ
- **Business**: Data Science for Business, Big Data Computing, Building Data Science Teams
- **Technical**: Mining of Massive Datasets (free PDF), Ensemble Methods in Data Mining, Causality (Judea Pearl)
- **Programming**: Big Data Analytics with R and Hadoop, Hadoop in Practice, Practical Data Science with R

### Data Sets สำหรับฝึก
- DMOZ (1 ล้าน URLs จัดหมวด), Quantcast top 1 ล้าน domains, search engine query logs

### Conference & Organization
- Predictive Analytics World, ACM, IEEE, SAS Data Mining, Text Analytics News
- 7 วารสาร: Big Data, EPJ Data Science, Journal of Data Science, The R Journal

### บริษัทที่จ้าง Data Scientist มากที่สุด
- Top 20: Microsoft, IBM, Amazon, SAS, Google, Accenture, Oracle, LinkedIn, FICO, Bank of America, Citi, Tata, Facebook, Cognizant, Wells Fargo, Capgemini, eBay, Apple, HP, EMC
- สหรัฐฯ (NYC, SF Bay Area) ยังเป็นตลาดหลัก แต่ Singapore, London, Spain กำลังโต

### Resume Tips
- ทักษะที่พบบ่อยสุด: R (50%), Python (50%), SQL, Statistics/ML, Java, Tableau, Excel
- ต้องมี career progression ชัดเจน, แสดง ongoing training, certifications

---

## ประเด็นสำคัญรวม (Key Takeaways)

1. **Data Science ≠ Statistics ≠ Programming** — เป็นการผสมผสาน 3 ด้าน + domain expertise + business acumen + communication

2. **Fake Data Science มีเยอะ** — ต้องระวังหลักสูตรที่แค่เปลี่ยนชื่อสถิติเป็น data science

3. **Big Data มี Curse** — ยิ่งข้อมูลเยอะ ยิ่งเจอ false correlation ต้องใช้ techniques ที่ถูกต้อง

4. **DAD สำคัญกว่า ETL** — Data Scientist ต้อง Discover/Access/Distill ไม่ใช่แค่ Extract/Transform/Load

5. **Cross-validation เป็นหัวใจ** — โมเดลจะดีแค่ไหนก็ตาม ถ้า cross-validation ไม่ดี ก็ไร้ค่า

6. **Correlation ≠ Causation** — อย่าอ้าง causal โดยไม่พิสูจน์

7. ** lean Startup สำหรับ Data Science** — ไม่ต้องใช้ VC money เยอะ, outsource ได้, open-source IP

8. **Continuously Learn** — วงการเปลี่ยนเร็ว ต้องติดตาม blog, conference, communities ตลอด

9. **Communication Skills สำคัญเท่า Technical Skills** — ต้องนำเสนอผลวิเคราะห์ให้ผู้บริหารเข้าใจได้

10. **Domain Expertise สำคัญ** — ไม่ใช่แค่มี algorithm ดี แต่ต้องเข้าใจปัญหาจริงของธุรกิจด้วย
