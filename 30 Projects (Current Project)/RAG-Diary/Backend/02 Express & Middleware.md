---
created: 2026-07-02
type: lesson
series: backend
number: 02
topic: express-middleware
status: published
---

# Express & Middleware — สร้าง REST API Server

> **เปรียบ:** Express = พนักงานต้อนรับโรงแรม  
> Middleware = เจ้าหน้าที่ตรวจ (เช็ค ID, ถามวัตถุประสงค์) ก่อนเข้าห้อง  
> Routes = ป้ายบอกทางไปแต่ละห้อง

---

## 1. Express คืออะไร?

**Express** = Web framework สำหรับ Node.js — ช่วยให้สร้าง REST API ได้ง่าย

**สิ่งที่ Express ทำให้:**
- Routing (จัดการ path ต่างๆ)
- Middleware (สอดแทรก logic ก่อน/หลัง request)
- Request/Response handling
- Error handling

> **เชื่อมกับ:** [[Fundamental/01 REST API & JSON]] — Express เป็นเครื่องมือสร้าง REST API

## 2. Project Setup

```bash
npm init -y
npm install express cors helmet dotenv
npm install -D typescript ts-node-dev @types/node @types/express
```

```typescript
// src/app.ts
import express from 'express';
import cors from 'cors';
import helmet from 'helmet';
import dotenv from 'dotenv';

dotenv.config();

const app = express();

// Middleware chain — ทำงานตามลำดับ
app.use(helmet());                    // Security headers
app.use(cors({                       // อนุญาต cross-origin
    origin: process.env.FRONTEND_URL || 'http://localhost:5173'
}));
app.use(express.json());              // Parse JSON body
app.use(express.urlencoded({ extended: true })); // Parse form data

export default app;
```

```typescript
// src/server.ts — Entry point
import app from './app';

const PORT = process.env.PORT || 3000;

app.listen(PORT, () => {
    console.log(`Server running on http://localhost:${PORT}`);
});
```

## 3. Middleware — หัวใจของ Express

**Middleware = ฟังก์ชันที่ทำงานระหว่าง Request → Response**

```
Request → Middleware 1 → Middleware 2 → Route Handler → Response
               ↑              ↑              ↑
            Logger        Auth Check     Business Logic
```

### สร้าง Middleware ด้วยตัวเอง

```typescript
// Logger middleware
app.use((req, res, next) => {
    console.log(`[${new Date().toISOString()}] ${req.method} ${req.path}`);
    next();  // สำคัญ! ต้อง next() เพื่อไป middleware ถัดไป
});

// Request timer middleware
app.use((req, res, next) => {
    const start = Date.now();
    res.on('finish', () => {
        const duration = Date.now() - start;
        console.log(`${req.method} ${req.path} — ${duration}ms`);
    });
    next();
});
```

### Middleware ที่ใช้บ่อย

```typescript
import cors from 'cors';
import helmet from 'helmet';
import morgan from 'morgan';

app.use(helmet());           // Security headers
app.use(cors());             // Cross-origin
app.use(morgan('dev'));      // HTTP logger (alternative to custom)
app.use(express.json());     // Parse JSON body
app.use(express.static('public'));  // Serve static files
```

## 4. Routing — จัดการเส้นทาง

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

// RESTful API Design (เชื่อมกับ Fundamental/01)
router.get('/', getAllEntries);              // GET /api/entries
router.get('/:id', getEntry);              // GET /api/entries/:id
router.post('/', createEntry);              // POST /api/entries
router.put('/:id', updateEntry);           // PUT /api/entries/:id
router.delete('/:id', deleteEntry);          // DELETE /api/entries/:id

export default router;
```

```typescript
// src/app.ts — ลงทะเบียน routes
import entryRoutes from './routes/entries';
import authRoutes from './routes/auth';
import chatRoutes from './routes/chat';

app.use('/api/entries', entryRoutes);
app.use('/api/auth', authRoutes);
app.use('/api/chat', chatRoutes);
```

