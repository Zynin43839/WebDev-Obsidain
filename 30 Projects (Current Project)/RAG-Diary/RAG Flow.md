---
type: concept
topic: rag
status: seedling
created: 2026-06-30
---

# RAG Flow — Retrieval-Augmented Generation

## What is this?

RAG (Retrieval-Augmented Generation) = **เทคนิคที่ทำให้ AI ตอบคำถามโดยใช้ความรู้จากข้อมูลจริงของคุณ** ไม่ใช่สุ่มเดาจาก training data เท่านั้น

```
User: "สัปดาห์ที่แล้วฉันกินอะไรเป็นมื้อเย็นบ้าง?"

AI Without RAG:     "I'm sorry, I don't have access to your personal data..."
                    ↑ รู้แค่สิ่งที่เรียนมา ไม่รู้ประวัติคุณ

AI With RAG:        "สัปดาห์ที่แล้วคุณกิน..."
                    [1] 🔍 ค้นหา diary entries ของคุณ
                    [2] 📖 อ่านเจอ "Monday: Pad Thai", "Wednesday: Sushi"
                    [3] 💬 ตอบตามข้อมูลที่ค้นเจอ
                    ↑ รู้เพราะไปค้นจาก database ของคุณ!
```

## Why This Matters

RAG-Diary = **RAG app โดยตรง** — flow ทั้ง app หมุนรอบ RAG:

```
Diary Entry → create embedding → save + embedding → search → LLM → answer
```

ถ้าไม่เข้าใจ RAG จะไม่เห็นภาพว่า:
- `POST /api/diary` ทำไมต้อง generateEmbedding?
- `POST /api/diary/search` ค้นหา semantic search ยังไง?
- `POST /api/chat` ส่ง `entries` เข้า LLM ไปทำไม?

---

## Part 1: RAG Architecture (Big Picture)

> **NotebookLM เรียบเรียง:** RAG enhances an AI model's text generation by **retrieving relevant information from external memory sources**. In a diary app, your past diary entries act as this external memory. Rather than relying solely on the model's pre-trained knowledge, a RAG system retrieves your specific journal entries to construct a personalized context. [Source: NotebookLM]

```
┌─────────────────────────────────────────────────────────────────┐
│                         RAG FLOW                                │
│                                                                 │
│  ┌─────────┐     ┌──────────┐     ┌──────────────┐            │
│  │  User    │     │ Embedding │     │  Vector DB   │            │
│  │ Query    │────→│  Model    │────→│  (pgvector)  │            │
│  │ "Today?" │     │ (OpenAI)  │     │  search top-5│            │
│  └─────────┘     └──────────┘     └──────┬───────┘            │
│                                          │                     │
│                                          ▼                     │
│                               ┌──────────────────┐            │
│                               │   Retrieved       │            │
│                               │   Entries (5)     │            │
│                               └────────┬─────────┘            │
│                                        │                       │
│  ┌─────────┐     ┌──────────┐          │                       │
│  │  LLM     │◄────│  Prompt  │◄─────────┘                       │
│  │ (Gemini) │     │ Query +  │                                  │
│  │          │     │ Context  │                                  │
│  └────┬────┘     └──────────┘                                  │
│       │                                                        │
│       ▼                                                        │
│  ┌─────────┐                                                    │
│  │  Answer  │                                                    │
│  │  to User │                                                    │
│  └─────────┘                                                    │
└─────────────────────────────────────────────────────────────────┘
```

### 3 ขั้นตอนหลักของ RAG:

| Step | เรียกว่า | ทำอะไร | ใน RAG-Diary |
|------|---------|--------|-------------|
| **1. Indexing** | Ingest | แปลงข้อมูลเป็น vector + เก็บ | เมื่อเขียน diary → embedding + save |
| **2. Retrieval** | Search | ค้นหาข้อมูลที่เกี่ยวข้อง | semantic search ด้วย cosine similarity |
| **3. Generation** | Chat | ส่งข้อมูลที่เจอ + คำถาม → LLM ตอบ | POST /api/chat |

