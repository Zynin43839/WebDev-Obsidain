# Hands-On Machine Learning with Scikit-Learn and TensorFlow

> **Author:** Aurélien Géron

---

## Part I: พื้นฐาน Machine Learning

### Chapter 1: The Machine Learning Landscape — ภาพรวมของ ML

Machine Learning คือวิทยาศาสตร์ที่ทำให้คอมพิวเตอร์เรียนรู้จากข้อมูล โดยไม่ต้องเขียน rule แบบ manual

**ทำไมต้องใช้ ML?** แทนที่จะเขียน rule ยาวๆ เหมือน spam filter แบบเก่า ML ทำให้โค้ดสั้นลง ดูแลง่าย และปรับตัวตามข้อมูลใหม่ได้เอง

**ประเภทของ ML Systems** แบ่งตาม 3 มิติ:
- **Supervised vs Unsupervised Learning**: Supervised มี label (เช่น spam/ham), Unsupervised ไม่มี label (เช่น clustering)
- **Batch vs Online Learning**: Batch .train ทีเดียวทั้งชุด, Online เรียน incrementally ทีละนิด
- **Instance-based vs Model-based**: Instance-based จำตัวอย่างแล้วเปรียบเทียบ, Model-based สร้าง model แล้ว predict

**Supervised Learning** มี 2 งานหลัก — Classification (ทำนาย class) เช่น k-NN, SVM, Decision Trees, Neural Networks และ Regression (ทำนายค่าตัวเลข) เช่น Linear Regression

**Unsupervised Learning** มีหลายประเภท — Clustering (k-Means, HCA), Dimensionality Reduction (PCA, t-SNE), Anomaly Detection, Association Rules (Apriori)

**Reinforcement Learning** — Agent เรียนรู้จาก environment ผ่าน reward/penalty เช่น AlphaGo

**Challenges หลักของ ML**:
- **ข้อมูลไม่พอ (Insufficient Data)** — ต้องมี training data มากพอ ยิ่งซับซ้อนยิ่งต้องการมาก
- **ข้อมูลไม่ representative (Nonrepresentative)** — sampling bias เช่น โพลเลือกตั้ง 1936 ผิดเพราะคนจนไม่ได้โหวต
- **ข้อมูลคุณภาพต่ำ (Poor Quality)** — มี noise, outliers, missing values
- **Feature ไม่เกี่ยวข้อง (Irrelevant Features)** — ต้องทำ feature engineering
- **Overfitting** — model จำ training data มากเกินไป  generalize ไม่ได้ แก้โดย regularization, เพิ่มข้อมูล, ลด noise
- **Underfitting** — model ง่ายเกินไป จับ pattern ไม่ได้ แก้โดยใช้ model ที่ซับซ้อนขึ้น, เพิ่ม feature

**Testing & Validation** — แบ่ง data เป็น train/test set (80/20) แล้วใช้ cross-validation เพื่อ tune hyperparameter โดยไม่ data snooping bias

**No Free Lunch Theorem** — ไม่มี model  nào ดีที่สุดสำหรับทุก dataset ต้องเลือกและทดลอง

---

### Chapter 2: End-to-End ML Project — โปรเจค ML ตั้งแต่ต้นจนจบ

ใช้ California Housing Dataset เป็น case study ครอบคลุมทุกขั้นตอนของ ML project จริง

**ขั้นตอนที่ 1: มองภาพรวม (Look at the Big Picture)**
- Frame ปัญหาว่าเป็น Supervised/Unsupervised? Regression/Classification?
- เลือก Performance Measure เช่น RMSE สำหรับ regression
- ตรวจสอบ assumptions ที่ทำไว้

**ขั้นตอนที่ 2: ได้มาซึ่งข้อมูล (Get the Data)**
- สร้าง workspace ติดตั้ง Python, Jupyter, Scikit-Learn
- Download data แล้วดูโครงสร้างด้วย head(), info(), describe()
- สร้าง Test Set ด้วย Stratified Sampling เพื่อไม่ให้ sampling bias

