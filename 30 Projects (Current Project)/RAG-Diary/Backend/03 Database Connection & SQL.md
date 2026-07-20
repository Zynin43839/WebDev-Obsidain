---
created: 2026-07-02
type: lesson
series: backend
number: 03
topic: database
status: published
---

# Database Connection & SQL — เชื่อม PostgreSQL ในโค้ด

> **เปรียบ:** Database = ตู้เอกสารในออฟฟิศ  
> SQL Query = คำสั่งให้เสมียนไปหาข้อมูล  
> Connection Pool = เสมียนหลายคนที่รอรับคำสั่ง

---

## 1. ทำไมต้อง PostgreSQL + pgvector?

**PostgreSQL** = ฐานข้อมูล relational ที่รองรับ:
- JSON columns (เก็บข้อมูลซับซ้อน)
- **pgvector extension** (เก็บ embeddings 1536 dimensions)
- Full-text search
- Transactions (ACID)

> **เชื่อมกับ:** [[Backend/00 Database Design & SQL]] — การออกแบบตาราง  
> [[Backend/01 RAG Flow]] — pgvector สำหรับค้นหา semantic

## 2. Database Connection (pg Pool)

```typescript
// src/services/db.ts
import { Pool } from 'pg';
import dotenv from 'dotenv';

dotenv.config();

const pool = new Pool({
    host: process.env.DB_HOST || 'localhost',
    port: parseInt(process.env.DB_PORT || '5432'),
    database: process.env.DB_NAME || 'rag_diary',
    user: process.env.DB_USER || 'postgres',
    password: process.env.DB_PASSWORD || 'password',
    max: 20,              // max connections in pool
    idleTimeoutMillis: 30000,
    connectionTimeoutMillis: 2000,
});

// Test connection
pool.query('SELECT NOW()')
    .then(() => console.log('✅ Database connected'))
    .catch(err => console.error('❌ Database connection failed:', err));

export { pool };
```

**Pool vs Client:**

| Pool | Client |
|------|--------|
| มีหลาย connection พร้อมใช้ | connection เดียว |
| เหมาะกับ web server (หลาย request) | เหมาะกับ script เดี่ยวๆ |
| auto-release กลับ pool | ต้อง manually release |

## 3. CRUD Operations

```typescript
// CREATE
export const createEntry = async (userId: number, title: string, content: string) => {
    const result = await pool.query(
        `INSERT INTO diary_entries (user_id, title, content)
         VALUES ($1, $2, $3)
         RETURNING *`,  // RETURNING = คืนค่าที่เพิ่ง insert
        [userId, title, content]
    );
    return result.rows[0];
};

// READ (all)
export const getAllEntries = async (userId: number) => {
    const result = await pool.query(
        'SELECT * FROM diary_entries WHERE user_id = $1 ORDER BY created_at DESC',
        [userId]
    );
    return result.rows;
};

// READ (one)
export const getEntryById = async (id: number) => {
    const result = await pool.query(
        'SELECT * FROM diary_entries WHERE id = $1',
        [id]
    );
    return result.rows[0] || null;  // ส่ง null ถ้าไม่เจอ
};

// UPDATE
export const updateEntry = async (id: number, title: string, content: string) => {
    const result = await pool.query(
        `UPDATE diary_entries
         SET title = $1, content = $2, updated_at = NOW()
         WHERE id = $3
         RETURNING *`,
        [title, content, id]
    );
    return result.rows[0] || null;
};

// DELETE
export const deleteEntry = async (id: number) => {
    const result = await pool.query(
        'DELETE FROM diary_entries WHERE id = $1 RETURNING id',
        [id]
    );
    return result.rows[0] || null;
};
```

> **สำคัญ:** ใช้ parameterized queries (`$1`, `$2`) — **ห้าม** ต่อ string โดยตรง (SQL injection!)

## 4. SQL Injection — ทำไมต้องระวัง?

```typescript
// ❌ อันตราย! — SQL Injection
const query = `SELECT * FROM users WHERE email = '${email}'`;
// ถ้า email = "admin' --" → query กลายเป็น:
// SELECT * FROM users WHERE email = 'admin' --'

// ✅ ปลอดภัย — Parameterized Query
const query = 'SELECT * FROM users WHERE email = $1';
// PostgreSQL แยกค่าเป็น parameter, ไม่ใช่ส่วนหนึ่งของ SQL
```

## 5. Relationships — JOIN

