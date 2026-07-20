---
type: concept
topic: javascript
status: seedling
created: 2026-06-30
---

# Async/Await & Event Loop

## What is this?

JavaScript เป็น **single-threaded** — รันทีละคำสั่ง ถ้าต้องรอโหลดข้อมูลจาก API หรืออ่านไฟล์ — ถ้ารอแบบ synchronous ทั้งหน้าเว็บจะค้าง จนกว่าข้อมูลมา

**async/await** + **Promises** + **Event Loop** = วิธีที่ JavaScript จัดการงานที่ต้องรอ โดยไม่ block main thread

```
Synchronous:   [fetch API 🟡🟡🟡🟡... wait 2s ... fetch done → show UI]
               ↑ หน้า freeze ระหว่างรอ!

Asynchronous:  [fetch API → "กำลังโหลด..." → main thread ทำงานอื่นต่อ]
               [2 seconds later → แสดงผล] 
               ↑ หน้าไม่ freeze!
```

## Why This Matters

RAG-Diary ใช้ async/await ทุกที่:
```typescript
// RAG-Diary — การเรียก API ทุกครั้งเป็น async
app.post("/api/diary", async ({ body }) => {
  const embedding = await generateEmbedding(body.content);  // ← รอ OpenAI
  const { data } = await supabase.from("diary_entries")     // ← รอ Database
    .insert({ content: body.content, embedding })
    .select()
    .single();
  return { success: true, data };
});
```

ถ้าไม่เข้าใจ async/await — จะงงว่า `await` ทำอะไร, ทำไมต้อง `async`, และPromiseคืออะไร

---

## Part 1: Problem — Synchronous Blocking

> **NotebookLM Analogy:** JavaScript runs on a **single thread** — like a **single chef in a kitchen**. If the chef puts bread in the toaster and just stands there staring at it until it pops, nothing else gets done. The vegetables don't get chopped, the soup doesn't get stirred. That's **synchronous blocking**. [Source: NotebookLM]
>
> **คำอธิบาย:** JavaScript = **พ่อครัวคนเดียวในครัว** — ทำทีละอย่าง ถ้าพ่อครัวยืนรอมันปิ้ง (synchronous) งานอื่นก็ทำไม่ได้ ผักก็ไม่ได้หั่น ซุปก็ไม่ได้คน หน้าเว็บเรา freeze เพราะ main thread หมดไปกับการ "ยืนรอ" อย่างเดียว

```javascript
// ❌ Synchronous — หน้า freeze!
function fetchUser() {
  const response = networkRequestSync("/api/user");  // รอ 2 วิ — block!
  return response;
}

console.log("Start");
const user = fetchUser();    // ← freeze 2 วินาที
console.log("End");          // ← ต้องรอ fetchUser เสร็จก่อน
```

**ผล:** ปุ่มกดไม่ได้, animation หยุด, user งง

---

## Part 2: Callbacks — Solution แรก

Callback = **function ที่ส่งเป็น argument ให้เรียกทีหลังเมื่องานเสร็จ**

> **NotebookLM:** A callback is like giving a friend your phone number and asking them to call you back when they finish an errand. [Source: NotebookLM]
>
> **คำอธิบาย:** Callback = **"บอกเบอร์โทรให้เพื่อน แล้วให้โทรกลับเมื่อธุระเสร็จ"** — แทนที่เราจะยืนรอเพื่อนทำธุระตรงนั้น (synchronous blocking) ก็แค่บอกว่า "เสร็จแล้วโทรบอกนะ" แล้วเราก็ไปทำอย่างอื่นต่อได้ callback คือฟังก์ชันที่เราส่งเป็น argument ไป แล้วมันจะถูกเรียกกลับมาเมื่อ operation ใหญ่เสร็จ

```javascript
// ✅ Asynchronous with callback
function fetchUser(callback) {
  networkRequest("/api/user", (response) => {
    callback(response);
  });
}

console.log("Start");
fetchUser((user) => {
  console.log("Got user:", user);  // ← ทำงานทีหลัง เมื่อ data มา
});
console.log("End");  // ← รันทันที ไม่ต้องรอ!
```

