# สรุปหนังสือ Finding Alphas (1st Edition) — WorldQuant

> **ผู้เขียนรวม:** Igor Tulchinsky, Geoffrey Lauprete, Scott Bender, Yongfeng He, Hongzhi Chen, Pankaj Bakliwal และนักวิจัยคนอื่นๆ ของ WorldQuant
> **สรุปโดย:** AI Agent | 34 chunks, ~130,000+ ตัวอักษร

---

## Part I: ภาพรวมและแนวคิดพื้นฐาน

### 1. นิยาม Alpha — มันคืออะไร?

Alpha คือ **แบบจำลองทางคณิตศาสตร์ที่ใช้ทำนายการเคลื่อนไหวของราคาสินทรัพย์ในอนาคต** ประกอบด้วย 3 องค์ประกอบหลัก:

- **สมการ/โค้ด** — นิพจน์ทางคณิตศาสตร์หรือโปรแกรมคอมพิวเตอร์
- **พารามิเตอร์** — ค่าตั้งต้นที่ปรับแต่งได้
- **ข้อมูล** — ใช้ร่วมกับข้อมูลในอดีตเพื่อทำนาย

Alpha สามารถแสดงเป็น **เมตริกซ์ของ positions** (ตำแหน่งการลงทุน) ที่เปลี่ยนแปลงตามเวลา ค่าในเมตริกซ์ตรงกับ position ของหุ้นแต่ละตัวในแต่ละวัน

**หลักการสำคัญ:**
- Alpha คือ **การทำนายผลตอบแทนของหุ้นแต่ละตัว** เมื่อเทียบกัน
- Information Ratio (IR) จะสูงสุดเมื่อ position ของหุ้นแต่ละตัว **เป็นสัดส่วนกับผลตอบแทนที่คาดการณ์**
- ทุก alpha คือ **สมมติฐาน** (hypothesis) — เช่น `1/price` หมายถึง "ลงทุนมากขึ้นเมื่อราคาต่ำ"

**คุณภาพของ Alpha ที่ดี:**
1. แนวคิดและนิพจน์ **เรียบง่าย**
2. โค้ด **สง่างาม**
3. In-sample Sharpe Ratio **ดี**
4. **ไม่ไวต่อการเปลี่ยนแปลง** เล็กๆ ของข้อมูลและพารามิเตอร์
5. ใช้ได้กับ **หลาย universe** ของหุ้น
6. ใช้ได้กับ **หลายภูมิภาค**
7. กำไร **แตะจุดสูงสุดใหม่** เรื่อยๆ

### 2. Alpha Genesis — วงจรชีวิตของ Alpha

**วงจรชีวิตของ Alpha มี 4 ขั้นตอน:**

1. **กำเนิด (Birth)** — Alpha เกิดจากปฏิสัมพันธ์ของผู้เข้าร่วมตลาด
2. **เติบโต (Growth)** — เมื่อ alpha แข็งแกร่งพอและถูกตรวจสอบทางสถิติได้ จะดึงดูด capital
3. **เสื่อม (Decay)** — capital ที่ไหลเข้ามาเยอะทำให้ pie ถูกแบ่งเป็นชิ้นเล็กลง → alpha หดตัวและผันผวนมากขึ้น
4. **ตาย (Death)** — capital มากเกินไปทำให้ alpha ใช้ไม่ได้อีก

**แต่วงจรนี้ไม่จบแค่นั้น** — การที่ alpha ตาย สร้างรูปแบบใหม่ในตลาด ซึ่งเป็นวัฏจักรต่อเนื่องของการเกิดและตายของ alpha

**ข้อมูล Input:**
- ราคา history, volume, fundamental data, ข่าวสาร, และอื่นๆ
- คุณภาพข้อมูลมีผลมากต่อผลลัพธ์ของ alpha

