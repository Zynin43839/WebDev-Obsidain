---
created: 2026-07-02
type: curriculum
status: published
---

# 📚 Full-Stack Web Development Curriculum

> **ภาพรวม:** เรียนตั้งแต่พื้นฐานเว็บ → สร้าง Backend → สร้าง Frontend → เชื่อมต่อทุกอย่าง
> แต่ละซีรีส์เรียงจาก **พื้นฐาน → เจาะลึก → ประยุกต์ใช้จริง**

---

## 🧱 ซีรีส์ที่ 1: Fundamental (พื้นฐานที่ต้องรู้ก่อน)

> เรียนรู้ว่าเว็บทำงานยังไง, API คืออะไร, TypeScript, และ async programming

| # | บทเรียน | เนื้อหาหลัก | เวลา |
|---|---------|------------|------|
| 00 | [[Fundamental/00 Internet Basics\|Internet Basics]] | DNS, TCP, HTTP/HTTPS, TLS, CORS, WebSocket | 2-3 ชม. |
| 01 | [[Fundamental/01 REST API & JSON\|REST API & JSON]] | REST constraints, URL design, JWT, JSON, error formats | 2-3 ชม. |
| 02 | [[Fundamental/02 TypeScript Basics\|TypeScript Basics]] | types, interfaces, generics, discriminated unions, tsconfig | 2-3 ชม. |
| 03 | [[Fundamental/03 Async Await & Event Loop\|Async Await & Event Loop]] | Promise, Event Loop, microtask/macrotask, pitfalls | 2-3 ชม. |

**ลำดับการเรียนรู้:** 00 → 01 → 02 → 03  
**ความเชื่อมโยง:** Internet → REST API → TypeScript → Async (ต่อเนื่องกัน)

---

## ⚙️ ซีรีส์ที่ 2: Backend (หลังบ้าน)

> เรียนรู้การสร้างเซิร์ฟเวอร์, ฐานข้อมูล, Authentication, และ RAG

| # | บทเรียน | เนื้อหาหลัก | เวลา |
|---|---------|------------|------|
| 00 | [[Backend/00 Database Design & SQL\|Database Design & SQL]] | Normalization 1NF-3NF, Relationships, ERD, SQL schemas | 3-4 ชม. |
| 01 | [[Backend/01 RAG Flow\|RAG Flow]] | Embedding, pgvector, Retrieval-Augmented Generation, streaming | 2-3 ชม. |
| 02 | [[Backend/02 Express & Middleware\|Express & Middleware]] | Express setup, Routing, Middleware chain, Error handling | 2-3 ชม. |
| 03 | [[Backend/03 Database Connection & SQL\|Database Connection & SQL]] | pg Pool, CRUD queries, Migrations, Relationships in code | 2-3 ชม. |
| 04 | [[Backend/04 JWT Authentication\|JWT Authentication]] | bcrypt hashing, JWT sign/verify, Auth middleware, Protected routes | 2-3 ชม. |
| 05 | [[Backend/05 RAG Integration\|RAG Integration]] | Embedding service, pgvector search, LLM chat, Full RAG endpoint | 2-3 ชม. |
| 06 | [[Backend/06 Refactoring\|Refactoring]] | Strangler Fig, Code Smells, Extract Function, DDD, Error middleware | 2-3 ชม. |
| 07 | [[Backend/07 Testing\|Testing]] | Test Pyramid, Unit/Integration/E2E, Dependency Injection, Mock | 2-3 ชม. |
| 08 | [[Backend/08 Bun & Elysia\|Bun & Elysia]] | Bun runtime, Elysia framework, Validation, Eden Treaty client | 2-3 ชม. |
| 09 | [[Backend/09 Hybrid Search & Tag Filtering\|Hybrid Search & Tag Filtering]] | RRF, Smooth/Sharp embedding, Bridge table, Tag CRUD, Full-text search | 2-3 ชม. |

**ลำดับการเรียนรู้:** 00 → 01 → 02 → 03 → 04 → 05 → (06, 07, 08) → 09  
**ต้องเรียน Fundamental มาก่อน:** ซีรีส์ 1 (โดยเฉพาะ REST API + TypeScript + Async)

---

## 🎨 ซีรีส์ที่ 3: Frontend (หน้าบ้าน)

> เรียนรู้การสร้าง UI ด้วย HTML, ตกแต่งด้วย CSS, ทำให้มีชีวิตด้วย JS, และจัดระเบียบด้วย React

| # | บทเรียน | เนื้อหาหลัก | เวลา |
|---|---------|------------|------|
| 00 | [[Frontend/00 HTML\|HTML]] | Semantic tags, Forms, Accessibility, Structure | 2-3 ชม. |
| 01 | [[Frontend/01 CSS\|CSS]] | Selectors, Box Model, Flexbox, Grid, Responsive Design | 3-4 ชม. |
| 02 | [[Frontend/02 JavaScript DOM\|JavaScript DOM]] | DOM Tree, querySelector, Events, CRUD with DOM | 2-3 ชม. |
| 03 | [[Frontend/03 React Basics\|React Basics]] | Components, Props, State, useEffect, Conditional render | 3-4 ชม. |
| 04 | [[Frontend/04 Frontend-Backend Integration\|Frontend-Backend Integration]] | fetch API, Auth flow, RAG chat UI, Error/loading states | 2-3 ชม. |
| 05 | [[Frontend/05 TanStack Query\|TanStack Query]] | useQuery, useMutation, Query Key Factories, Optimistic Updates, Prefetching | 2-3 ชม. |
| 06 | [[Frontend/06 shadcn-ui\|shadcn/ui]] | Code generation, Theming with CSS variables, Dark mode, Component customization | 2-3 ชม. |
| 07 | [[Frontend/07 Placeholder Pages\|Placeholder Pages]] | ADT states, Skeleton/Empty/Error, useReducer, Widget dashboard, BFF | 2-3 ชม. |