**ขั้นตอนที่ 3: สำรวจและ visualize ข้อมูล**
- Plot scatterplot ทางภูมิศาสตร์ (latitude/longitude vs price)
- หา correlation ด้วย Pearson's r และ scatter matrix
- สร้าง new features จาก combinations ที่มีความหมาย เช่น rooms_per_household

**ขั้นตอนที่ 4: เตรียมข้อมูล (Data Preparation)**
- **Data Cleaning** — จัดการ missing values ด้วย Imputer (median strategy)
- **Categorical Features** — แปลง text เป็น numbers ด้วย LabelEncoder + OneHotEncoder
- **Custom Transformers** — เขียน class ที่มี fit/transform สำหรับ feature engineering
- **Feature Scaling** — MinMaxScaler (0-1) หรือ StandardScaler (mean=0, std=1)
- **Pipelines** — ใช้ Pipeline + FeatureUnion จัดลำดับ transformation อัตโนมัติ

**ขั้นตอนที่ 5: เลือกและ train model**
- Linear Regression → RMSE = 68,628 (underfitting)
- DecisionTreeRegressor → RMSE = 0 (overfitting รุนแรง)
- RandomForestRegressor → RMSE = 52,634 (romise ที่ดีที่สุดตอนนี้)

**ขั้นตอนที่ 6: Fine-tune model**
- **GridSearchCV** — ลองทุก combination ของ hyperparameters
- **RandomizedSearchCV** — สุ่ม combination เหมาะกับ search space ใหญ่
- **Ensemble Methods** — รวม model หลายตัวเข้าด้วยกัน

**ขั้นตอนที่ 7-8: ทดสอบบน test set แล้ว deploy**
- ประเมิน final model บน test set (RMSE ≈ 48,209)
- Deploy แล้ว monitor performance ต่อเนื่อง retrain ด้วย fresh data เป็นประจำ

---

### Chapter 3: Classification — การจำแนกประเภท

**MNIST Dataset** — 70,000 ภาพตัวเลข 28×28 pixels (784 features) เป็น "Hello World" ของ ML

**Binary Classifier** — ตัวอย่าง: SGDClassifier สำหรับ detect digit 5

**Performance Measures** สำคัญมากสำหรับ classification:
- **Accuracy** — ไม่เหมาะกับ skewed dataset (ตัวอย่าง Never5Classifier ได้ 90% accuracy ทั้งที่เดาผิดหมด)
- **Confusion Matrix** — นับ TP, TN, FP, FN
- **Precision** = TP/(TP+FP) — ทำนาย positive ได้แม่นแค่ไหน
- **Recall** = TP/(TP+FN) — จับ positive ได้ครบแค่ไหน
- **F1 Score** — harmonic mean ของ precision กับ recall
- **Precision/Recall Tradeoff** — เพิ่ม precision ลด recall และกลับกัน ขึ้นกับ threshold
- **ROC Curve** — plot TPR vs FPR, วัดด้วย AUC (ยิ่งใกล้ 1 ยิ่งดี)

**Multiclass Classification** — ใช้ OvA (One-vs-All) หรือ OvO (One-vs-One) แปลง binary classifier เป็น multiclass

**Error Analysis** — วิเคราะห์ confusion matrix เพื่อหาว่า classes ไหนสับสนกัน แล้วแก้ไข เช่น เพิ่ม training data, ทำ data augmentation

**Multilabel & Multioutput Classification** — ทำนายหลาย labels พร้อมกัน เช่น face recognition, image denoising

---

### Chapter 4: Training Models — การ train model เชิงคณิตศาสตร์

**Linear Regression** — ทำนายค่าด้วย weighted sum: y = θ₀ + θ₁x₁ + θ₂x₂ + ...
- **Normal Equation** — แก้สมการปิด (closed-form) θ = (XᵀX)⁻¹Xᵀy เร็วสำหรับ training set เล็ก แต่ช้าเมื่อ feature มาก (>100k)
- **Gradient Descent** — ค่อยๆ ปรับ θ เพื่อลด cost function เหมาะกับข้อมูลใหญ่
  - **Batch GD** — ใช้ทั้ง dataset ทุก step ช้าแต่แม่น
  - **Stochastic GD** — ใช้ 1 instance ต่อ step เร็วแต่กระโดดไปมา
  - **Mini-batch GD** — ใช้กลุ่มเล็กๆ balance ระหว่างสองแบบ
