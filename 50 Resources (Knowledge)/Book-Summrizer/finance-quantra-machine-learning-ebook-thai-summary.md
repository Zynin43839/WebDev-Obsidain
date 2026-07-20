# สรุปหนังสือ: How to Trade Using Machine Learning — QuantInsti (Quantra)

> **ที่มา:** QuantInsti's Machine Learning eBook
> **สรุปโดย:** Agent (วันที่ 2026-07-20)

---

## ภาพรวม

หนังสือเล่มนี้เป็นคู่มือแนะนำการใช้ Machine Learning (ML) โดยเฉพาะ **Artificial Neural Network (ANN)** ในการทำนายทิศทางราคาหุ้นและสร้างกลยุทธ์เทรดอัตโนมัติ เนื้อหาครอบคลุมตั้งแต่พื้นฐาน ML ไปจนถึงการเขียนโค้ดสร้าง ANN สำหรับทำนายความเคลื่อนไหวของราคาหุ้น และคำนวณผลตอบแทนของกลยุทธ์

---

## 1. Machine Learning คืออะไร?

- **Machine Learning** เป็นสาขาหนึ่งของ Artificial Intelligence (AI) ที่ให้โปรแกรม "เรียนรู้" จากข้อมูลเพื่อทำนายผลลัพธ์ได้ด้วยตนเอง ไม่ใช่แค่ทำตามคำสั่งตายตัว
- ML มีความเชื่อมโยงกับ **computational statistics** — ใช้คอมพิวเตอร์ช่วยคำนวณและทำนาย
- ตัวอย่างง่ายๆ: Facebook ใช้ ML จดจำใบหน้าและ tag รูปจากฐานข้อมูลผู้ใช้หลายพันล้านคนได้อย่างแม่นยำ
- จุดต่างสำคัญระหว่าง algorithm ธรรมดา กับ ML algorithm คือ **"learning model"** — ทำให้เครื่องจักรเรียนรู้จากข้อมูลและตัดสินใจเองได้ ตั้งแต่任务ง่ายๆ เช่น การเขียนตัวอักษร ไปจนถึง任务ซับซ้อน เช่น รถขับเคลื่อนอัตโนมัติ

---

## 2. Machine Learning ในตลาดการเงิน

### ทำไมเทรดเดอร์ถึงหันมาใช้ ML?
- เพื่อใช้ประโยชน์จากการคำนวณทางคณิตศาสต์/สถิติที่ซับซ้อนเกินกว่าจะทำด้วยวิธีธรรมดา
- **Feature Selection** — ML สามารถวิเคราะห์ indicator หลายร้อยตัวพร้อมกัน แล้วเลือกตัวที่ทำนายได้ดีที่สุดเอง แทนที่เทรดเดอร์จะเลือกแค่ RSI หรือ MA ไม่กี่ตัว

### ตัวอย่างเปรียบเทียบ
| เทรดเดอร์ธรรมดา | เทรดเดอร์ + ML |
|:---|:---|
| ใช้ indicator ไม่กี่ตัวที่คุ้นเคย (RSI, MA) | วิเคราะห์ indicator หลายร้อยตัว |
| ตัดสินใจด้วยประสบการณ์ส่วนตัว | ปล่อยให้ ML เลือก indicator ที่มีประสิทธิภาพสูงสุด |
| Feature ที่ใช้จำกัด | Feature ได้แก่: price change, buy/sell signals, volatility, volume weights ฯลฯ |

### ข้อควรระวัง
- กลยุทธ์ที่ backtest ได้ผลดีบนข้อมูลในอดีต **ไม่รับประกันว่าจะได้ผลในตลาดจริง** — ประสิทธิภาพที่แท้จริงขึ้นอยู่กับสิ่งที่เกิดขึ้นใน live market

---

## 3. Artificial Neural Network (ANN)

### 3.1 ANN คืออะไร?
- **ANN** เป็นโครงข่ายประมวลผลข้อมูลที่เลียนแบบการทำงานของสมองมนุษย์ ใช้จำลองพฤติกรรมของระบบซับซ้อนผ่านคอมพิวเตอร์