**ผลลัพธ์:**
```
Start
End
Got user: { id: 1 }   ← มา 2 วิทีหลัง
```

### Callback Hell (Pyramid of Doom)

ปัญหา callback — ยิ่งซ้อนหลายอัน ยิ่งอ่านยาก:

```javascript
// 😱 Callback Hell — 3 nested levels
getUser(id, (user) => {
  getPosts(user.id, (posts) => {
    getComments(posts[0].id, (comments) => {
      getReplies(comments[0].id, (replies) => {
        console.log(replies);
      });
    });
  });
});
```

เรียกว่า **"Pyramid of Doom"** — ซ้อนเยอะจนอ่านแทบไม่ออก, error handling ยาก, debug ลำบาก

---

## Part 3: Promises — The Solution

> **NotebookLM:** A Promise is an object that represents the **future result** of an asynchronous operation. Think of a Promise like a **receipt** — when you order coffee, the barista gives you a receipt. You don't get coffee immediately, but the receipt represents your **promise** of future coffee. You can go do other things while waiting. When the coffee is ready, the receipt is "redeemed" with the actual coffee. [Source: NotebookLM]
>
> **คำอธิบาย:** Promise = **ใบเสร็จจากร้านกาแฟ** — สั่งกาแฟแล้วได้ใบเสร็จ (Promise) กลับมา ใบเสร็จนี้ "เป็นตัวแทน" ของกาแฟในอนาคต ระหว่างรอก็ไปทำอย่างอื่นได้ พอกาแฟพร้อม ก็เอาใบเสร็จไปรับกาแฟ (`.then()`) ถ้าเกิดเมล็ดหมด (error) ก็แจ้งว่า "ขอโทษค่ะ หมด" (`.catch()`) ทำให้เราไม่ต้องยืนรอที่เคาน์เตอร์

### Promise States

```
Pending    → กำลังรอ (ยังไม่เสร็จ)
Fulfilled  → เสร็จแล้ว ได้ result
Rejected   → error เกิดขึ้น
```

```
[Order Coffee] ──→ ⏳ Pending ──→ ☕ Fulfilled (result)
                                     └── ❌ Rejected (error)
```

### Creating a Promise

```javascript
const promise = new Promise((resolve, reject) => {
  // do something async
  setTimeout(() => {
    const success = true;
    if (success) {
      resolve("Coffee is ready! ☕");
    } else {
      reject("Machine broken 💀");
    }
  }, 2000);
});
```

### Using a Promise — .then()

```javascript
fetchUser(1)
  .then((user) => {
    console.log("User:", user);
    return getPosts(user.id);   // ← return Promise → chain ได้
  })
  .then((posts) => {
    console.log("Posts:", posts);
  })
  .catch((error) => {
    console.error("Error:", error);
  });
```

### Promise vs Callback — ต่างกันยังไง?

```javascript
// Callback Hell
getUser(id, (err, user) => {
  if (err) handleError(err);
  getPosts(user.id, (err, posts) => {
    if (err) handleError(err);
    getComments(posts[0].id, (err, comments) => {
      if (err) handleError(err);
      console.log(comments);
    });
  });
});

// Promise Chain — flat, readable!
getUser(id)
  .then((user) => getPosts(user.id))
  .then((posts) => getComments(posts[0].id))
  .then((comments) => console.log(comments))
  .catch((error) => handleError(error));  // ← error ตรงกลางแค่ที่เดียว!
```

### Common Promise Patterns

