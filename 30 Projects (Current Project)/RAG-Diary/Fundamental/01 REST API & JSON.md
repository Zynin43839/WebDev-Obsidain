---
type: concept
topic: api
status: growing
created: 2026-06-30
---

# REST API + JSON

## What is this?

REST API (Representational State Transfer) = **แนวทางการออกแบบ web service** ที่ใช้ HTTP protocol เป็นพื้นฐาน — มองทุกอย่างเป็น **resource** (user, diary, tag) ที่เข้าถึงผ่าน URL และจัดการด้วย HTTP methods (GET/POST/PUT/DELETE)

JSON (JavaScript Object Notation) = **รูปแบบข้อมูล** ที่ REST API ส่วนใหญ่ใช้ส่งข้อมูลระหว่าง frontend กับ backend — เบากว่า XML, อ่านง่าย, ทุกภาษารองรับ

## Why This Matters

RAG-Diary ทั้ง frontend และ backend คุยกันผ่าน REST API + JSON:
- `POST /api/diary` — สร้าง entry (JSON body)
- `GET /api/diary` — list entries (JSON response)
- `POST /api/diary/search` — semantic search (JSON body → JSON response)
- `POST /api/chat` — chat กับ AI (JSON body → JSON stream)

การเข้าใจ REST API = **ภาษากลางที่ frontend และ backend ใช้สื่อสารกัน** ถ้าไม่เข้าใจ จะงง:
- Route แต่ละอัน map กับ action อะไร
- JSON ที่ส่งไป-มากลับมีโครงสร้างยังไง
- Status codes แต่ละตัวหมายถึงอะไร
- ทำไมบาง endpoint ต้องส่ง JWT token
- Error handling ฝั่ง frontend ต้องรับมือยังไง

---

## Part 1: REST Principles (Richardson Maturity Model)

> **NotebookLM Analogy:** REST is like a **standardized language of verbs and nouns for web communication** [Source: NotebookLM].
>
> Think of REST as a **restaurant menu**: the menu (API documentation) lists available dishes (resources) like "Pad Thai" (/dishes/pad-thai) and "Tom Yum" (/dishes/tom-yum). The waiter serves you via standard actions: "I'd like to order (POST) a new dish", "Please show me (GET) the menu", "I want to change my order (PUT)", "Cancel my order (DELETE)". Everyone understands these actions because they're standard. Instead of creating custom actions like "createDish" or "deleteDish", REST uses the same actions for every resource.
>
> **คำอธิบาย:** REST เปรียบเหมือน **ภาษาเมนูอาหารมาตรฐาน** — ไม่ว่าเข้าร้านไหน (API ไหน) ถ้าใช้ REST จะรู้ว่าสั่ง (POST), ดู (GET), เปลี่ยน (PUT), ยกเลิก (DELETE) ได้เหมือนกันหมด ไม่ต้องเรียนวิธีเฉพาะของแต่ละร้าน ทำให้ frontend และ backend คุยกันรู้เรื่องโดยไม่ต้องมี "คู่มือพิเศษ"

Leonard Richardson แบ่ง REST API เป็น 4 ระดับ:

```
Level 0: POX (Plain Old XML) — ใช้ endpoint เดียว + ใส่ action ใน body
Level 1: Resources — แยก endpoint ตาม resource
Level 2: HTTP Verbs — ใช้ GET/POST/PUT/DELETE ตามความหมาย
Level 3: HATEOAS — response บอก client ว่าทำอะไรต่อได้บ้าง
```

### Level 0 — Swamp of POX

```http
POST /api
Content-Type: application/xml

<action>createEntry</action>
<content>Hello</content>
```

ถ้าใช้ POST อย่างเดียวทุกอย่าง + ใส่ action ใน body → Level 0

### Level 1 — Resources

```http
POST /api/createEntry     ← action in URL
POST /api/editEntry       ← action in URL
POST /api/deleteEntry     ← action in URL
```

ดีขึ้นหน่อย — แต่ยังใช้ POST อย่างเดียว

### Level 2 — HTTP Verbs ✅ (RAG-Diary อยู่ที่นี่)

```http
GET    /api/diary        ← list entries
POST   /api/diary        ← create entry
PUT    /api/diary/:id    ← update entry
DELETE /api/diary/:id    ← delete entry
```

