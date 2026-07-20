---
type: concept
topic: typescript
status: seedling
created: 2026-06-30
---

# TypeScript Basics

## What is this?

> **NotebookLM:** TypeScript is a **statically typed superset of JavaScript**. While JavaScript is dynamically typed and evaluates your code at runtime, TypeScript analyzes your code and checks for errors at **compile time** — before the code even runs. [Source: NotebookLM]
>
> **คำอธิบาย:** TypeScript = **JavaScript ที่มีระบบ type** — ตอนเขียนโค้ด TypeScript จะตรวจสอบ error ให้ก่อน (compile time) โดยที่ไม่ต้องรัน code จริง ต่างจาก JavaScript ที่รู้ว่าพังก็ต่อเมื่อโค้ดถูกรันแล้ว (runtime) เท่านั้น เปรียบเหมือนมีติวเตอร์ตรวจการบ้านก่อนส่ง vs ส่งไปแล้วอาจารย์ถึงจะตรวจเจอ

```typescript
// JavaScript — ไม่รู้ error จนกว่าจะรัน
function squareOf(n) {
  return n * n;
}
squareOf(2);   // 4 ✅
squareOf("z"); // NaN ❌ (runtime error — ไม่มีใครบอก!)

// TypeScript — เจอ error ตั้งแต่เขียน
function squareOf(n: number) {
  return n * n;
}
squareOf(2);   // 4 ✅
squareOf("z"); // ❌ Error in editor! "Argument of type 'string' is not assignable to parameter of type 'number'"
```

**TypeScript ≠ ภาษาใหม่** — มันคือ JavaScript + type system

```
TypeScript Code (.ts)
     │
     ▼  tsc (TypeScript Compiler)
JavaScript Code (.js)  ← types หายไปแล้ว
     │
     ▼  node / browser รัน
```

## Why This Matters

RAG-Diary ทั้ง backend (`index.ts`) และ frontend (React + Vite) เขียนด้วย TypeScript ทั้งหมด ถ้าไม่เข้าใจ TypeScript จะงมโค้ด:
- Type annotations คืออะไร (`: string`, `: number`)
- `interface` vs `type` ต่างกันยังไง
- `async` function return `Promise<>` — แล้ว type ของ Promise คืออะไร
- Generics `<T>` — เห็นในโค้ด Supabase (`supabase.from<DiaryEntry>`)

---

## Part 1: Why TypeScript?

### 1.1 Runtime vs Compile-time Errors

> **NotebookLM:** In plain JavaScript, implicit type conversions can cause hard-to-track bugs. For instance, if you accidentally add a number to an array (`3 + [3]`), JavaScript will implicitly convert them and output `"31"`. TypeScript immediately catches this invalid operation at compile time. [Source: NotebookLM]
>
> **คำอธิบาย:** JavaScript มีพฤติกรรม "ใจดีเกินไป" — เวลาเจอ type ต่างกัน มันจะ **แปลง type ให้อัตโนมัติ (implicit conversion)** ซึ่งทำให้เกิด bug หายาก เช่น `3 + [3]` แทนที่จะ error กลับกลายเป็น `"31"` (string) TypeScript จะดักไว้ได้ตั้งแต่ตอนคอมไพล์ เพราะเห็นว่าเอา number กับ array มาบวกกันมันไม่สมเหตุสมผล

```javascript
// JavaScript — silent failures 😶
const user = { name: "John" };
user.email;              // undefined (ไม่มี error!)
user.name.toFixed(2);    // ❌ runtime error! (string ไม่มี toFixed)

function greet(name) {
  return `Hello ${name.toUpperCase()}`;
}
greet(42);  // ❌ runtime! number ไม่มี toUpperCase()
```

```typescript
// TypeScript — compiler เตือนก่อน 🛡️
const user: { name: string } = { name: "John" };
user.email;              // ❌ Property 'email' does not exist
user.name.toFixed(2);    // ❌ Property 'toFixed' does not exist on type 'string'

function greet(name: string) {
  return `Hello ${name.toUpperCase()}`;
}
greet(42);  // ❌ Argument of type 'number' is not assignable to parameter of type 'string'
```

### 1.2 TypeScript คือ Documentation ที่รันได้

```typescript
// ถ้าเห็น interface นี้ — รู้เลยว่า DiaryEntry มีอะไรบ้าง
interface DiaryEntry {
  id: string;
  content: string;
  tag: string;
  created_at: string;
  embedding?: number[];  // ? = optional (อาจไม่มี)
}

// ได้ autocomplete, รู้ structure, เปลี่ยน schema → compiler บอก
function displayEntry(entry: DiaryEntry) {
  console.log(entry.content);  // ✅ autocomplete!
  console.log(entry.nonexist); // ❌ compiler เตือน!
}
```