```javascript
// Promise.all — รอทั้งหมดพร้อมกัน (parallel!)
const [user, posts, tags] = await Promise.all([
  fetchUser(1),
  fetchPosts(1),
  fetchTags()
]);
// 3 requests พร้อมกัน — เร็วสุด!

// Promise.race — อันไหนเสร็จก่อนเอาอันนั้น
const result = await Promise.race([
  fetch("/api/data"),
  timeout(5000)     // ถ้า 5 วิไม่เสร็จ → timeout
]);

// Promise.allSettled — รอทั้งหมด ไม่สน success/fail
const results = await Promise.allSettled([
  fetch("/api/a"),
  fetch("/api/b"),
  fetch("/api/c")
]);
// [{status: "fulfilled", value: ...}, {status: "rejected", reason: ...}]
```

---

## Part 4: async/await — Syntax Sugar

> **NotebookLM:** async/await is syntactic sugar over Promises — it makes asynchronous code look and behave like synchronous code, without blocking the main thread. [Source: NotebookLM]
>
> **คำอธิบาย:** async/await = **น้ำตาลเทียมที่ทำให้โค้ด async อ่านง่ายเหมือน sync** — ปกติ Promise ต้องใช้ `.then()` chain ซึ่งอ่านยากเมื่องานเยอะ async/await ทำให้เราสามารถเขียน `await fetch(...)` แล้วโค้ดบรรทัดถัดไปจะรันต่อหลังจาก fetch เสร็จ โดยไม่ต้อง嵌套 callback หรือ chain `.then()` — แต่ข้างในยังคงเป็น asynchronous (ไม่ block main thread) เหมือนเดิม

### Basic Usage

```javascript
// Promise version
function getData() {
  return fetch("/api/data")
    .then((res) => res.json())
    .then((data) => {
      console.log(data);
      return data;
    });
}

// async/await version — อ่านง่ายเหมือน sync!
async function getData() {
  const res = await fetch("/api/data");
  const data = await res.json();
  console.log(data);
  return data;
}
```

### กฏของ async/await

1. **`async` หน้า function** → function นั้น return Promise เสมอ
2. **`await` ข้างใน** → รอ Promise นั้น resolve แล้วคืนค่า result
3. **`await` ใช้ได้แค่ใน `async` function** (ยกเว้น top-level module)

```javascript
async function example() {
  // await ใช้ข้างในได้ ✅
  const result = await somePromise();
  return result;
}

// ❌ await นอก async function ไม่ได้
const x = await somePromise();  // Error!
```

### Error Handling with try/catch

```javascript
// ❌ Promise: .catch() อาจลืม
async function getData() {
  const res = await fetch("/api/data");
  const data = await res.json();
  return data;
}
// ถ้า fetch ล้ม → Unhandled Promise Rejection!

// ✅ async/await: try/catch ปกติ
async function getData() {
  try {
    const res = await fetch("/api/data");
    const data = await res.json();
    return data;
  } catch (error) {
    console.error("Failed to fetch:", error);
    throw error;  // หรือ return fallback data
  }
}
```

### Sequential vs Parallel

```javascript
// ❌ Sequential — 1 ต่อ 1 (ช้า!)
async function getEverything() {
  const user = await fetchUser(1);     // รอ 2 วิ
  const posts = await fetchPosts(1);   // รอ 1 วิ ← ไม่ต้องรอก็ได้!
  const tags = await fetchTags();      // รอ 1 วิ
  return { user, posts, tags };
  // Total: 4 วินาที
}

// ✅ Parallel — พร้อมกัน (เร็ว!)
async function getEverything() {
  const [user, posts, tags] = await Promise.all([
    fetchUser(1),     // ← เริ่มพร้อมกัน
    fetchPosts(1),    // ←
    fetchTags()       // ←
  ]);
  return { user, posts, tags };
  // Total: ~2 วินาที (นานที่สุดแค่ 2 วิ)
}
```

### Real RAG-Diary Example