```typescript
// 1:1 — users → profile
export const getUserWithProfile = async (userId: number) => {
    const result = await pool.query(
        `SELECT u.id, u.email, p.display_name, p.avatar_url
         FROM users u
         LEFT JOIN profiles p ON p.user_id = u.id
         WHERE u.id = $1`,
        [userId]
    );
    return result.rows[0];
};

// 1:N — user → entries
export const getEntriesWithUser = async () => {
    const result = await pool.query(
        `SELECT e.*, u.email as author_email
         FROM diary_entries e
         JOIN users u ON u.id = e.user_id
         ORDER BY e.created_at DESC`
    );
    return result.rows;
};

// M:N — entries → tags
export const getEntryWithTags = async (entryId: number) => {
    const result = await pool.query(
        `SELECT e.*, 
            COALESCE(
                json_agg(json_build_object('id', t.id, 'name', t.name)) 
                FILTER (WHERE t.id IS NOT NULL), 
                '[]'
            ) as tags
         FROM diary_entries e
         LEFT JOIN entry_tags et ON et.entry_id = e.id
         LEFT JOIN tags t ON t.id = et.tag_id
         WHERE e.id = $1
         GROUP BY e.id`,
        [entryId]
    );
    return result.rows[0];
};
```

## 6. Transactions — ทำหลายอย่างพร้อมกัน

```typescript
// Transaction = หลาย query ที่ต้องสำเร็จหรือล้มเหลวพร้อมกัน
export const createEntryWithTags = async (userId: number, title: string, content: string, tagNames: string[]) => {
    const client = await pool.connect();  // ขอยืม connection
    
    try {
        await client.query('BEGIN');  // เริ่ม transaction
        
        // Insert entry
        const entryResult = await client.query(
            `INSERT INTO diary_entries (user_id, title, content) VALUES ($1, $2, $3) RETURNING *`,
            [userId, title, content]
        );
        const entry = entryResult.rows[0];
        
        // Insert tags และเชื่อมความสัมพันธ์
        for (const name of tagNames) {
            // INSERT ... ON CONFLICT = insert ถ้าไม่มี, หรือข้ามไปถ้ามี
            const tagResult = await client.query(
                `INSERT INTO tags (name) VALUES ($1) ON CONFLICT (name) DO UPDATE SET name = EXCLUDED.name RETURNING *`,
                [name]
            );
            
            await client.query(
                `INSERT INTO entry_tags (entry_id, tag_id) VALUES ($1, $2) ON CONFLICT DO NOTHING`,
                [entry.id, tagResult.rows[0].id]
            );
        }
        
        await client.query('COMMIT');  // ยืนยัน transaction
        return entry;
    } catch (error) {
        await client.query('ROLLBACK');  // ยกเลิกทั้งหมด
        throw error;
    } finally {
        client.release();  // คืน connection กลับ pool
    }
};
```

## 7. pgvector — Semantic Search

```typescript
// สร้าง embedding index (run once ใน database)
// CREATE INDEX ON diary_entries USING hnsw (embedding vector_l2_ops);

// ค้นหา semantic — ใช้ <<->> (cosine distance)
export const searchSimilarEntries = async (embedding: number[], userId: number, limit: number = 5) => {
    const result = await pool.query(
        `SELECT id, title, content, 
                1 - (embedding <=> $1::vector) as similarity
         FROM diary_entries
         WHERE user_id = $2 AND embedding IS NOT NULL
         ORDER BY embedding <=> $1::vector
         LIMIT $3`,
        [embedding, userId, limit]
    );
    return result.rows;
    // similarity = 0-1, ยิ่งใกล้ 1 = ยิ่งคล้าย
};
```

## 8. Query Performance Tips

```typescript
// EXPLAIN ANALYZE — ดูว่า query ทำงานยังไง (run ใน database)
// EXPLAIN ANALYZE SELECT * FROM diary_entries WHERE user_id = 1;

// INDEX — ทำให้ query เร็วขึ้น
// CREATE INDEX idx_entries_user_id ON diary_entries(user_id);
// CREATE INDEX idx_entries_created_at ON diary_entries(created_at DESC);

// LIMIT — อย่าโหลดทั้งหมด
pool.query('SELECT * FROM diary_entries LIMIT 20 OFFSET 0');
```

## 9. แบบฝึกหัด

เขียนฟังก์ชัน `searchEntriesByKeyword(keyword: string, userId: number)` ที่:
1. ใช้ `ILIKE` (case-insensitive search)
2. ค้นหาทั้ง title และ content
3. เรียงตาม relevance (title match ก่อน content match)
4. LIMIT 10 รายการ
5. ส่งคืน `{ entries: [...], total: number }`

---

## 🔗 Links

- ก่อนหน้า: [[Backend/02 Express & Middleware|Express & Middleware]]
- ต่อไป: [[Backend/04 JWT Authentication|JWT Authentication]]
- ต้องเข้าใจ: [[Backend/00 Database Design & SQL]]
- กลับไป: [[_Curriculum]]
