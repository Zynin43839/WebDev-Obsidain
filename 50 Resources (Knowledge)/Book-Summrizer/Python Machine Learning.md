# Python Machine Learning
> **Author:** Sebastian Raschka

---

## บทนำ

หนังสือเล่มนี้เขียนโดย Sebastian Raschka ตีพิมพ์โดย Packt Publishing ความยาว 454 หน้า ครอบคลุม machine learning ครบตั้งแต่พื้นฐานจนถึง neural network เหมาะสำหรับคนที่อยากเริ่มต้นในวงการ data science ด้วย Python

---

## Chapter 1: Giving Computers the Ability to Learn from Data

แนะนำ machine learning ว่าคืออะไร machine learning คือ field ที่ทำให้ computer เรียนรู้จากข้อมูลและตัดสินใจได้เอง โดยไม่ต้องเขียน rule ตายตัว

machine learning มี 3 ประเภทหลัก:

**Supervised Learning (การเรียนรู้แบบมีผู้สอน)**
- ใช้ข้อมูลที่มี label บอกคำตอบแล้ว train model เพื่อทำนายข้อมูลใหม่
- Classification: ทำนายค่าเป็น category เช่น spam หรือไม่ spam
- Regression: ทำนายค่าต่อเนื่อง เช่น ทำนายคะแนนสอบจากเวลาที่อ่านหนังสือ

**Reinforcement Learning (การเรียนรู้แบบเสริมแรง)**
- Agent เรียนรู้จากการ interact กับ environment โดยไม่มี label แต่มี reward signal เช่น chess engine ที่เรียนรู้จากผลชนะ/แพ้

**Unsupervised Learning (การเรียนรู้แบบไม่มีผู้สอน)**
- ใช้ข้อมูลที่ไม่มี label เพื่อค้นหา pattern ที่ซ่อนอยู่
- Clustering: จัดกลุ่มข้อมูลที่คล้ายกัน เช่น จัดกลุ่มลูกค้าจากพฤติกรรม
- Dimensionality Reduction: ลดมิติข้อมูลเพื่อ compression หรือ visualization

**Pipeline ของ machine learning:**
1. Preprocessing — ทำความสะอาดและเตรียมข้อมูล
2. Training — เลือกและ train model
3. Evaluation — ทดสอบว่า model ทำนายได้ดีแค่ไหนบนข้อมูลใหม่
4. Prediction — นำ model ไปใช้งานจริง

**Python Ecosystem:**
- ใช้ Python 3.4.3+ พร้อม library หลัก: NumPy, SciPy, scikit-learn, matplotlib, pandas
- ติดตั้งผ่าน pip หรือ Anaconda distribution

---

## Chapter 2: Training Machine Learning Algorithms for Classification

แนะนำ algorithm แรกสุดของ machine learning คือ Perceptron ของ Rosenblatt ที่สร้างจาก MCP Neuron model ปี 1943

**Perceptron Algorithm:**
- รับ input features x คูณกับ weight w แล้ว sum เป็น net input z
- Activation function (unit step function) ตัดสินใจว่า output = 1 หรือ -1
- ถ้าทำนายผิด จะ update weight ด้วยสูตร: w = w + η(y - ŷ)x
- Learning rate η คุมความเร็วในการเรียนรู้
- ทำงานได้เฉพาะข้อมูลที่ linearly separable เท่านั้น ไม่ convergent ถ้าแยกไม่ได้ด้วยเส้นตรง

**Adaline (ADAptive LInear NEuron):**
- ต่างจาก Perceptron ตรงที่ใช้ linear activation function (ไม่ใช่ unit step) ในการคำนวณ weight update
- ใช้ cost function คือ Sum of Squared Errors (SSE)
- Minimize cost ด้วย Gradient Descent
- Gradient Descent คือการเดินลงจากเนินเขาใน cost surface จนถึงจุดต่ำสุด
- Learning rate สำคัญมาก: ถ้าใหญ่เกินไปจะ overshoot global minimum, ถ้าเล็กเกินไปจะช้ามาก

**Feature Scaling (Standardization):**
- ทำให้ feature อยู่ใน scale เดียวกันด้วย standardization: (x - μ) / σ
- สำคัญมากสำหรับ gradient descent ให้ convergent เร็ว