---

## Part 2: Step 1 — Indexing (Ingestion)

> **NotebookLM:** An embedding is a **vector (a list of numbers) that captures the underlying meaning and essence of the original data**. For example, the sentence "the cat sits on a mat" might be transformed into a vector like `[0.11, 0.02, 0.54]`. [Source: NotebookLM]

### 2.1 What is an Embedding?

Embedding = แปลง **ความหมายของข้อความ** → **ตัวเลข (vector)**

```
"I love programming"
         ↓  (OpenAI Embedding API)
[0.023, -0.045, 0.112, 0.087, ..., 0.003]  ← 1536 ตัวเลข
```

**สมบัติของ embedding:**
- ข้อความที่มี **ความหมายคล้ายกัน** → vector **nearby** (ใกล้กัน)
- ข้อความที่มี **ความหมายตรงข้าม** → vector **far apart** (ไกลกัน)
- 1536 dimensions (ถ้าใช้ OpenAI `text-embedding-3-small`)

```
"I love coding"    → [0.021, -0.044, 0.115, ...]  ← ใกล้ "I love programming"
"Paris is capital" → [0.312, 0.045, -0.201, ...]  ← ไกล
"I hate coding"    → [-0.018, 0.089, -0.109, ...] ← ตรงข้าม!
```

### 2.2 Indexing Flow in RAG-Diary

```
User writes diary → "Today I learned about REST APIs"

POST /api/diary { content: "Today I learned about REST APIs" }

    │
    ▼
1. generateEmbedding(content)
   → "Today I learned about REST APIs"
   → call OpenAI Embedding API
   → [0.023, -0.045, 0.112, ..., 0.003]  ← vector(1536)
    │
    ▼
2. INSERT into Supabase
   INSERT INTO diary_entries (content, embedding)
   VALUES ('Today I learned about REST APIs', '[0.023, -0.045, ...]');
    │
    ▼
3. pgvector HNSW index จัดเรียง vector ใน index
   → เร็วขึ้น 100x เวลาค้นหา!
```

```typescript
// RAG-Diary: create entry with embedding
app.post("/api/diary", async ({ body }) => {
  // Step 1: create embedding from text
  const embedding = await generateEmbedding(body.content);
  // embedding = Float32Array(1536)

  // Step 2: save both text + embedding to Supabase
  const { data } = await supabase
    .from("diary_entries")
    .insert({
      content: body.content,
      tag: body.tag,
      embedding: embedding  // ← pgvector column!
    })
    .select()
    .single();

  return { success: true, data };
});
```

### 2.3 Why Store Embeddings?

**Alternatives ที่ไม่ใช่ RAG:**
- **Full-text search** (SQL `LIKE '%keyword%'`) — หาแค่คำตรง ไม่เจอความหมาย
- **Ask LLM directly** — LLM ไม่รู้ diary ของคุณ

**RAG = ผสมทั้งสอง:** ค้นหา semantic + เอาเข้าบริบท LLM

---

## Part 3: Step 2 — Retrieval (Search)

> **NotebookLM:** Vector search is the process of querying a database to find vectors closest to a given query's embedding. It's typically framed as a nearest-neighbor search where the goal is to retrieve the *k* most relevant vectors. [Source: NotebookLM]

### 3.1 Semantic Search Flow

```
User asks: "What did I learn about computers?"
    │
    ▼
1. Embed query → same model
   "What did I learn about computers?"
   → OpenAI Embedding API
   → query_vector = [0.045, -0.012, ...]
    │
    ▼
2. Find nearest vectors in DB
   SELECT * FROM diary_entries
   ORDER BY embedding <=> query_vector  -- cosine distance
   LIMIT 5;
    │
    ▼
3. Get top-5 most relevant entries
   "Today I learned about REST APIs"  ← similarity 0.92
   "DNS converts domain to IP"        ← similarity 0.85
   "TCP three-way handshake"          ← similarity 0.78
   ...
```