**ลำดับการเรียนรู้:** 00 → 01 → 02 → 03 → 04 → (05, 06, 07) ตามลำดับ  
**ต้องเรียน Backend มาก่อน:** บทที่ 04-07 ต้องเข้าใจ API + Auth + RAG จาก Backend

---

## 🔗 แผนภาพความสัมพันธ์ระหว่างซีรีส์

```
  Fundamental  ──────────────────────────────────────────────┐
  ├─ 00 Internet Basics ──→ REST API เข้าใจ HTTP methods      │
  ├─ 01 REST API & JSON ──→ Backend สร้าง API ตามมาตรฐาน      ├──→ ใช้เขียน Backend
  ├─ 02 TypeScript Basics ──→ Backend/Frontend เขียน TS       │
  └─ 03 Async Await ──────→ ทุกบทเรียน (async ทุกที่!)        │
                                                              │
  Backend (ต้องมี Fundamental)                                │
  ├─ 00 Database & SQL ──→ เก็บข้อมูล                        ├──→ ให้ API + Data
  ├─ 01 RAG Flow ────────→ AI search logic                   │
  ├─ 02 Express & MW ────→ HTTP server                       │
  ├─ 03 DB Connection ───→ SQL in code                       │
  ├─ 04 JWT Auth ────────→ Security                          │
  ├─ 05 RAG Integration ──→ Full RAG endpoint                │
  ├─ 06 Refactoring ─────→ จัดระเบียบโค้ด                    │
  ├─ 07 Testing ─────────→ ทำให้โค้ดไม่พัง                   │
  ├─ 08 Bun & Elysia ────→ Fast runtime + framework          │
  └─ 09 Hybrid + Tag ────→ Search + Filter                   │
                                                               │
  Frontend (ต้องมี Fundamental + Backend basics)              │
  ├─ 00 HTML ────────────→ โครงสร้าง                          │
  ├─ 01 CSS ─────────────→ สวยงาม                             ├──→ เรียก Backend API
  ├─ 02 JS DOM ──────────→ โต้ตอบ                             │
  ├─ 03 React ───────────→ Component UI                      │
  ├─ 04 Integration ─────→ เชื่อม Frontend + Backend         │
  ├─ 05 TanStack Query ──→ จัดการ server state               │
  ├─ 06 shadcn/ui ───────→ UI component system               │
  └─ 07 Placeholder Pages─→ หน้าโครงสร้าง                    │
```

---

## ✅ Progress Tracking

### Fundamental (4 บท)
- [ ] [[Fundamental/00 Internet Basics|Internet Basics]]
- [ ] [[Fundamental/01 REST API & JSON|REST API & JSON]]
- [ ] [[Fundamental/02 TypeScript Basics|TypeScript Basics]]
- [ ] [[Fundamental/03 Async Await & Event Loop|Async Await & Event Loop]]

### Backend (10 บท)
- [ ] [[Backend/00 Database Design & SQL|Database Design & SQL]]
- [ ] [[Backend/01 RAG Flow|RAG Flow]]
- [ ] [[Backend/02 Express & Middleware|Express & Middleware]]
- [ ] [[Backend/03 Database Connection & SQL|Database Connection & SQL]]
- [ ] [[Backend/04 JWT Authentication|JWT Authentication]]
- [ ] [[Backend/05 RAG Integration|RAG Integration]]
- [ ] [[Backend/06 Refactoring|Refactoring]]
- [ ] [[Backend/07 Testing|Testing]]
- [ ] [[Backend/08 Bun & Elysia|Bun & Elysia]]
- [ ] [[Backend/09 Hybrid Search & Tag Filtering|Hybrid Search & Tag Filtering]]

### Frontend (8 บท)
- [ ] [[Frontend/00 HTML|HTML]]
- [ ] [[Frontend/01 CSS|CSS]]
- [ ] [[Frontend/02 JavaScript DOM|JavaScript DOM]]
- [ ] [[Frontend/03 React Basics|React Basics]]
- [ ] [[Frontend/04 Frontend-Backend Integration|Frontend-Backend Integration]]
- [ ] [[Frontend/05 TanStack Query|TanStack Query]]
- [ ] [[Frontend/06 shadcn-ui|shadcn/ui]]
- [ ] [[Frontend/07 Placeholder Pages|Placeholder Pages]]

---

## 🔄 สถานะบทเรียน

| สถานะ | ความหมาย |
|--------|---------|
| `seedling` | เพิ่มสร้าง, เนื้อหายังไม่สมบูรณ์ |
| `growing` | เนื้อหาพอใช้ได้, กำลังเพิ่มเติม |
| `evergreen` | เนื้อหาสมบูรณ์, ใช้เป็น reference ได้ |
| `published` | สมบูรณ์ พร้อมใช้ |

---

*Last updated: 2026-07-02 (added Backend 06-09, Frontend 05-07)*
