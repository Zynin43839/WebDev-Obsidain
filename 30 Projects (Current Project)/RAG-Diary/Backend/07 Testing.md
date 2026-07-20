---
created: 2026-07-02
type: lesson
series: backend
number: 07
topic: testing
status: seedling
---

# Testing — ทำให้โค้ดไม่พัง (และรู้ก่อน deploy)

> **เปรียบ:** Testing = **การตรวจเช็คเครื่องบินก่อนบิน**  
> - Unit test = เช็คเครื่องยนต์แต่ละลูก
> - Integration test = ทดสอบระบบ fuel + engine + control พร้อมกัน
> - E2E test = บินจริงทั้งลำ

---

## 1. Test Pyramid

```
        ╱╲
       ╱  ╲          E2E Tests (น้อย — ช้าที่สุด)
      ╱    ╲         เรียก browser → คลิก → assert
     ╱──────╲
    ╱        ╲       Integration Tests (กลาง)
   ╱          ╲      Route + DB + external service
  ╱────────────╲
 ╱              ╲    Unit Tests (มาก — เร็วที่สุด)
╱                ╲   Function เดี่ยว, ไม่มี I/O
```

| Layer | จำนวน | ความเร็ว | ติดอะไร |
|-------|-------|---------|---------|
| Unit | 70%+ | มิลลิวินาที | function logic |
| Integration | 20% | วินาที | route → DB → response |
| E2E | 10% | 10+ วินาที | user flow ทั้งระบบ |

### Golden Rule

- **Unit test** ควรเยอะที่สุด (feedback loop สั้น)
- **E2E test** ควรเท่าที่จำเป็น (path สำคัญ)
- **Integration test** = sweet spot สำหรับ web API

---

## 2. Unit Testing

### 2.1 ลักษณะของ unit test ที่ดี

```typescript
// ✅ Good
describe('validateDiaryInput', () => {
  it('throws when content is missing', () => {
    expect(() => validateDiaryInput({ title: 'test' }))
      .toThrow('Missing required fields');
  });

  it('passes with valid input', () => {
    expect(() => validateDiaryInput({ title: 'x', content: 'y' }))
      .not.toThrow();
  });
});

// ❌ Bad (depends on DB — นี่คือ integration test)
describe('createDiary', () => {
  it('saves to database', async () => {
    const result = await createDiary({ title: 'test' }); // calls DB
    expect(result.id).toBeDefined();
  });
});
```

### 2.2 Dependency Injection ทำให้ testable

```typescript
// ❌ Hard to test (calls supabase directly)
async function createEntry(data: EntryInput) {
  const { data: result } = await supabase.from('diary').insert(data);
  return result;
}

// ✅ Easy to test (inject repository)
async function createEntry(
  data: EntryInput,
  repo: DiaryRepository = defaultRepo
) {
  return repo.create(data);
}

// Test
it('creates entry', async () => {
  const mockRepo = { create: vi.fn().mockResolvedValue({ id: 1 }) };
  const result = await createEntry({ title: 'test' }, mockRepo);
  expect(mockRepo.create).toHaveBeenCalledWith({ title: 'test' });
});
```

---

## 3. Integration Testing

### 3.1 Route Testing with Elysia

```typescript
import { describe, it, expect } from 'vitest';
import { app } from '../src/index';

describe('POST /api/diary', () => {
  it('creates a diary entry', async () => {
    const response = await app.handle(
      new Request('http://localhost/api/diary', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ title: 'Test', content: 'Test content' }),
      })
    );

    expect(response.status).toBe(200);
    const body = await response.json();
    expect(body.success).toBe(true);
    expect(body.data.title).toBe('Test');
  });

  it('returns 400 for missing fields', async () => {
    const response = await app.handle(
      new Request('http://localhost/api/diary', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({}),
      })
    );

    expect(response.status).toBe(400);
  });
});
```

### 3.2 Ephemeral Test Database

```typescript
// ใช้ test container หรือ Supabase project แยก
const TEST_DB_URL = process.env.TEST_SUPABASE_URL;

// ก่อนทดสอบ: รัน migration
beforeAll(async () => {
  await runMigrations(TEST_DB_URL);
  await seedTestData(TEST_DB_URL);
});

// หลังทดสอบ: clean up
afterAll(async () => {
  await clearTestData(TEST_DB_URL);
});
```