### โครงสร้างของ Neuron
| ชีววิทยา (สมอง) | คอมพิวเตอร์ (ANN) |
|:---|:---|
| **Dendrites** — รับสัญญาณ | **Input Layer** — รับข้อมูล |
| **Axon** — ส่งสัญญาณ | **Output Layer** — ส่งผลลัพธ์ |
| **Cell Body** — ประมวลผล | **Hidden Layer** — คำนวณ |

### วิธีการทำงาน
1. **Input** แต่ละตัวมี **Weight (Wi)** กำกับ
2. Weight คูณกับค่า input แล้วเก็บเป็น **weighted sum**
3. ผ่าน **Activation Function** → ได้ output สัญญาณ
4.  weights เริ่มต้นจะถูกปรับแก้ไขในขั้นตอน **training** ผ่าน:
   - **Gradient Descent** — ปรับ weight ให้ loss ต่ำสุด
   - **Backpropagation** — คำนวณ gradient ย้อนกลับจาก output ไป input
5. ถ้าผลลัพธ์ "ดี" → ไม่ต้องแก้ weight / ถ้าผลลัพธ์ "ไม่ดี" → แก้ weight เพื่อปรับปรุงผลลัพธ์ถัดไป

### 3.2 ANN ใน Scikit-Learn (MLP Classifier)

**Multi-Layer Perceptron (MLP)** เป็น classifier ที่มีหลายชั้น (layer) เชื่อมต่อกันแบบ fully connected

```python
from sklearn.neural_network import MLPClassifier
X = [[0., 0.], [1., 1.]]
y = [0, 1]
clf = MLPClassifier(solver='lbfgs', alpha=1e-5,
                    hidden_layer_sizes=(5, 2), random_state=1)
clf.fit(X, y)
clf.predict([[2., 2.], [-1., -2.]])  # → array([1, 0])
```

**คุณสมบัติสำคัญของ MLP:**
- ใช้ **Backpropagation** ในการ train
- ใช้ **Cross-Entropy loss** สำหรับ classification
- รองรับ **multi-class classification** ผ่าน Softmax output
- รองรับ **multi-label classification** (ข้อมูลหนึ่งตัว belongs to หลาย class ได้)
- ใช้ **logistic function** สำหรับ multi-label output (≥ 0.5 = 1, < 0.5 = 0)

---

## 4. Implementing ANN ทำนายการเคลื่อนไหวของหุ้น — Walkthrough

### 4.1 เตรียมข้อมูล
- ข้อมูล: **RELIANCE.NS** (ห้ニ Reliance บน NSE ของอินเดีย) OHLC ตั้งแต่ 1996-2018
- ใช้แค่ **Open, High, Low, Close** (ไม่ใช้ Adjusted Close หรือ Volume)

### 4.2 Feature ที่สร้างขึ้น (Input Features)

| # | Feature | สูตร |
|:---:|:---|:---|
| 1 | **H-L** | High − Low |
| 2 | **O-C** | Close − Open |
| 3 | **3-day MA** | ค่าเฉลี่ยเลื่อน 3 วัน |
| 4 | **10-day MA** | ค่าเฉลี่ยเลื่อน 10 วัน |
| 5 | **30-day MA** | ค่าเฉลี่ยเลื่อน 30 วัน |
| 6 | **Std_dev** | Standard deviation 5 วัน |
| 7 | **RSI** | Relative Strength Index (period=9) |
| 8 | **Williams %R** | Williams %R (period=7) |

**Output (Target Variable):** `price_rise` — 1 ถ้าราคาปิดพรุ่งนี้ > ราคาปิดวันนี้, 0 ถ้าไม่ใช่

### 4.3 แบ่งข้อมูล Train/Test
- **Train 80% / Test 20%** (split = int(len(dataset) * 0.8))
- **StandardScaler** — ปรับ mean = 0, variance = 1 เพื่อป้องกัน bias จาก scale ที่ต่างกันของ feature

