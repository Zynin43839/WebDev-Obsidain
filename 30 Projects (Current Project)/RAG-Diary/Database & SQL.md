---
type: concept
topic: database
status: seedling
created: 2026-06-30
---

# Database & SQL

## What is this?

Database คือ **ระบบเก็บข้อมูล** ที่ทำงานเหมือนตาราง Excel ขนาดใหญ่แต่ทรงพลังกว่า — มีโครงสร้างชัดเจน (table, row, column), ค้นหาเร็ว (index), และเชื่อมข้อมูลข้ามตารางได้ (foreign key) ส่วน SQL คือ **ภาษาที่ใช้คุยกับ database** — สั่งให้ insert, select, update, delete ข้อมูล

## Why This Matters

RAG-Diary ใช้ **Supabase** (PostgreSQL) เป็น database — ทุกอย่างที่คุณบันทึก (diary entries), ค้นหา (vector search), และ chat กับ AI ล้วนต้องผ่าน SQL หรือ API ที่ทำงานบน SQL ถ้าไม่เข้าใจ database จะงงว่า:
- ข้อมูลเก็บยังไง
- ค้นหายังไงให้เร็ว
- join ข้อมูลยังไง
- แล้ว **pgvector** (vector search) ทำงานบน database ยังไง

## Database Fundamentals

### Table, Row, Column

```
TABLE: diary_entries
┌──────────┬─────────────────────┬─────────┬──────────────────────┐
│  id (PK) │      content        │   tag   │     created_at       │
├──────────┼─────────────────────┼─────────┼──────────────────────┤
│ abc-111  │ Today I learned...  │ Learning│ 2026-06-30T17:00:00Z │
│ abc-222  │ Went to the beach   │ Personal│ 2026-06-29T10:30:00Z │
│ abc-333  │ Buy groceries       │ Todo    │ 2026-06-28T08:00:00Z │
└──────────┴─────────────────────┴─────────┴──────────────────────┘
     ↑               ↑                   ↑            ↑
   Column          Column              Column       Column
   (field)         (field)             (field)      (field)
```

- **Table** = ตาราง (เช่น `diary_entries`)
- **Column** = หัวข้อ (id, content, tag, created_at) — แต่ละ column มี **data type**
- **Row** = 1 record (1 diary entry)
- **Cell** = จุดตัด row x column (1 ค่า)

### Primary Key (PK)

คอลัมน์ที่ **unique** ทุก row — ใช้ identify entry แต่ละตัว

```sql
-- ใน RAG-Diary (schema.sql)
id uuid primary key default gen_random_uuid()
```

- `uuid` = universally unique identifier (ไม่ซ้ำกันทั้งโลก)
- `primary key` = บอก DB ว่า column นี้คือ PK → unique + not null + index อัตโนมัติ
- `default gen_random_uuid()` = ถ้าไม่ระบุ id, DB สร้างให้เอง

### Foreign Key (FK)

เชื่อมข้อมูลข้ามตาราง — เช่น RAG-Diary ถ้ามี users table:

```sql
-- สมมติว่า RAG-Diary มี users
CREATE TABLE diary_entries (
  id       uuid primary key default gen_random_uuid(),
  user_id  uuid references users(id),  -- ← foreign key
  content  text not null
);
```

FK ทำให้:
- แต่ละ entry รู้ว่าเป็นของ user ไหน
- DB บังคับว่า `user_id` ต้องมีใน users table จริงๆ (referential integrity)
- สามารถ JOIN เพื่อดึงข้อมูล user + entry พร้อมกัน

### Index

โครงสร้างที่ทำให้ค้นหาเร็ว — **เหมือนสารบัญหนังสือ**

```sql
-- RAG-Diary มี index สำหรับ vector search
create index on diary_entries using hnsw (embedding vector_cosine_ops);
```

- **ไม่มี index** → DB scan ทุก row (full table scan) → ช้าเมื่อข้อมูลเยอะ
- **มี index** → DB กระโดดไปที่ข้อมูลตรงๆ → เร็ว
- RAG-Diary ใช้ **HNSW index** สำหรับค้นหา vector (1M rows ใน ms)

> Analog: ถ้าห้องสมุดไม่มีระบบจัดเรียง (index) → ต้องเดินดูหนังสือทุกเล่ม  
> ถ้ามี index (we simplified) → เปิดสารบัญแล้วเดินไปชั้นที่ถูกต้อง

## SQL — Structured Query Language

SQL = **ภาษาประกาศ (declarative)** — แค่บอก **"เอาอะไร"** ไม่ต้องบอก **"ทำยังไง"**

### SELECT — อ่านข้อมูล

