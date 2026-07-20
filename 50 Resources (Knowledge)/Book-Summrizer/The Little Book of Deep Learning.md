# The Little Book of Deep Learning

> **Author:** François Fleuret
> **Source:** `meow-ai` | **Slug:** `meow-ai-the-little-book-of-deep-learning` | **Pages:** 185

---

## Part I: Foundations

### Chapter 1 — Machine Learning

Deep Learning (การเรียนรู้เชิงลึก) เป็นสาขาหนึ่งของ Machine Learning (การเรียนรู้ของเครื่อง) ซึ่งเป็นส่วนหนึ่งของ Statistical Machine Learning (สถิติศาสตร์สำหรับการเรียนรู้ของเครื่อง) จุดประสงค์คือให้คอมพิวเตอร์ **เรียนรู้จากข้อมูล** แล้วตัดสินใจหรือทำนายโดยไม่ต้องเขียนโปรแกรมแบบ hard-code

- **Supervised Learning (การเรียนรู้แบบมีผู้ supervision):** ใช้ข้อมูลที่มี label กำกับ — เช่น รูปแมว vs รูปสุนัข แล้วให้โมเดลเรียนรู้ความสัมพันธ์
- **Unsupervised Learning (การเรียนรู้แบบไม่มี supervision):** หา patterns ในข้อมูลที่ไม่มี label — เช่น จัดกลุ่มลูกค้าตามพฤติกรรม
- **Classification (การจำแนกประเภท):** ทำนาย label เช่น "แมว" หรือ "สุนัข" — ตัวอย่างเช่น ถ้า input = [35, 1.80, "ผู้ชาย"] โมเดลต้องทำนายว่า "ป่วย" หรือ "ไม่ป่วย"
- **Regression (การถดถอย):** ทำนายค่าต่อเนื่อง เช่น ราคาบ้าน — ตัวอย่างเช่น input = [3 ห้อง, 80 ตรม., "กรุงเทพ"] → ทำนายราคา 2.5 ล้านบาท

**Loss Function (ฟังก์ชันขาดทุน):** วัดว่าโมเดลทำนายได้ดีแค่ไหน ยิ่งค่า loss ต่ำยิ่งดี ตัวอย่างเช่น MSE (Mean Squared Error) สำหรับ regression

**Overfitting (การ overfit):** โมเดลเรียนรู้ข้อมูล training ดีเกินไป จนจำ noise ได้ด้วย → ทำนายข้อมูลใหม่ไม่ได้ วิธีแก้: ใช้ dropout, regularization, หรือเพิ่มข้อมูล

**Gradient Descent (การคำนวณ gradient):** อัลกอริทึมที่ปรับค่าพารามิเตอร์ของโมเดลทีละนิด เพื่อลด loss — คล้ายการเดินลงเขาในเทือกเขา แล้วหาจุดต่ำสุด

---

### Chapter 2 — Neural Networks

Neural Network (โครงข่ายประสาทเทียม) เลียนแบบสมองมนุษย์ ประกอบด้วย **nodes (โหนด)** หลายชั้นเชื่อมต่อกัน

- **Input Layer (ชั้น input):** รับข้อมูลดิบ — เช่น ค่าพิกเซลรูปภาพ, ค่า sensor
- **Hidden Layer (ชั้นซ่อน):** ประมวลผลข้อมูล — แต่ละ node คำนวณ weighted sum แล้วใส่ activation function
- **Output Layer (ชั้น output):** คายผลลัพธ์ — เช่น ความน่าจะเป็นว่าเป็น "แมว" หรือ "สุนัข"

** Activation Functions (ฟังก์ชันกระตุ้น):** ตัดสินใจว่า node จะ "fire" หรือไม่
- Sigmoid: ค่าอยู่ระหว่าง 0-1 — เหมาะกับ binary classification
- ReLU (Rectified Linear Unit): ค่า >= 0 เท่านั้น — เร็วกว่า sigmoid มาก
- Softmax: แปลง output เป็น probability distribution ที่รวมกันได้ 1

