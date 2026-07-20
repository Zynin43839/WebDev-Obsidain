---
created: 2026-07-02
type: note
topic: backend
status: seedling
---

# Backend Development — Node.js + Express + TypeScript

**เป้าหมาย:** สร้าง Backend API สำหรับ RAG-Diary โดยอาศัพท์จากสิ่งที่เรียนมาแล้ว

---

## 🔗 ความสัมพันธ์กับสิ่งที่เรียนมา

```
[Frontend] ←→ [Backend API] ←→ [Database] ←→ [AI Service]
    นั่นคือหน้าเว็บ (React)      นั่นคือเซิร์ฟเวอร์ (Node.js)    นั่นคือฐานข้อมูล (PostgreSQL)   นั่นคือ AI (OpenAI)
     ↑              ↑                   ↑                    ↑
  เรียนจาก:    เรียนจาก:         เรียนจาก:            เรียนจาก:
  - HTML/CSS    - REST API      - Database Design      - RAG Flow
  - JS DOM      - TypeScript     - SQL                 - Embedding
  - React       - Express       - pgvector             - Prompt Engineering
```

---

## 🏗️ 1. Project Structure — โครงสร้างโปรเจค

### ทำไมต้องมีโครงสร้างที่ดี?

**เปรียบเทียบ:** Backend เป็นแผนกอาคาร
- **Express** = ประตูหลัก (รับ request เข้า)
- **Routes** = ประตูห้อง (แยกเส้นทาง)
- **Controllers** = ฝ่ายเจ้าหน้าที่ (ทำงานจริง)
- **Services** = ฝ่ายแม่บ้าน (เชื่อม database, AI)
- **Middleware** = เจ้าหน้าที่ตรวจตราม (check request ก่อนเข้า)

### โครงสร้างไฟล์แนะนำ

```
backend/
├── src/
│   ├── app.ts              # Express แอปพลิเคชันหลัก
│   ├── server.ts           # Entry point เปิดเซิร์ฟเวอร์
│   ├── routes/
│   │   ├── entries.ts      # API จัดการ diary entries
│   │   ├── auth.ts         # API จัดการ authentication
│   │   └── chat.ts         # API จัดการ RAG chat
│   ├── controllers/
│   │   ├── entryController.ts
│   │   ├── authController.ts
│   │   └── chatController.ts
│   ├── services/
│   │   ├── db.ts           # Database connection
│   │   ├── embedding.ts    # AI embedding service
│   │   └── llm.ts          # LLM service
│   ├── middleware/
│   │   ├── auth.ts         # JWT middleware
│   │   └── error.ts        # Error handler
│   └── config/
│       └── env.ts          # Environment variables
├── package.json
├── tsconfig.json
└── .env
```

---

## 🔧 2. Project Setup — ตั้งค่าเริ่มต้น

### ขั้นตอนเริ่มต้น

**1. สร้าง package.json**

```bash
npm init -y
```

**2. ติดตั้ง dependencies หลัก**

```bash
# Runtime dependencies
npm install express cors helmet dotenv pg jsonwebtoken bcrypt

# Development dependencies
npm install -D typescript ts-node-dev @types/node @types/express @types/pg @types/jsonwebtoken
```

**3. สร้าง tsconfig.json**

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "commonjs",
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true
  }
}
```

### ไฟล์ Server พื้นฐาน

```typescript
// src/server.ts — Entry point
import app from './app';

const PORT = process.env.PORT || 3000;

app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});
```

```typescript
// src/app.ts — Express app configuration
import express from 'express';
import cors from 'cors';
import helmet from 'helmet';
import dotenv from 'dotenv';

dotenv.config();

const app = express();

// Middleware chain — ทำงานตามลำดับ
app.use(helmet());                    // ความปลอดภัย HTTP headers
app.use(cors({                       // อนุญาต cross-origin requests
  origin: process.env.FRONTEND_URL || 'http://localhost:5173'
}));
app.use(express.json());              // Parse JSON body

export default app;
```

---

## 🛣️ 3. Express Middleware & Routing

### Middleware คืออะไร?

**Middleware** = ฟังก์ชันที่ทำงานระหว่าง request กับ response — เช่น ตรวจสอบ token, log การเข้าชม

```typescript
// Logger middleware - เปรียบเสมือนเจ้าหน้าที่บันทึก
app.use((req, res, next) => {
  console.log(`[${new Date().toISOString()}] ${req.method} ${req.path}`);
  next();  // ไป middleware ถัดไป
});

// Error handling middleware
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).json({ error: 'Something broke!' });
});
```

### Routing รูปแบบ RESTful

**เชื่อมกับ:** [[REST API JSON]] — Richardson Maturity Model

```typescript
// src/routes/entries.ts
import { Router } from 'express';
import { 
  getAllEntries, 
  getEntry, 
  createEntry,
  updateEntry, 
  deleteEntry 
} from '../controllers/entryController';

