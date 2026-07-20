---
created: 2026-07-02
type: lesson
series: backend
number: 08
topic: bun-elysia
status: seedling
---

# Bun & Elysia — ทางเลือกใหม่แทน Node.js + Express

> **เปรียบ:**  
> Node + Express = **รถยนต์เครื่องยนต์เก่า** — ใช้งานได้ดี แต่ต้อง tune เยอะ  
> Bun + Elysia = **รถ EV สมัยใหม่** — ออกแบบมาให้เร็วและใช้งานง่ายกว่าตั้งแต่แรก

---

## 1. Bun คืออะไร?

**Bun** = JavaScript runtime + package manager + bundler + test runner รวมอยู่ในตัวเดียว

| คุณสมบัติ | Node.js | Bun |
|----------|---------|-----|
| Engine | V8 (C++) | JavaScriptCore (WebKit) |
| Package manager | npm/yarn/pnpm | bun (built-in, เร็วกว่า npm 10-30x) |
| Test runner | jest/vitest | bun test (built-in) |
| TypeScript | ต้อง transpile | รันตรงๆ (built-in transpiler) |
| Bundle size | 30MB+ | ~8MB |
| Node API | ✅ | ~~95% compatible |

### Bun ทำอะไรได้บ้าง?

```bash
# รัน TypeScript โดยไม่ต้อง compile
bun run server/src/index.ts

# Package manager (เร็วกว่า npm)
bun add @elysiajs/eden

# Test runner
bun test

# Dev mode with hot reload
bun --watch server/src/index.ts
```

---

## 2. Elysia คืออะไร?

**Elysia** = web framework สำหรับ Bun — คล้าย Express แต่มี type-safety ในตัว

### เปรียบเทียบ Express vs Elysia

```typescript
// Express (TypeScript)
import express from 'express';
const app = express();
app.use(express.json());

app.post('/api/diary', async (req, res) => {
  const { title, content } = req.body;
  // req.body: any ❌
  // ไม่มี type safety
  res.json({ success: true });
});

// Elysia (TypeScript)
import { Elysia, t } from 'elysia';

const app = new Elysia()
  .post('/api/diary', async ({ body }) => {
    // body: { title: string; content: string } ✅
    return { success: true };
  }, {
    body: t.Object({
      title: t.String(),
      content: t.String(),
    })
  });
```

### Key Features

```
1. Type Validation: ใช้ Elysia validation (schema-based)
2. Type Inference: Eden Treaty รู้ type ตอน client เรียก
3. Lifecycle: onStart, onRequest, onError, onStop
4. Plugin System: e.g. @elysiajs/cors, @elysiajs/jwt
5. Performance: ~fastest Node.js compatible framework
```

---

## 3. Validation with Elysia (t.)

```typescript
import { Elysia, t } from 'elysia';

// Define schemas
const DiaryEntrySchema = t.Object({
  title: t.String({ minLength: 1 }),
  content: t.String({ minLength: 1 }),
  tags: t.Optional(t.Array(t.String())),
});

const ParamsSchema = t.Object({
  id: t.Numeric(),
});

// Use in routes
app
  .post('/api/diary', ({ body }: { body: EntryInput }) => {
    // body validated & typed automatically
    return createEntry(body);
  }, {
    body: DiaryEntrySchema
  })
  .get('/api/diary/:id', ({ params }: { params: { id: number } }) => {
    return getEntry(params.id);
  }, {
    params: ParamsSchema
  })
  .delete('/api/diary/:id', ({ params }) => {
    return deleteEntry(params.id);
  }, {
    params: ParamsSchema
  });
```

### Schema Types ที่มี

| Schema | ใช้กับ |
|--------|-------|
| `t.String()`, `t.Numeric()`, `t.Boolean()` | Primitives |
| `t.Object({...})` | Request body |
| `t.Array(t.String())` | Array |
| `t.Optional(t.String())` | Optional field |
| `t.Union([t.String(), t.Null()])` | Union types |
| `t.File()` | File upload |
| `t.Cookie({...})` | Cookie validation |

---

## 4. Eden Treaty — Type-Safe Client

**Eden Treaty** = สร้าง client จาก Elysia server type — ถ้า server change type → client error ทันทีที่ compile

```typescript
// server/src/index.ts
import { Elysia, t } from 'elysia';

const app = new Elysia()
  .post('/api/diary', ({ body }) => {
    return createEntry(body);
  }, {
    body: t.Object({
      title: t.String(),
      content: t.String(),
    })
  });

export type App = typeof app;
export { app };
```

