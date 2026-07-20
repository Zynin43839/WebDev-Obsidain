---
created: 2026-07-02
type: lesson
series: backend
number: 05
topic: rag-integration
status: published
---

# RAG Integration — เชื่อม AI Search + Chat เข้ากับ Backend

> **เปรียบ:** RAG = **บรรณารักษ์อัจฉริยะ**  
> 1. คุณถาม — บรรณารักษ์ไปค้นหนังสือที่เกี่ยวข้อง  
> 2. อ่านหนังสือที่หาเจอ  
> 3. สรุปคำตอบจากหนังสือเหล่านั้น

---

## 1. RAG Flow เต็มรูปแบบ

```
User: "เมื่อวานเราเรียนอะไร?"
    ↓
[1] สร้าง Embedding จากคำถาม (OpenAI)
    ↓
[2] ค้นหา entries ที่ semantic ใกล้เคียง (pgvector)
    ↓
[3] สร้าง Prompt = context + question
    ↓
[4] ส่งให้ LLM (OpenAI) → ได้คำตอบ
    ↓
User: "เมื่อวานเราเรียน TypeScript และ REST API ครับ"
```

> **ต้องเข้าใจ:** [[Backend/01 RAG Flow]] — ทฤษฎี RAG ทั้งหมด

## 2. ติดตั้ง Dependencies

```bash
npm install openai
```

## 3. Embedding Service

```typescript
// src/services/embedding.ts
import OpenAI from 'openai';
import dotenv from 'dotenv';

dotenv.config();

const openai = new OpenAI({
    apiKey: process.env.OPENAI_API_KEY,
});

export const createEmbedding = async (text: string): Promise<number[]> => {
    const response = await openai.embeddings.create({
        model: 'text-embedding-3-small',  // 1536 dimensions
        input: text,
    });
    
    return response.data[0].embedding;
};
```

## 4. Semantic Search Service

```typescript
// src/services/search.ts
import { pool } from './db';
import { createEmbedding } from './embedding';

export const searchEntries = async (query: string, userId: number, limit: number = 5) => {
    // Step 1: สร้าง embedding จากคำถาม
    const embedding = await createEmbedding(query);
    
    // Step 2: ค้นหา entries ที่ semantic ใกล้เคียง
    const result = await pool.query(
        `SELECT id, title, content, created_at,
                1 - (embedding <=> $1::vector) as similarity
         FROM diary_entries
         WHERE user_id = $2 AND embedding IS NOT NULL
         ORDER BY embedding <=> $1::vector
         LIMIT $3`,
        [embedding, userId, limit]
    );
    
    return result.rows;
    // similarity = 0-1, ยิ่งใกล้ 1 = ยิ่ง semantic ใกล้เคียง
};
```

## 5. LLM Service — สร้างคำตอบ

```typescript
// src/services/llm.ts
import OpenAI from 'openai';

const openai = new OpenAI({
    apiKey: process.env.OPENAI_API_KEY,
});

interface ChatMessage {
    role: 'system' | 'user' | 'assistant';
    content: string;
}

export const generateResponse = async (
    userQuery: string,
    contextEntries: Array<{ title: string; content: string; similarity: number }>,
    history: ChatMessage[] = []
): Promise<string> => {
    // สร้าง context string จาก entries ที่ค้นหาเจอ
    const context = contextEntries
        .map((e, i) => `[Entry ${i + 1} (ความคล้าย: ${(e.similarity * 100).toFixed(0)}%)]
หัวข้อ: ${e.title}
เนื้อหา: ${e.content}`)
        .join('\n\n');
    
    // System prompt — บอก LLM ว่าต้องทำยังไง
    const systemPrompt = `คุณคือผู้ช่วย AI ที่ตอบคำถามจากไดอารี่ส่วนตัว

กฎ:
1. ตอบจาก context ที่ให้เท่านั้น
2. ถ้า context ไม่มีข้อมูลที่เกี่ยวข้อง — บอกว่า "ไม่พบข้อมูลในไดอารี่"
3. ตอบเป็นภาษาไทย
4. กระชับ ได้ใจความ
5. ถ้า context มีข้อมูลที่เกี่ยวข้อง — อ้างอิงวันที่และหัวข้อด้วย

Context:
${context || 'ไม่มี context'}`;
    
    const messages: ChatMessage[] = [
        { role: 'system', content: systemPrompt },
        ...history.slice(-10),  // ส่งประวัติล่าสุด 10 ข้อความ (context window)
        { role: 'user', content: userQuery },
    ];
    
    const response = await openai.chat.completions.create({
        model: 'gpt-4o-mini',  // ถูกกว่า gpt-4o
        messages,
        temperature: 0.3,       // ต่ำ = deterministic, สูง = creative
        max_tokens: 500,
    });
    
    return response.choices[0].message.content || 'ขออภัย ไม่สามารถสร้างคำตอบได้';
};
```

## 6. Chat Route — รวมทุกอย่าง