- **Learning Rate** — ถ้าเล็กเกินไปช้า ถ้าใหญ่เกินไป diverge

**Polynomial Regression** — เพิ่ม powers ของ features เพื่อ fit ข้อมูล nonlinear ด้วย linear model

**Learning Curves** — plot performance vs training set size เพื่อดู overfitting/underfitting
- Underfitting: ทั้ง train และ validation error สูงและชิดกัน
- Overfitting: train error ต่ำ แต่ gap กับ validation error ใหญ่

**Bias/Variance Tradeoff** — generalization error = bias + variance + irreducible error
- เพิ่ม complexity → ลด bias แต่เพิ่ม variance
- ลด complexity → เพิ่ม bias แต่ลด variance

**Regularization** — จำกัด degrees of freedom ของ model เพื่อลด overfitting
- **Ridge Regression (L2)** — เพิ่ม penalty = αΣθᵢ²
- **Lasso Regression (L1)** — เพิ่ม penalty = αΣ|θᵢ| ทำ feature selection อัตโนมัติ
- **Elastic Net** — ผสม L1+L2
- **Early Stopping** — หยุด train เมื่อ validation error เริ่มเพิ่มขึ้น

**Logistic Regression** — ใช้ sigmoid function ทำนาย probability สำหรับ classification
- Cost function = log loss (cross-entropy) convex แก้ด้วย Gradient Descent ได้
- Decision boundary เป็นเส้นตรง (linear)

**Softmax Regression** — ขยาย Logistic Regression เป็น multiclass โดยตรง ใช้ cross-entropy cost function

---

### Chapter 5: Support Vector Machines (SVM)

**Linear SVM** — หา hyperplane ที่กว้างที่สุด (large margin classification) ระหว่าง classes
- **Support Vectors** — instances ที่อยู่บนขอบ margin เป็นตัวกำหนด decision boundary
- **Soft Margin Classification** — ยอมให้มี margin violations บ้าง ควบคุมด้วย hyperparameter C (C น้อย = margin กว้าง แต่ violations มาก)

**Nonlinear SVM** — จัดการข้อมูลที่ไม่ linearly separable
- **Polynomial Features** — เพิ่ม features เช่น x², x³ เพื่อทำให้ linearly separable
- **Kernel Trick** — ไม่ต้องคำนวณ polynomial features จริงๆ แต่ได้ผลเหมือนกัน เร็วมาก
  - Polynomial Kernel: kernel="poly"
  - Gaussian RBF Kernel: kernel="rbf" — เหมาะกับ dataset ซับซ้อน ควบคุมด้วย γ (gamma)
- **Similarity Features** — เพิ่ม features จาก Gaussian RBF similarity กับ landmarks

**SVM Regression** — ใช้ SVM สำหรับ regression โดยพลิก concept: หา margin ที่มี instances น้อยที่สุดอยู่นอก margin

**Under the Hood** — SVM แปลงเป็น Quadratic Programming problem, มี Dual Problem ที่แก้ได้เร็วกว่า, Kernelized SVM ทำให้ไม่ต้องคำนวณ feature space ขนาดใหญ่

---

### Chapter 6: Decision Trees

**Decision Tree** — แยกข้อมูลด้วย questions ซ้ำๆ เหมือน flowchart
- เหมาะมากสำหรับ classification และ regression
- **CART Algorithm** —  split ทีละ feature ทีละ threshold หา split ที่ลด impurity มากที่สุด
- **Gini Impurity vs Entropy** — Gini เร็วกว่าเล็กน้อย, Entropy ให้ tree สมดุลกว่า

**Prediction** — เดินตาม tree จาก root ถึง leaf แล้ว predict ค่าของ leaf นั้น

