---
type: concept
topic: database
status: seedling
created: 2026-06-30
---

# Database Design & SQL Deep Dive

## What is this?

Database design คือ **กระบวนการออกแบบโครงสร้างข้อมูล** ก่อนลงมือเขียนโค้ด — ตัดสินใจว่าต้องเก็บอะไร, ความสัมพันธ์ระหว่างข้อมูลเป็นยังไง, แล้วแปลงเป็น SQL tables ดีไซน์ดี → app เร็ว, ง่ายต่อการพัฒนา, แก้ไขทีหลังไม่ปวดหัว

## Why This Matters

โค้ดเปลี่ยนได้ทุก sprint แต่ **schema เปลี่ยนยาก** — ถ้าออกแบบ database ไม่ดีตั้งแต่แรก:
- ต้องเขียนโค้ด workaround เยอะ
- query ช้าเพราะ table design ไม่เหมาะ
- migrate ข้อมูลทีหลังเสียเวลาหนัก
- RAG-Diary มีแค่ 1 table (`diary_entries`) — แต่พอมี feature อย่าง users, comments, tags ต้องออกแบบ relationship ให้ดี

---

## Part 1: Principles of Database Design

### 1.1 Entities = ตาราง

ก่อนเขียน SQL — ถามว่า **"ระบบนี้มีอะไรบ้าง?"** แต่ละสิ่ง = 1 table

```
RAG-Diary:
  ├── diary_entries     ← entries
  ├── users             ← ผู้ใช้ (ยังไม่มี)
  ├── tags              ← ป้ายกำกับ
  ├── comments          ← ความคิดเห็น
  └── favorites         ← รายการโปรด
```

### 1.2 Relationships = ความสัมพันธ์

3 ประเภทหลัก:

#### 🔗 One-to-One (1:1)
1 user ↔ 1 profile
```
users ──────── user_profiles
  1  ────────   1
```
```sql
CREATE TABLE users (
  id uuid primary key,
  email text unique
);

CREATE TABLE user_profiles (
  id uuid primary key references users(id),  -- PK = FK = 1:1
  display_name text,
  avatar_url text
);
```

#### 🔗 One-to-Many (1:N) ← ที่พบบ่อยที่สุด
1 user → หลาย diary entries
```
users ──────── diary_entries
  1  ────────   N
```
```sql
CREATE TABLE users (
  id uuid primary key,
  email text
);

CREATE TABLE diary_entries (
  id uuid primary key,
  user_id uuid references users(id),  -- ← FK ไป users
  content text
);
```

#### 🔗 Many-to-Many (M:N)
1 entry → หลาย tags, 1 tag → หลาย entries
```
diary_entries ──── entry_tags ──── tags
      M          N         M          1
                 (junction table)
```
```sql
CREATE TABLE tags (
  id uuid primary key,
  name text unique
);

CREATE TABLE entry_tags (
  entry_id uuid references diary_entries(id),
  tag_id   uuid references tags(id),
  primary key (entry_id, tag_id)  -- composite PK
);
```

---

### 1.3 Normalization (ทำไมต้องแยกตาราง)

#### ❌ Unnormalized (Bad)
```sql
-- 1 ตารางเดียวเก็บทุกอย่าง
CREATE TABLE entries_bad (
  id uuid primary key,
  content text,
  tags text  -- "learning,personal,todo" ← string ค่าเดียว!
);
```
ปัญหา: ค้นหา tag ยาก, แก้ไข tag ต้อง parse string, เพิ่ม tag ใหม่ต้อง update ทุก row

#### ✅ 1NF (First Normal Form)
แต่ละ cell มีค่า atomic (แยก tags เป็นหลาย rows)
```sql
-- solution: junction table
```

#### ✅ 2NF (Second Normal Form)
ทุก non-key column ขึ้นกับ PK ทั้งหมด (ไม่ใช่แค่บางส่วน)