```typescript
// src/routes/chat.ts
import { Router } from 'express';
import { authMiddleware } from '../middleware/auth';
import { searchEntries } from '../services/search';
import { generateResponse } from '../services/llm';
import { AppError } from '../middleware/error';

const router = Router();
router.use(authMiddleware);

// POST /api/chat — ส่งข้อความ → ได้คำตอบ
router.post('/', async (req, res, next) => {
    try {
        const { message } = req.body;
        const userId = req.user!.userId;
        
        if (!message) {
            throw new AppError('Message is required', 400);
        }
        
        // 1. ค้นหา entries ที่เกี่ยวข้อง
        const relevantEntries = await searchEntries(message, userId);
        
        // 2. สร้างคำตอบจาก LLM
        const answer = await generateResponse(message, relevantEntries);
        
        // 3. ส่งคำตอบ + แหล่งอ้างอิง
        res.json({
            success: true,
            data: {
                answer,
                sources: relevantEntries.map(e => ({
                    id: e.id,
                    title: e.title,
                    similarity: e.similarity,
                })),
            }
        });
    } catch (error) {
        next(error);
    }
});

// POST /api/chat/:entryId — แชทเฉพาะ entry นั้น
router.post('/:entryId', async (req, res, next) => {
    try {
        const { message } = req.body;
        const { entryId } = req.params;
        const userId = req.user!.userId;
        
        // ดึงเฉพาะ entry นั้น
        const entryResult = await pool.query(
            'SELECT * FROM diary_entries WHERE id = $1 AND user_id = $2',
            [entryId, userId]
        );
        if (entryResult.rows.length === 0) {
            throw new AppError('Entry not found', 404);
        }
        
        const entry = entryResult.rows[0];
        const answer = await generateResponse(message, [{
            title: entry.title,
            content: entry.content,
            similarity: 1.0,  // แน่ใจว่าเกี่ยวข้อง (entry ที่เลือก)
        }]);
        
        res.json({ success: true, data: { answer } });
    } catch (error) {
        next(error);
    }
});

export default router;
```

## 7. สร้าง Embedding เมื่อเขียน Entry

```typescript
// src/services/entryService.ts
import { pool } from './db';
import { createEmbedding } from './embedding';

export const createEntry = async (userId: number, title: string, content: string) => {
    // สร้าง embedding สำหรับ search
    const textToEmbed = `${title}\n${content}`;
    const embedding = await createEmbedding(textToEmbed);
    
    const result = await pool.query(
        `INSERT INTO diary_entries (user_id, title, content, embedding)
         VALUES ($1, $2, $3, $4::vector)
         RETURNING *`,
        [userId, title, content, embedding]
    );
    
    return result.rows[0];
};
```

## 8. Streaming Response (Advanced)

```typescript
// src/routes/chat-stream.ts — ส่ง response แบบ streaming
router.post('/stream', authMiddleware, async (req, res) => {
    const { message } = req.body;
    const userId = req.user!.userId;
    
    // ตั้ง headers สำหรับ SSE (Server-Sent Events)
    res.setHeader('Content-Type', 'text/event-stream');
    res.setHeader('Cache-Control', 'no-cache');
    res.setHeader('Connection', 'keep-alive');
    
    // ค้นหา entries
    const entries = await searchEntries(message, userId);
    const context = entries.map(e => e.content).join('\n');
    
    // ส่งแบบ stream — ผู้ใช้เห็นข้อความทีละคำ
    const stream = await openai.chat.completions.create({
        model: 'gpt-4o-mini',
        messages: [
            { role: 'system', content: `คุณคือผู้ช่วย... (context: ${context})` },
            { role: 'user', content: message }
        ],
        stream: true,  // เปิด streaming
    });
    
    for await (const chunk of stream) {
        const content = chunk.choices[0]?.delta?.content || '';
        if (content) {
            res.write(`data: ${JSON.stringify({ content })}\n\n`);
        }
    }
    
    res.write(`data: ${JSON.stringify({ done: true })}\n\n`);
    res.end();
});
```

## 9. Full Controller Example

```typescript
// src/controllers/chatController.ts
import { Request, Response, NextFunction } from 'express';
import { searchEntries } from '../services/search';
import { generateResponse } from '../services/llm';

interface ChatRequest {
    message: string;
    history?: Array<{ role: string; content: string }>;
}

export const chat = async (req: Request, res: Response, next: NextFunction) => {
    try {
        const { message, history } = req.body as ChatRequest;
        const userId = req.user!.userId;
        
        // Parallel — ค้นหา entries พร้อมกับเตรียม response
        const entries = await searchEntries(message, userId);
        const answer = await generateResponse(message, entries, history || []);
        
        res.json({
            success: true,
            data: {
                answer,
                sources: entries.map(e => ({
                    id: e.id,
                    title: e.title,
                    similarity: Math.round(e.similarity * 100) / 100,
                })),
                timestamp: new Date().toISOString(),
            }
        });
    } catch (error) {
        next(error);
    }
};
```

## 10. แบบฝึกหัด

เพิ่ม endpoint `POST /api/chat/ask` ที่:
1. รับ `{ question: string }`
2. ค้นหา entries ที่เกี่ยวข้อง 3 อัน
3. สร้าง prompt พร้อม title + date + content ของ entries
4. ส่งให้ LLM → ตอบกลับ
5. return `{ answer, relevantEntries: [{ id, title, date }] }`

---

## 🔗 Links

- ก่อนหน้า: [[Backend/04 JWT Authentication|JWT Authentication]]
- ต้องเข้าใจ: [[Backend/01 RAG Flow]], [[Backend/03 Database Connection & SQL]]
- กลับไป: [[_Curriculum]]