**Backpropagation (การ propagate ย้อนกลับ):** วิธีคำนวณ gradient ของ loss ทุกพารามิเตอร์ในเครือข่าย — เริ่มจาก output layer แล้ว propagate กลับไป input layer ทีละชั้น

---

### Chapter 3 — Optimization

**SGD (Stochastic Gradient Descent):** ปรับพารามิเตอร์โดยใช้ mini-batch (กลุ่มข้อมูลเล็กๆ) แทนทั้ง dataset — เร็วกว่า batch gradient descent

**Learning Rate (อัตราการเรียนรู้):** ควบคุมขนาดก้าวตอนปรับพารามิเตอร์ ถ้าใหญ่เกินไป → converge ช้าหรือ diverge; ถ้าเล็กเกินไป → เรียนรู้ช้ามาก

**Momentum (โมเมนตัม):** เพิ่ม "แรงกระตุ้น" ให้ optimizer ไม่ติดอยู่ที่ local minima — คล้ายลูกบอลกลิ้งลงเขา

**Adam Optimizer:** ผสมระหว่าง momentum + adaptive learning rate — ใช้กัน rộng rãiที่สุดในปัจจุบัน

---

## Part II: Architectures

### Chapter 4 — Deep Networks

**Convolutional Neural Networks - CNN (โครงข่ายประสาท convolution):** ออกแบบมาสำหรับรูปภาพ ใช้ filters (ตัวกรอง) เลื่อนไปทั่วรูปเพื่อจับ features เช่น ขอบ, จุด, รูปทรง

- **Convolution Layer:** ใช้ filter ขนาด 3x3 หรือ 5x5 เลื่อนไปทั่วรูป → output เป็น feature map
- **Pooling Layer (ชั้น pooling):** ลดขนาด feature map — เช่น max pooling เลือกค่าสูงสุดในแต่ละหน้าต่าง
- **Fully Connected Layer (ชั้นเชื่อมต่อเต็ม):** เชื่อม nodes ทั้งหมดเข้าด้วยกันก่อน output สุดท้าย

**LeNet (1998):** ต้นแบบ CNN สำหรับ handwriting recognition — ถือกำเนิดยุค Deep Learning สมัยใหม่

**AlexNet (2012):** ชนะการแข่ง ImageNet ด้วย error rate ต่ำกว่าคู่แข่งถึง 10% — จุดเริ่มต้นของ Deep Learning revolution

**ResNet (2015):** ใช้ **skip connections** เชื่อมชั้นที่อยู่ห่างกันโดยตรง — แก้ปัญหา vanishing gradient ทำให้ training เร็วขึ้นและโมเดลลึกขึ้น (100+ ชั้น)

---

### Chapter 5 — ResNets & Computer Vision

**Transfer Learning (การเรียนรู้แบบถ่ายโอน):** เอาโมเดลที่ train บน dataset ใหญ่มา fine-tune บน task ใหม่ — ประหยัดเวลาและข้อมูล

**Object Detection (การตรวจจับวัตถุ):** ไม่แค่จำแนกประเภท แต่ยังหาตำแหน่ง (bounding box) ของวัตถุในรูป — เช่น YOLO, Faster R-CNN

**Semantic Segmentation (การแบ่งส่วนภาพ):** กำหนด label ทุก pixel ในรูป — เช่น แยกถนน, อาคาร, ต้นไม้

---

### Chapter 6 — Recurrent Networks & Sequence Models

**Recurrent Neural Networks - RNN (โครงข่ายประสาทแบบวนซ้ำ):** จัดการข้อมูล sequence (ลำดับ) เช่น ข้อความ, เสียง, ข้อมูลเวลา

- มี **hidden state (สถานะซ่อน)** ที่เก็บข้อมูลจาก time step ก่อนหน้า
- ข้อเสีย: **vanishing gradient** — จำ long-term dependencies ได้ไม่ดี

