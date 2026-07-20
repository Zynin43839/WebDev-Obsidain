# AI Engineering (for True Epub)
> **Author:** Chip Huyen

---

## บทนำ — หนังสือเล่มนี้พูดถึงอะไร

หนังสือเล่มนี้ครอบคลุม 10 บท เกี่ยวกับ AI Engineering ตั้งแต่พื้นฐาน model จนถึงการสร้าง production system จริง

---

## Chapter 1 — ภาพรวม AI Engineering

AI Engineering คือการสร้าง application บน top ของ foundation model ที่มีอยู่แล้ว ต่างจาก ML Engineering แบบเดิมที่ต้อง train model เอง

AI มี 3 ปัจจัยที่ทำให้ discipline นี้เติบโตเร็วมาก:
1. **General-purpose capabilities** — ทำได้หลาย task พร้อมกัน
2. **Increased investments** — VC และ enterprise ลงทุน AI มากขึ้น
3. **Low barrier to entry** — Model-as-a-Service ทำให้ใช้ model ได้ทันทีผ่าน API

### AI Engineering Stack มี 3 ชั้น
- **Application development**: Prompt engineering, evaluation, UI
- **Model development**: Training, finetuning, dataset engineering
- **Infrastructure**: Model serving, compute, monitoring

### Use Cases หลักของ AI
- Coding (GitHub Copilot ทำ revenue $100M+ ภายใน 2 ปี)
- Image/Video production (Midjourney, Runway, Sora)
- Writing (ช่วยเขียน email, บทความ, SEO)
- Education (tutoring ส่วนตัว, quiz generation)
- Conversational bots (customer support, companion)
- Information aggregation (summarization, talk-to-your-docs)
- Data organization (extract ข้อมูลจาก unstructured data)
- Workflow automation (agents ที่ใช้ tools ทำ tasks ซับซ้อน)

### การวางแผน AI Application
ก่อนสร้าง ต้องถามว่า "ทำไมต้องสร้าง?" — มี 3 ระดับ:
1. คู่แข่งใช้ AI แล้ว คุณจะล้าสมัยถ้าไม่ใช้
2. ไม่ใช้จะพลาดโอกาส profit
3. ไม่อยากตกเทรนด์

### บทบาท AI กับ Human
- **Critical vs Complementary**: Face ID ต้องพึ่ง AI, แต่ Gmail ยังทำงานได้โดยไม่มี Smart Compose
- **Reactive vs Proactive**: Chatbot ตอบเมื่อ user ถาม, Traffic alert แจ้งเมื่อมีโอกาส
- **Dynamic vs Static**: Face ID อัปเดตเรื่อยๆ, Object detection อัปเดตเป็นครั้งคราว
- ใช้ framework **Crawl-Walk-Run** ค่อยๆ เพิ่ม automation

### Defensibility
โมเดล competitive advantage มี 3 แบบ: technology, data, distribution — data มักเป็น moat ที่สำคัญที่สุด

---

## Chapter 2 — เข้าใจ Foundation Model

### Training Data
- Common Crawl คือ dataset หลักที่หลาย model ใช้ แต่คุณภาพมีปัญหา (clickbait, misinformation)
- **Multilingual**: English ครอบงำ internet (45.88%) — model ภาษาอื่น performance ต่ำกว่ามาก
- **Domain-specific models** (AlphaFold, Med-PaLM2) ทำได้ดีกว่า general model ใน domain เฉพาะทาง

### Model Architecture — Transformer
- แก้ปัญหา seq2seq 2 จุด: ใช้ **attention mechanism** เข้าถึง input ทุกจุดได้แทนที่จะพึ่ง summary เดียว, และ parallelize input แทน sequential processing
- Inference มี 2 ขั้น: **Prefill** (process input แบบ parallel) → **Decode** (generate token ทีละตัว)
- Attention ใช้ **Key, Value, Query** vectors — query ค้นหา key ที่เกี่ยวข้อง แล้วเอา value มาใช้

### Alternative Architectures
- **RWKV**: RNN-based, parallelize ได้, ไม่มี context length limit ในทางทฤษฎี
- **Mamba/Jamba (SSM)**: State Space Models ที่ inference ใช้ computation แบบ linear (ไม่ใช่ quadratic เหมือน transformer)
- Transformer ยังครอง market อยู่ แต่ alternative เริ่มมี traction