#### ✅ 3NF (Third Normal Form)
ทุก column ขึ้นกับ PK โดยตรง ไม่ transitively

**Practical advice:** ปกติแค่ 3NF ก็พอแล้ว — 4NF, 5NF, 6NF เกินจำเป็น

---

### 1.4 Denormalization (เมื่อไหร่ควร "ไม่ทำตามกฏ")

**Denormalize เมื่อ:** performance สำคัญกว่า data integrity

```sql
-- Normalized: ต้อง JOIN เพื่อเอา username
SELECT e.content, u.display_name
FROM diary_entries e
JOIN users u ON e.user_id = u.id;

-- Denormalized: เก็บ username ซ้ำใน entries (เร็วขึ้น แต่ data ซ้ำซ้อน)
CREATE TABLE diary_entries (
  id uuid primary key,
  content text,
  user_id uuid,
  user_name text  -- ← denormalized! ถ้า user เปลี่ยนชื่อ ต้อง update ทุก entry
);
```

**กฏ:** Denormalize เฉพาะเมื่อ:
1. JOIN ช้าจริง (วัดก่อน!)
2. ข้อมูลไม่ค่อยเปลี่ยน (เช่น created_at)
3. อ่านเยอะกว่าเขียนมาก

---

## Part 2: Design Process Step-by-Step

สมมติเรา **ออกแบบ RAG-Diary ใหม่ตั้งแต่ต้น** ที่มี features:
- users (login)
- diary entries (content + tag)
- comments (reply on entries)
- favorites (save entries)
- tags (many-to-many)

### Step 1: Identify Entities

```
Users ──────── เขียน ──────── Diary Entries
                                    │
                               ┌────┴────┐
                           Comments   Tags
                                         │
                               Favorites │
```

### Step 2: Define Relationships

```
Users 1 ──── N Diary Entries    (one user → many entries)
Users 1 ──── N Comments         (one user → many comments)
Diary Entries 1 ──── N Comments (one entry → many comments)
Diary Entries M ──── N Tags     (M:N → junction table entry_tags)
Users M ──── N Diary Entries    (favorites: M:N → junction table favorites)
```

### Step 3: Draw ER Diagram (Entity-Relationship)

```
┌───────────┐     ┌──────────────────┐     ┌───────────┐
│   users   │     │  diary_entries   │     │   tags    │
├───────────┤     ├──────────────────┤     ├───────────┤
│ id (PK)   │←──┐ │ id (PK)          │     │ id (PK)   │
│ email     │    │ │ user_id (FK)─────┘     │ name      │
│ password  │    │ │ content          │     └───────────┘
│ name      │    │ │ created_at       │          │
└───────────┘    │ │ embedding        │          │
                 │ └──────────────────┘          │
                 │      │          │             │
                 │      │          │    ┌────────────────┐
                 │      │          └────│  entry_tags    │
                 │      │               ├────────────────┤
                 │      │               │ entry_id (FK)──┤
                 │      │               │ tag_id (FK)──┘
                 │      │               └────────────────┘
           ┌─────┘      │
           │            │
    ┌───────────┐  ┌───────────┐
    │ comments  │  │ favorites │
    ├───────────┤  ├───────────┤
    │ id (PK)   │  │ user_id───┤
    │ entry_id──┤  │ entry_id──┤
    │ user_id───┘  └───────────┘
    │ content     (composite PK:
    │ created_at   user_id + entry_id)
    └───────────┘
```

### Step 4: Write SQL

