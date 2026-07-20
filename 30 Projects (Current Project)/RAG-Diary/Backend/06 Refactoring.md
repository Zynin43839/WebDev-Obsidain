---
created: 2026-07-02
type: lesson
series: backend
number: 06
topic: refactoring
status: seedling
---

# Refactoring — ปรับโครงสร้างโค้ดโดยไม่เปลี่ยนพฤติกรรม

> **เปรียบ:** Refactoring = **ปรับปรุงห้องครัว** โดยไม่เปลี่ยนเมนูอาหาร  
> ห้องครัวรก → ทำกับข้าวช้า, หาของไม่เจอ, เผลอทำหกบ่อย  
> จัดระเบียบใหม่ → เร็วขึ้น, ปลอดภัยขึ้น, ต่อเติมง่ายขึ้น

---

## 1. Refactoring คืออะไร?

**Refactoring** = การเปลี่ยนโครงสร้างภายในของโค้ด โดยไม่เปลี่ยนพฤติกรรมภายนอก

```
Before: ฟังก์ชัน 500 บรรทัดที่ทำทุกอย่าง
After:  10 ฟังก์ชันสั้นๆ แต่ละอันมีหน้าที่เดียว

พฤติกรรม: response เหมือนเดิม
โครงสร้าง: maintenance ง่ายขึ้น
```

### สัญญาณว่าโค้ดต้อง refactor (Code Smells)

| Smell | อาการ | ตัวอย่าง |
|-------|-------|---------|
| **God Class** | ไฟล์เดียวทำทุกอย่าง | `index.ts` 500+ บรรทัด |
| **Long Function** | ฟังก์ชันยาวเกิน 20 บรรทัด | handler ที่รวม validation + business logic + response |
| **Duplicated Code** | โค้ดซ้ำ 3+ ที่ | try-catch block เหมือนกันทุก route |
| **Shotgun Surgery** | เปลี่ยน 1 feature ต้องแก้ 10 ไฟล์ | error handling กระจาย |
| **Primitive Obsession** | ใช้ string แทน type | `status: "active" | "inactive"` ไม่มี type |

---

## 2. ทำไมต้อง Refactor?

### 2.1 Technical Debt

```
เขียนเร็ว ← → เขียนดี
                ↓
       Technical Debt เพิ่มขึ้น
       ดอกเบี้ย = เวลาที่เสียไป每天
```

แต่ละครั้งที่เลือก "แปะของก่อน เดี๋ยวมาเก็บ" — เท่ากับกู้ Technical Debt

### 2.2 Cost of Change

```
 feature 1: ใช้เวลา 2 ชม. (ไม่มี debt)
 feature 5: ใช้เวลา 5 ชม. (debt สะสม)
feature 10: ใช้เวลา 20 ชม. (โค้ดพังทุกครั้ง)
```

**Refactoring จ่ายดอกเบี้ยลดลง:**

```
 feature 1 (refactor 1 ชม. + feature 2 ชม.) = 3 ชม.
 feature 5 (refactor 1 ชม. + feature 2 ชม.) = 3 ชม.
feature 10 = 3 ชม. (เหมือนเดิม)
```

---

## 3. Strangler Fig Pattern

> **ที่มา:** มะเดื่อฝรั่ง (Strangler Fig) เติบโตพันรอบต้นไม้ใหญ่ ค่อยๆ แย่งแสง จนต้นเดิมตายไปเอง

### วิธีใช้ในการ refactor โค้ด

```
┌──────────────────────┐      ┌──────────────────────┐
│   โค้ดเก่า (monolith) │      │  เก่า + ใหม่ (ควบคู่)  │      │   โค้ดใหม่ทั้งหมด    │
│   index.ts 500 บรรทัด  │ ──→  │  index.ts (บางส่วน)   │ ──→  │  routes/  modules/  │
│                      │      │  routes/diary.ts     │      │  services/         │
└──────────────────────┘      └──────────────────────┘      └──────────────────────┘
```

### ขั้นตอนปฏิบัติ

1. **Route:** สร้าง routes/diary.ts ย้ายเฉพาะ diary routes
2. **Middleware:** สร้าง middleware/auth.ts ย้าย auth logic
3. **Service:** สร้าง services/embedding.ts ย้าย embedding logic
4. **DB:** สร้าง lib/db.ts, lib/queries/diary.ts
5. **Cleanup:** เมื่อทุกอย่างย้ายหมด → ลบ `index.ts`

**ข้อดี:** ระบบยังทำงานได้ตลอด ไม่ต้องหยุด deploy มี safety net (test)

---

## 4. Domain-Driven Design (DDD) ฉบับย่อ

### 4.1 Domain = ขอบเขตธุรกิจ

```
RAG-Diary domain:
├── Diary (main entity)
│   ├── create, read, update, delete
│   ├── search (semantic, keyword)
│   └── tag (filtering)
├── Auth (identity)
│   ├── login, register
│   └── JWT management
└── AI (intelligence)
    ├── embedding
    ├── chat
    └── RAG pipeline
```

### 4.2 Structure ตาม DDD