**LSTM (Long Short-Term Memory):** แก้ vanishing gradient ด้วย **gates (ประตู)** 3 ตัว:
- Forget Gate: ตัดสินใจว่าจะทิ้งข้อมูลเก่าหรือไม่
- Input Gate: ตัดสินใจว่าจะเก็บข้อมูลใหม่หรือไม่
- Output Gate: ตัดสินใจว่าจะส่งต่อข้อมูลอะไร

**Transformer (2017):** สถาปัตยกรรมที่เปลี่ยนวงการ NLP — ใช้ **self-attention** แทน recurrent connections
- ประมวลผล parallel ได้ทั้ง sequence — เร็วกว่า LSTM มาก
- กลายเป็นพื้นฐานของ BERT, GPT, และ LLM ทุกตัวในปัจจุบัน

---

## Part III: Generative Models

### Chapter 7 — Large Language Models

**Tokenization (การแบ่งคำ):** แปลงข้อความเป็น tokens (รหัส) ที่โมเดลเข้าใจ — เช่น BPE (Byte Pair Encoding) แบ่งคำเป็น subword units

**Attention Mechanism (กลไก attention):** ให้โมเดล "จดจ่อ" กับส่วนที่สำคัญของ input — เช่น เวลาตอบคำถาม โมเดลจะ focus บนคำที่เกี่ยวข้อง

**Self-Attention:** แต่ละ token มองทุก token ใน sequence เพื่อคำนวณความสัมพันธ์ — ทำให้โมเดลเข้าใจ context ได้ดี

**Pre-training (การ pre-train):** train โมเดลบน corpus ขนาดใหญ่มาก (เช่น Wikipedia, books) ให้เรียนรู้ภาษา general → แล้ว fine-tune บน task เฉพาะ

**GPT (Generative Pre-trained Transformer):** โมเดล language ที่สร้างข้อความทีละ token — ใช้ decoder-only architecture

**BERT (Bidirectional Encoder Representations from Transformers):** โมเดลที่อ่าน input ทั้งสองทิศทางพร้อมกัน — เหมาะกับ classification tasks

**RLHF (Reinforcement Learning from Human Feedback):** ฝึกโมเดลให้ alignment กับความต้องการมนุษย์โดยให้ human evaluator ให้ feedback — ทำให้ output ปลอดภัยและมีประโยชน์มากขึ้น

---

## Part IV: Deployment

### Chapter 8 — Practical Guide

**Prompt Engineering (การออกแบบ prompt):** เขียนคำสั่งให้ LLM เข้าใจ task ชัดเจน — เช่น few-shot learning (ให้ตัวอย่าง), chain-of-thought (ให้คิดทีละขั้น)

**Fine-tuning (การ fine-tune):** เอา pre-trained model มา train ต่อบนข้อมูลเฉพาะ — ต้องการข้อมูลน้อยกว่า training จากศูนย์มาก

**Quantization (การ quantize):** ลด precision ของค่าในโมเดล (เช่น float32 → int8) → โมเดลเล็กลง เร็วขึ้น แต่ accuracy ลดเล็กน้อย

**Distillation (การ distill):** ฝึกโมเดลเล็ก (student) ให้เลียนแบบโมเดลใหญ่ (teacher) → ได้โมเดลที่เบาแต่ยังฉลาด

**Edge Deployment (การ deploy บน edge):** รันโมเดลบนอุปกรณ์ท้องถิ่น (เช่น มือถือ, Raspberry Pi) แทน cloud — เร็วขึ้น ไม่ต้องพึ่ง internet

---

**Deep Learning กำลังเปลี่ยนแปลงทุกอุตสาหกรรม — จาก healthcare ไปจนถึง finance, จาก autonomous vehicles ไปจนถึง creative arts การเข้าใจพื้นฐานจะช่วยให้เราใช้เครื่องมือเหล่านี้ได้อย่างมีประสิทธิภาพและรับผิดชอบ**

---