**Regularization** — ป้องกัน overfitting ด้วย:
- max_depth — จำกัดความลึกของ tree
- min_samples_split — ต้องมี samples มากพอถึงจะ split
- min_samples_leaf — ต้องมี samples ใน leaf มากพอ

**Regression** — Decision Tree ทำ regression ได้ด้วย โดย predict ค่าเฉลี่ยของ samples ใน leaf

**Instability** — Decision Tree ค่อนข้างไม่เสถียร เปลี่ยน training data เล็กน้อย ผลลัพธ์อาจเปลี่ยนมาก แก้ด้วย Random Forest

---

### Chapter 7: Ensemble Learning & Random Forests

**Ensemble Learning** — รวม model หลายตัวเข้าด้วยกัน ผลลัพธ์มักดีกว่า model เดี่ยว

**Voting Classifiers** — แต่ละ model vote แล้วเลือก class ที่ได้ vote มากที่สุด (Hard Voting) หรือ average probability (Soft Voting)

**Bagging & Pasting** — train model หลายตัวบน random subsets ต่างกัน
- **Bagging** — สุ่มแบบมีคืน (with replacement)
- **Pasting** — สุ่มแบบไม่มีคืน (without replacement)
- **Out-of-Bag Evaluation** — ใช้ samples ที่ไม่ถูกเลือกเป็น validation set

**Random Forests** — ensemble ของ Decision Trees ที่ train บน random subsets ของ data และ features พร้อมกัน ดีกว่า Decision Tree เดี่ยวมาก

**Extra-Trees** — เหมือน Random Forest แต่ใช้ random thresholds ในการ split ทำให้เร็วขึ้นอีก

**Boosting** — train model ทีละตัว โดยแต่ละตัวแก้ไขข้อผิดพลาดของตัวก่อนหน้า
- **AdaBoost** — เพิ่ม weight ให้ instances ที่ผิด แล้ว train classifier ใหม่
- **Gradient Boosting** — train ใหม่บน residual errors ของตัวก่อนหน้า ได้ผลดีมากแต่ช้า

**Stacking** — train final model (meta-learner) บน outputs ของ base models หลายตัว

---

### Chapter 8: Dimensionality Reduction — การลดมิติ

**Curse of Dimensionality** — ยิ่งมี features มาก ยิ่งต้องการ data มากแบบ exponential เพื่อ generalize ดี

**Main Approaches**:
- **Projection** — ข้อมูลอาจอยู่ใน subspace ที่ต่ำกว่า dimension จริง
- **Manifold Learning** — ข้อมูลอาจอยู่บน manifold (surface) ที่บิดเบี้ยว

**PCA (Principal Component Analysis)** — หา principal components ที่ preserve variance ได้มากที่สุด
- ลด dimension โดย/project ลงบน principal components ที่เลือก
- **Explained Variance Ratio** — บอกว่า components ที่เลือกเก็บข้อมูลได้กี่%
- **Choosing Number of Dimensions** — เลือก d ที่ cumulative variance > threshold (เช่น 95%)
- **Incremental PCA** — แบ่ง data เป็น mini-batches สำหรับ dataset ใหญ่
- **Randomized PCA** — เร็วกว่าเมื่อ d น้อย

**Kernel PCA** — จัดการ nonlinear data โดย mapping ไป high-dimensional space แล้วทำ PCA

**LLE (Locally Linear Embedding)** — รักษา relationships ระหว่าง neighbors แต่ละจุด

**Other Techniques** — MDS, Isomap, t-SNE, LDA

---

## Part II: Neural Networks & Deep Learning

### Chapter 9: Up and Running with TensorFlow

**TensorFlow Basics** — สร้าง computation graph แล้ว run ใน session
- **Nodes** = operations, **Tensors** = data flow ผ่าน graph
- **Linear Regression ด้วย TensorFlow** — สร้าง graph, compute gradients ด้วย autodiff, optimize ด้วย optimizers

**TensorBoard** — visualize graph และ training curves

**Modularity & Sharing Variables** — สร้าง reusable functions (name scopes) และ share variables ระหว่าง parts ของ graph

---