### 3.2 Cosine Similarity

> **NotebookLM:** Cosine similarity measures the **angle between two vectors** (not distance). Two embeddings with the exact same meaning have a similarity score of **1**, while entirely opposite embeddings score **-1**. [Source: NotebookLM]

```
                    ↑
                    │  vector A
                    │   ╱
                    │  ╱  θ  ← angle 越小 → semantic 越近
                    │ ╱
                    ──────────→
                   ╱│
                  ╱ │ vector B
                 ╱  │

cosine_similarity(A, B) = cos(θ)

cos(0°)   = 1.0   → ความหมายเหมือนกัน
cos(30°)  = 0.87  → คล้ายกันมาก
cos(45°)  = 0.71  → คล้ายกัน
cos(90°)  = 0.0   → ไม่เกี่ยวข้อง
cos(180°) = -1.0  → ตรงข้าม
```

**In RAG-Diary's SQL:**
```sql
-- cosine distance (0-2)
-- ใกล้ 0 → similar, ใกล้ 2 → dissimilar
embedding <=> query_embedding

-- convert to similarity (0-1)
1 - (embedding <=> query_embedding) AS similarity
```

### 3.3 The match_diary_entries Function

```sql
-- RAG-Diary's search function
create or replace function match_diary_entries(
  query_embedding vector(1536),
  match_threshold float,   -- minimum similarity (0-1)
  match_count int          -- max results
) returns table (
  id uuid,
  content text,
  tag text,
  created_at timestamptz,
  similarity float
) language sql stable as $$
  select
    id, content, tag, created_at,
    1 - (embedding <=> query_embedding) as similarity
  from diary_entries
  where embedding <=> query_embedding < 1 - match_threshold
  order by embedding <=> query_embedding
  limit match_count;
$$;
```

### 3.4 HNSW Index — ทำไมถึงเร็ว

> **NotebookLM:** pgvector supports both exact and approximate nearest-neighbor search. HNSW (Hierarchical Navigable Small World) is an approximate index that is **100x faster** than scanning every row. [Source: NotebookLM]

```
Without index (brute force):
  scan 10,000 rows → compute cosine distance each → O(n)

With HNSW index:
  navigate multi-layer graph → O(log n)
  "เหมือนหาเพื่อนใน Facebook → เดินตาม connection ที่ใกล้ที่สุด"
```

---

## Part 4: Step 3 — Generation (Chat + RAG)

### 4.1 Prompt Construction

```
System Prompt:
  "You are a helpful assistant that answers based on the user's diary entries.
   Use only the context provided. If the answer isn't in the context, say so."

Context (จาก Vector Search):
  - "Today I learned about REST APIs — GET, POST, PUT, DELETE"
  - "DNS converts domain names to IP addresses"
  - "TCP uses three-way handshake for connection"

User Question:
  "What did I learn about computers?"

AI Response:
  "คุณเรียนเกี่ยวกับ 3 หัวข้อหลัก:
   1. REST APIs — GET, POST, PUT, DELETE
   2. DNS — แปลง domain name เป็น IP address
   3. TCP — three-way handshake สำหรับ establish connection"
```

### 4.2 RAG-Diary Chat Flow

```typescript
// server/src/index.ts — RAG chat endpoint
app.post("/api/chat", async ({ body }) => {
  // [1] Embed user question
  const queryEmbedding = await generateEmbedding(body.message);

  // [2] Semantic search in diary entries
  const { data: entries } = await supabase
    .rpc("match_diary_entries", {
      query_embedding: queryEmbedding,
      match_threshold: 0.7,
      match_count: 5
    });
  // entries = [{ content: "...", similarity: 0.92 }, ...]

  // [3] Build prompt with context
  const context = entries.map(e => e.content).join("\n");
  const prompt = `
    Context from diary:
    ${context}

    User: ${body.message}
    Answer in Thai:`;

  // [4] Send to LLM
  const response = await generateLLMResponse(prompt);

  return {
    success: true,
    response,
    sources: entries.map(e => ({ id: e.id, content: e.content }))
  };
});
```

