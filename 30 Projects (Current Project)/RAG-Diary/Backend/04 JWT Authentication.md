---
created: 2026-07-02
type: lesson
series: backend
number: 04
topic: authentication
status: published
---

# JWT Authentication — ระบบยืนยันตัวตน

> **เปรียบ:** JWT = **บัตรผ่าน (ID Badge)** ที่มีลายเซ็นปลอมไม่ได้  
> - Login = ไปรับบัตรที่แผนกต้อนรับ  
> - ทุก request = แสดงบัตร  
> - Server ตรวจลายเซ็น = อนุญาต/ปฏิเสธ

---

## 1. Authentication vs Authorization

| Auth... | แปลว่า | หน้าที่ |
|---------|--------|--------|
| **Authentication** | ยืนยันตัวตน | "คุณคือใคร?" (login, check password) |
| **Authorization** | อนุมัติสิทธิ์ | "คุณทำอะไรได้?" (admin, user, guest) |

**Flow ทั้งหมด:**

```
Client                              Server
  |                                   |
  |── POST /auth/login ──────────────→|  ตรวจ email + password
  |   { email, password }             |  
  |                                   |  hash → compare → ok!
  |←── { token: "eyJ..." } ─────────|  สร้าง JWT ส่งกลับ
  |                                   |
  |── GET /api/entries ──────────────→|  
  |   Authorization: Bearer eyJ...   |  ตรวจ JWT → ถูกต้อง!
  |                                   |  ดึง userId จาก token
  |←── { entries: [...] } ───────────|  คืนข้อมูลเฉพาะ user นี้
```

> **เชื่อมกับ:** [[Fundamental/01 REST API & JSON]] — JWT structure (Header.Payload.Signature)

## 2. ติดตั้ง Dependencies

```bash
npm install bcrypt jsonwebtoken
npm install -D @types/bcrypt @types/jsonwebtoken
```

## 3. Hashing Password (bcrypt)

```typescript
// src/services/auth.ts
import bcrypt from 'bcrypt';

const SALT_ROUNDS = 10;  // ยิ่งมาก = ยิ่งช้าแต่ปลอดภัย

export const hashPassword = async (password: string): Promise<string> => {
    return bcrypt.hash(password, SALT_ROUNDS);
};

export const comparePassword = async (password: string, hash: string): Promise<boolean> => {
    return bcrypt.compare(password, hash);
};
```

**ทำไมต้อง hash?** — ไม่ควรเก็บ password ตรงๆ ใน database
- ถ้า database leak → hacker เห็นเป็นตัวเลขเบลอๆ
- bcrypt = one-way (hash แล้วถอดกลับไม่ได้)
- เปรียบเทียบโดย hash ค่าใหม่ → เทียบกับที่เก็บไว้

## 4. JWT Sign & Verify

```typescript
import jwt from 'jsonwebtoken';

const JWT_SECRET = process.env.JWT_SECRET || 'fallback-secret';

export interface JwtPayload {
    userId: number;
    email: string;
}

// Sign — สร้าง token
export const generateToken = (payload: JwtPayload): string => {
    return jwt.sign(payload, JWT_SECRET, {
        expiresIn: '24h'  // token หมดอายุใน 24 ชม.
    });
};

// Verify — ตรวจสอบ token
export const verifyToken = (token: string): JwtPayload => {
    return jwt.verify(token, JWT_SECRET) as JwtPayload;
};
```

### JWT Structure

```
Header:  { "alg": "HS256", "typ": "JWT" }
Payload: { "userId": 1, "email": "user@example.com", "iat": 1680000000, "exp": 1680086400 }
Signature: HMACSHA256(base64Url(header) + "." + base64Url(payload), secret)

Result: eyJhbGciOiJIUzI1NiJ9.eyJ1c2VySWQiOjF9.abc123...
         └── Header ──┘ └───── Payload ─────┘ └── Sig ──┘
```

## 5. Auth Routes