**Evaluation — ไม่มีเมตริกเดียวที่ตอบทุกอย่าง:**
- In-sample ที่ดี ≠ Out-of-sample ที่ดี
- Outlier อาจทำลายโมเดลได้
- ยิ่งข้อมูลเยอะ ยิ่งมั่นใจ — แต่ยิ่งนาน ยิ่งเสี่ยงที่ alpha จะ decay

### 3. Cutting Losses — หลักการ UnRule

**หลักการสำคัญที่สุด:** กฎเกณฑ์ทุกข้อไม่สมบูรณ์แบบ — เรียกว่า **"UnRule"**

- **Karl Popper (1934)**: ไม่สามารถพิสูจน์กฎเกณฑ์สากลได้ ต้องใช้ counter-instance ลบล้าง
- กฎเกณฑ์ทุกข้อ **�述ลักษณะความจริงได้บางส่วน** แต่ไม่มีข้อไหนสมบูรณ์
- ในโลกของการเทรด: **กฎที่เรียกว่า alpha ทุกข้อ** สักวันจะหยุดทำงาน

**วิธีจัดการ:**
1. รวบรวม alpha ให้มากที่สุดเท่าที่จะทำได้
2. ไม่ยึดติดกับกฎเกณฑ์ใดกฎเกณฑ์หนึ่ง
3. **สังเกตว่ากฎไหนกำลังทำงานอยู่** — ถ้าใช้ได้ ลงทุน; ถ้าไม่ได้ หยุด
4. **Cut losses** — เมื่อกฎหยุดทำงาน ตัดทิ้งทันที
5. ทำแบบ **หลายกฎพร้อมกัน** (simultaneous strategies)

---

## Part II: การออกแบบและประเมิน Alpha

### 4-6. วิธีออกแบบ Alpha — 7 ขั้นตอน

**Case Study จากหนังสือ (Chapter 6):**

| ขั้นตอน | คำอธิบาย | ผลลัพธ์ |
|---------|----------|---------|
| **Alpha1** | `-(5-day return)` — mean reversion ธรรมดา | Sharpe 1.0, MaxDD 39% |
| **Alpha2** | + Industry Neutralization | Sharpe 1.4, MaxDD 9% ✅ |
| **Alpha3** | + Rank (ใช้ relative rank แทน absolute value) | Sharpe 1.76, MaxDD 5.8% ✅✅ |
| **Alpha4** | + Decay (smooth signal 3 วัน) | Sharpe 1.82, MaxDD 7.7% ✅✅✅ |

**เทคนิคสำคัญ:**
- **Neutralization** — ทำให้ long/short สมดุลในแต่ละ sector → ลด risk
- **Ranking** — ใช้ลำดับสัมพัทธ์แทนค่าสัมบูรณ์ → ทนทานกว่า
- **Decay** — ถัวเฉลี่ยสัญญาณในช่วงเวลา → ลด turnover

**ขั้นตอนทั่วไป:**
1. รวบรวมข้อมูล
2. คิดไอเดีย (เช่น mean reversion)
3. เขียนเป็นนิพจน์คณิตศาสตร์
4. แปลงด้วย operation (neutralization, rank, decay)
5. ตรวจสอบความ robust
6. แปลงเป็น positions
7. Backtest

### 7. Fundamental Analysis

**เครื่องมือหลัก:**
- **Balance Sheet** — สินทรัพย์ = หนี้สิน + ส่วนของผู้ถือหุ้น
- **Income Statement** — รายได้ กำไรสุทธิ อัตรากำไร
- **Cash Flow Statement** — เงินสดจากการดำเนินงาน, การลงทุน, การจัดหาเงิน

**_factors ที่มีความสัมพันธ์เชิงบวกกับผลตอบแทนในอนาคต:**
- เพิ่มสภาพคล่อง (current assets / current liabilities)
- ไม่ออกหุ้นใหม่
- หนี้ยาว-term น้อยลง
- Cash flow จากการดำเนินงาน > 0
- Cash flow > กำไรสุทธิ (คุณภาพ earnings สูง)

