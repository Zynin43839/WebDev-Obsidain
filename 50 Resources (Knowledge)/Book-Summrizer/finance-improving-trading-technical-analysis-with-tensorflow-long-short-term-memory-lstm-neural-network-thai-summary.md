# สรุปภาษาไทย: Improving Trading Technical Analysis with TensorFlow LSTM Neural Network

> **ผู้เขียน:** C. Sang, M. Di Pierro
> **ตีพิมพ์:** The Journal of Finance and Data Science 5 (2019) 1–11
> **โครงสร้าง:** 8 บท (Introduction → TA Trading → NN/LSTM Model → Results → Sector Performance → Optimizer/LR → Conclusions → References)

---

## 1. ภาพรวมและแรงจูงใจ (Introduction)

- **Artificial Neural Network (ANN)** อยู่มานานกว่า 70 ปี เป็น model ที่ fitting ข้อมูลแบบ non-linear โดยปรับ weights ระหว่างชั้น neurons คล้ายสมองมนุษย์
- ANN มีจุดแข็งคือ **feature detection** — ข้อดีกว่า traditional algorithms ตรงที่ไม่ต้อง describe features ด้วยมือ
- งานวิจัยนี้ตั้งคำถามใหม่ว่า: แทนที่จะใช้ NN ทำนายราคาตรงๆ เราสามารถใช้ NN **เรียนรู้ว่า traditional TA strategy ตัวไหนทำงานได้ดีในช่วงเวลาไหน** ได้หรือไม่?
- แรงจูงใจสำคัญมาจาก 3 ปัจจัย:
  1. ผลลัพธ์ที่ดีจาก Google ในการใช้ RNN กับ image/speech recognition
  2. Open source libraries เช่น **TensorFlow** แพร่หลาย
  3. GPU ราคาถูกลงมาก (multi-core NVIDIA)

### สมมติฐานหลัก
- TA strategies ทำงานได้เพราะ **self-fulfilling prophecy** — นักลงทุนจำนวนมากใช้ algorithm เดียวกัน ทำให้ราคากลายเป็น pattern จริงๆ
- แต่นักลงทุนไม่ได้ follow algorithm 机械istically — มี subjective judgment ผสมอยู่
- **LSTM สามารถเรียนรู้ subjective judgment เหล่านั้น** และทำนายว่า strategy จะ profitable เมื่อไหร่

---

## 2. Technical Analysis Trading Strategies

ใช้ 3 strategies ที่เป็นที่นิยมและ implement ง่าย:

### 2.1 Simple Moving Average (SMA)
- **สูตร:** SMA(t) = (1/N) × Σ P(t-i) จาก i=1 ถึง N
- **พารามิเตอร์:** N = 20 (based on งานวิจัย Praekhaow ที่พบว่า 20 วันให้ผลดีที่สุดสำหรับ short-term trading)
- **กลยุทธ์:** ซื้อเมื่อ P(t) > SMA(t), ขายเมื่อ P(t) < SMA(t)

### 2.2 Relative Strength Index (RSI)
- **สูตร:** RSI(t) = 100 - 100/(1 + RS(t)) โดย RS = average gain / average loss
- **พารามิเตอร์:** N = 14 (แนะนำจากงานวิจัย Terence et al.)
- **กลยุทธ์:** ซื้อเมื่อ RSI(t) > 50, ขายเมื่อ RSI(t) < 50

### 2.3 Moving Average Convergence Divergence (MACD)
- **สูตร:** MACD(t) = EMA(M) - EMA(N) ที่ lag L
- **พารามิเตอร์:** M=12, N=26, L=9 (มาตรฐานจาก literature)
- **กลยุทธ์:** ซื้อเมื่อ MACD(t) > 0, ขายเมื่อ MACD(t) < 0

**ข้อจำกัดสำคัญ:** ทุก trade สมมติว่า execute ตอน end-of-day ด้วย closing price — ไม่มี slippage, commission, หรือ cost ใดๆ

---

## 3. Neural Network และ TensorFlow LSTM Model