### 4.3 Why Not Just Search?

**Search alone** → แค่ list entries ที่ตรง — user อ่านเอง

**Search + LLM (RAG)** → **อ่าน + สรุป + ตอบ** — เหมือนมีผู้ช่วยอ่าน diary ให้

```
Search only:
  "What did I learn this month?"
  → [15 entries about various topics]
  → User ต้องอ่าน 15 อันเอง 😩

RAG:
  "What did I learn this month?"
  → [ค้นหา 15 entries → ส่งเข้าบริบท LLM]
  → "เดือนนี้คุณเรียนเกี่ยวกับ REST APIs, DNS, TCP, Database Design..."
  → สรุปให้เลย ✅
```

### 4.4 Chunking Strategy

สำหรับ diary entries — แต่ละ entry เป็น chunk อยู่แล้ว (ข้อความสั้น ~1-2 ประโยค)

แต่ถ้าเป็น document ยาว:
```
Document (1000 words)
  ├── Chunk 1 (200 words) → embedding 1
  ├── Chunk 2 (200 words) → embedding 2
  └── Chunk 3 (200 words) → embedding 3

Search → หาเฉพาะ chunks ที่ตรง → LLM ได้ context แคบ = ตอบตรง
```

---

## Part 5: Full End-to-End Flow

### 5.1 Writing a Diary Entry

```
[User types diary]             [Browser]              [Server]              [Supabase]
      │                            │                      │                     │
      │  "Today I learned REST"    │                      │                     │
      │ ─────────────────────────→ │                      │                     │
      │                            │  POST /api/diary     │                     │
      │                            │  {content, tag}      │                     │
      │                            │ ───────────────────→ │                     │
      │                            │                      │  generateEmbedding   │
      │                            │                      │  → OpenAI            │
      │                            │                      │  ← [0.023, -0.045..] │
      │                            │                      │                     │
      │                            │                      │  INSERT + embedding  │
      │                            │                      │ ───────────────────→ │
      │                            │                      │  ← {id, content..}  │
      │                            │  ← 201 Created       │                     │
      │  ← Show new entry          │                      │                     │
```

### 5.2 Chatting with AI

```
[User asks question]           [Browser]              [Server]               [Supabase]
      │                            │                      │                       │
      │  "What did I learn?"       │                      │                       │
      │ ─────────────────────────→ │                      │                       │
      │                            │  POST /api/chat      │                       │
      │                            │  {message}           │                       │
      │                            │ ───────────────────→ │                       │
      │                            │                      │  generateEmbedding    │
      │                            │                      │  → OpenAI            │
      │                            │                      │  ← query_vector      │
      │                            │                      │                       │
      │                            │                      │  match_diary_entries  │
      │                            │                      │  (query_vector, 5)   │
      │                            │                      │ ───────────────────→ │
      │                            │                      │  ← [top-5 entries]   │
      │                            │                      │                       │
      │                            │                      │  Build prompt        │
      │                            │                      │  (context + query)   │
      │                            │                      │                       │
      │                            │                      │  generateLLMResponse │
      │                            │                      │  → Gemini/OpenAI     │
      │                            │                      │  ← "คุณเรียน..."     │
      │                            │                      │                       │
      │                            │  ← {response: "..."} │                       │
      │  ← Show AI response        │                      │                       │
```

---

## Part 6: RAG Variations

### 6.1 Naive RAG (RAG-Diary 🔵)

```
Query → Embed → Search → LLM → Answer
```

Simple, ใช้งานได้ดี — เป็น pattern ของ RAG-Diary

### 6.2 Advanced RAG

```
Query → Rewrite → HyDE → Embed → Hybrid Search → Re-rank → LLM → Answer
```