**Accrual Anomaly (Sloan 2011):** ส่วนของ cash flow มีพลังในการทำนายมากกว่า accrual — นักลงทุนควรเชื่อ cash flow มากกว่าตัวเลขบัญชี

### 8. Equity Price and Volume

** Efficient Market Hypothesis (EMH):**
- **Weak**: ราคาสะท้อนข้อมูลในอดีตแล้ว
- **Semi-strong**: สะท้อนข้อมูลสาธารณะทั้งหมด
- **Strong**: สะท้อนแม้แต่ข้อมูลภายใน

**ตลาดไม่สมบูรณ์แบบ 100%** — นั่นคือโอกาสของ quantitative trader

**สูตรสำคัญ:**
```
IR = IC × √(Breadth)
```
- IC (Information Coefficient): correlation ระหว่าง predicted return กับ realized return
- Breadth: จำนวนตราสารที่เทรด
- ยิ่งเทรดบ่อย ยิ่งมีโอกาสแสดงสถิติได้ชัดเจน

### 9. Turnover (อัตราการหมุนเวียน)

**Turnover** = มูลค่าที่เทรด / มูลค่าที่ถือ

**Trade-off สำคัญ:**
- Alpha จาก price/volume มัก turnover สูงกว่า fundamental alpha
- Turnover สูง = ค่า transaction cost สูง
- ตลาดที่มี liquidity ต่ำ (เช่น Southeast Asia: spread 25-30 bps) ต้องระวังเรื่อง turnover เป็นพิเศษ

**Alpha ที่ turnover ต่ำและรักษาผลตอบแทนได้** จะ leverage ได้ดีกว่า

### 10. Overfitting — ภัยร้ายของ Backtest

**Overfitting ทำได้ง่ายมาก:**
- ลอง 7 กลยุทธ์ ก็มีโอกาสเจอ backtest Sharpe > 1 ได้แล้ว (expected out-of-sample Sharpe = 0)
- Correlation อาจหลอกได้ — ถ้ามี random time series เยอะ enough, correlation สูงได้

**วิธีหลีกเลี่ยง:**
1. **Out-of-sample test จริงๆ** — ไม่ใช่ใช้ข้อมูลเก่าแทน
2. **เพิ่ม in-sample** — ข้อมูลเยอะขึ้น, Sharpe สูงขึ้น, universe กว้างขึ้น
3. **โมเดลต้อง elegant** — เรียบง่าย มีตรรกะรองรับ
4. **พารามิเตอร์น้อย** = ไม่ sensitive
5. **Visualization** — กราฟบอกข้อมูลมากกว่าตัวเลข
6. **บันทึกจำนวนครั้งที่ทดลอง** — ช่วยประเมิน overfitting risk
7. **ใช้ artificial data** ทดสอบ
8. **เรียนรู้แบบ dynamic** (ไม่ static)

### 11-12. Alpha กับ Risk Factors และ Portfolio Risk

**CAPM:**
```
Expected Return = Risk-free Rate + Beta × Market Risk Premium
```

**Fama-French (1992):** เพิ่ม Size effect + Value effect

**Risk Factors ที่รู้จักกันดี:**
- Market risk
- Size effect (small caps > large caps)
- Value effect (high book-to-market)
- Liquidity effect
- Momentum (Jegadeesh & Titman 1993)

**ปัญหาของ Risk Factors:**
- Sharpe ratio ไม่สูงได้ (smart money จะเข้ามา arbitrage)
- Risk factor ที่ popular มาก อาจถูก **overcrowded** → ขายหนีพร้อมกัน → Quant Crisis (August 2007)