```
server/src/
├── diary/
│   ├── diary.controller.ts    # route handlers
│   ├── diary.service.ts       # business logic
│   ├── diary.repository.ts    # database access
│   └── diary.types.ts         # DTOs, types
├── auth/
│   ├── auth.controller.ts
│   ├── auth.service.ts
│   └── auth.types.ts
├── ai/
│   ├── rag.service.ts
│   └── embedding.service.ts
└── lib/
    ├── db.ts
    ├── error.ts
    └── middleware.ts
```

---

## 5. Refactoring Techniques

### 5.1 Extract Function

```typescript
// Before
app.post('/api/diary', async (req) => {
  const { content, title } = req.body;
  if (!content || !title) throw new Error('Missing fields');
  const embedding = await generateEmbedding(content);
  const { data } = await supabase.from('diary_entries').insert({
    title, content, embedding
  }).select().single();
  return { success: true, data };
});

// After
app.post('/api/diary', async (req) => {
  validateDiaryInput(req.body);
  const embedding = await generateEmbedding(req.body.content);
  const entry = await createDiaryEntry(req.body, embedding);
  return { success: true, data: entry };
});

function validateDiaryInput(body: any) {
  if (!body?.content || !body?.title) {
    throw new Error('Missing required fields');
  }
}

async function createDiaryEntry(body: any, embedding: number[]) {
  const { data } = await supabase
    .from('diary_entries')
    .insert({ title: body.title, content: body.content, embedding })
    .select()
    .single();
  return data;
}
```

### 5.2 Error Handling Middleware

```typescript
// Before (ซ้ำทุก route)
app.get('/api/entries', async () => {
  try {
    const result = await db.query(...);
    return result;
  } catch (err) {
    console.error(err);
    return { error: 'Something went wrong' };
  }
});

// After (centralized)
class AppError extends Error {
  constructor(public statusCode: number, message: string) {
    super(message);
  }
}

function errorHandler(app: Elysia) {
  app.onError(({ code, error }) => {
    if (error instanceof AppError) {
      return new Response(error.message, { status: error.statusCode });
    }
    console.error('Unhandled:', error);
    return new Response('Internal Server Error', { status: 500 });
  });
}
```

### 5.3 Replace Switch with Polymorphism

```typescript
// Before
function getSearchResults(type: string, query: string) {
  if (type === 'semantic') return semanticSearch(query);
  else if (type === 'keyword') return keywordSearch(query);
  else if (type === 'hybrid') return hybridSearch(query);
  else throw new Error('Unknown type');
}

// After
const searchStrategies = {
  semantic: semanticSearch,
  keyword: keywordSearch,
  hybrid: hybridSearch,
} as const;

function getSearchResults(type: keyof typeof searchStrategies, query: string) {
  return searchStrategies[type](query);
}
```

---

## 6. กรณีศึกษา: Refactor index.ts Monolith

### โครงสร้างเดิม

```typescript
// server/src/index.ts ~300+ บรรทัด
// cors → routes → middleware → error handling → listen

app.post('/api/diary', async (ctx) => { ... });
app.get('/api/diary', async (ctx) => { ... });
app.post('/api/search', async (ctx) => { ... });
app.post('/api/chat', async (ctx) => { ... });
app.post('/api/auth/login', async (ctx) => { ... });
app.post('/api/auth/register', async (ctx) => { ... });
// ... 15+ routes ในไฟล์เดียว
```

### แผนการ refactor (Strangler Fig)

```
Phase 1: สร้าง modules
├── modules/diary/diary.routes.ts     # ย้าย diary routes
├── modules/ai/ai.routes.ts           # ย้าย search + chat routes
├── modules/auth/auth.routes.ts       # ย้าย auth routes
└── index.ts                          # เหลือแค่ import + register

Phase 2: แยก services
├── modules/diary/diary.service.ts    # business logic
├── modules/diary/diary.repository.ts # database access
└── modules/diary/diary.types.ts      # types

Phase 3: Error handling middleware
├── lib/error.ts                      # AppError class
├── lib/middleware.ts                 # error handler
└── index.ts                          # .use(errorHandler)

Phase 4: Config + DI
├── config/index.ts                   # env vars
├── lib/db.ts                         # Supabase client (single instance)
└── index.ts                          # clean import
```

---

## คำถามให้คิด

1. **Strangler Fig:** ถ้าเรา refactor ทีละ route แล้ว deploy — จะมั่นใจได้ยังไงว่า route เก่ากับใหม่ทำงานเหมือนกัน?
2. **Error handling:** centralized error handler กับ per-route try-catch — trade-off คืออะไร?
3. **DDD:** โค้ด RAG-Diary ตอนนี้อยู่ใน domain ไหน? (Diary / Auth / AI) — มี overlap ไหม?
4. **Code Smell:** ใน `server/src/index.ts` ของ RAG-Diary เจอ smell อะไรบ้าง? (เปิดไฟล์ดู)
5. **Testing:** Refactoring โดยไม่มี test — ถ้าเลือกได้ ควรเขียน test ก่อนหรือ refactor ก่อน?

## Links

- Related: [[Backend/07 Testing]], [[Backend/02 Express & Middleware]]
- Code ref: [[server/src/index.ts]]
- Source: Alex Xu — System Design Interview (NotebookLM), Martin Fowler — Refactoring
- Q&A from: Socratic session 2026-07-02