const router = Router();

// RESTful routes — ตามแนวที่เรียนมา
router.get('/', getAllEntries);              // GET /api/entries
router.get('/:id', getEntry);              // GET /api/entries/:id
router.post('/', createEntry);              // POST /api/entries
router.put('/:id', updateEntry);           // PUT /api/entries/:id
router.delete('/:id', deleteEntry);          // DELETE /api/entries/:id

export default router;
```

---

## 🗄️ 4. Database Connection — เชื่อม PostgreSQL

### ทำไมต้อง PostgreSQL?

**PostgreSQL** = ฐานข้อมูลแบบ relational (เชื่อมโยง relations)
- รองรับ JSON columns (เก็บข้อมูลซับซ้อน)
- มี pgvector extension (เก็บ embeddings 1536 dimensions)

**เชื่อมกับ:** [[Database Design & SQL Deep Dive]]

### Database Connection Service

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
});

export { pool };
```

### SQL Tables — อ้างอิง Database Design

```sql
-- Users table
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Diary entries — เชื่อมกับ embedding
CREATE TABLE diary_entries (
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(id),
    title VARCHAR(255) NOT NULL,
    content TEXT NOT NULL,
    embedding VECTOR(1536),  -- เก็บ embedding
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Tags — หลายต่อหลาย (M:N)
CREATE TABLE tags (
    id SERIAL PRIMARY KEY,
    name VARCHAR(50) UNIQUE NOT NULL
);

CREATE TABLE entry_tags (
    entry_id INTEGER REFERENCES diary_entries(id),
    tag_id INTEGER REFERENCES tags(id),
    PRIMARY KEY (entry_id, tag_id)
);
```

---

## 🎮 5. Controllers — จุดศูนย์กลางการทำงาน

### Entry Controller — CRUD Operations

**เชื่อมกับ:** [[Async Await & Event Loop]] — ใช้ async/await กับ try-catch

```typescript
// src/controllers/entryController.ts
import { pool } from '../services/db';
import { Request, Response, NextFunction } from 'express';

// GET /api/entries — โหลดทั้งหมด
export const getAllEntries = async (req: Request, res: Response, next: NextFunction) => {
  try {
    // async/await — รอ database query เสร็จ
    const result = await pool.query(
      'SELECT * FROM diary_entries ORDER BY created_at DESC'
    );
    res.json(result.rows);  // ส่งข้อมูลกลับ
  } catch (error) {
    next(error);  // ส่ง error ไป error handler
  }
};

// POST /api/entries — สร้างใหม่
export const createEntry = async (req: Request, res: Response, next: NextFunction) => {
  try {
    const { title, content } = req.body;
    
    // async/await — สร้าง embedding จาก content
    const embedding = await generateEmbedding(content);
    
    const result = await pool.query(
      `INSERT INTO diary_entries (title, content, embedding) 
       VALUES ($1, $2, $3) 
       RETURNING *`,
      [title, content, embedding]
    );
    
    res.status(201).json(result.rows[0]);
  } catch (error) {
    next(error);
  }
};
```

---

## 🔐 6. Authentication — JWT System

### JWT Flow ทำงานยังไง?

**เชื่อมกับ:** [[REST API JSON]] — JWT authentication

```
Login Request
    ↓
Check Email/Password
    ↓
Generate JWT Token = Header.Payload.Signature
    ↓
Send Token to Client
    ↓
Client stores Token (localStorage/sessionStorage)
    ↓
Next requests: Authorization: Bearer <token>
    ↓
Middleware verifies token signature
    ↓
Extract userId → Attach to req.user
```

### Auth Service

```typescript
// src/services/auth.ts
import jwt from 'jsonwebtoken';
import bcrypt from 'bcrypt';

export const hashPassword = async (password: string): Promise<string> => {
  return bcrypt.hash(password, 10);
};

export const comparePassword = async (password: string, hash: string): Promise<boolean> => {
  return bcrypt.compare(password, hash);
};

export const generateToken = (userId: number): string => {
  return jwt.sign({ userId }, process.env.JWT_SECRET!, {
    expiresIn: '24h'
  });
};

export const verifyToken = (token: string) => {
  return jwt.verify(token, process.env.JWT_SECRET!) as { userId: number };
};
```

### Auth Middleware