ใช้ HTTP methods ตามความหมาย — **นี่คือ REST API ที่ใช้กันทั่วไป**

### Level 3 — HATEOAS

**HATEOAS** = Hypermedia As The Engine Of Application State — response มี **links** บอก client ว่าทำอะไรต่อ:

```json
{
  "id": "abc-123",
  "content": "Hello",
  "_links": {
    "self": { "href": "/api/diary/abc-123" },
    "update": { "href": "/api/diary/abc-123", "method": "PUT" },
    "delete": { "href": "/api/diary/abc-123", "method": "DELETE" },
    "comments": { "href": "/api/diary/abc-123/comments" }
  }
}
```

> **HATEOAS** — client ไม่ต้อง hardcode URL structure, server บอกให้หมด → flexible กว่า แต่ยุ่งยาก在实际使用

---

## Part 2: REST Constraints (6 ข้อของ REST)

Roy Fielding (คนคิด REST) กำหนด 6 constraints:

### 1. Client-Server

แยก client (UI) ออกจาก server (data storage)

```
✅ Client เปลี่ยนจาก React เป็น mobile app → server ไม่ต้องเปลี่ยน
✅ Server เปลี่ยนจาก Elysia เป็น Fastify → client ไม่ต้องเปลี่ยน
```

### 2. Stateless (ไม่มี state)

ทุก request มีข้อมูลครบ — server ไม่รู้จัก client

```typescript
// ✅ REST: client ส่ง token ทุกครั้ง
app.get("/api/diary", async ({ headers }) => {
  const user = verifyToken(headers.authorization);
  // user = { id, role, ... } — มาจาก token ไม่ใช่ session
});

// ❌ NOT REST: server จำ session
app.get("/api/diary", async ({ session }) => {
  // session.user — server จำว่า client นี้ login แล้ว
});
```

**ข้อดีของ Stateless:**
- Scale แนวนอน (เพิ่ม server ได้) — server A กับ B ไม่ต้อง sync session
- Debug ง่าย — request แต่ละอัน independent
- Cache ได้ — client request เดิม → cached response

**ข้อเสีย:** request ใหญ่ขึ้น (ส่ง credentials ทุกครั้ง)

### 3. Cacheable

Response ต้องบอกว่า cache ได้หรือไม่ — ใช้ `Cache-Control`, `ETag`:

```http
HTTP/1.1 200 OK
Cache-Control: public, max-age=3600
ETag: "abc123"

GET /api/diary (อีก 5 นาที)
If-None-Match: "abc123"

HTTP/1.1 304 Not Modified (body ว่าง — browser ใช้ cache เก่า)
```

### 4. Uniform Interface

ทุก resource ใช้ interface เดียวกัน:
- Resource ถูก identify ด้วย URL
- Manipulate resource ผ่าน HTTP methods
- Response มี format มาตรฐาน (JSON)

### 5. Layered System

Client ไม่รู้ว่า request ผ่านอะไรมาบ้าง:

```
[Browser] ──→ [CDN] ──→ [Load Balancer] ──→ [API Server] ──→ [DB]
```

Client เห็นแค่ CDN — ไม่รู้ว่าข้างในมี load balancer หรือ database

### 6. Code on Demand (optional)

Server ส่ง code (JavaScript) ให้ client รัน — เช่น `<script>` tag

---

## Part 3: URL Design — RESTful Conventions

### 3.1 Naming Conventions

```
✅ Plurals:    /api/diary
✅ Plurals:    /api/users
✅ Plurals:    /api/tags
❌ Singular:   /api/diary_entry
❌ Verbs:      /api/getDiaryEntries
❌ Action URLs: /api/createEntry
```

### 3.2 Resources vs Actions

**Resources (ควรใช้ REST):**
```
GET    /api/diary            → list entries
POST   /api/diary            → create entry
GET    /api/diary/:id        → get one entry
PUT    /api/diary/:id        → update entry
DELETE /api/diary/:id        → delete entry
```

**Actions (ไม่ใช่ resource — ใช้ POST):**
```
POST   /api/diary/search     → search entries (action)
POST   /api/chat             → chat with AI (action)
POST   /api/auth/login       → login (action)
```