### 3.1 องค์ประกอบ 5 อย่างของ NN
1. **Computer program** — ใช้ TensorFlow (จาก Google) เพราะติดตั้งง่าย, เร็ว, customize ด้วย Python ได้
2. **Model** — architecture ของ neurons, layers, connections (สำคัญที่สุด)
3. **Weights** — ค่าที่ train ได้
4. **Training data** — ใช้ train model
5. **Test data** — ใช้ benchmark ผลลัพธ์ (ไม่ใช้ตอน train)

### 3.2 LSTM Architecture
- LSTM เป็น sub-type ของ **Recurrent Neural Network (RNN)** ที่ออกแบบมาสำหรับ time-series/sequential data โดยเฉพาะ
- แต่ละ LSTM unit มี **4 gates**:
  1. **Forget Gate (f_t)** — ตัดสินใจว่าข้อมูลเก่าตัวไหนควรลืม
  2. **Input Gate (i_t)** — ตัดสินใจว่าข้อมูลใหม่ตัวไหนควรเก็บ
  3. **Output Gate (o_t)** — ตัดสินใจว่าจะ output อะไร
  4. **State Update (c_t)** — อัพเดท memory state

- ใช้ **Truncated Backpropagation Through Time (BPTT)** แทน full BPTT — ป้องกัน **gradient vanishing problem** ที่เกิดบ่อยใน RNN

### 3.3 Hyperparameters ที่ใช้
| พารามิเตอร์ | ค่า | เหตุผล |
|:---|:---|:---|
| Learning rate | 0.0001 | ดีที่สุดจาก实验 (ไม่太大 = gradient explosion, ไม่เล็กเกิน = stuck in local min) |
| Batch size | 15 | เหมาะกับ dataset เล็ก |
| Hidden layer | 1 (shallow) | Dataset เล็ก → ไม่ต้องลึก |
| Cell size | hidden unit ÷ 4 | เพราะ 1 LSTM unit = 4 gates |
| Dropout | ไม่ใช้ | ทำให้ under-fitting กับ data เล็ก |
| Batch Normalization | ใช้ | Normalize ก่อน activation → train เร็วขึ้น |

### 3.4 ข้อมูล
- **Training:** 1,764 samples (ข้อมูลปี 2014)
- **Testing:** 504 samples (ข้อมูลปี 2015)
- Input: ราคาปิดย้อนหลัง 7 วัน P(t-i) สำหรับ i=1..7
- Output: ทำนายว่า strategy จะ profit หรือ loss

---

## 4. ผลลัพธ์ (Results)

### 4.1 Sector ETF Performance (9 sectors)
| Strategy | TF outperform sectors | Alpha (รวม) |
|:---|:---:|:---:|
| SMA | 6/9 | **+$18.25** |
| RSI | 6/9 | **+$43.63** |
| MACD | 5/9 | **+$16.58** |

- TensorFlow **outperform ทุก strategy เมื่อรวม profit**
- Alpha สูงสุดจาก RSI strategy (+$43.63 per share)

### 4.2 Individual Stock Performance (45 หุ้น)
| Strategy | TF outperform stocks | Alpha (รวม) |
|:---|:---:|:---:|
| SMA | 24/45 | **+$198.66** |
| RSI | 22/45 | **+$10.81** |
| MACD | 24/45 | **+$368.96** |

- MACD strategy ให้ alpha สูงสุด (+$368.96) แต่ SMA มี consistency สูงสุด

### 4.3 ตัวอย่างหุ้นที่ TF ช่วยได้มาก (SMA strategy)
| หุ้น | ก่อน TF | หลัง TF | Alpha |
|:---|:---:|:---:|:---:|
| GOOGL | $202.53 | $211.01 | +$413.53 |
| AMZN | $100.90 | $332.11 | +$231.21 |
| MSFT | $41.58 | $14.33 | +$55.90 |
| UNP | $6.81 | $48.18 | +$54.99 |

---

## 5. Optimizer เปรียบเทียบ

| Optimizer | เวลา (s) | Cross Entropy Cost |
|:---|:---:|:---:|
| Gradient Descent | 0.305 | 1.030 |
| **Adam** | **0.321** | **0.910** |
| Adadelta | 0.336 | 1.187 |
| **Adagrad** | **0.315** | **0.831** |
| RMSProp | 0.331 | 0.945 |
| Momentum | 0.316 | 1.010 |