### Model Size
- วัดด้วย 3 ตัวเลข: **parameters** (capacity), **training tokens** (ข้อมูล), **FLOPs** (compute cost)
- **Chinchilla scaling law**: tokens ที่ optimal ≈ 20× parameters
- **Mixture-of-Experts (MoE)**: เช่น Mixtral 8x7B มี 46.7B params แต่ใช้แค่ 12.9B ต่อ token
- Scaling bottlenecks: **ข้อมูล** (internet data กำลังหมด) และ **ไฟฟ้า** (data center ใช้ 1-2% ไฟโลก)

### Post-Training
1. **Supervised Finetuning (SFT)**: สอน model ให้คุยเป็น (ไม่ใช่แค่ complete text) ด้วย demonstration data
2. **Preference Finetuning**: สอน model ให้ทำตาม human preference
   - **RLHF**: สร้าง reward model แล้ว optimize ด้วย reinforcement learning (PPO)
   - **DPO**: ง่ายกว่า RLHF, Meta ใช้กับ Llama 3

### Sampling
- **Temperature**: ยิ่งสูง ยิ่ง creative; ยิ่งต่ำ ยิ่ง consistent
- **Top-k**: เลือก k token ที่มี probability สูงสุด
- **Top-p (Nucleus)**: เลือก token จน cumulative probability ถึง p
- **Structured outputs**: JSON mode, constrained sampling, finetuning สำหรับ output ที่ต้อง format ชัดเจน

### Probabilistic Nature
- **Inconsistency**: ถามเหมือนกัน 2 ครั้ง ได้คำตอบต่างกัน → แก้ด้วย caching, fix temperature/seed
- **Hallucination**: Model สร้างข้อมูลเท็จ — มี 2 สมมติฐาน:
  1. Model แยกไม่ออกว่าข้อมูลไหนเป็น input ข้อมูลไหนเป็น output ของตัวเอง (self-delusion)
  2. Labeler สอน model ด้วยความรู้ที่ model ไม่มี → model เรียนรู้ที่จะ "แต่ง" เรื่อง

---

## Chapter 3 — Evaluation Methodology

### ทำไม Evaluation ถึงยากกับ Foundation Models
1. Model ยิ่งฉลาด ยิ่งยากจะ eval — ต้อง fact-check, reason, ใช้ domain expertise
2. Open-ended outputs — ไม่มี ground truth เดียว
3. Black box — ไม่รู้ architecture/training data
4. Benchmarks อิ่มตัวเร็ว (GLUE → SuperGLUE → MMLU → MMLU-Pro)

### Language Modeling Metrics
- **Cross entropy**: วัดว่า model ทำนาย token ถูกแค่ไหน → ยิ่งต่ำ ยิ่งดี
- **Perplexity**: exponential ของ cross entropy — ยิ่งต่ำ ยิ่งมั่นใจ
- **BPC/BPB**: ปรับ cross entropy ให้เทียบกันได้ข้าม model ที่ tokenize ต่างกัน

### Human Evaluation
- ยังจำเป็นสำหรับ task ที่ซับซ้อน
- แต่ช้า แพง และ subject bias

### Exact Evaluation
- Exact match, Lexical similarity (BLEU, ROUGE), Semantic similarity (cosine similarity บน embeddings)
- เหมาะกับ task ที่มี ground truth ชัดเจน

### AI as a Judge
- ใช้ model ตัวนึง eval model อีกตัว — ถูกกว่า human มาก
- 3 วิธี eval: single response quality, comparison กับ reference, head-to-head comparison
- ข้อจำกัด: ต้อง judge ว่า model ตัวไหนเป็น strong/weak ก่อน, bias อาจเกิดได้

---

## Chapter 4 — สร้าง Evaluation Pipeline

### Model Selection
1. กรอง model ที่ hard attributes ไม่ตรง (price, latency, data privacy)
2. ดู benchmark performance สาธารณะ
3. รัน eval pipeline กับ data ของคุณเอง
4. Monitor production performance ตลอด

### Benchmarks สำคัญ
- **MMLU**: 57 วิชา, 14K ข้อสอบ multiple choice
- **HumanEval**: coding benchmark (generate function จาก docstring)
- **MT-Bench**: multi-turn conversation
- **TruthfulQA**: วัด ability ตอบตามความจริง

### Hallucination Detection
- **Claim decomposition**: แยก response เป็น statements เล็กๆ
- **Fact-checking pipeline**: แต่ละ statement → สร้าง search query → verify กับ retrieval
- ใช้ LLM-as-judge กับ human spot-check ร่วมกัน