**ความสัมพันธ์ Alpha-Portfolio Risk:**
- Alpha ที่ดี = **orthogonal กับ risk factors ที่รู้จัก** + **orthogonal กับ alpha อื่นๆ**
- คำถามสำคัญ: "ใครไม่รู้จัก alpha นี้?" — ยิ่งคนรู้น้อย ยิ่งมีค่า
- Alpha ที่ทุกคนรู้จัก = ไม่ใช่ alpha อีกต่อไป

### 13. Risk และ Drawdowns

**วิธีจัดการ Drawdown:**
1. จำกัด position concentration ในหุ้น/กลุ่มอุตสาหกรรมเดียว
2. ลด exposure กับ risk factors ที่รู้จักกันดี
3. ลด exposure กับ market โดยรวม
4. ตรวจสอบ performance concentration — ต้องกระจายทั่ว sectors
5. **Quintile Analysis** — ควรมี monotonic relationship ระหว่าง quintile กับ return

### 14. Data และ Alpha Design

**แหล่งข้อมูล:**
- Academic literature (SSRN, Google Scholar)
- Data vendors (ข้อมูล price/volume, fundamental, news sentiment)
- Big Data era — Level 1 tick data ~50 GB/วัน, Social media ~58 ล้าน tweets/วัน

**Data Validation — สำคัญมาก:**
- **Timestamp** ต้องถูกต้อง — ไม่งั้น forward-looking bias
- **Data sanity check** — จุดข้อมูลเสียจุดเดียวอาจทำลาย alpha ทั้งหมดได้
- **Survival bias** — vendor อาจลอง 1000 แบบแล้วเลือกมาแค่ตัวที่ดีที่สุด

### 15. Statistical Arbitrage และ Alpha Diversity

**หลักการ:** ราคาสินทรัพย์ถูกขับเคลื่อนด้วยกฎที่ **consistent** หลายข้อ — แต่ละข้ออาจใช้ได้กับบางตราสารในบางเวลา

**Overfitting = การค้นพบ "กฎปลอม":**
- ดูดีในอดีต แต่หายไปเมื่อเวลาผ่านไป
- ตัวอย่าง: สูตรทำนายแชมป์ฟุตบอลโลก "3964" — ใช้ได้ถึงปี 2006 แล้วก็พัง

**Alpha Diversity:** บนกฎเดียวกัน สามารถสร้าง alpha หลายแบบได้ด้วย implementation ที่ต่างกัน

### 16. เทคนิคเพิ่มความ Robust

**Simple methods:**
- **Ranking** — ทนทานกว่า Pearson correlation
- **Quantiles approximation** (LQS, median squares)
- **Fisher Transform** — ทำให้ distribution เข้าใกล้ normal
- **Z-scoring** — mean=0, std=1
- **Truncation** — จำกัด position สูงสุด
- **Winsorizing** — ตัด outlier

**Advanced:** Bootstrapping, Cross-validation

### 17. Automated Alpha Search

**3 องค์ประกอบ:**
1. Input data (financial data ที่มีความหมาย)
2. Search algorithm
3. Signal testing

**เคล็ดลับ:**
- Input data ควรเป็น **ratio-like** (เปรียบเทียบระหว่างหุ้นได้)
- ไม่ควรใช้ข้อมูลจาก **หลาย category เกินไป** (overfitting)
- ทดสอบ period ยาวไม่ใช่จะดีเสมอไป — market dynamics เปลี่ยน
- **Sensitivity test** และ **Significance test** สำคัญมาก

### 18. Algorithms และ Special Techniques

- **Boosting (AdaBoost)** — รวม weak classifiers เป็น strong classifier
- **Digital Filtering** — Butterworth filter, trend/cycle decomposition
- **Feature Extraction (PCA)** — ลดมิติของข้อมูล

---

## Part III: หัวข้อเสริม

### 19. ข่าวสารและ Social Media