| Technique | ทำอะไร |
|-----------|--------|
| **Query Rewrite** | LLM แก้คำถามให้ดีขึ้นก่อน search |
| **HyDE** | สร้าง hypothetical answer → ใช้ embedding ของ answer search |
| **Hybrid Search** | semantic + keyword search รวมกัน |
| **Re-ranking** | ค้นมาก่อน → ค่อย re-rank ด้วย model ละเอียด |
| **Rerank** | ปรับลำดับ relevance |

### 6.3 RAG with Memory

```
Session 1: "What did I learn?"
  → Search diary → "REST APIs"
  → AI: "คุณเรียน REST APIs"

Session 2: "Tell me more about it"
  → Search diary → "REST APIs" (เจออันเดิม)
  → + Chat History: "session 1 เค้าถามถึง REST"
  → AI: "GET = อ่าน, POST = สร้าง..."
```

**Chat history** สำคัญ — ทำให้ RAG ตอบต่อเนื่อง ไม่ใช่เริ่มใหม่ทุกครั้ง

---

## Part 7: Performance Considerations

### 7.1 Bottlenecks in RAG

| Step | Latency | Optimization |
|------|---------|-------------|
| Embedding API | 200-500ms | Cache embedding (ถ้าข้อความซ้ำ) |
| Vector Search | 5-50ms | HNSW index (ลดจาก 500ms) |
| LLM Response | 1-5s | Streaming (แสดงผลทีละ token) |
| Network | 10-100ms | Edge functions (ใกล้ user) |

### 7.2 Streaming — แสดงผลแบบ Real-time

แทนที่จะรอ LLM 5 วิแล้วค่อยแสดง — ใช้ **stream** แสดงผลทีละ token:

```
Without Streaming:
  [User asks] → รอ 5 วิ → [answer 200 words] → แสดงทั้งหมด

With Streaming:
  [User asks] → 1 วิแรก → "คุณ"
  → 1.5 วิ → "คุณเรียน"
  → 2 วิ → "คุณเรียนเกี่ยวกับ..."
  → 3 วิ → "คุณเรียนเกี่ยวกับ REST APIs..."
  → User เห็นผลค่อยๆ ขึ้น เหมือนพิมพ์!
```

### 7.3 Caching

```typescript
// Cache embedding ของ query ที่ซ้ำกัน
const cache = new Map<string, number[]>();

async function getEmbedding(text: string) {
  if (cache.has(text)) return cache.get(text);
  const embedding = await generateEmbedding(text);
  cache.set(text, embedding);
  return embedding;
}
```

---

## Question to Think About

1. **Embedding:** ข้อความ 2 ประโยคที่มีความหมายเหมือนกันแต่ใช้คำต่างกัน (เช่น "I love coding" vs "Programming is my passion") — embedding จะใกล้กันไหม?
2. **Chunking:** ถ้า chunk entry เล็กเกินไป (1-2 คำ) หรือใหญ่เกินไป (1000 คำ) — จะเกิดปัญหาอะไร?
3. **Threshold:** ถ้า `match_threshold` = 0.9 (สูงมาก) — จะเกิดอะไรขึ้นเวลาค้นหา? ถ้า = 0.1 (ต่ำมาก) — จะเกิดอะไรขึ้น?
4. **HNSW:** ทำไมต้องเป็น approximate search — แทนที่จะ exact search? (คำใบ้: performance vs accuracy)
5. **RAG-Diary:** ถ้า search แล้วไม่เจอ entries ที่เกี่ยวข้อง — LLM ควรตอบยังไง?

## Links

- Related concept: [[Database & SQL]], [[Database Design & SQL Deep Dive]], [[Async Await & Event Loop]]
- Code ref: [[server/src/index.ts]], [[server/src/lib/schema.sql]], [[server/src/lib/generate-embedding.ts]]
- Source: Alex Xu — System Design Interview (NotebookLM), Traversy Media