---

## Part 2: Basic Types

### 2.1 Primitive Types

```typescript
const name: string = "RAG-Diary";
const age: number = 25;           // int, float, negative → number
const isActive: boolean = true;   // true / false (lowercase)
const data: null = null;          // null
const id: undefined = undefined;  // undefined
const big: bigint = 9007199254740991n;
const sym: symbol = Symbol("id");
```

### 2.2 Type Inference (ไม่ต้องเขียน type ก็ได้)

> **NotebookLM:** TypeScript has powerful **type inference**. If you leave off the annotation, TypeScript automatically infers the type for you. [Source: NotebookLM]
>
> **คำอธิบาย:** Type Inference = **ความสามารถของ TypeScript ในการเดา type ให้อัตโนมัติ** โดยไม่ต้องเขียน `: string` หรือ `: number` ต่อท้าย ถ้าคุณเขียน `let name = "RAG-Diary"` — TypeScript จะเดาว่า `name` เป็น `string` ทันที ทำให้เขียนน้อยลง แต่ยังได้ความปลอดภัยของ type

```typescript
let name = "RAG-Diary";   // TypeScript infer: string ✅
name = 42;                // ❌ Type 'number' is not assignable to type 'string'

let count = 0;            // infer: number
count = "hello";          // ❌

// ถ้า type ไม่ชัด → ใส่ annotation
let data: any = "hello";  // any = ปิด type checking (ไม่แนะนำ)
data = 42;                // OK (any = bypass)
```

### 2.3 `any` — The Escape Hatch

> **NotebookLM:** `any` turns off type checking for a variable — use it sparingly. [Source: NotebookLM]
>
> **คำอธิบาย:** `any` = **ปิดระบบ type checking สำหรับตัวแปรนั้น** — ทำให้ TypeScript ทำงานเหมือน JavaScript ธรรมดา (ไม่ตรวจ error) ควรใช้เท่าที่จำเป็น เพราะถ้าใช้ `any` ทั้งโปรเจค = ไม่ต่างจากใช้ JavaScript ล้วนๆ สูญเสียประโยชน์ของ TypeScript ทั้งหมด

```typescript
// ❌ any = JavaScript ธรรมดา
function log(data: any) {
  console.log(data.toUpperCase());  // compiler ไม่ error — แต่ runtime พังถ้า data ไม่ใช่ string
}

// ✅ unknown = "ยังไม่รู้ type" — ต้อง check ก่อนใช้
function log(data: unknown) {
  if (typeof data === "string") {
    console.log(data.toUpperCase());  // ✅ safe
  }
}
```

### 2.4 Arrays

```typescript
const entries: string[] = ["Hello", "World"];
const numbers: Array<number> = [1, 2, 3];  // Array<number> === number[]

// Mixed types
const mixed: (string | number)[] = ["hello", 42];
```

### 2.5 Tuples (array ที่มี length + type แน่นอน)

```typescript
type Entry = [string, string, string];  // [id, content, tag]
const entry: Entry = ["abc-123", "Hello", "Learning"];

// Destructuring
const [id, content, tag] = entry;
// id: string, content: string, tag: string
```

---

## Part 3: Interfaces & Types

### 3.1 Interface — ประกาศ structure

```typescript
interface User {
  id: string;
  email: string;
  name: string;
  age?: number;          // ? = optional
  readonly createdAt: string;  // readonly = แก้ไม่ได้
}

const user: User = {
  id: "abc-123",
  email: "john@test.com",
  name: "John",
  createdAt: "2026-06-30"
};
user.createdAt = "2025";  // ❌ Cannot assign to 'createdAt' because it is a read-only property
```

### 3.2 Type Alias — interface แบบอื่น

```typescript
// type ใช้กับ primitive union ได้
type ID = string;
type Status = "active" | "inactive" | "deleted";  // union type
type Callback = (id: ID) => void;  // function signature

// interface extend ได้ — type ก็ extend ได้ (ใช้ intersection)
interface BaseEntry {
  id: string;
  content: string;
}

interface DiaryEntry extends BaseEntry {
  tag: string;
  embedding?: number[];
}

// Type intersection (&)
type DiaryEntry = BaseEntry & {
  tag: string;
  embedding?: number[];
};
```

### 3.3 Interface vs Type — เมื่อไหร่ใช้อะไร

| | interface | type |
|--|-----------|------|
| Extends/Augment | ✅ extends (open — เพิ่ม property ทีหลังได้) | ✅ intersection (&) (closed) |
| Union / Tuple | ❌ | ✅ |
| Primitive alias | ❌ | ✅ (`type ID = string`) |
| Declaration merging | ✅ | ❌ |