### Safety Evaluation
- ทดสอบว่า model ปฏิเสธ harmful requests ได้ไหม
- ทดสอบ toxic output, bias, stereotype
- ใช้ **red-teaming** (จงใจ attack model) เพื่อหา vulnerabilities

---

## Chapter 5 — Prompt Engineering

### Techniques หลัก
1. **Zero-shot**: ให้ task อย่างเดียว ไม่มี example
2. **Few-shot**: ใส่ example 2-5 ตัวให้ model เข้าใจ pattern
3. **Chain-of-thought (CoT)**: สั่งให้ model คิดทีละ step ก่อนตอบ
4. **Least-to-most**: แยก task ซับซ้อนเป็น sub-tasks เล็กๆ แล้วทำทีละอัน

### Prompt Structure
- **System prompt**: กำหนดบทบาทและ behavior ของ model
- **User prompt**: คำสั่งจาก user
- **Tool outputs**: ข้อมูลจาก tools ที่ model ใช้

### Advanced Techniques
- **Intent classification**: ระบุ intent ของ query ก่อน generate response
- **Role-playing**: ให้ model ปลอมเป็น expert เพื่อ improve quality
- **Prompt optimization**: ใช้ AI ช่วย generate/optimize prompts อัตโนมัติ

### Prompt Injection
- **Passive phishing**: แทรก instruction ในข้อมูลที่ model อ่าน
- **Active injection**: แก้ system prompt ตรงๆ
- ป้องกันด้วย input validation, output filtering, sandboxing

---

## Chapter 6 — RAG และ Agents

### RAG (Retrieval-Augmented Generation)
- ดึงข้อมูลจาก knowledge base มาใส่ใน prompt ก่อน generate
- ขั้นตอน: Query → Embed → Vector search → ดึง documents → ใส่ prompt → Model generate
- ประเมินผล 2 ระดับ: **retrieval quality** (ได้ข้อมูลที่เกี่ยวข้องไหม?) และ **generation quality** (คำตอบถูกต้องไหม?)

### Embedding Models
- แปลง text/image เป็น vector ที่ capture ความหมาย
- **CLIP** ทำ multimodal embedding (text + image)
- Vector similarity search ใช้ cosine similarity หา documents ที่ใกล้เคียง

### Agents
- Model ที่ไม่ใช่แค่ generate text แต่ **คิด plan, ใช้ tools, ทำ actions**
- Architecture: **Reasoning → Plan → Execute tools → Reflect → Repeat**
- Tools: SQL query, web search, code execution, API calls
- Agent evaluation วัด: plan validity, tool call accuracy, parameter correctness

### Agentic Patterns
- **ReAct**: Reason + Act แบบ interleaved
- **Plan-and-execute**: สร้าง plan ทั้งหมดก่อน แล้ว execute ทีละ step
- **Reflection**: ตรวจสอบ output ของตัวเองก่อนส่ง

### Memory Management
- Short-term memory: conversation history ใน context window
- Long-term memory: vector store สำหรับข้อมูลสำคัญ
- ต้อง decide ว่าข้อมูลไหนเก็บ ข้อมูลไหนทิ้ง

---

## Chapter 7 — Finetuning

### เมื่อไหร่ควร Finetuning
1. ลอง prompting ก่อน — ถ้าไม่พอ ค่อย finetune
2. เพิ่ม few-shot examples — ถ้ายังไม่พอ
3. ต่อ RAG — ถ้า model ขาดข้อมูล
4. ลอง technique อื่นๆ — ถ้ายัง fail
5. **Finetuning + RAG** ใช้ร่วมกันได้

### Memory Management สำหรับ Finetuning
- Foundation model มี parameters มหาศาล → finetuning ต้องใช้ memory มาก
- **Quantization**: ลด precision จาก 32-bit → 8-bit หรือ 4-bit
- **Gradient checkpointing**: เก็บ checkpoint แทนเก็บทุก step

### Parameter-Efficient Finetuning
- **LoRA (Low-Rank Adaptation)**: แช่แข็ง model เดิม แล้ว train matrices ขนาดเล็ก A, B แทน — ลด memory อย่างมาก
- **QLoRA**: LoRA + quantization — finetune 7B model ได้ด้วย GPU ตัวเดียว

### Model Merging
- รวม weights ของ 2+ models เข้าด้วยกัน เช่น model ที่ finetune มาต่างกัน
- วิธี: summing layers, TIES merging, DARE