```sql
-- ขอทุก column (*) ทุก row
SELECT * FROM diary_entries;

-- ขอแค่ content + tag
SELECT content, tag FROM diary_entries;

-- ขอเฉพาะ tag = 'Learning'
SELECT * FROM diary_entries WHERE tag = 'Learning';

-- ขอ 10 อันล่าสุด
SELECT * FROM diary_entries ORDER BY created_at DESC LIMIT 10;
```

### INSERT — เพิ่มข้อมูล

```sql
INSERT INTO diary_entries (content, tag)
VALUES ('Today I learned SQL!', 'Learning');
```

### UPDATE — แก้ไขข้อมูล

```sql
UPDATE diary_entries
SET tag = 'Updated'
WHERE id = 'abc-111';
```

⚠️ **WHERE สำคัญมาก!** — ถ้าลืม WHERE → update ทุก row
```sql
UPDATE diary_entries SET tag = 'Oops';  -- ← ทุก entry กลายเป็น 'Oops'
```

### DELETE — ลบข้อมูล

```sql
DELETE FROM diary_entries WHERE id = 'abc-111';
```

⚠️ **WHERE สำคัญอีกแล้ว** — DELETE โดยไม่มี WHERE = ลบทั้งตาราง!
```sql
DELETE FROM diary_entries;  -- ← bye bye ข้อมูลทั้งหมด
```

## RAG-Diary: Database จริง (from schema.sql)

```sql
-- ตารางหลักของ RAG-Diary
create table if not exists diary_entries (
  id         uuid primary key default gen_random_uuid(),
  content    text not null,
  tag        text default 'General',
  created_at timestamp with time zone default now(),
  embedding  vector(1536)            -- ← vector ของ OpenAI (1536 dimensions)
);

-- HNSW index สำหรับค้นหา vector เร็วขึ้น 100x
create index on diary_entries
  using hnsw (embedding vector_cosine_ops);

-- RPC function สำหรับ semantic search
create or replace function match_diary_entries(
  query_embedding vector(1536),
  match_threshold float,
  match_count int
) returns table (
  id         uuid,
  content    text,
  tag        text,
  created_at timestamp with time zone,
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

### ความหมายของแต่ละบรรทัด:

| SQL | ความหมาย |
|-----|---------|
| `uuid` | ชนิดข้อมูล — unique ID 128-bit |
| `text` | ข้อความยาว |
| `timestamp with time zone` | วันที่ + เวลา + timezone |
| `vector(1536)` | array ของ float 1536 ตัว (embedding) |
| `default now()` | ถ้าไม่ระบุ ใช้เวลาปัจจุบัน |
| `hnsw` | ชนิด index สำหรับ vector search |

### ตัวดำเนินการ `<=>` (cosine distance)

```
embedding <=> query_embedding
               ↓
        cosine distance (0-2)
        ใกล้ 0 = เหมือนกันมาก
        ใกล้ 2 = ไม่เหมือนกัน
```

```sql
1 - (embedding <=> query_embedding) as similarity
               ↓
        cosine similarity (0-1)
        1 = เหมือนกันที่สุด
        0 = ไม่เหมือนเลย
```

## SQL 4 คำสั่งพื้นฐาน → HTTP Methods

| SQL | HTTP | REST Meaning |
|-----|------|-------------|
| SELECT | GET | อ่านข้อมูล |
| INSERT | POST | สร้างข้อมูล |
| UPDATE | PUT/PATCH | แก้ไขข้อมูล |
| DELETE | DELETE | ลบข้อมูล |

โยงมาเป็น REST API ของ RAG-Diary:
```
GET    /api/diary       → SELECT * FROM diary_entries
POST   /api/diary       → INSERT INTO diary_entries (content) VALUES (...)
PUT    /api/diary/:id   → UPDATE diary_entries SET ... WHERE id = :id
DELETE /api/diary/:id   → DELETE FROM diary_entries WHERE id = :id
GET    /api/diary/search → SELECT ... match_diary_entries(...)
```

## Question to Think About

- `SELECT * FROM diary_entries` — ทำไมถึงไม่ควรใช้ `*` ใน production? (คำใบ้: performance, schema change)
- ถ้าไม่มี index บน `created_at` แล้วสั่ง `ORDER BY created_at DESC` — เกิดอะไรขึ้น?
- vector(1536) ใช้พื้นที่เท่าไหร่ต่อ 1 row? (1536 floats × 4 bytes = ?)

## Links

- Related concept: [[REST API JSON]], [[RAG Flow]], [[pgvector Indexing]]
- Code ref: [[server/src/lib/schema.sql]], [[server/src/lib/db.ts]]
- Source: Alex Xu — System Design Interview (NotebookLM)