**Rule of thumb:** ใช้ `interface` สำหรับ object types, ใช้ `type` สำหรับ union/tuple/primitive aliases

### 3.4 Real RAG-Diary Types

```typescript
// From RAG-Diary's actual code
interface DiaryEntry {
  id: string;
  content: string;
  tag: string;
  created_at: string;
  embedding?: number[];
}

interface DiaryResponse {
  success: boolean;
  data?: DiaryEntry;
  error?: {
    code: string;
    message: string;
  };
}

interface ChatRequest {
  message: string;
}

interface ChatResponse {
  success: boolean;
  response?: string;
  sources?: { id: string; content: string }[];
}
```

---

## Part 4: Functions

### 4.1 Parameter & Return Types

```typescript
// Parameter types + return type
function add(a: number, b: number): number {
  return a + b;
}

// Arrow function
const multiply = (a: number, b: number): number => a * b;

// Void return (ไม่ return อะไร)
function logEntry(content: string): void {
  console.log(content);
  // ไม่ต้อง return
}

// Optional parameters
function greet(name: string, greeting?: string): string {
  return `${greeting || "Hello"}, ${name}!`;
}

// Default parameters
function greet(name: string, greeting: string = "Hello"): string {
  return `${greeting}, ${name}!`;
}
```

### 4.2 Async Functions

```typescript
// async function return Promise<T> เสมอ
async function fetchEntry(id: string): Promise<DiaryEntry> {
  const res = await fetch(`/api/diary/${id}`);
  const data = await res.json();
  return data as DiaryEntry;  // type assertion
}

// Promise<void> — async ที่ไม่ return ค่า
async function saveEntry(entry: DiaryEntry): Promise<void> {
  await fetch("/api/diary", {
    method: "POST",
    body: JSON.stringify(entry)
  });
}

// RAG-Diary real route handler
app.post("/api/diary", async ({ body }): Promise<DiaryResponse> => {
  try {
    const embedding = await generateEmbedding(body.content);
    const { data } = await supabase
      .from("diary_entries")
      .insert({ content: body.content, embedding })
      .select()
      .single();
    return { success: true, data };
  } catch (error) {
    return { success: false, error: { code: "ERROR", message: String(error) } };
  }
});
```

---

## Part 5: Generics

### 5.1 What is a Generic?

> **NotebookLM:** Generics allow you to write functions, classes, and types that work with any type while still maintaining type safety. [Source: NotebookLM]
>
> **คำอธิบาย:** Generics = **การเขียนฟังก์ชันหรือ type ที่ "ยืดหยุ่น" กับ type ต่างๆ ได้** โดยไม่ต้องเสีย type safety — แทนที่จะเขียน `getFirstString()` สำหรับ string และ `getFirstNumber()` สำหรับ number แยกกัน ก็เขียน `getFirst<T>()` อันเดียว แล้วบอกว่า "T คืออะไร" ตอนเรียกใช้

```typescript
// ❌ Without generics — ต้องเขียนหลาย version
function getFirstString(arr: string[]): string { return arr[0]; }
function getFirstNumber(arr: number[]): number { return arr[0]; }

// ✅ With generics — 1 function ใช้ได้ทุก type
function getFirst<T>(arr: T[]): T | undefined {
  return arr[0];
}

const firstStr = getFirst<string>(["a", "b"]);  // string
const firstNum = getFirst<number>([1, 2, 3]);    // number
// TypeScript infer type: getFirst(["a", "b"]) → string โดยอัตโนมัติ!
```

### 5.2 Generic in Real RAG-Diary

```typescript
// Supabase generic
const { data } = await supabase
  .from("diary_entries")
  .select<DiaryEntry>();    // ← generic — บอกว่า data จะเป็น DiaryEntry[]

// Generic helper function
async function fetchFromApi<T>(url: string): Promise<T> {
  const res = await fetch(url);
  const data = await res.json();
  return data as T;
}

// ใช้ได้ทุก API endpoint
const entries = await fetchFromApi<DiaryEntry[]>("/api/diary");
const user = await fetchFromApi<User>("/api/user");
const tags = await fetchFromApi<Tag[]>("/api/tags");
```

### 5.3 Generic Constraints

```typescript
// Constrain T ว่า ต้องมี property id
interface HasId {
  id: string;
}

async function getById<T extends HasId>(items: T[], id: string): T | undefined {
  return items.find(item => item.id === id);
}

// ✅ ใช้ได้กับ DiaryEntry (มี id)
const entry = getById<DiaryEntry>(entries, "abc-123");

// ❌ ถ้า type ไม่มี id → error
const nums = [1, 2, 3];
getById(nums, "1");  // Error! number doesn't have id property
```