```typescript
// src/middleware/auth.ts
import { Request, Response, NextFunction } from 'express';
import { verifyToken } from '../services/auth';

export const authMiddleware = (req: Request, res: Response, next: NextFunction) => {
  const authHeader = req.headers.authorization;
  
  if (!authHeader || !authHeader.startsWith('Bearer ')) {
    return res.status(401).json({ error: 'No token provided' });
  }
  
  const token = authHeader.substring(7);
  
  try {
    const decoded = verifyToken(token);
    (req as any).user = decoded;  // attach userId ไปกับ request
    next();
  } catch (error) {
    res.status(401).json({ error: 'Invalid token' });
  }
};
```

---

## ❌ 7. Error Handling — จัดการ error อย่างมืออาชีพ

### Custom Error Class

```typescript
// src/middleware/error.ts
export class AppError extends Error {
  statusCode: number;
  
  constructor(message: string, statusCode: number = 500) {
    super(message);
    this.statusCode = statusCode;
  }
}

// Usage in controller
export const getEntry = async (req: Request, res: Response, next: NextFunction) => {
  try {
    const result = await pool.query(
      'SELECT * FROM diary_entries WHERE id = $1',
      [req.params.id]
    );
    
    if (result.rows.length === 0) {
      throw new AppError('Entry not found', 404);  // 404 Not Found
    }
    
    res.json(result.rows[0]);
  } catch (error) {
    next(error);
  }
};
```

### Global Error Handler

```typescript
export const errorHandler = (err: any, req: any, res: any, next: any) => {
  const statusCode = err.statusCode || 500;
  const message = err.message || 'Internal Server Error';
  
  res.status(statusCode).json({
    success: false,
    message,
    // แสดง stack trace แต่ต้อง development mode
    ...(process.env.NODE_ENV === 'development' && { stack: err.stack })
  });
};
```

---

## ⚙️ 8. Environment Configuration

### .env File

```env
# Server
PORT=3000
NODE_ENV=development

# Database (PostgreSQL)
DB_HOST=localhost
DB_PORT=5432
DB_NAME=rag_diary
DB_USER=postgres
DB_PASSWORD=your-password

# Security
JWT_SECRET=your-super-secret-jwt-key
JWT_EXPIRES_IN=24h

# AI Services
OPENAI_API_KEY=sk-your-openai-key
FRONTEND_URL=http://localhost:5173

# Logging
LOG_LEVEL=debug
```

---

## 🔄 9. การเชื่อมโยงทุกส่วน (Integration Flow)

```
Frontend (React)
    ↓ (fetch API)
Backend (Express)
    ↓ (middleware chain)
Auth Middleware → Check token → attach userId to req
    ↓ (route handler)
Entry Controller → Service Layer
    ↓ (service layer)
Database Service → PostgreSQL (with pgvector)
    ↓ (for RAG)
Embedding Service → OpenAI API → Generate embedding
    ↓ (response)
JSON response ← Frontend Updates State
```

### ตัวอย่าง Request ทั้งหมด

**POST /api/entries — สร้าง diary entry พร้อม RAG**

```typescript
// 1. Frontend sends: { title, content } + Authorization header
// 2. Auth middleware verifies token
// 3. Controller receives:

export const createEntry = async (req: Request, res: Response) => {
  const { title, content } = req.body;
  const userId = req.user.userId;
  
  // สร้าง embedding (RAG Flow)
  const embedding = await embeddingService.create(content);
  
  // บันทึก database
  const result = await pool.query(
    'INSERT INTO diary_entries (user_id, title, content, embedding) VALUES ($1, $2, $3, $4) RETURNING *',
    [userId, title, content, embedding]
  );
  
  res.status(201).json(result.rows[0]);
};

// 4. Response → Frontend แสดง entry ใหม่
```

---

## 📋 สรุป Backend Development Path

| หัวข้อ | เชื่อมกับสิ่งที่เรียนมา | Status |
|--------|--------------------------|--------|
| Project Structure | [[TypeScript Basics]] | ✅ เรียน |
| Express Setup | [[Internet Basics]] | ✅ เรียน |
| Middleware/Routing | [[REST API JSON]] | ✅ เรียน |
| Database Connection | [[Database Design]] | ✅ เรียน |
| CRUD Operations | [[SQL]], [[Async Await]] | ✅ เรียน |
| Authentication | [[JWT]], [[REST API]] | ✅ เรียน |
| Error Handling | [[TypeScript Basics]] | ✅ เรียน |
| Environment Config | [[TypeScript Basics]] | ✅ เรียน |

---

## 🔜 Learning Path ต่อไป

- **Backend Implementation** — เขียนโค้ดจริงตามบทเรียนนี้
- **Frontend Integration** — เชื่อม React กับ API
- **Testing** — Unit tests สำหรับ Controllers
- **Deployment** — Docker + Production setup

---

*Links:*
- ก่อนหน้า: [[Frontend Fundamentals]]
- ถัดไป: [[Backend Implementation Guide]]
- กลับไป: [[_Curriculum]]