## 5. Route Parameters & Query

```typescript
// :id = route parameter
router.get('/:id', (req, res) => {
    const id = req.params.id;        // "/entries/5" → "5"
    const page = req.query.page;     // "/entries?page=2" → "2"
    const limit = req.query.limit;   // "/entries?limit=10" → "10"
    
    res.json({ id, page, limit });
});
```

## 6. Error Handling Middleware

```typescript
// Custom Error Class
export class AppError extends Error {
    statusCode: number;
    constructor(message: string, statusCode: number = 500) {
        super(message);
        this.statusCode = statusCode;
    }
}

// Global Error Handler — ต้องมี 4 parameters
app.use((err: any, req: any, res: any, next: any) => {
    const statusCode = err.statusCode || 500;
    const message = err.message || 'Internal Server Error';
    
    console.error(`[ERROR] ${statusCode} - ${message}`);
    if (process.env.NODE_ENV === 'development') {
        console.error(err.stack);
    }
    
    res.status(statusCode).json({
        success: false,
        message,
        ...(process.env.NODE_ENV === 'development' && { stack: err.stack })
    });
});
```

### 404 Handler — path ไม่มี

```typescript
// วางไว้ท้ายสุด — ถ้าไม่มี route ไหน match
app.use((req, res) => {
    res.status(404).json({
        success: false,
        message: `Route ${req.originalUrl} not found`
    });
});
```

## 7. Request Flow — ตัวอย่างเต็ม

```typescript
// app.ts — Middleware เรียงตามลำดับ
app.use(helmet());              // 1. Security
app.use(cors());                // 2. CORS
app.use(express.json());        // 3. Parse body
app.use(morgan('dev'));         // 4. Logger

// Routes
app.use('/api/entries', entryRoutes);  // 5. Entry routes
app.use('/api/auth', authRoutes);      // 6. Auth routes

// Error handlers (ท้ายสุด)
app.use(notFoundHandler);       // 7. 404
app.use(errorHandler);          // 8. Global error
```

## 8. Controller Pattern

```typescript
// src/controllers/entryController.ts
import { Request, Response, NextFunction } from 'express';
import { pool } from '../services/db';

// Pattern: async handler → try/catch → next(error)
export const getAllEntries = async (req: Request, res: Response, next: NextFunction) => {
    try {
        const result = await pool.query(
            'SELECT * FROM diary_entries ORDER BY created_at DESC'
        );
        res.json({ success: true, data: result.rows });
    } catch (error) {
        next(error);  // ส่งไป error handler middleware
    }
};

// สร้าง wrapper เพื่อไม่ต้องเขียน try/catch ทุกครั้ง
const asyncHandler = (fn: Function) =>
    (req: Request, res: Response, next: NextFunction) =>
        Promise.resolve(fn(req, res, next)).catch(next);

export const getEntry = asyncHandler(async (req: Request, res: Response) => {
    const result = await pool.query(
        'SELECT * FROM diary_entries WHERE id = $1',
        [req.params.id]
    );
    if (result.rows.length === 0) {
        throw new AppError('Entry not found', 404);
    }
    res.json({ success: true, data: result.rows[0] });
});
```

## 9. แบบฝึกหัด

สร้าง route `GET /api/entries/search?q=keyword` ที่:
1. รับ query parameter `q`
2. ค้นหา entries ที่ title หรือ content มี keyword
3. ใช้ SQL: `WHERE title ILIKE $1 OR content ILIKE $1`
4. ส่ง error 400 ถ้าไม่มี `q`
5. ส่ง error 404 ถ้าไม่เจอ

---

## 🔗 Links

- ก่อนหน้า: [[Backend/01 RAG Flow|RAG Flow]]
- ต่อไป: [[Backend/03 Database Connection & SQL|Database Connection & SQL]]
- ต้องเข้าใจ: [[Fundamental/01 REST API & JSON]], [[Fundamental/03 Async Await & Event Loop]]
- กลับไป: [[_Curriculum]]