Search เป็น action — เพราะ filter parameter ซับซ้อนเกิน GET query string

### 3.3 Nested Resources

```http
GET    /api/diary/:id/comments        ← comments ของ entry นี้
POST   /api/diary/:id/comments        ← สร้าง comment ใน entry นี้
DELETE /api/diary/:id/comments/:cid   ← ลบ comment นั้น

GET    /api/diary/:id/tags            ← tags ของ entry นี้
```

**ความลึก:** ไม่ควรเกิน 2-3 level → `/api/diary/:id/comments/:cid/replies` เริ่มลึกเกิน

### 3.4 Filtering, Sorting, Pagination

**Filtering:**
```http
GET /api/diary?tag=Learning           ← filter by tag
GET /api/diary?created_after=2026-01-01  ← filter by date
```

**Sorting:**
```http
GET /api/diary?sort=created_at        ← ascending
GET /api/diary?sort=-created_at       ← descending
GET /api/diary?sort=tag,created_at    ← multiple
```

**Pagination:**
```http
# Offset-based
GET /api/diary?offset=0&limit=10      ← page 1, 10 items
GET /api/diary?offset=10&limit=10     ← page 2, 10 items

# Cursor-based (recommended for large data)
GET /api/diary?cursor=abc-123&limit=10  ← items ต่อจาก id abc-123
```

**ข้อดีของ Cursor pagination:**
- เร็ว (ใช้ index บน PK)
- ไม่พลิก (ถ้า insert กลาง list — offset จะซ้ำซ้อน)
- ใช้กับ infinite scroll ได้ดี

### 3.5 Field Selection

```http
GET /api/diary?fields=id,content,tag
→ [{ "id": "...", "content": "...", "tag": "..." }]

GET /api/diary?fields=id
→ [{ "id": "..." }]
```

ลด bandwidth — โดยเฉพาะ JSON ใหญ่ๆ

---

## Part 4: API Versioning

### 4.1 URL-based

```http
GET /api/v1/diary
GET /api/v2/diary
```

### 4.2 Header-based

```http
GET /api/diary
Accept: application/vnd.rag-diary.v1+json
```

### 4.3 Query-based

```http
GET /api/diary?version=1
```

**Recommendation:** ใช้ URL-based versioning — ชัดเจน, cache ได้, debug ง่าย

---

## Part 5: Error Handling Design

### 5.1 Standard Error Response

```json
// ❌ Bad — ไม่มี structure
{ "error": "something broke" }

// ✅ Good — มี structure
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "content field is required",
    "details": {
      "field": "content",
      "value": null,
      "constraint": "required"
    }
  }
}
```

### 5.2 RAG-Diary Error Format (Proposed)

```json
// 400 Bad Request
{
  "success": false,
  "error": {
    "code": "BAD_REQUEST",
    "message": "Invalid request body",
    "details": [
      { "field": "content", "reason": "required" }
    ]
  }
}

// 401 Unauthorized
{
  "success": false,
  "error": {
    "code": "UNAUTHORIZED",
    "message": "Invalid or expired token"
  }
}

// 404 Not Found
{
  "success": false,
  "error": {
    "code": "NOT_FOUND",
    "message": "Entry with id abc-123 not found"
  }
}

// 500 Server Error
{
  "success": false,
  "error": {
    "code": "INTERNAL_ERROR",
    "message": "An unexpected error occurred"
    // ⚠️ ไม่ส่ง stack trace ใน production!
  }
}
```

### 5.3 HTTP Methods → Error Logic

| Action | Success | Client Error | Server Error |
|--------|---------|-------------|-------------|
| GET /diary | 200 + array | 404 (no entries) | 500 |
| GET /diary/:id | 200 + object | 404 (not found) | 500 |
| POST /diary | 201 + object | 400 (validation) | 500 |
| PUT /diary/:id | 200 + object | 400 / 404 | 500 |
| DELETE /diary/:id | 204 (no body) | 404 | 500 |

---

## Part 6: Authentication Methods

### 6.1 Basic Auth (ไม่ควรใช้)

```http
GET /api/diary
Authorization: Basic base64(username:password)
```

❌ ไม่ปลอดภัย — ส่ง username:password ทุก request

### 6.2 API Key (适合 server-to-server)