```typescript
// frontend/src/lib/client.ts
import { edenTreaty } from '@elysiajs/eden';
import type { App } from '../../server/src/index';

const client = edenTreaty<App>('http://localhost:3000');

// ✅ Type-safe: autocomplete path, body, response
const { data } = await client.api.diary.post({
  title: 'Hello',
  content: 'World',
});
// data: { id: number; title: string; content: string; created_at: string }
```

### Eden Treaty API

```typescript
// GET
const { data } = await client.api.diary.get({ query: { page: 1 } });

// POST with body
const { data, error } = await client.api.diary.post({
  title: 'Entry',
  content: '...',
});

// Dynamic params
const { data } = await client.api.diary[':id'].get({ params: { id: 1 } });

// Error handling
if (error) {
  console.error(error.status, error.value);
}
```

---

## 5. Express vs Elysia: Side-by-Side

### 5.1 Setup

```typescript
// Express
const express = require('express');
const app = express();
app.use(express.json());
app.use(cors());

// Elysia
import { Elysia } from 'elysia';
import { cors } from '@elysiajs/cors';

const app = new Elysia()
  .use(cors());  // plugin-based
```

### 5.2 Middleware Chain

```typescript
// Express (callback-based)
app.use('/api', authMiddleware, (req, res, next) => {
  // ...
});

// Elysia (decorator + plugin)
const auth = new Elysia()
  .derive(({ headers }) => {
    const user = verifyToken(headers.authorization);
    return { user };
  });

app.use(auth)
  .get('/api/diary', ({ user }) => {
    // user พร้อมใช้
  })
  .get('/api/profile', ({ user }) => {
    // user พร้อมใช้ทุกเส้น
  });
```

### 5.3 Error Handling

```typescript
// Express
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).send('Something broke!');
});

// Elysia
app.onError(({ code, error }) => {
  if (code === 'VALIDATION') {
    return new Response('Invalid input', { status: 400 });
  }
  return new Response('Internal Server Error', { status: 500 });
});
```

### 5.4 File Structure Comparison

```
Express:                            Elysia:
├── src/                            ├── src/
│   ├── index.ts (app setup)        │   ├── index.ts (app setup)
│   ├── routes/                     │   ├── modules/     ← grouped by domain
│   │   ├── diary.ts                │   │   ├── diary/
│   │   ├── auth.ts                 │   │   ├── auth/
│   │   └── ai.ts                   │   │   └── ai/
│   ├── middleware/                  │   └── lib/
│   │   ├── auth.ts                 │       ├── db.ts
│   │   └── error.ts                │       └── middleware.ts
│   ├── services/
│   └── lib/
├── package.json                    ├── package.json
└── tsconfig.json                   └── tsconfig.json
```

---

## 6. Migration Guide: Express → Elysia

### Step-by-step สำหรับ RAG-Diary

```bash
# 1. Bun
curl -fsSL https://bun.sh/install | bash

# 2. Dependencies
bun add elysia @elysiajs/cors @elysiajs/jwt
bun add -d @types/bun

# 3. Run
bun run --watch src/index.ts
```

### แปลง route:

```typescript
// Express
router.post('/api/diary', async (req, res) => {
  const { title, content } = req.body;
  const embedding = await generateEmbedding(content);
  const result = await db.insert({ title, content, embedding });
  res.json(result);
});

// Elysia
app.post('/api/diary', async ({ body, error }) => {
  try {
    const embedding = await generateEmbedding(body.content);
    return await db.insert({ ...body, embedding });
  } catch (err) {
    return error(500, 'Failed to create entry');
  }
}, {
  body: t.Object({
    title: t.String(),
    content: t.String(),
  })
});
```

---

## คำถามให้คิด

1. **Bun vs Node.js:** Bun อ้างว่าเร็ว 10x — จริงเทียมั้ย? ข้อเสียของ Bun มีอะไรบ้าง?
2. **Elysia Validation:** Validation ใน Elysia ทำงานตอนไหน? มี overhead แค่ไหน?
3. **Eden Treaty:** ถ้า server กับ frontend อยู่ใน repo เดียวกัน — Eden ช่วยลด bug ประเภทไหนได้?
4. **Migration:** ถ้าจะย้าย RAG-Diary จาก Express → Elysia — ควรย้ายทั้งทีเดียวหรือทีละ route?
5. **Plugin System:** `@elysiajs/cors` vs `cors` middleware ใน Express — ต่างกันยังไงในทางปฏิบัติ?

## Links

- Related: [[Backend/02 Express & Middleware]], [[Frontend/05 TanStack Query]]
- Code ref: [[server/src/index.ts]], [[server/package.json]]
- Source: Web search — ElysiaJS tutorial, Eden Treaty docs
- Q&A from: Socratic session 2026-07-02
