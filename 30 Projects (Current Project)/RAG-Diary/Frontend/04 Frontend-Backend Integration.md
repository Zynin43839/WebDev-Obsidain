---
created: 2026-07-02
type: lesson
series: frontend
number: 04
topic: integration
status: seedling
---

# Frontend-Backend Integration — เชื่อม React กับ Backend API

> **เปรียบ:** Frontend = ร้านอาหาร, Backend = ครัว, **Integration = พนักงานเสิร์ฟ**  
> คอยรับออเดอร์ (request) → ส่งไปครัว → นำอาหารกลับมา (response)

---

## 1. Flow การเชื่อมต่อ

```
React Component
    ↓
fetch('/api/entries', { headers: { Authorization: 'Bearer <token>' } })
    ↓
HTTP Request → Express Server
    ↓
Auth Middleware → Controller → Database/AI
    ↓
Response (JSON)
    ↓
React อัปเดต State → Re-render UI
```

> **ต้องเข้าใจก่อน:** [[Fundamental/01 REST API & JSON]], [[Backend/04 JWT Authentication]]

## 2. fetch API — พื้นฐาน

```javascript
// GET — โหลดข้อมูล
async function loadEntries() {
    try {
        const res = await fetch('/api/entries');
        if (!res.ok) throw new Error(`HTTP ${res.status}`);
        const data = await res.json();
        return data;
    } catch (err) {
        console.error('Failed to load:', err);
        throw err;  // ส่ง error ต่อให้ caller
    }
}

// POST — สร้างข้อมูล
async function createEntry(title, content) {
    const res = await fetch('/api/entries', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ title, content }),
    });
    if (!res.ok) throw new Error(`HTTP ${res.status}`);
    return res.json();
}
```

## 3. API Service Layer

สร้างไฟล์แยกสำหรับจัดการ API — แยก logic ออกจาก UI

```javascript
// src/services/api.js
const BASE_URL = '/api';

async function request(endpoint, options = {}) {
    const token = localStorage.getItem('token');
    
    const res = await fetch(`${BASE_URL}${endpoint}`, {
        headers: {
            'Content-Type': 'application/json',
            ...(token && { Authorization: `Bearer ${token}` }),
        },
        ...options,
    });
    
    if (!res.ok) {
        if (res.status === 401) {
            // Token หมดอายุ → redirect to login
            localStorage.removeItem('token');
            window.location.href = '/login';
        }
        const error = await res.json().catch(() => ({}));
        throw new Error(error.message || `HTTP ${res.status}`);
    }
    
    return res.json();
}

// API methods
export const api = {
    // Auth
    login: (email, password) =>
        request('/auth/login', { method: 'POST', body: JSON.stringify({ email, password }) }),
    register: (email, password) =>
        request('/auth/register', { method: 'POST', body: JSON.stringify({ email, password }) }),
    
    // Entries
    getEntries: () => request('/entries'),
    getEntry: (id) => request(`/entries/${id}`),
    createEntry: (data) => request('/entries', { method: 'POST', body: JSON.stringify(data) }),
    updateEntry: (id, data) => request(`/entries/${id}`, { method: 'PUT', body: JSON.stringify(data) }),
    deleteEntry: (id) => request(`/entries/${id}`, { method: 'DELETE' }),
    
    // Chat
    sendMessage: (entryId, message) =>
        request(`/chat/${entryId}`, { method: 'POST', body: JSON.stringify({ message }) }),
};
```

## 4. useApi Hook — จัดการ Loading + Error + Data

```jsx
// src/hooks/useApi.js
import { useState, useEffect } from 'react';

function useApi(fetchFn, deps = []) {
    const [data, setData] = useState(null);
    const [loading, setLoading] = useState(true);
    const [error, setError] = useState(null);
    
    useEffect(() => {
        let cancelled = false;  // ป้องกัน state update หลัง unmount
        
        async function load() {
            try {
                setLoading(true);
                setError(null);
                const result = await fetchFn();
                if (!cancelled) setData(result);
            } catch (err) {
                if (!cancelled) setError(err.message);
            } finally {
                if (!cancelled) setLoading(false);
            }
        }
        
        load();
        return () => { cancelled = true; };
    }, deps);
    
    return { data, loading, error };
}

// Usage
function EntryList() {
    const { data: entries, loading, error } = useApi(api.getEntries, []);
    
    if (loading) return <Spinner />;
    if (error) return <Error message={error} />;
    return entries.map(e => <EntryItem key={e.id} entry={e} />);
}
```

## 5. Auth Flow ใน React