```http
GET /api/diary
X-API-Key: rag-diary-secret-abc123
```

✅ ง่าย, ใช้กับ internal services
❌ ไม่เหมาะกับ client-side (key leak)

### 6.3 Bearer Token (JWT) ✅ — มาตรฐาน REST

```http
POST /api/login {"email":"a@b.com","password":"..."}
→ 200 OK { "token": "eyJhbGciOiJIUzI1NiIs..." }

GET /api/diary
Authorization: Bearer eyJhbGciOiJIUzI1NiIs...
```

### 6.4 JWT Structure

> **NotebookLM Analogy:** A JWT is like a **secure, tamper-proof ID badge issued by a trusted security desk** [Source: NotebookLM].
> - Instead of every guard in a building constantly calling the main security desk to verify your identity, they just look at your badge.
> - They know the badge is valid because it has an official, unbreakable holographic seal (the **signature**).
> - If anyone tries to alter the name or permissions on the badge (the **payload**), the seal breaks, and the guards will reject it.
>
> **Why JWT is stateless:** The server doesn't need to store session info in a database. All the information is **inside the token itself**. Any server in the cluster can verify the token locally by checking the signature — no shared session store needed. [Source: NotebookLM]
>
> **คำอธิบาย:** JWT = **บัตรพนักงานที่เซ็นลายเซ็นปลอมไม่ได้** — แทนที่ server แต่ละตัวต้องโทรถาม "คนนี้ login รึยัง" กับ database ส่วนกลางตลอดเวลา server ก็แค่ดูบัตร (JWT) ที่ client ส่งมา ถ้าลายเซ็น (signature) ยังเรียบร้อยก็เชื่อถือได้ เพราะข้อมูลทุกอย่างอยู่ใน token นี้แล้ว (stateless) ถ้าใครแก้ข้อมูลในบัตร (payload) — ลายเซ็นจะขาดและ server จะปฏิเสธทันที

```
JWT = Header.Payload.Signature

Header (base64):
  { "alg": "HS256", "typ": "JWT" }

Payload (base64):
  {
    "sub": "user-123",
    "role": "admin",
    "iat": 1719782400,
    "exp": 1719786000,
    "iss": "rag-diary"
  }

Signature:
  HMACSHA256(base64(Header) + "." + base64(Payload), secret)

→ eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIn0.dozjgNryP4J3jVmNHl0w5N_XgL0n3I9PlFUP0THsR8
```

**Important Claims:**
| Claim | Meaning | ใช้ทำอะไร |
|-------|---------|----------|
| `sub` | subject — user ID | ระบุ user |
| `exp` | expiration timestamp | token หมดอายุ |
| `iat` | issued at | token สร้างเมื่อไหร่ |
| `iss` | issuer | ใครสร้าง token |
| `role` | custom — user role | admin, user, etc. |

### 6.5 OAuth2 — Third-party Login

login with Google/GitHub:

```
1. [Client] → "Login with Google"
2. [Google] → "อนุญาตไหม?" → User clicks ✅
3. [Google] → Authorization code → [RAG-Diary Backend]
4. [Backend] → Exchange code → Access Token
5. [Backend] → Fetch user info → Create JWT → [Client]
6. [Client] → ใช้ JWT สำหรับ REST API
```

### 6.6 Token Storage

| Storage | XSS Safe? | CSRF Safe? | Persistent? |
|---------|-----------|------------|-------------|
| localStorage | ❌ (JS อ่านได้) | ✅ | ✅ |
| sessionStorage | ❌ (JS อ่านได้) | ✅ | ❌ (ปิด tab = ตาย) |
| Cookie (HttpOnly) | ✅ | ❌ (ต้อง SameSite) | ✅ |

**Recommendation:** access token → Cookie (HttpOnly, Secure, SameSite=Lax), refresh token → Cookie with path=/auth

---

## Part 7: JSON — Deep Dive

### 7.1 JSON Grammar

```
JSON        → Value
Value       → String | Number | Boolean | Null | Object | Array
String      → " chars "
Number      → -? [0-9]+ (\.[0-9]+)? ([eE][+-]? [0-9]+)?
Boolean     → true | false
Null        → null
Object      → { Pair ("," Pair)* }
Pair        → String ":" Value
Array       → "[" Value ("," Value)* "]"
```