### Chapter 10: Introduction to Artificial Neural Networks

**From Biological to Artificial Neurons** — Perceptron คำนวณ weighted sum แล้ว output 0 หรือ 1

**MLP (Multi-Layer Perceptron)** — stack หลาย layers ของ perceptrons เข้าด้วยกัน

**Backpropagation** — algorithm สำหรับ train neural network โดยคำนวณ gradients จาก output กลับไป input ผ่าน chain rule

**Training with TensorFlow** — ใช้ high-level API (DNNClassifier) หรือ plain TensorFlow (construction phase + execution phase)

**Fine-tuning Hyperparameters**:
- Number of hidden layers — มากขึ้นดีแต่ช้า
- Number of neurons per layer — ลดจากล่างขึ้นบน ( funnel architecture)
- Activation functions — ReLU เป็น default ที่ดี

---

### Chapter 11: Training Deep Neural Nets

**Vanishing/Exploding Gradients** — gradients หายไปหรือระเบิดใน network ลึก แก้ด้วย:
- **Xavier/He Initialization** — weight ที่เหมาะสมกับ activation function
- **Nonsaturating Activations** — ReLU, ELU, SELU ดีกว่า sigmoid/tanh
- **Batch Normalization** — normalize inputs ของแต่ละ layer
- **Gradient Clipping** — ตัด gradient ไม่ให้ใหญ่เกินไป

**Transfer Learning (Reusing Pretrained Layers)** — ใช้ weights จาก model ที่ train แล้วมาต่อยอด
- Freeze layers ล่าง แล้ว train layers บนใหม่
- Fine-tune ทีละนิดจากบนลงล่าง

**Faster Optimizers**:
- **Momentum** — accumulate velocity ข้าม valleys
- **Nesterov Accelerated Gradient** — .look ahead ก่อน compute gradient
- **AdaGrad** — adaptive learning rate ต่อ feature
- **RMSProp** — fix AdaGrad's learning rate decay
- **Adam** — combines Momentum + RMSProp ดีที่สุดสำหรับ大多数 cases

**Learning Rate Scheduling** — ลด learning rate ตามเวลา เช่น power scheduling, exponential scheduling

**Regularization**:
- **Early Stopping** — หยุดเมื่อ validation error ต่ำสุด
- **L1/L2 Regularization** — ลด weight magnitudes
- **Dropout** — สุ่มปิด neurons ระหว่าง training (อัตรา 50% สำหรับ hidden layers)
- **Max-Norm Regularization** — จำกัด norm ของ weight vectors
- **Data Augmentation** — สร้าง training samples ใหม่จากเดิม เช่น หมุน/flip ภาพ

---

### Chapter 12: Distributing TensorFlow Across Devices and Servers

**Multiple GPUs on Single Machine** — จัดการ GPU RAM, place operations, parallel execution

**Multiple Servers** — cluster ของ machines ที่ train ด้วยกัน
- **Master/Worker Services** — architecture สำหรับ distributed training
- **Parameter Servers** — shard variables ข้าม machines
- **Queues** — asynchronous data loading

**Parallelization Strategies**:
- **Model Parallelism** — แยก layers ไปต่าง GPUs
- **Data Parallelism** — train บน data subsets ต่างกัน แล้ว aggregate gradients
- **In-Graph vs Between-Graph Replication** — เปรียบเทียบ two approaches

---

### Chapter 13: Convolutional Neural Networks (CNN)

**Convolutional Layer** — ตรวจจับ patterns ในภาพด้วย filters/kernels
- **Filters** — small matrices ที่ slide ทั่วภาพ ตรวจ features เช่น edges, textures
- **Feature Maps** — output ของ convolution แต่ละ filter
- **Pooling Layer** — downsample feature maps เช่น MaxPooling ลด dimension

**CNN Architectures** — วิวัฒนาการของ CNN:
- **LeNet-5 (1998)** — CNN แรกที่ใช้จริง สำหรับ digit recognition
- **AlexNet (2012)** — เริ่มใช้ReLU, dropout, data augmentation ชนะ ImageNet
- **GoogLeNet/Inception (2014)** — inception modules ที่มี parallel convolutions
- **ResNet (2015)** — skip connections แก้ vanishing gradients train network ได้ลึกมาก