**Stochastic Gradient Descent (SGD):**
- แทนที่จะคำนวณ gradient จากข้อมูลทั้งหมด (batch) จะคำนวณทีละตัวอย่าง
- เร็วกว่า batch gradient descent มาก เหมาะกับข้อมูลมหาศาล
- สามารถใช้กับ online learning (ข้อมูล streaming ทีละนิด)

---

## Chapter 3: A Tour of Machine Learning Classifiers Using Scikit-learn

แนะนำ classifier หลายตัวพร้อมวิธีใช้ผ่าน scikit-learn

**Logistic Regression:**
- ถึงชื่อ regression แต่ใช้สำหรับ classification
- ใช้ sigmoid function: φ(z) = 1 / (1 + e^(-z)) แปลง net input เป็นค่า probability ระหว่าง 0-1
- Cost function คือ log-likelihood ที่ penalize การทำนายผิด
- ใช้ Regularization (L2) เพื่อป้องกัน overfitting: ลดค่า weight ที่ใหญ่เกินไป
- Parameter C ควบคุม regularization strength: C น้อย = regularization แรง

**Support Vector Machine (SVM):**
- หา decision boundary ที่มี margin (ระยะห่าง) มากที่สุดระหว่างคลาส
- Support Vectors คือจุดข้อมูลที่อยู่ใกล้ decision boundary ที่สุด
- Kernel Trick: แปลงข้อมูล nonlinear ให้เป็น linear ได้ใน dimension สูง โดยไม่ต้องคำนวณ expensive dot product
- RBF kernel (Gaussian kernel) เป็น kernel ที่นิยมที่สุด
- Gamma parameter คุมความกว้างของ influence ของแต่ละ training sample

**Decision Tree:**
- ตั้งคำถามทีละขั้นเพื่อแบ่งข้อมูลออกเป็นกลุ่ม
- ใช้ Information Gain ตัดสินใจว่าจะ split ที่ feature ไหน (Entropy หรือ Gini Index)
- ง่ายต่อการ interpret แต่ overfit ง่ายถ้า tree ลึกเกินไป

**Random Forest:**
- Ensemble ของ Decision Tree หลายตัว
- แต่ละตัว train จาก bootstrap sample + random feature subset
- ลด overfitting ได้ดีกว่า Decision Tree ตัวเดียว
- แทบไม่ต้อง tune hyperparameter

**K-Nearest Neighbors (KNN):**
- Lazy learning: ไม่ได้เรียนรู้อะไร แค่จำ training data ไว้
- ตอน predict จะหา k จุดที่ใกล้ที่สุดแล้ว vote
- เรียบง่ายแต่ computation แพงตอน predict ข้อมูลเยอะ
- ระวัง curse of dimensionality: มิติเยอะ = distance ไม่มีความหมาย

---

## Chapter 4: Building Good Training Sets – Data Preprocessing

สอนวิธีเตรียมข้อมูลให้พร้อมสำหรับ machine learning

**Missing Data:**
- ลบ row/column ที่มี NaN (ง่ายแต่อาจเสียข้อมูล)
- หรือใช้ Imputation (เติมค่า mean, median, most_frequent) ด้วย sklearn Imputer

**Categorical Data:**
- Ordinal features (มีลำดับ เช่น S/M/L): แปลงเป็นตัวเลขด้วย mapping dict
- Nominal features (ไม่มีลำดับ เช่น สี): ใช้ One-Hot Encoding สร้าง binary column แต่ละค่า
- ห้ามใช้ LabelEncoder กับ nominal features จะทำให้ model เข้าใจผิดว่ามีลำดับ

**Train/Test Split:**
- แบ่งข้อมูลเป็น training set (70%) และ test set (30%)
- Test set คือ final exam ห้ามใช้ตอน train เด็ดขาด

**Feature Scaling:**
- Normalization (min-max): ปรับค่าเป็น [0, 1]
- Standardization: ปรับ mean=0, std=1 (แนะนำสำหรับ logistic regression และ SVM)

**Feature Selection:**
- L1 Regularization: ทำให้ weight บางตัวเป็น 0 (sparse solution) = automatic feature selection
- Sequential Feature Selection: เลือก subset ของ features ที่ดีที่สุด
- Random Forest Feature Importance: ดูว่า feature ไหนมีผลต่อ classification มากที่สุด

---

## Chapter 5: Compressing Data via Dimensionality Reduction

สอนวิธีลดมิติข้อมูลเพื่อลด storage และ computation