### 7.2 JSON vs XML — ทำไม JSON ชนะ

```json
// JSON — 256 chars
{
  "entries": [
    {
      "id": "abc-123",
      "content": "Hello",
      "tags": ["learning", "dev"],
      "created_at": "2026-06-30T17:00:00Z"
    }
  ]
}
```

```xml
<!-- XML — 412 chars (+61% ใหญ่กว่า) -->
<?xml version="1.0" encoding="UTF-8"?>
<entries>
  <entry>
    <id>abc-123</id>
    <content>Hello</content>
    <tags>
      <tag>learning</tag>
      <tag>dev</tag>
    </tags>
    <created_at>2026-06-30T17:00:00Z</created_at>
  </entry>
</entries>
```

> **NotebookLM เรียบเรียงให้เข้าใจง่ายขึ้น** [Source: NotebookLM]:
> - **Simplicity:** JSON is far less verbose than XML. No bulky opening/closing tags for every piece of data.
> - **Native browser support:** JSON is a subset of JavaScript — `JSON.parse()` works out of the box.
> - **Direct mapping:** JSON maps perfectly to native objects and arrays in almost all programming languages.
> - **Smaller size:** Less bandwidth, faster parsing.
>
> **คำอธิบาย:** JSON ชนะ XML เพราะ **สั้นกว่า (ไม่มี opening/closing tags), browser รองรับตรงตัว (JSON.parse() ไม่ต้องลงไลบรารีเพิ่ม), map กับ object ในภาษาโปรแกรมต่างๆ ได้ตรง (ไม่มี type casting เยอะ), และไฟล์เล็กกว่า → ส่งข้อมูลน้อยลง → โหลดเร็วขึ้น**

**ทำไม JSON ชนะ XML ใน REST API:**
1. **เบากว่า** — 61% เล็กกว่า
2. **JavaScript native** — `JSON.parse()` / `JSON.stringify()` — ไม่ต้องมี parser library
3. **Object mapping ตรงตัว** — JSON object → JavaScript object
4. **Array native** — XML ไม่มี array concept (ต้อง wrapper)
5. **อ่านง่าย** — โดยเฉพาะ JSON ซ้อนกัน

### 7.3 JSON Data Types — In Detail

> **NotebookLM Analogies for Each JSON Type** [Source: NotebookLM]:
>
> | Type | Analogy | ตัวอย่าง |
> |------|---------|---------|
> | **Object** | **ตู้เอกสารติดป้าย** — เก็บ key-value pairs, key ต้องเป็น string | `{"name":"John","age":30}` |
> | **Array** | **รายการช้อปปิ้ง** — ลำดับมีหมายเลข (index) | `["apples","oranges"]` |
> | **String** | **ข้อความบนกระดาษ** — ต้องมี double quotes เสมอ | `"Hello"` |
> | **Number** | **จอเครื่องคิดเลขดิจิตอล** — รวม int/decimal ไม่แยกชนิด | `42`, `3.14` |
> | **Boolean** | **สวิตช์เปิด-ปิดไฟ** — มีแค่ on (`true`) / off (`false`) | `true` |
> | **Null** | **กล่องเปล่าที่ตั้งใจวางไว้** — "ตั้งใจว่าไม่มีค่า" | `null` |
>
> **ข้อแตกต่างระหว่าง `null` และ `undefined` ใน JSON:**
> - `null` = valid JSON — แทน "ตั้งใจว่าไม่มีค่า" (กล่องเปล่า)
> - `undefined` = **ไม่มีใน JSON** — ถ้า `JSON.stringify()` เจอ `undefined` → **ข้าม field นั้นไปเลย**
>
> ```javascript
> const obj = { a: 1, b: undefined, c: null };
> JSON.stringify(obj);
> // → '{"a":1,"c":null}'   ← b หายไป!
> ```

#### String
```json
{ "name": "John", "emoji": "🔥" }
```
- Unicode (UTF-8) — emoji, ไทย, จีน ได้
- Escape sequences: `\"`, `\\`, `\/`, `\b`, `\f`, `\n`, `\r`, `\t`, `\uXXXX`