---

### Chapter 14: Recurrent Neural Networks (RNN)

**Recurrent Neurons** — มี loops ที่ส่ง hidden state ข้าม time steps เหมาะกับ sequential data

**Basic RNNs in TensorFlow** — static unrolling (固定 length) หรือ dynamic unrolling (variable length)

**Training RNNs** — ใช้ backpropagation through time (BPTT) สำหรับ sequence classification และ time series prediction

**LSTM Cell (Long Short-Term Memory)** — แก้ vanishing gradients ด้วย gates: forget gate, input gate, output gate จดจำ long-term dependencies ได้

**GRU Cell (Gated Recurrent Unit)** — เวอร์ชันง่ายกว่า LSTM ของ门数量 少

**Natural Language Processing** — ใช้ word embeddings (Word2Vec, GloVe) แปลง words เป็น vectors

**Encoder-Decoder Network** — สำหรับ machine translation encoder อ่าน input sequence แล้ว decoder สร้าง output sequence

---

### Chapter 15: Autoencoders

**Autoencoder** — neural network ที่เรียนรู้ compressed representation ของข้อมูล (dimensionality reduction)

**Stacked Autoencoders** — หลาย layers ของ autoencoders train ทีละชั้น (greedy layer-wise pretraining)

**Denoising Autoencoders** — เพิ่ม noise เข้าไปใน input แล้วให้ reconstruct ต้นฉบับ เรียนรู้ features ที่ robust

**Sparse Autoencoders** — บังคับให้ hidden units ส่วนใหญ่เป็น 0 เรียนรู้ features ที่ compact

**Variational Autoencoders (VAE)** — generative model ที่เรียนรู้ latent space แล้ว generate ข้อมูลใหม่ เช่น สร้างภาพ digit ใหม่ที่ไม่ซ้ำกัน

---

### Chapter 16: Reinforcement Learning

**Reinforcement Learning** — Agent เรียนรู้ policy ที่ดีที่สุดจาก interactions กับ environment

**Policy Search** — สุ่ม policy parameters แล้ว evaluate ใช้ได้แต่ช้า

**OpenAI Gym** — toolkit สำหรับทดสอบ RL algorithms กับ Atari games, robotics

**Policy Gradients** — ปรับ policy โดยตรงผ่าน gradient ascent บน expected rewards

**Markov Decision Processes (MDPs)** — formal framework ประกอบด้วย states, actions, transitions, rewards

**Temporal Difference Learning & Q-Learning** — เรียนรู้ value function ของ state-action pairs
- **Exploration Policies** — epsilon-greedy, exploration rate decay
- **Approximate Q-Learning** — ใช้ neural network แทน Q-table

**Deep Q-Learning** — เล่น Ms. Pac-Man ด้วย deep neural network ที่ approximate Q-values

---

## สรุปภาพรวมหนังสือ

หนังสือเล่มนี้ครอบคลุม ML ตั้งแต่พื้นฐานจนถึง Deep Learning อย่างครบถ้วน มีทั้ง theory คณิตศาสตร์และ code จริงด้วย Scikit-Learn + TensorFlow

**Part I** เน้น classical ML algorithms ที่ใช้ได้จริงในงานส่วนใหญ่ — Random Forests, Ensemble Methods, SVM มักแก้ปัญหาได้ดีโดยไม่ต้องใช้ Deep Learning

**Part II** เข้าสู่ Deep Learning — Neural Networks, CNN สำหรับภาพ, RNN/LSTM สำหรับ sequences, Autoencoders สำหรับ generative models, และ RL สำหรับ decision making

**คำแนะนำจากผู้แต่ง**: เริ่มจากพื้นฐานก่อน อย่ากระโดดเข้า Deep Learning เลย ส่วนใหญ่ปัญหา classical ML แก้ได้แล้ว Deep Learning เหมาะกับ image, speech, NLP ที่ data มากพอ

---