```typescript
// src/routes/auth.ts
import { Router } from 'express';
import { pool } from '../services/db';
import { hashPassword, comparePassword, generateToken } from '../services/auth';
import { AppError } from '../middleware/error';

const router = Router();

// POST /api/auth/register
router.post('/register', async (req, res, next) => {
    try {
        const { email, password } = req.body;
        
        // Validation
        if (!email || !password) {
            throw new AppError('Email and password required', 400);
        }
        if (password.length < 6) {
            throw new AppError('Password must be at least 6 characters', 400);
        }
        
        // Check duplicate
        const existing = await pool.query(
            'SELECT id FROM users WHERE email = $1', [email]
        );
        if (existing.rows.length > 0) {
            throw new AppError('Email already registered', 409);
        }
        
        // Create user
        const hashedPassword = await hashPassword(password);
        const result = await pool.query(
            'INSERT INTO users (email, password_hash) VALUES ($1, $2) RETURNING id, email, created_at',
            [email, hashedPassword]
        );
        
        const user = result.rows[0];
        const token = generateToken({ userId: user.id, email: user.email });
        
        res.status(201).json({ success: true, data: { user, token } });
    } catch (error) {
        next(error);
    }
});

// POST /api/auth/login
router.post('/login', async (req, res, next) => {
    try {
        const { email, password } = req.body;
        
        if (!email || !password) {
            throw new AppError('Email and password required', 400);
        }
        
        // Find user
        const result = await pool.query(
            'SELECT * FROM users WHERE email = $1', [email]
        );
        if (result.rows.length === 0) {
            throw new AppError('Invalid email or password', 401);
        }
        
        const user = result.rows[0];
        
        // Check password
        const valid = await comparePassword(password, user.password_hash);
        if (!valid) {
            throw new AppError('Invalid email or password', 401);
        }
        
        const token = generateToken({ userId: user.id, email: user.email });
        
        res.json({ success: true, data: { user: { id: user.id, email: user.email }, token } });
    } catch (error) {
        next(error);
    }
});

export default router;
```

## 6. Auth Middleware — ป้องกัน routes

```typescript
// src/middleware/auth.ts
import { Request, Response, NextFunction } from 'express';
import { verifyToken, JwtPayload } from '../services/auth';

// ขยาย Request type ให้มี user
declare global {
    namespace Express {
        interface Request {
            user?: JwtPayload;
        }
    }
}

export const authMiddleware = (req: Request, res: Response, next: NextFunction) => {
    const authHeader = req.headers.authorization;
    
    // ต้องมี Authorization header
    if (!authHeader || !authHeader.startsWith('Bearer ')) {
        return res.status(401).json({
            success: false,
            message: 'Access denied. No token provided.'
        });
    }
    
    const token = authHeader.substring(7);  // ตัด "Bearer " ออก
    
    try {
        const decoded = verifyToken(token);
        req.user = decoded;  // attach user info ไปกับ request
        next();
    } catch (error) {
        if (error instanceof jwt.TokenExpiredError) {
            return res.status(401).json({
                success: false,
                message: 'Token expired. Please login again.'
            });
        }
        return res.status(401).json({
            success: false,
            message: 'Invalid token.'
        });
    }
};
```

## 7. Protected Routes

```typescript
// src/routes/entries.ts — protected routes
import { Router } from 'express';
import { authMiddleware } from '../middleware/auth';
import { getAllEntries, createEntry } from '../controllers/entryController';

const router = Router();

// ทุก route ต้องมี auth
router.use(authMiddleware);

router.get('/', getAllEntries);
router.post('/', createEntry);

export default router;
```

```typescript
// Controller — ใช้ req.user
export const getAllEntries = async (req: Request, res: Response, next: NextFunction) => {
    try {
        const userId = req.user!.userId;  // จาก JWT
        const result = await pool.query(
            'SELECT * FROM diary_entries WHERE user_id = $1 ORDER BY created_at DESC',
            [userId]
        );
        res.json({ success: true, data: result.rows });
    } catch (error) {
        next(error);
    }
};
```

## 8. Security Best Practices

```typescript
// 1. ตั้งเวลาหมดอายุ
jwt.sign(payload, secret, { expiresIn: '24h' });

// 2. ใช้ secret ที่แข็งแรง (environment variable)
// JWT_SECRET=a-complex-random-string-with-special-chars

// 3. ตรวจสอบ token ทุก request (middleware)

// 4. ไม่เก็บ sensitive data ใน payload
// ❌ jwt.sign({ userId: 1, password: '1234' }, secret)
// ✅ jwt.sign({ userId: 1 }, secret)

// 5. ใช้ HTTPS เสมอ — ป้องกัน token leak

// 6. Refresh token (advanced) — token อายุสั้น + refresh token อายุยาว
```

## 9. แบบฝึกหัด

สร้าง `GET /api/auth/me` endpoint ที่:
1. ใช้ `authMiddleware`
2. ดึงข้อมูล user จาก `req.user.userId`
3. Query users table → return `{ id, email, created_at }`
4. ถ้าไม่เจอ user → 404

---

## 🔗 Links

- ก่อนหน้า: [[Backend/03 Database Connection & SQL|Database Connection & SQL]]
- ต่อไป: [[Backend/05 RAG Integration|RAG Integration]]
- ต้องเข้าใจ: [[Fundamental/01 REST API & JSON]] (JWT section)
- กลับไป: [[_Curriculum]]