#### Number
```json
{ "age": 25, "pi": 3.14159, "negative": -42, "sci": 1.5e10 }
```
- **No distinction** between int/float — ทุกภาษา parse ต่างกัน
- JSON **ไม่มี NaN, Infinity** — ต้องแปลงเป็น string/null

#### Boolean
```json
{ "is_active": true, "is_deleted": false }
```
- lowercase only: `true`, `false`

#### Null
```json
{ "middle_name": null }
```
- `null` = intentional absence — ต่างจาก `undefined` (ไม่มี field)

#### Object
```json
{
  "user": {
    "id": "abc",
    "profile": {
      "name": "John",
      "address": {
        "city": "Bangkok"
      }
    }
  }
}
```

#### Array
```json
{
  "tags": ["dev", "learning"],
  "matrix": [[1, 2], [3, 4]],
  "mixed": [1, "two", true, null, { "key": "value" }]
}
```

### 7.4 What JSON Doesn't Have

| Concept | JSON | วิธีแก้ |
|---------|------|--------|
| Date | ❌ | ISO 8601 string: `"2026-06-30T17:00:00Z"` |
| Undefined | ❌ | ไม่มี field นั้น หรือ null |
| Infinity / NaN | ❌ | null หรือ string |
| RegExp | ❌ | string: `"/pattern/flags"` |
| Map / Set | ❌ | object / array |
| Circular refs | ❌ | ไม่สามารถ serialize |
| Comments | ❌ | ต้องลบ comment ก่อน parse |

### 7.5 JSON Schema (Validation)

คุณสามารถ validate JSON structure ด้วย JSON Schema:

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "required": ["content"],
  "properties": {
    "content": {
      "type": "string",
      "minLength": 1,
      "maxLength": 10000
    },
    "tag": {
      "type": "string",
      "enum": ["Learning", "Personal", "Todo", "General"]
    }
  }
}
```

ใน RAG-Diary — Elysia validation middleware ทำหน้าที่นี้ (Zod schema)

### 7.6 JSON Patch vs JSON Merge Patch

**PUT (replace ทั้ง resource):**
```http
PUT /api/diary/abc-123
{ "content": "Hello", "tag": "Learning" }
// ต้องส่งทุก field — ถ้าส่งแค่ "tag" → content หาย!
```

**PATCH — JSON Merge Patch (RFC 7386):**
```http
PATCH /api/diary/abc-123
{ "tag": "Updated" }
// content ยังเหมือนเดิม!
```

**PATCH — JSON Patch (RFC 6902):**
```http
PATCH /api/diary/abc-123
Content-Type: application/json-patch+json

[
  { "op": "replace", "path": "/tag", "value": "Updated" },
  { "op": "add", "path": "/tags/-", "value": "new-tag" }
]
```

JSON Patch มี operation: `add`, `remove`, `replace`, `move`, `copy`, `test`

---

## Part 8: REST vs Alternatives

### REST vs GraphQL

| Aspect | REST | GraphQL |
|--------|------|---------|
| Endpoints | หลาย endpoint (diary, users, tags) | 1 endpoint (`POST /graphql`) |
| Data Fetching | server กำหนด structure | client กำหนด field ที่ต้องการ |
| Over-fetching | ได้ข้อมูลเกินจำเป็น | ✅ ได้เท่าที่ขอ |
| Under-fetching | ต้อง request หลายครั้ง | ✅ 1 request ขอหลาย resource |
| Caching | ✅ HTTP cache ทำงานดี | ❌ URL เดียว — cache ยาก |
| Learning Curve | ต่ำ | สูง (ต้องเข้าใจ schema + resolver) |
| File Upload | native HTTP | ต้องยุ่งยาก |
| Real-time | WebSocket แยก | Subscription built-in |

**เมื่อไหร่ใช้ REST:** CRUD API, public API, microservices (RAG-Diary = REST ✅)
**เมื่อไหร่ใช้ GraphQL:** Complex UIs ที่ต้องการ data flexibility, dashboard

### REST vs gRPC

| Aspect | REST (JSON) | gRPC (Protobuf) |
|--------|-------------|-----------------|
| Data Format | JSON (text, readable) | Protobuf (binary, unreadable) |
| Size | Bigger | 10x smaller |
| Speed | Slower | Faster |
| Browser | Native | ต้อง gRPC-web + proxy |
| Code Gen | Manual | Auto-generated client/server |
| Streaming | WebSocket | Native bidirectional streaming |

**เมื่อไหร่ใช้ gRPC:** Internal microservices, high-performance systems, streaming

---

## Part 9: Rate Limiting & Throttling

### 9.1 Why Rate Limit

ป้องกัน abuse — brute force, DDoS, หรือ client ที่ request มากเกินไป

### 9.2 Rate Limit Headers

```http
HTTP/1.1 200 OK
X-RateLimit-Limit: 100         ← limit 100 requests
X-RateLimit-Remaining: 87      ← เหลือ 87
X-RateLimit-Reset: 1719786000  ← reset timestamp