**News Sentiment Analysis:**
- Polarity: good/bad/neutral
- Novelty: ข่าวใหม่ vs ข่าวต่อเนื่อง
- Relevance: ข่าวเกี่ยวข้องกับหุ้นตัวไหน
- Categories: earnings, legal, macro
- "Expected" vs "Unexpected" — ข่าวที่ไม่คาดคิดมี impact มากกว่า

**Social Media:**
- Twitter, blog, forum ให้ sentiment data
- ปัญหา: noise เยอะ, ข้อมูลล้น, format ไม่เป็นทางการ
- ตัวอย่าง: ข่าวปลอม AP ระเบิด White House → DJIA ตก 100+ จุดใน 2 นาที ($136B loss)

### 20. Options Market

**สัญญาณจาก Options:**
- **Volatility Skew** — OTM put IV สูงกว่า call → บ่งบอก negative sentiment
- **Volatility Spread** — ทำนายผลตอบแทนหุ้นได้ (Cremers & Weinbaum: outperformance 50 bps/สัปดาห์)
- **Options/Stock Volume Ratio (O/S)** — สูง = informed traders ถือ negative info
- **Open Interest Changes** — put OI เพิ่ม → หุ้น underperform

### 21. Momentum Alphas

**Jegadeesh & Titman (1993):** หุ้นที่ชนะใน 3-12 เดือนที่ผ่านมา มีแนวโน้มจะชนะต่อ

**คำอธิบาย:**
- **Investor underreaction** — ตลาดใช้เวลา absorb ข้อมูลใหม่
- **Analyst underreaction** — นักวิเคราะห์ค่อยๆ ปรับประมาณการ
- **Factor momentum** — ผลตอบแทนของ factor มี momentum
- **Group momentum / Co-movement** — หุ้นที่เกี่ยวข้องเคลื่อนที่พร้อมกัน

### 22. Financial Statement Analysis

**Balance Sheet Factors:**
- Liquidity เพิ่มขึ้น → return บวก
- ไม่ออกหุ้นใหม่ → return บวก
- หนี้ยาว-term น้อย → return บวก

**Income Statement Factors:**
- กำไรสุทธิ > 0
- อัตรากำไรขั้นต้นเพิ่มขึ้น

**Growth Stocks:**
- R&D / total assets > median อุตสาหกรรม
- Capital expenditure > median
- Advertising expense > median

### 23. Institutional Research

**Academic Research:**
- แหล่ง: SSRN, arXiv, Journals (JFE, JF, RFS, AER)
- ติดตาม citation เพื่อดู debate
- ไม่เชื่อ EMH 100% — แต่ต้องตรวจสอบอย่างระมัดระวัง

**Analyst Research:**
- ดูได้จาก Yahoo Finance, MorningStar, Bloomberg
- สำคัญที่สุดคือ **วิธีคิด** ของ analyst ไม่ใช่แค่ recommendation
- **Positive bias** — analyst มัก recommend "buy" มากกว่า "sell"
- **Herding** — analyst มักไม่กล้าต่างจาก consensus

### 24-25. Futures และ Currency Trading

**Futures Trading:**
- **COT Report** — รู้ว่า "smart money" กำลังเดิมพันอะไร
- **Seasonality** — อุตสาหกรรมเกษตรมี seasonal pattern ชัดเจน
- **Risk On/Risk Off** — VIX สูง = money ไหลออกจาก equity
- **Contango/Backwardation** — ขายใน contango, ซื้อใน backwardation

**Currency Alpha:**
- Futures/forwards ได้ exposure กับ underlying factors
- แต่ละกลุ่ม instrument (commodities, currencies, bonds) มี **hedger group** ต่างกัน
- Checklist: ระบุ sector → timescale → ทดสอบ in-sample → ทดสอบ robustness → out-of-sample

---

## Part IV: WebSim™ — เครื่องมือ Backtest

**WebSim** เป็น web-based simulation platform ของ WorldQuant:

**คุณสมบัติสำคัญ:**
- สร้าง alpha จาก expression หรือ Python code
- Backtest บนข้อมูลจริง In-Sample → Out-of-sample
- Universe: TOP3000, TOP2000, TOP1000 (หุ้นที่ liquidity สูงสุด)
- Neutralization: market, industry, sub-industry, none
- วัดผล: Sharpe Ratio, PnL, Drawdown, Turnover

**เกณฑ์ Alpha ที่ดี:**
- Sharpe > 3.95 (delay 0) หรือ > 2.5 (delay 1)
- Turnover < 40-50%
- Max Drawdown < 10%
- PnL graph มี upward trend สม่ำเสมอ

**API Operators สำคัญ:**
- `Rank(x)` — จัดลำดับค่าระหว่างหุ้น
- `Decay_linear(x, n)` — ถัวเฉลี่ยถ่วงน้ำหนัก
- `IndNeutralize(x, y)` — ทำ neutralization ตาม sector
- `StdDev(x, n)`, `Correlation(x, y, n)` — สถิติพื้นฐาน
- `Ts_Rank(x, n)` — rank ภายในเวลา

---

## Part V: 7 นิสัยของ Quants ที่ประสบความสำเร็จ

1. **มุ่งมั่นทุ่มเท** — ความพยายามสม่ำเสมอชนะ IQ สูง
2. **ปรับเปลี่ยนอย่างสมเหตุสมผล** — อย่า fit ข้อมูล ให้เข้าใจเหตุผล
3. **กระตือรือร้นทดลองไอเดียใหม่** — อย่ารอให้คนอื่นทำก่อน
4. **ทำงานที่สร้างคุณค่า** — alpha จากไอเดียใหม่ > alpha จากการ tweak เล็กน้อย
5. **มีความเร่งด่วน** — มีไอเดียแล้วต้องลองทันที
6. **สร้างทีมที่ synergistic** — ทำงานร่วมกับคนที่ไว้ใจได้
7. **ตั้งเป้าสูง** — "Shoot for the moon"

---

## สรุป为核心 Concept

### Alpha ที่ดีต้องมี:
- ✅ Simplicity — เรียบง่าย สวยงาม
- ✅ Robustness — ทนทานต่อการเปลี่ยนแปลง
- ✅ Uniqueness — ไม่ correlated กับ alpha อื่น
- ✅ Low Turnover — ค่า transaction cost ต่ำ
- ✅ Out-of-sample validation — ไม่ overfit
- ✅ Risk awareness — เข้าใจ drawdown และ risk factors

### วัฏจักรของ Alpha:
```
ค้นพบ → ทดสอบ → ใช้งาน → ดึงดูด capital → decay → ตาย → เกิดใหม่
```

### บทเรียนสำคัญ:
1. **ทุก alpha จะตาย** — ต้อง cut losses เมื่อไม่ทำงาน
2. **Overfitting คือศัตรูตัวร้ายที่สุด** — backtest ดี ≠ อนาคตดี
3. **Data quality สำคัญกว่า quantity** — ข้อมูลเสียจุดเดียวทำลายได้
4. **Alpha ที่ทุกคนรู้ = ไม่มีค่า** — ต้องหาสิ่งที่คนอื่นยังไม่รู้
5. **Diversification ใช้ได้กับ alpha** — มีหลาย alpha ดีกว่าพึ่ง alpha เดียว
6. **Transaction cost สำคัญ** — alpha ที่ turnover สูงอาจถูกกินกำไรหมด
7. **Fundamental + Technical + Sentiment** — แหล่งข้อมูลที่หลากหลาย = alpha ที่หลากหลาย

---

> **หมายเหตุ:** เนื้อหาในสรุปนี้มาจากการอ่าน raw text chunks ทั้ง 34 ไฟล์ บางส่วนเป็น table of contents, figure captions, และ FAQ ของ WebSim ซึ่งรวมอยู่ด้วย สาระหลักอยู่ใน Part I-III และ Part V