### 4.4 สร้าง ANN ด้วย Keras

```
Input Layer (8 features) → Hidden Layer 1 (128 neurons, ReLU) → Hidden Layer 2 (128 neurons, ReLU) → Output Layer (1 neuron, Sigmoid)
```

**พารามิเตอร์สำคัญ:**
| Parameter | ค่า | ความหมาย |
|:---|:---:|:---|
| `units` | 128 | จำนวน neurons ใน hidden layer |
| `kernel_initializer` | 'uniform' | เริ่มต้น weight จาก uniform distribution |
| `activation` | 'relu' | Rectified Linear Unit function |
| `optimizer` | 'adam' | ตัวปรับ weight (extension ของ SGD) |
| `loss` | 'mean_squared_error' | ค่าที่ต้องการ minimize |
| `metrics` | ['accuracy'] | วัดผลด้วย accuracy |
| `batch_size` | 10 | จำนวน data points ต่อการคำนวณ 1 ครั้ง |
| `epochs` | 100 | จำนวนรอบที่ train ทั้ง dataset |

### 4.5 ทำนายและคำนวณผลตอบแทน

1. **ทำนาย** ด้วย `classifier.predict(X_test)` → แปลงเป็น True/False (ถ้า > 0.5)
2. **Long position** ถ้าทำนายว่าราคาขึ้น (True), **Short position** ถ้าทำนายว่าราคาลง (False)
3. คำนวณ **log returns** ของราคาปิดวันนี้ vs เมื่อวาน
4. **Strategy Returns** = Long: +Tomorrows Returns / Short: −Tomorrows Returns
5. Plot เปรียบเทียบ **Cumulative Market Returns** vs **Cumulative Strategy Returns**

### 4.6 ผลลัพธ์
- กราฟสีเขียว = Strategy Returns (ผลตอบแทนกลยุทธ์)
- กราฟสีแดง = Market Returns (ผลตอบแทนตลาด)
- กลยุทธ์ ANN แสดงให้เห็นว่าสามารถ outperform market ได้ในบางช่วง

---

## 5. ข้อสรุปและข้อแนะนำ

### ข้อจำกัดของโมเดลนี้
- โมเดลนี้สร้างจาก **daily price data** เพื่อความเข้าใจง่าย — จริงๆ ควรใช้ **minute data หรือ tick data** เพื่อให้มีข้อมูลมากพอ (แนะนำ > 100,000 data points) สำหรับ training ที่มีประสิทธิภาพ
- การทำนายหุ้นด้วย ANN เป็นเพียง **ส่วนเล็กๆ** ของ ML application ใน trading — ยังมี Regression, Classification, SVM และเทคนิคอื่นๆ อีกมาก

### สำหรับการเทรดจริง
- **Feature engineering สำคัญมาก** — เลือก feature ที่เหมาะสมมีผลโดยตรงต่อประสิทธิภาพโมเดล
- **StandardScaler จำเป็น** — ป้องกัน bias จาก scale ที่ต่างกันของ feature
- **Train/Test split 80/20** เป็นมาตรฐานเบื้องต้น — ควรใช้ walk-forward validation ในงานจริง
- **Backtesting ≠ Live trading** — ผลลัพธ์ในอดีตไม่รับประกันผลลัพธ์ในอนาคต

---

## สรุปสั้นใน 3 บรรทัด

หนังสือเล่มนี้สอนการใช้ **Artificial Neural Network (ANN)** สำหรับเทรดหุ้น โดยใช้ feature 8 ตัว (H-L, O-C, MA3/10/30, Std_dev, RSI, Williams %R) สร้างโมเดลทำนายว่าราคาจะขึ้นหรือลง แล้วเปรียบเทียบผลตอบแทนกับการถือครองเฉยๆ (buy and hold) เป็นคู่มือเบื้องต้นที่ดีสำหรับผู้เริ่มต้น แต่ควรใช้ minute/tick data มากกว่า daily data และควรใช้ walk-forward validation แทน simple train/test split เพื่อความน่าเชื่อถือมากขึ้น