### Distillation
- Train model เล็กให้เลียนแบบ model ใหญ่ — ได้ model ที่เล็กกว่า เร็วกว่า แต่ performance ใกล้เคียง

---

## Chapter 8 — Dataset Engineering

### Data Sources
- **Public datasets**: Hugging Face, Kaggle, government open data
- **Web scraping**: Common Crawl, แต่ต้อง filter คุณภาพ
- **Proprietary data**: สัญญา, medical records, contract — มีค่าสูง แต่เข้าถึงยาก

### Data Quality
- คุณภาพสำคัญกว่าปริมาณ — model ที่ train บน data น้อยแต่คุณภาพสูง胜ได้ model ที่ train บน data เยอะแต่คุณภาพต่ำ
- **Deduplication**: ลบ data ซ้ำ
- **Filtering**: ลบ toxic, offensive, low-quality data
- **Toxicity detection**: ใช้ classifier ตรวจ有害 content

### Synthetic Data
- ใช้ AI model 生成 data สำหรับ finetuning
- วิธี: seed examples → weak model generate → filter → strong model validate
- เหมาะสำหรับ domain ที่หา data ยาก

### Data Annotation
- Human annotation ยังจำเป็นสำหรับ high-quality labels
- แต่แพง — ใช้ AI pre-annotate แล้ว human review ช่วยได้
- วัด inter-annotator agreement เพื่อ ensure quality

---

## Chapter 9 — Inference Optimization

### ทำไมต้อง Optimize
- Foundation model มี parameters มหาศาล → inference ช้าและแพง
- Autoregressive decoding 逐 token → latency สูง
- ผู้ใช้ expected latency ต่ำ (~100ms สำหรับ internet apps)

### Quantization
- ลด precision ของ model weights: FP32 → FP16 → INT8 → INT4
- ยิ่งต่ำ ยิ่งเร็วและประหยัด memory แต่ quality ลดลง
- **Post-training quantization**: ทำหลัง train เสร็จ
- **Quantization-aware training**: คำนึงถึง quantization ตั้งแต่ตอน train

### KV Cache
- เก็บ key-value vectors ของ tokens ที่คำนวณแล้ว ไม่ต้องคำนวณใหม่ทุก step
- ลด computation จาก O(n²) เหลือ O(n) ต่อ new token

### Speculative Decoding
- ใช้ model เล็ก draft tokens → model ใหญ่ verify แบบ parallel
- ได้ speed-up โดยไม่เสีย quality

### Parallelism
- **Data parallelism**: แบ่ง data ให้หลาย GPUs พร้อมกัน
- **Tensor parallelism**: แบ่ง model weights หลาย GPUs
- **Pipeline parallelism**: แบ่ง layers หลาย GPUs

### Batching
- รวม requests เข้าด้วยกันเพื่อใช้ GPU ได้เต็มที่
- **Continuous batching**: ไม่ต้องรอ batch เต็ม → ลด latency

### Caching
- Cache ผลลัพธ์ของ queries ที่ซ้ำกัน
- ใช้ embedding similarity จับคู่ queries ที่ใกล้เคียง

---

## Chapter 10 — AI Engineering Architecture

### Architecture Patterns
- **Enhancement layer**: RAG, tools, memory — เพิ่มข้อมูลให้ model
- **Guardrails**: Safety filters, output validation — ป้องกัน harmful outputs
- **Model router/gateway**: เลือก model ที่เหมาะสมกับ task
- **Caching**: ลด latency และ cost
- **Complex logic**: Multi-step pipelines, agents

### Guardrails Implementation
- Input guardrails: ตรวจ有害 queries ก่อนส่งเข้า model
- Output guardrails: ตรวจ有害 responses ก่อนส่งให้ user
- ใช้ classifiers, keyword filters, LLM-as-judge

### Monitoring
- **Metrics**: Latency, throughput, error rates, cost
- **Logging**: เก็บ inputs/outputs สำหรับ debugging
- **Alerting**: แจ้งเตือนเมื่อ metrics ผิดปกติ
- ต้อง connect metrics → logs → traces เพื่อ root cause analysis

### Production Checklist
1. เข้าใจ use case และ success metrics
2. เลือก model ที่合适 (size, cost, performance)
3. Implement evaluation pipeline
4. Build monitoring infrastructure
5. Plan for iteration — model และ requirements เปลี่ยนเรื่อยๆ

---