```sql
-- ========================================
-- RAG-Diary: Full Schema Design
-- ========================================

-- 1. Users
CREATE TABLE users (
  id         uuid primary key default gen_random_uuid(),
  email      text not null unique,
  password   text not null,       -- hashed, not plaintext!
  name       text default '',
  created_at timestamptz default now()
);

-- 2. Diary Entries
CREATE TABLE diary_entries (
  id         uuid primary key default gen_random_uuid(),
  user_id    uuid not null references users(id) on delete cascade,
  content    text not null,
  created_at timestamptz default now(),
  embedding  vector(1536)
);

-- Index สำหรับค้นหา
CREATE INDEX idx_entries_user ON diary_entries(user_id);
CREATE INDEX idx_entries_created ON diary_entries(created_at desc);
CREATE INDEX ON diary_entries USING hnsw (embedding vector_cosine_ops);

-- 3. Tags
CREATE TABLE tags (
  id   uuid primary key default gen_random_uuid(),
  name text not null unique
);

-- 4. Entry-Tags (M:N junction)
CREATE TABLE entry_tags (
  entry_id uuid not null references diary_entries(id) on delete cascade,
  tag_id   uuid not null references tags(id) on delete cascade,
  primary key (entry_id, tag_id)
);

-- 5. Comments
CREATE TABLE comments (
  id         uuid primary key default gen_random_uuid(),
  entry_id   uuid not null references diary_entries(id) on delete cascade,
  user_id    uuid not null references users(id) on delete cascade,
  content    text not null,
  created_at timestamptz default now()
);

CREATE INDEX idx_comments_entry ON comments(entry_id);
CREATE INDEX idx_comments_user ON comments(user_id);

-- 6. Favorites (M:N between users + entries)
CREATE TABLE favorites (
  user_id    uuid not null references users(id) on delete cascade,
  entry_id   uuid not null references diary_entries(id) on delete cascade,
  created_at timestamptz default now(),
  primary key (user_id, entry_id)
);
```

---

## Part 3: Database Design for Different Web Apps

### Type A: Blog / Diary / CMS (RAG-Diary)

**Pattern:** 1 user → N entries, entries มี tags (M:N)

```
Users ──→ Entries ──→ Comments
           │
      Entry_Tags ──→ Tags
```

**Key considerations:**
- Full-text search (`to_tsvector`, `@@`)
- Vector search (pgvector) — RAG specific
- Soft delete (เพิ่ม `deleted_at`) — เผื่อกู้คืน
- Versioning (เพิ่ม `version` column) — สำหรับ undo

### Type B: E-commerce

**Entities:** products, categories, users, orders, order_items, payments, reviews, carts

```
Categories M ── N Products
                    │
Users 1 ── N Orders ── N Order_Items ── N Products
                    │
               Payments
                    
Users 1 ── N Reviews ── N Products
Users M ──── N Carts ──── N Products (M:N with quantity)
```

```sql
-- Core schema snippet
CREATE TABLE products (
  id uuid primary key,
  name text not null,
  price decimal(10,2) not null,
  stock int not null default 0
);

CREATE TABLE orders (
  id uuid primary key,
  user_id uuid references users(id),
  status text default 'pending',  -- pending, paid, shipped, delivered, cancelled
  total decimal(12,2),
  created_at timestamptz default now()
);

CREATE TABLE order_items (
  order_id   uuid references orders(id),
  product_id uuid references products(id),
  quantity   int not null,
  price      decimal(10,2) not null,  -- snapshot price at order time
  primary key (order_id, product_id)
);

CREATE TABLE carts (
  user_id    uuid references users(id),
  product_id uuid references products(id),
  quantity   int not null default 1,
  primary key (user_id, product_id)
);
```

**E-commerce DB Design Tips:**
- `price` ใน order_items = snapshot (ของไม่เปลี่ยนตาม product.price)
- `status` ใน orders — ใช้ state machine (pending → paid → shipped → delivered)
- stock — ใช้ `SELECT ... FOR UPDATE` เพื่อ locking (กันขายของซ้ำ)
- soft delete สำหรับ products (เก็บ history)

### Type C: Social Media

**Entities:** users, posts, comments, likes, follows, notifications, messages