- **Adam** ให้ผลดีที่สุดโดยรวม (เร็ว + cost ต่ำ + return สูงสุดหลัง train)
- เวลา train ทุก optimizer อยู่ที่ ~0.3s ต่อ sector (CPU ตัวเดียว)

---

## 6. Learning Rate Analysis

| Learning Rate | SMA Cost | RSI Cost | MACD Cost |
|:---|:---:|:---:|:---:|
| 0.00001 | 0.99 | 1.20 | 1.05 |
| 0.00005 | 0.91 | 0.95 | 0.90 |
| **0.0001** | **0.89** | **0.81** | **0.85** |
| 0.0005 | 0.89 | 0.95 | 0.88 |
| 0.001 | 1.05 | 1.04 | 0.94 |
| 0.005 | 1.63 | 1.77 | 1.65 |
| 0.01 | 2.05 | 2.23 | 2.03 |
| 0.05 | 2.21 | 2.42 | 2.39 |

- **ช่วงที่ดีที่สุด:** 0.00005 ถึง 0.001
- **Optimal:** 0.0001 — ให้ cost ต่ำสุดในทุก strategy
- Learning rate สูง (>0.005) → cost พุ่งขึ้น 2-3 เท่า

---

## 7. บทสรุป (Conclusions)

1. **LSTM/RNN ใช้ได้ดีกับ financial time series** เพราะข้อมูลการเงินมี time-series correlation อยู่แล้ว
2. **TensorFlow ช่วย improve traditional TA strategies** ได้จริง — ไม่ได้มาแทนที่ แต่เป็น **filter layer** ที่ช่วยตัดสินใจว่าเมื่อไหร่ควรใช้ strategy
3. ผลลัพธ์ consistently positive ทั้ง SMA, RSI, MACD
4. **Adam optimizer + learning rate 0.0001** เป็น configuration ที่ดีที่สุด
5. Parameter sensitivity สูง — learning rate และ cell size เปลี่ยนเล็กน้อยก็กระทบผลลัพธ์มาก

---

## 8. ข้อคิดสำหรับงาน trading ของเรา

### สิ่งที่ paper นี้สอน
1. **TA strategies ไม่ได้ profit เสมอไป** — ต้องมี filter ว่าเมื่อไหร่ควรเข้า/ไม่เข้า
2. **NN เป็น "meta-strategy"** ที่เรียนรู้ว่า strategy ตัวไหน work เมื่อไหร่ — ต่างจาก NN ที่ทำนายราคาตรงๆ
3. **Simple NN (shallow LSTM)** ก็ให้ผลดีได้ — ไม่ต้อง deep network เสมอไป
4. **Data split สำคัญ:** train 2014, test 2015 — ถ้า data leakage จะได้ผลเกินจริง

### ข้อจำกัดที่ต้องระวัง
- **ไม่มี transaction costs** — ผลจริงจะต่ำกว่ามาก
- **Daily close only** — ไม่考虑 intraday execution
- **Sample size เล็ก** (1,764 train / 504 test) — อาจ overfit
- **ปี 2014-2015 เท่านั้น** — ไม่覆盖 bear market หรือ crisis
- **Single CPU** — speed ~0.3s ต่อ sector, GPU จะเร็วกว่ามาก

### สำหรับ project ของเรา
- แนวคิด **"use NN as filter for traditional strategies"** ตรงกับ approach ที่เราใช้ — BOCPD regime gate ทำหน้าที่คล้ายกัน (filter ว่าเมื่อไหร่ SMC strategy จะ work)
- **LSTM สำหรับ regime detection** อาจเป็นทางเลือกที่ดี — ลอง train LSTM ทำนาย trending/ranging regime แทน HMM
- **Batch Normalization** ควรลองใช้กับ model ของเรา — ช่วย train เร็วขึ้น
- **Parameter sensitivity** สูง → ต้อง cross-validate ดีๆ ไม่งั้น overfit แน่ๆ