---

## Part 6: Union & Intersection Types

### 6.1 Union — OR (`|`)

```typescript
// ค่าเป็นได้หลาย type
type ID = string | number;
function getEntry(id: ID): DiaryEntry | undefined {
  // id เป็นได้ทั้ง string และ number
  return entries.find(e => e.id === String(id));
}

// Discriminated union — object ที่มี type field บอกว่าเป็นอันไหน
type Result<T> =
  | { status: "success"; data: T }
  | { status: "error"; error: string };

function handleResult(result: Result<DiaryEntry>) {
  if (result.status === "success") {
    console.log(result.data.content);  // ✅ TypeScript รู้ว่า data มี!
  } else {
    console.log(result.error);  // ✅ TypeScript รู้ว่า error มี!
  }
}
```

### 6.2 Intersection — AND (`&`)

```typescript
// รวม properties จากหลาย type
interface Timestamped {
  created_at: string;
  updated_at?: string;
}

interface SoftDeletable {
  deleted_at?: string;
}

type DiaryEntry = BaseEntry & Timestamped & SoftDeletable;
// ตอนนี้ DiaryEntry มีทุก property จากทั้ง 3
```

### 6.3 Type Narrowing

```typescript
// TypeScript แคบ type ให้อัตโนมัติตามเงื่อนไข
function process(value: string | number | null) {
  if (value === null) {
    // value = null
    return;
  }

  if (typeof value === "string") {
    // value = string ✅
    console.log(value.toUpperCase());
  } else {
    // value = number ✅
    console.log(value.toFixed(2));
  }
}
```

---

## Part 7: Type Assertion & Type Guards

### 7.1 Type Assertion (`as`)

```typescript
// เมื่อเรารู้ว่า type จริงดีกว่า TypeScript
const rawData = JSON.parse('{"id":"abc","content":"Hello"}') as DiaryEntry;
// rawData.content — ✅ เข้าใจว่าเป็น string

// Non-null assertion (!)
const first = entries.find(e => e.id === id)!;  // ! = "มันไม่ null แน่"
// ถ้า find เจอ → first มีค่า ถ้าไม่เจอ → runtime error!
```

### 7.2 Type Guards (custom)

```typescript
// Custom type guard function
function isDiaryEntry(obj: any): obj is DiaryEntry {
  return obj && typeof obj.id === "string" && typeof obj.content === "string";
}

// ใช้ในโค้ด
function processEntry(data: unknown) {
  if (isDiaryEntry(data)) {
    console.log(data.content);  // ✅ TypeScript รู้ว่า data = DiaryEntry
  }
}
```

---

## Part 8: TypeScript Config — tsconfig.json

### Key Options

```json
{
  "compilerOptions": {
    "target": "ES2022",           // compile to ES2022 JavaScript
    "module": "ESNext",           // module system
    "strict": true,               // เปิด type checking ทั้งหมด
    "outDir": "./dist",           // output folder
    "rootDir": "./src",           // source folder
    "esModuleInterop": true,      // import CommonJS modules
    "skipLibCheck": true,          // ไม่ check .d.ts files
    "forceConsistentCasingInFileNames": true
  },
  "include": ["src/**/*"]
}
```

**`strict: true`** = เปิด type checking ทั้งชุด (noImplicitAny, strictNullChecks, etc.)
→ ควรเปิดเสมอ!

---

## Question to Think About

1. **any vs unknown:** `function f(data: any)` vs `function f(data: unknown)` — ต่างกันยังไง? (try toUpperCase ทั้งคู่)
2. **Readonly:** `readonly` ใน interface — ใช้ป้องกันอะไร? ถ้า object เปลี่ยน property ได้ — มี readonly มีประโยชน์ยังไง?
3. **Generics:** `Promise<DiaryEntry[]>` — `<DiaryEntry[]>` หมายถึง array ของ DiaryEntry หรือ Promise ที่ resolve เป็นอะไร? (ทั้งคู่!)
4. **Type Assertion:** `JSON.parse(...) as DiaryEntry` — ถ้า JSON จริงไม่มี field `content` — TypeScript เตือนไหม?
5. **Discriminated Union:** ถ้า `status: "success"` แล้ว TypeScript รู้ได้ยังไงว่าต้องมี `data`?

## Links

- Related concept: [[Async Await & Event Loop]], [[RAG Flow]], [[REST API JSON]]
- Code ref: [[server/src/index.ts]], [[server/src/lib/db.ts]], [[frontend/src/lib/api.ts]]
- Source: Alex Xu — System Design Interview (NotebookLM), Traversy Media