```
Users 1 ── N Posts
Users 1 ── N Comments ── N Posts
Users 1 ── N Likes ── N Posts
Users M ── N Follows ── N Users  (self-referencing M:N)
Users 1 ── N Messages ── N Users (chat)
```

```sql
-- Self-referencing: followers
CREATE TABLE follows (
  follower_id uuid references users(id),  -- คนที่ follow
  followed_id uuid references users(id),  -- คนที่ถูก follow
  created_at timestamptz default now(),
  primary key (follower_id, followed_id),
  check (follower_id <> followed_id)  -- follow ตัวเองไม่ได้
);

-- Likes (polymorphic — like post หรือ comment)
CREATE TABLE likes (
  user_id     uuid references users(id),
  resource_id uuid not null,
  resource_type text not null check (resource_type IN ('post', 'comment')),
  created_at timestamptz default now(),
  primary key (user_id, resource_id, resource_type)
);

-- Messages (direct chat)
CREATE TABLE messages (
  id uuid primary key,
  sender_id   uuid references users(id),
  receiver_id uuid references users(id),
  content text not null,
  read_at timestamptz,
  created_at timestamptz default now()
);
CREATE INDEX idx_messages_conversation ON messages(
  least(sender_id, receiver_id),
  greatest(sender_id, receiver_id)
);
```

**Social Media DB Design Tips:**
- `resource_type` = polymorphic association (like on post or comment)
- Denormalize `like_count`, `comment_count` ใน posts (อ่านเยอะกว่าเขียน)
- ใช้ `least/greatest` สำหรับสร้าง conversation key
- Partition messages ตามเวลา (เก่า → cold storage)

### Type D: Multi-tenant SaaS

**Entities:** tenants, users, projects (tenant-aware everything)

```
Tenants 1 ── N Users
Tenants 1 ── N Projects
```

**3 วิธีทำ multi-tenant:**

#### 1. Shared DB, Shared Table + tenant_id column ✅
```sql
-- ง่ายสุด, ถูกสุด — ใช้กับ 90% ของ app
CREATE TABLE diary_entries (
  id uuid primary key,
  tenant_id uuid not null references tenants(id),  -- ← ระบุ tenant
  content text
);
CREATE INDEX idx_entries_tenant ON diary_entries(tenant_id);

-- ทุก query ต้องมี WHERE tenant_id = ?
SELECT * FROM diary_entries WHERE tenant_id = $1;
```

**RAG-Diary ใช้ pattern นี้** ถ้าต้องทำ multi-user

#### 2. Shared DB, Separate Schema (PostgreSQL schema)
```
tenant1.diary_entries
tenant2.diary_entries
```
แยก schema แต่ละ tenant — ดีตรง isolate ข้อมูล แต่ management ยาก

#### 3. Separate DB
```
tenant1-db
tenant2-db
```
ปลอดภัยที่สุด (เหมาะกับ banking/health) — แต่แพง

---

## Part 4: RAG-Diary Deep Dive

### ปัจจุบัน RAG-Diary มีแค่ 1 table

```sql
create table if not exists diary_entries (
  id         uuid primary key default gen_random_uuid(),
  content    text not null,
  tag        text default 'General',    -- ← ตอนนี้ tag = column ตรงๆ (1 entry = 1 tag)
  created_at timestamp with time zone default now(),
  embedding  vector(1536)
);
```

### ถ้าจะเพิ่ม Users + Tags (M:N) + Comments