**PCA (Principal Component Analysis):**
- Unsupervised dimensionality reduction
- หา principal components ที่ capture variance ได้มากที่สุด
- แปลงข้อมูลเป็น coordinate ใหม่ที่ alignment กับ direction ของ variance

**LDA (Linear Discriminant Analysis):**
- Supervised dimensionality reduction (ใช้ label ช่วย)
- หา projection ที่ maximizes between-class variance และ minimizes within-class variance

**Kernel PCA:**
- ใช้กับข้อมูล nonlinear
- ใช้ kernel trick เหมือน SVM เพื่อ project ข้อมูลไป dimension สูงก่อนทำ PCA

---

## Chapter 6: Learning Best Practices for Model Evaluation and Hyperparameter Tuning

สอนวิธี evaluate และ tune model อย่างถูกต้อง

**Pipelines:**
- รวม preprocessing steps + classifier เข้าด้วยกัน เพื่อให้ workflow ไม่ยุ่งยากและป้องกัน data leakage

**K-Fold Cross-Validation:**
- แบ่ง training data เป็น k ส่วน สลับ train/validate k ครั้ง
- ได้ estimate ที่เสถียรกว่า holdout method

**Learning Curves & Validation Curves:**
- Learning curve: plot train vs validation accuracy ตามขนาดข้อมูล
- Validation curve: plot accuracy ตาม hyperparameter value
- ช่วย diagnosis bias-variance tradeoff

**Grid Search:**
- ลองทุก combination ของ hyperparameters แล้วเลือกตัวที่ดีที่สุด
- Nested cross-validation ป้องกัน information leak

**Performance Metrics:**
- Confusion Matrix: TP, TN, FP, FN
- Precision: ความแม่นยำในการทำนาย positive
- Recall: ความครอบคลุมของ positive cases ที่ทำนายได้
- ROC Curve: plot True Positive Rate vs False Positive Rate

---

## Chapter 7: Combining Different Models for Ensemble Learning

สอนวิธี combine หลาย model เพื่อให้ prediction ดีขึ้น

**Ensemble Learning:**
- .combine weak learners เพื่อสร้าง strong learner ที่ robust กว่า

**Majority Vote Classifier:**
- เลือก class ที่ได้รับ vote มากที่สุดจากหลาย classifier
- Hard vote: vote ตรงๆ / Soft vote: เอา average probability

**Bagging (Bootstrap Aggregating):**
- Train หลาย classifier จาก bootstrap samples ต่างกัน
- ทำให้ variance ลดลง (reduce overfitting)

**AdaBoost (Adaptive Boosting):**
- Train classifiers ทีละตัว โดยตัวถัดไปจะให้ความสำคัญกับตัวที่ทำนายผิดมากกว่า
- Weak learners ที่ combine กันแล้ว strong กว่าตัวเดียวมาก

---

## Chapter 8: Applying Machine Learning to Sentiment Analysis

สอนวิธีใช้ ML กับข้อความ

**Bag-of-Words Model:**
- แปลงข้อความเป็น feature vector โดยนับ frequency ของแต่ละคำ
- TF-IDF: คำที่ปรากฏใน document น้อยจะมี weight สูงกว่า

**Text Preprocessing:**
- Cleaning:ลบ HTML tags, stop words, stem/lemmatize
- Tokenization: ตัดคำออกจากกัน

**Online Learning:**
- ใช้ SGDClassifier กับข้อมูล streaming ได้ (out-of-core learning)
- เหมาะกับ dataset ที่ใหญ่เกินกว่าจะโหลดเข้า memory

---

## Chapter 9: Embedding a Machine Learning Model into a Web Application

สอนวิธี deploy model ขึ้น web

**Model Serialization:**
- ใช้ pickle/joblib เพื่อ save fitted model ลงไฟล์

**Flask Web App:**
- สร้าง web interface ให้ user ใส่ข้อความแล้ว predict sentiment
- ใช้ SQLite เก็บผลทำนาย
- Deploy ขึ้น server สาธารณะ

---

## Chapter 10: Predicting Continuous Target Variables with Regression Analysis

สอน regression สำหรับทำนายค่าต่อเนื่อง

**Linear Regression:**
- หาเส้นตรงที่ fit ข้อมูลได้ดีที่สุด (OLS)
- ใช้ gradient descent หา coefficients

**RANSAC Regression:**
- Robust regression ที่ทนต่อ outliers