```typescript
// server/src/index.ts — async ทุกขั้นตอน
app.post("/api/diary/search", async ({ body }) => {
  // 1. สร้าง embedding จากคำค้น (async — รอ OpenAI)
  const queryEmbedding = await generateEmbedding(body.query);

  // 2. ค้นหา vector ใน Supabase (async — รอ Database)
  const { data: entries } = await supabase
    .rpc("match_diary_entries", {
      query_embedding: queryEmbedding,
      match_threshold: 0.7,
      match_count: 10
    });

  // 3. ส่งเข้าหา LLM เพื่อ generate คำตอบ (async — รอ Gemini/OpenAI)
  const aiResponse = await generateResponse(body.query, entries);

  return { success: true, entries, response: aiResponse };
  // ถ้าไม่มี await — ทุกอย่าง return เป็น Promise ที่ยังไม่ resolve!
});
```

---

## Part 5: Event Loop — เบื้องหลังทั้งหมด

> **NotebookLM Analogy:** Think of the JavaScript main thread as a **single chef in a kitchen** [Source: NotebookLM]:
> - If the chef puts bread in the toaster, they don't just stand there staring at it. They set a timer and go chop vegetables.
> - When the toast pops, a mental "event" occurs, and the chef grabs the toast **when they are ready** (after finishing the current task).
>
> This is the **Event Loop** — it constantly checks if the main call stack is empty, and if it is, it picks up waiting tasks from the queue.
>
> **คำอธิบาย:** Event Loop = **พ่อครัวอัจฉริยะที่ทำงานแบบ non-blocking** — แทนที่จะยืนรอมันปิ้ง (เหมือน synchronous blocking) พ่อครัวก็ตั้งเวลา (setTimeout / fetch) แล้วไปสับผักต่อ พอมันปิ้งเสร็จ (event เกิดขึ้น) วางไว้ก่อน — พ่อครัวจะมาจัดการเมื่อจัดการงานปัจจุบันเสร็จแล้ว (เมื่อ call stack ว่าง) Event Loop คือ loop ที่คอยเช็คตลอดว่า "call stack ว่างรึยัง? มีงานรอใน queue มั้ย?" ถ้าว่างก็หยิบงานใน queue ขึ้นมา做

### Event Loop Components

```
Call Stack:     สิ่งที่กำลังรันตอนนี้ (LIFO)
Web APIs:       setTimeout, fetch, DOM events ( browser-provided )
Callback Queue: งานที่รอถูกเรียก (FIFO)
Microtask Queue: Promise .then/catch/await (priority สูงกว่า callback)
Event Loop:     ตรวจสอบ call stack ว่าง → ดึงจาก queue มารัน
```

### How setTimeout Works

```javascript
console.log("1");                        // 1. Call Stack

setTimeout(() => {                       // 2. Web API → ตั้ง timer
  console.log("3");                      // 4. Callback Queue → Event Loop → Call Stack
}, 0);                                   //    (0 ms → แต่ไม่รันทันที!)

console.log("2");                        // 3. Call Stack (รันทันที)
```

**Output:** `1`, `2`, `3` — ถึง setTimeout 0ms ก็ไม่รันทันที เพราะต้องรอ call stack ว่างก่อน!

### Microtasks vs Macrotasks

```javascript
console.log("1");                          // Call Stack

setTimeout(() => console.log("2"), 0);     // Macrotask (callback queue)

Promise.resolve().then(() => {             // Microtask (microtask queue)
  console.log("3");
});

console.log("4");                          // Call Stack

// Output: 1, 4, 3, 2
// ทำไม Promise ก่อน setTimeout?
```

**ลำดับการทำงานของ Event Loop:**
```
1. รัน Call Stack ทั้งหมด (1, 4)
2. รัน Microtask Queue ทั้งหมด (3) ← Promise.resolve().then()
3. รัน 1 Macrotask จาก Callback Queue (2) ← setTimeout
4. กลับไป step 2
```

> **Microtasks (Promise) มาก่อน Macrotasks (setTimeout) เสมอ**

### Async/Await Under the Hood

```javascript
async function example() {
  console.log("A");
  const result = await somePromise();
  console.log("C");
  return result;
}

// จริงๆ แล้ว async/await ≈
function example() {
  console.log("A");
  return somePromise().then((result) => {
    console.log("C");
    return result;
  });
}
```