```sql
-- Step 1: Add users table
CREATE TABLE users (
  id       uuid primary key default gen_random_uuid(),
  email    text not null unique,
  password text not null,
  name     text
);

-- Step 2: Add user_id to diary_entries
ALTER TABLE diary_entries ADD COLUMN user_id uuid references users(id);
ALTER TABLE diary_entries ADD COLUMN updated_at timestamptz;

-- Step 3: Create tags (normalize)
CREATE TABLE tags (
  id   uuid primary key default gen_random_uuid(),
  name text not null unique
);

-- Migrate existing tags from diary_entries.tag
INSERT INTO tags (name)
SELECT DISTINCT tag FROM diary_entries WHERE tag IS NOT NULL;

-- Step 4: Create junction table
CREATE TABLE entry_tags (
  entry_id uuid references diary_entries(id) on delete cascade,
  tag_id   uuid references tags(id) on delete cascade,
  primary key (entry_id, tag_id)
);

-- Migrate existing entry-tag relationships
INSERT INTO entry_tags (entry_id, tag_id)
SELECT e.id, t.id
FROM diary_entries e
JOIN tags t ON e.tag = t.name;

-- Step 5: Drop old tag column
ALTER TABLE diary_entries DROP COLUMN tag;

-- Step 6: Comments
CREATE TABLE comments (
  id       uuid primary key default gen_random_uuid(),
  entry_id uuid references diary_entries(id) on delete cascade,
  user_id  uuid references users(id) on delete cascade,
  content  text not null,
  created_at timestamptz default now()
);
```

---

## Part 5: Hands-on Design Exercise

### Exercise: ออกแบบ "Meal Tracker" app

**Requirements:**
- ผู้ใช้บันทึกอาหารที่กินแต่ละวัน (breakfast, lunch, dinner, snack)
- แต่ละมื้อมีรูป + calories + note
- อาหารมีหมวดหมู่ (thai, italian, healthy, etc.)
- ผู้ใช้ตั้งเป้าหมาย calories ต่อวันได้
- มีฟีเจอร์ favorite meals (บันทึก menu ที่กินบ่อย)

**ลองออกแบบดูครับ:**

<details>
<summary>เฉลย (คลิกเมื่อคิดเสร็จ)</summary>

```sql
-- Entities: users, meals, meal_categories, daily_logs, favorite_meals

CREATE TABLE users (
  id uuid primary key,
  email text unique,
  daily_calorie_goal int default 2000
);

CREATE TABLE meal_categories (
  id uuid primary key,
  name text unique  -- breakfast, lunch, dinner, snack
);

CREATE TABLE meals (
  id uuid primary key,
  name text not null,
  calories int,
  image_url text,
  note text,
  category_id uuid references meal_categories(id)
);

CREATE TABLE daily_logs (
  id uuid primary key,
  user_id uuid references users(id),
  meal_id uuid references meals(id),
  eaten_at timestamptz default now(),
  portion decimal(5,2) default 1.0,  -- 0.5 = กินครึ่งจาน
  note text
);

CREATE TABLE favorite_meals (
  user_id uuid references users(id),
  meal_id uuid references meals(id),
  primary key (user_id, meal_id)
);

-- Query: รวม calories ของวันนี้
SELECT SUM(m.calories * dl.portion) as total_cal
FROM daily_logs dl
JOIN meals m ON dl.meal_id = m.id
WHERE dl.user_id = $1
  AND dl.eaten_at::date = CURRENT_DATE;
```
</details>

---

## Question to Think About

- RAG-Diary ใช้ `tag` เป็น column ตรงๆ (1 entry = 1 tag) — ข้อดีข้อเสียอะไรเมื่อเทียบกับ junction table?
- ถ้าเราต้องการให้ search "learning" เจอทั้งจาก content และ tag — query ยังไง?
- ใน e-commerce, ทำไม `order_items.price` ถึงต้องเป็น snapshot ไม่ใช่ JOIN ไป `products.price`?
- ถ้า favorite มี primary key = (user_id, entry_id) — user จะ favorite entry เดิมซ้ำได้มั้ย?

## Links

- Related concept: [[Database & SQL]], [[REST API JSON]], [[RAG Flow]]
- Code ref: [[server/src/lib/schema.sql]], [[server/src/lib/db.ts]]
- Source: Alex Xu — System Design Interview (NotebookLM)