HTTP/1.1 429 Too Many Requests
Retry-After: 60                 ← รอ 60 วินาที
```

### 9.3 Rate Limit Strategies

- **Token Bucket** — N requests/minute, refill rate
- **Sliding Window** — count requests in last N seconds
- **Fixed Window** — count requests per minute (อาจ spike ขอบ window)

---

## Part 10: RAG-Diary API Design — Complete

### 10.1 Current Routes

```http
### Diary CRUD
GET    /api/diary              → list entries
POST   /api/diary              → create entry
PUT    /api/diary/:id          → update entry
DELETE /api/diary/:id          → delete entry

### Search
POST   /api/diary/search       → semantic search (RAG)

### Chat
POST   /api/chat               → chat with AI
GET    /api/chat/history       → chat history

### Auth (proposed)
POST   /api/auth/register      → register
POST   /api/auth/login         → login
POST   /api/auth/refresh       → refresh token
POST   /api/auth/logout        → invalidate token
```

### 10.2 JSON Payloads

```json
// POST /api/diary — Request
{
  "content": "Today I learned REST API",
  "tag": "Learning"
}

// Response (201 Created)
{
  "success": true,
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "content": "Today I learned REST API",
    "tag": "Learning",
    "created_at": "2026-06-30T17:00:00.000Z"
  }
}

// Response (400 Bad Request)
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "content is required"
  }
}
```

### 10.3 Full HTTP Request/Response

**Request:**
```http
POST /api/diary HTTP/1.1
Host: localhost:3000
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...

{"content": "Hello REST!", "tag": "Learning"}
```

**Response:**
```http
HTTP/1.1 201 Created
Content-Type: application/json
X-Request-Id: req-abc-123
X-RateLimit-Remaining: 99

{"success":true,"data":{"id":"abc-123","content":"Hello REST!","tag":"Learning","created_at":"2026-06-30T17:00:00.000Z"}}
```

---

## Question to Think About

1. API versioning: สมมติ `GET /api/v1/diary` ส่ง array แบนๆ แต่ `GET /api/v2/diary` ส่ง `{ entries: [...] }` — client เก่าที่เรียก v1 อยู่จะพังไหม?
2. Idempotency: ทำไม PATCH ถึงไม่ idempotent? (คำใบ้: `PATCH /diary/:id { "tag": "Dev" }` — ทำ 2 ครั้ง ได้ผลเหมือนเดิมไหม?)
3. HATEOAS: ถ้า API response มี `_links` — client จะไม่ต้อง hardcode URL path ได้จริงไหม? ข้อเสียคืออะไร?
4. JWT: ถ้า JWT leak (hacker ได้ token) — เราจะ revoke token นั้นยังไง? (คำใบ้: JWT stateless — server ไม่รู้จัก token)
5. GraphQL vs REST: social media feed ที่รวม posts, comments, likes, followers (N+1 requests ใน REST) — GraphQL แก้ยังไง?
6. JSON: `JSON.parse('{"a": 1,}')` — ทำงานได้ไหม? (ลองรันดู)

## Links

- Related concept: [[Internet Basics DNS TCP HTTP]], [[Database & SQL]], [[RAG Flow]]
- Code ref: [[server/src/index.ts]], [[frontend/src/lib/api.ts]]
- Source: Alex Xu — System Design Interview (NotebookLM), Traversy Media
- RFCs: RFC 7231 (HTTP), RFC 7159 (JSON), RFC 7519 (JWT), RFC 6902 (JSON Patch)