### 3.3 BUILD-OPERATE-CHECK Pattern

```
BUILD:   prepare test data + state
         ↓
OPERATE: execute the function/route
         ↓
CHECK:   assert result + side effects
```

```typescript
it('updates diary entry', async () => {
  // BUILD
  const entry = await createTestEntry({ title: 'Original' });

  // OPERATE
  const response = await updateEntry(entry.id, { title: 'Updated' });

  // CHECK
  expect(response.title).toBe('Updated');
  const dbEntry = await getEntryById(entry.id);
  expect(dbEntry.title).toBe('Updated'); // ตรวจสอบ side effect
});
```

---

## 4. Testing Strategy สำหรับ RAG-Diary

### 4.1 Priority

```
1. Diary CRUD         ★★★★★  (core feature)
2. Auth               ★★★★☆  (security)
3. Search (semantic)  ★★★★☆  (core feature)
4. AI Chat            ★★★☆☆  (depends on OpenAI)
5. RAG Pipeline       ★★★☆☆  (complex logic)
```

### 4.2 Mock vs Real

| External Service | Test Approach |
|-----------------|---------------|
| **Supabase** | Test DB แยก หรือ mock repository |
| **OpenAI** | Mock API (ไม่เสียเงิน + เร็ว) |
| **JWT** | Test ด้วย secret จริง หรือ mock verify |
| **pgvector** | Integration test จริง (ผูกกับ DB) |

### 4.3 Mocking OpenAI

```typescript
import { vi } from 'vitest';

// Mock ฟังก์ชัน generateEmbedding
vi.mock('../src/modules/ai/rag', () => ({
  generateEmbedding: vi.fn().mockResolvedValue(
    Array(1536).fill(0.1) // mock embedding vector
  ),
  findSimilarEntries: vi.fn().mockResolvedValue([
    { id: 1, content: 'Mock entry', similarity: 0.95 }
  ]),
}));
```

---

## 5. E2E Testing (Optional)

```typescript
// playwright.config.ts
import { defineConfig } from '@playwright/test';

export default defineConfig({
  testDir: './e2e',
  webServer: {
    command: 'bun run dev',
    url: 'http://localhost:3000',
  },
});
```

```typescript
// e2e/diary.spec.ts
test('user can create and view diary entry', async ({ page }) => {
  await page.goto('/login');
  await page.fill('[name="email"]', 'test@example.com');
  await page.fill('[name="password"]', 'password123');
  await page.click('button[type="submit"]');

  await page.goto('/diary/new');
  await page.fill('[name="title"]', 'E2E Test Entry');
  await page.fill('[name="content"]', 'Testing in production');
  await page.click('button:has-text("Save")');

  await expect(page.locator('text=E2E Test Entry')).toBeVisible();
});
```

---

## 6. Testing Tools

| Tool | ใช้ทำอะไร |
|------|----------|
| **Vitest** | Unit + Integration test (fast, Vite-native) |
| **Supertest** | HTTP assertion สำหรับ route testing |
| **Playwright** | E2E browser test |
| **Test Containers** | Ephemeral DB สำหรับ integration test |
| **MSW (Mock Service Worker)** | Mock API ใน browser test |

---

## คำถามให้คิด

1. **Test Pyramid:** โค้ด RAG-Diary ตอนนี้ test coverage แค่ไหน? จะเริ่มเพิ่มจากตรงไหน?
2. **Mock vs Real:** ถ้า mock OpenAI → test เร็วขึ้น แต่พลาด error จริงจาก API ได้ — balance ที่ดีคือ?
3. **Testing Routes:** Elysia `app.handle(new Request(...))` เทียบกับ supertest — ต่างกันยังไง?
4. **CI/CD:** ควรให้ test รันตอนไหน? (pre-commit / pre-push / CI ทุก PR)
5. **Ephemeral DB:** สร้าง Supabase project แยกสำหรับ test — คุ้มกับ effort มั้ย?

## Links

- Related: [[Backend/06 Refactoring]], [[Backend/08 Bun & Elysia]]
- Code ref: [[server/src/index.ts]]
- Source: Alex Xu — System Design Interview (NotebookLM)
- Q&A from: Socratic session 2026-07-02