เมื่อเจอ `await` — function หยุดทำงานชั่วคราว (yield control) → ส่วนที่เหลือกลายเป็น `.then()` callback → ไปเข้า microtask queue

### Visualizing Event Loop with RAG-Diary

```typescript
console.log("1: Start request");

// async function → เริ่ม แต่ยังไม่ await
app.post("/api/diary", async ({ body }) => {
  console.log("2: Inside handler (before await)");

  const embedding = await generateEmbedding(body.content);
  // ↑ await → function พัก → ไปทำอย่างอื่นต่อ

  console.log("3: Embedding received");
  const { data } = await supabase.from("diary_entries").insert({...});
  // ↑ await อีก → พักอีก

  console.log("4: Data saved");
  return { success: true, data };
});

console.log("5: After route definition");

// Output:
// 1: Start request
// 2: Inside handler (before await)
// 5: After route definition
// 3: Embedding received  (after OpenAI responds)
// 4: Data saved          (after Supabase responds)
```

---

## Part 6: Common Pitfalls

### 1. Forgetting await

```javascript
// ❌ ลืม await
async function getData() {
  const data = fetch("/api/data");  // ← Promise! ไม่ใช่ data!
  console.log(data);  // Promise { <pending> }
  return data;
}

// ✅ ต้อง await
async function getData() {
  const response = await fetch("/api/data");
  const data = await response.json();
  return data;
}
```

### 2. Forgetting async

```javascript
// ❌ ลืม async — return เป็น Promise ไม่ได้
function getData() {
  return await fetch("/api/data");  // Error! await นอก async
}

// ✅ async ครอบ
async function getData() {
  return await fetch("/api/data");
}
```

### 3. Sequential when you mean parallel

```javascript
// ❌ Sequential ไม่จำเป็น
const user = await fetchUser();
const posts = await fetchPosts();  // รอ user ก่อน — ทั้งที่ไม่ได้ใช้ user

// ✅ Parallel
const [user, posts] = await Promise.all([
  fetchUser(),
  fetchPosts()   // ไม่ต้องรอ user
]);
```

### 4. Not catching errors

```javascript
// ❌ ถ้า fetch fail → unhandled rejection (crash!)
app.post("/api/diary", async ({ body }) => {
  const embedding = await generateEmbedding(body.content);
  // what if generateEmbedding throws?
});

// ✅ try/catch ทุก async route handler
app.post("/api/diary", async ({ body }) => {
  try {
    const embedding = await generateEmbedding(body.content);
    const { data } = await supabase.from("diary_entries").insert({...});
    return { success: true, data };
  } catch (error) {
    console.error("Create entry failed:", error);
    return { success: false, error: error.message };
  }
});
```

---

## Question to Think About

1. **Event Loop:** ถ้ามี `setTimeout(..., 0)` กับ `Promise.resolve().then(...)` อย่างไหนรันก่อน?
2. **Parallel:** `Promise.all` ใช้ request 3 อันพร้อมกัน — ถ้า 1 อัน fail จะเกิดอะไรกับอีก 2 อัน? (คำใบ้: fail-fast)
3. **Microtask:** ถ้า microtask สร้าง microtask ใหม่ (recursive Promise) — Event Loop จะเป็นยังไง?
4. **RAG-Diary:** Route handler ใน `index.ts` — ถ้าไม่ใส่ `await` หน้า `supabase.insert(...)` — จะเกิดอะไรขึ้นกับ response? (ลองหาในโค้ดจริงดู)
5. **Error Handling:** `try/catch` ใน async function จับ error จาก `Promise.reject()` ได้ไหม?

## Links

- Related concept: [[RAG Flow]], [[TypeScript Basics]], [[REST API JSON]]
- Code ref: [[server/src/index.ts]] (async routes), [[server/src/lib/generate-embedding.ts]]
- Source: Alex Xu — System Design Interview (NotebookLM), Traversy Media