**Regularized Regression:**
- Ridge (L2), Lasso (L1), Elastic Net ป้องกัน overfitting

**Polynomial Regression:**
- แปลง features เป็น polynomial degree สูงเพื่อ fit ข้อมูล nonlinear

**Random Forest Regression:**
- ใช้ ensemble of decision trees กับ regression problems

---

## Chapter 11: Working with Unlabeled Data – Clustering Analysis

สอน unsupervised learning สำหรับ clustering

**K-Means:**
- จัดข้อมูลเป็น k clusters โดย centroid คำนวณใหม่เรื่อยๆ
- K-Means++: เลือก initial centroids ดีกว่า random
- Elbow method: หา k ที่เหมาะสมจาก Within-Cluster Sum of Squares

**Hierarchical Clustering:**
- สร้าง dendrogram (tree diagram) แสดงลำดับการรวม clusters
- Agglomerative: เริ่มจากแต่ละจุดแล้วรวมขึ้น

**DBSCAN:**
- หา dense regions ในข้อมูล
- ทนต่อ outliers ได้ดี ไม่ต้องกำหนด k ล่วงหน้า

---

## Chapter 12: Training Artificial Neural Networks for Image Recognition

สอน neural network สำหรับ image classification

**Multi-Layer Neural Network:**
- Hidden layers ระหว่าง input กับ output
- Forward propagation: ส่งข้อมูลผ่าน network จาก input สู่ output
- Activation functions: sigmoid, tanh, ReLU

**Backpropagation:**
- คำนวณ gradient ย้อนกลับจาก output ถึง input เพื่อ update weights
- ใช้ gradient descent ในการ optimize weights

**MNIST Example:**
- Train neural network บน handwritten digit dataset
- Multi-layer perceptron ทำนายตัวเลข 0-9 ได้ถูกต้อง ~97%

**CNN (Convolutional Neural Network):**
- เหมาะกับ image data โดยเฉพาะ
- ใช้ convolutional layers เพื่อ extract features เช่น edges, textures

**RNN (Recurrent Neural Network):**
- เหมาะกับ sequence data เช่น text, time series
- มี memory ที่เชื่อมระหว่าง time steps

---

## Chapter 13: Parallelizing Neural Network Training with Theano

สอนวิธี train neural network ให้เร็วขึ้นด้วย GPU

**Theano:**
- Library สำหรับ define, optimize, evaluate mathematical expressions
- รองรับ GPU computation ทำให้ train เร็วขึ้นหลายเท่า

**Activation Functions:**
- Logistic sigmoid: สำหรับ binary output
- Softmax: สำหรับ multi-class classification
- Hyperbolic tangent: output range [-1, 1]

**Keras:**
- High-level library ที่ built บน Theano/TensorFlow
- สร้าง neural network ได้ด้วยโค้ดสั้นๆ
- รองรับ dropout, batch normalization, และ callbacks

---

## สรุปรวม

หนังสือเล่มนี้ครอบคลุม machine learning ครบจบในเล่มเดียว:

| หัวข้อ | Chapter |
|---------|---------|
| ML Concepts & Pipeline | 1 |
| Perceptron & Adaline | 2 |
| Classification Algorithms | 3 |
| Data Preprocessing | 4 |
| Dimensionality Reduction | 5 |
| Model Evaluation & Tuning | 6 |
| Ensemble Learning | 7 |
| NLP / Sentiment Analysis | 8 |
| Web App Deployment | 9 |
| Regression | 10 |
| Clustering | 11 |
| Neural Networks | 12 |
| GPU Parallelization | 13 |

**จุดเด่นของหนังสือ:**
- มีตัวอย่างโค้ด Python ครบทุก chapter
- อธิบาย theory ควบคู่กับ practice
- ใช้ scikit-learn เป็นหลัก
- เหมาะกับคนที่มีพื้นฐาน Python แล้ว
- ครอบคลุมตั้งแต่ classification ง่ายๆ จนถึง neural network

**ข้อจำกัด:**
- ตัวอย่างโค้ดบางส่วนเก่าแล้ว (scikit-learn API เปลี่ยนไปบ้าง)
- Theano หยุดพัฒนาแล้ว (แต่ concept ยังใช้ได้กับ TensorFlow/PyTorch)
- ไม่ครอบคลุม deep learning เชิงลึกมากนัก

---