```jsx
// AuthContext.jsx — จัดการ token ทั้งแอป
import { createContext, useContext, useState } from 'react';
import { api } from '../services/api';

const AuthContext = createContext();

export function AuthProvider({ children }) {
    const [user, setUser] = useState(() => {
        const saved = localStorage.getItem('user');
        return saved ? JSON.parse(saved) : null;
    });
    
    async function login(email, password) {
        const data = await api.login(email, password);
        localStorage.setItem('token', data.token);
        localStorage.setItem('user', JSON.stringify(data.user));
        setUser(data.user);
    }
    
    function logout() {
        localStorage.removeItem('token');
        localStorage.removeItem('user');
        setUser(null);
    }
    
    return (
        <AuthContext.Provider value={{ user, login, logout }}>
            {children}
        </AuthContext.Provider>
    );
}

export const useAuth = () => useContext(AuthContext);
```

## 6. RAG Chat UI — ตัวอย่างสมบูรณ์

```jsx
function ChatWindow({ entryId }) {
    const [messages, setMessages] = useState([]);
    const [input, setInput] = useState('');
    const [sending, setSending] = useState(false);
    const messagesEndRef = useRef(null);
    
    // Auto scroll เมื่อมีข้อความใหม่
    useEffect(() => {
        messagesEndRef.current?.scrollIntoView({ behavior: 'smooth' });
    }, [messages]);
    
    async function handleSend() {
        if (!input.trim() || sending) return;
        
        // เพิ่มข้อความผู้ใช้ทันที (optimistic update)
        const userMsg = { text: input, isUser: true };
        setMessages(prev => [...prev, userMsg]);
        setInput('');
        setSending(true);
        
        try {
            // เพิ่ม loading bubble
            setMessages(prev => [...prev, { text: 'กำลังพิมพ์...', isUser: false, loading: true }]);
            
            const data = await api.sendMessage(entryId, input);
            
            // แทนที่ loading bubble ด้วยคำตอบจริง
            setMessages(prev => [
                ...prev.slice(0, -1),
                { text: data.answer, isUser: false }
            ]);
        } catch (err) {
            setMessages(prev => [
                ...prev.slice(0, -1),
                { text: `❌ ${err.message}`, isUser: false, error: true }
            ]);
        } finally {
            setSending(false);
        }
    }
    
    return (
        <div className="chat-window">
            <div className="messages">
                {messages.map((msg, i) => (
                    <div key={i} className={`bubble ${msg.isUser ? 'user' : 'ai'} ${msg.loading ? 'loading' : ''} ${msg.error ? 'error' : ''}`}>
                        {msg.text}
                    </div>
                ))}
                <div ref={messagesEndRef} />
            </div>
            <div className="input-area">
                <input value={input} onChange={e => setInput(e.target.value)}
                    onKeyPress={e => e.key === 'Enter' && handleSend()}
                    disabled={sending}
                    placeholder="พิมพ์ข้อความ..." />
                <button onClick={handleSend} disabled={sending || !input.trim()}>
                    {sending ? '⏳' : 'ส่ง'}
                </button>
            </div>
        </div>
    );
}
```

## 7. Error & Loading States

```jsx
// Loading State
function Spinner() {
    return <div className="spinner">⏳ กำลังโหลด...</div>;
}

// Error State
function ErrorMessage({ message, onRetry }) {
    return (
        <div className="error-state">
            <p>❌ {message}</p>
            {onRetry && <button onClick={onRetry}>ลองใหม่</button>}
        </div>
    );
}

// Empty State
function EmptyState({ icon, title, description }) {
    return (
        <div className="empty-state">
            <span className="empty-icon">{icon || '📭'}</span>
            <h3>{title}</h3>
            <p>{description}</p>
        </div>
    );
}

// Usage in Component
function EntryList() {
    const { data, loading, error, refetch } = useApi(api.getEntries, []);
    
    if (loading) return <Spinner />;
    if (error) return <ErrorMessage message={error} onRetry={refetch} />;
    if (!data?.length) return <EmptyState icon="📝" title="ไม่มี Entry" description="สร้าง entry แรกเลย!" />;
    
    return <div>{data.map(e => <EntryItem key={e.id} entry={e} />)}</div>;
}
```

## 8. แบบฝึกหัด

สร้าง Registration form ที่:
1. มี input: email, password, confirm password
2. ตรวจสอบ password ตรงกัน ก่อน submit
3. เรียก `api.register(email, password)`
4. แสดง loading (ปุ่ม disable) ระหว่างส่ง
5. แสดง error ถ้า register ล้มเหลว
6. ถ้าสำเร็จ → redirect ไปหน้า login

---

## 🔗 Links

- ก่อนหน้า: [[Frontend/03 React Basics|React Basics]]
- ต้องเข้าใจ: [[Backend/04 JWT Authentication]]
- กลับไป: [[_Curriculum]]
