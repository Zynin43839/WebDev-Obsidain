---
created: 2026-07-02
type: lesson
series: frontend
number: 03
topic: react
status: published
---

# React Basics — สร้าง UI แบบ Component

> **เปรียบ:** Vanilla JS = สร้างบ้านด้วยการตอกตะปูทีละตัว  
> React = ใช้บล็อกเลโก้สำเร็จรูป — ประกอบกันเป็นบ้านเร็วขึ้น

---

## 1. ทำไมต้อง React?

### ปัญหาของ Vanilla JS

```javascript
let messages = [];

function addMessage(text) {
    messages.push(text);
    renderMessages();  // ต้องเรียก render เองทุกครั้ง
}

function renderMessages() {
    const container = document.querySelector('#messages');
    container.innerHTML = '';  // ลบของเก่า → สร้างใหม่ (ช้า)
    messages.forEach(msg => {
        const div = document.createElement('div');
        div.textContent = msg;
        container.appendChild(div);
    });
}
```

**ปัญหา:** ต้องคอย sync data ↔ DOM → ยุ่งเหยิงเมื่อแอปใหญ่

### React แก้ยังไง?

1. **Declarative** — บอก React ว่าอยากได้ UI แบบไหน → React จัดการ DOM ให้
2. **Component-based** — แบ่ง UI เป็นชิ้นเล็กๆ ประกอบกัน
3. **Re-render อัตโนมัติ** — State เปลี่ยน → UI เปลี่ยนตาม (ไม่ต้องเรียก render เอง)

> **เชื่อมกับ:** [[Fundamental/02 TypeScript Basics]] — React ใช้ JSX (JavaScript + XML)

## 2. Components — หัวใจของ React

**Component = ฟังก์ชันที่ return JSX (HTML ใน JavaScript)**

```jsx
// Component ธรรมดา
function Header() {
    return (
        <header>
            <h1>📔 RAG Diary</h1>
        </header>
    );
}

// Component ที่รับ Props
function ChatBubble({ text, isUser }) {
    return (
        <div className={`bubble ${isUser ? 'user' : 'ai'}`}>
            {text}
        </div>
    );
}
```

### การประกอบ Components

```jsx
function App() {
    return (
        <div className="app-layout">
            <Header />
            <Sidebar />
            <main>
                <ChatBubble text="สวัสดี" isUser={true} />
                <ChatBubble text="你好！" isUser={false} />
            </main>
        </div>
    );
}
```

> Component = ฟังก์ชัน → เรียกซ้ำได้, ส่ง Props ต่างกัน → UI ต่างกัน

## 3. Props — ส่งข้อมูลจากแม่ไปลูก

**Props = Properties = ข้อมูลที่ส่งจาก Component แม่ → Component ลูก**

```jsx
// แม่ส่ง Props
function App() {
    const entries = [
        { id: 1, title: 'เรียน TypeScript', date: '2026-07-01' },
        { id: 2, title: 'เรียน Trading', date: '2026-06-30' },
    ];
    
    return (
        <aside>
            {entries.map(entry => (
                <EntryItem 
                    key={entry.id}
                    title={entry.title}
                    date={entry.date}
                />
            ))}
        </aside>
    );
}

// ลูกรับ Props (destructuring)
function EntryItem({ title, date }) {
    return (
        <div className="entry-item">
            <strong>{date}</strong>: {title}
        </div>
    );
}
```

| Props เหมือน | เปรียบ |
|-------------|--------|
| ฟังก์ชัน arguments | ส่งค่าเข้าไปใน component |
| HTML attributes | `<img src="..." alt="...">` |
| ของที่แม่ส่งให้ลูก | ลูกใช้ได้แต่เปลี่ยนไม่ได้ |

## 4. State (useState) — ความจำของ Component

**State = ข้อมูลที่ Component จำไว้ — เมื่อเปลี่ยน → UI re-render อัตโนมัติ**

```jsx
import { useState } from 'react';

function ChatWindow() {
    const [messages, setMessages] = useState([]);  // messages = [], setMessages = setter
    const [input, setInput] = useState('');
    
    function handleSend() {
        if (!input.trim()) return;
        
        // ห้าม push โดยตรง! ต้องใช้ setter
        setMessages([...messages, { text: input, isUser: true }]);
        setInput('');
    }
    
    return (
        <div>
            <div className="messages">
                {messages.map((msg, i) => (
                    <ChatBubble key={i} text={msg.text} isUser={msg.isUser} />
                ))}
            </div>
            <div>
                <input 
                    value={input}
                    onChange={(e) => setInput(e.target.value)}
                    onKeyPress={(e) => e.key === 'Enter' && handleSend()}
                />
                <button onClick={handleSend}>ส่ง</button>
            </div>
        </div>
    );
}
```

### กฏของ State

1. **ห้ามเปลี่ยน state โดยตรง** — ใช้ setter function เสมอ
   ```jsx
   // ❌ ผิด
   messages.push(newMsg);
   
   // ✅ ถูก
   setMessages([...messages, newMsg]);
   ```
2. **State change → Component re-render** — React เรียกฟังก์ชัน Component ใหม่ทั้งก้อน
3. **State อยู่ภายใน Component** — Component อื่นเข้าถึงไม่ได้ (ต้องยก state ขึ้นไป)

## 5. useEffect — Side Effects

**useEffect = ทำอะไรบางอย่างหลังจาก Component แสดงผลเสร็จแล้ว**

เหมาะสำหรับ: ดึง API, setInterval, subscriptions, อัปเดต title

```jsx
import { useState, useEffect } from 'react';

function EntryList() {
    const [entries, setEntries] = useState([]);
    const [loading, setLoading] = useState(true);
    
    // [] = dependencies = รันครั้งเดียว (ตอน mount)
    useEffect(() => {
        async function fetchEntries() {
            try {
                const res = await fetch('/api/entries');
                const data = await res.json();
                setEntries(data);
            } catch (err) {
                console.error('Failed:', err);
            } finally {
                setLoading(false);
            }
        }
        fetchEntries();
    }, []);
    
    if (loading) return <div>Loading...</div>;
    
    return (
        <ul>
            {entries.map(e => <li key={e.id}>{e.title}</li>)}
        </ul>
    );
}
```

### Dependencies Array

| `[]` | รันครั้งเดียว (mount) |
|------|---------------------|
| `[count]` | รันทุกครั้งที่ `count` เปลี่ยน |
| ไม่ใส่ | รันทุกครั้งที่ re-render |
| Return fn | Cleanup — รันก่อน unmount |

```jsx
// Cleanup example
useEffect(() => {
    const timer = setInterval(() => console.log('tick'), 1000);
    return () => clearInterval(timer);  // cleanup ตอน unmount
}, []);
```

## 6. Conditional Rendering

```jsx
function ChatMessage({ message }) {
    if (!message) {
        return <p>ไม่มีข้อความ</p>;  // Early return
    }
    
    return (
        <div className={`bubble ${message.isUser ? 'user' : 'ai'}`}>
            {message.text}
            
            {/* แสดง badge ถ้าเป็น user */}
            {message.isUser && <span className="badge">👤</span>}
            
            {/* แสดง loading ถ้ากำลังโหลด */}
            {message.isLoading ? (
                <span className="loading">กำลังพิมพ์...</span>
            ) : null}
        </div>
    );
}
```

## 7. Lists and Keys

```jsx
function EntryList({ entries }) {
    return (
        <ul>
            {entries.map(entry => (
                <li key={entry.id}>  {/* key = identifier ที่ไม่ซ้ำ */}
                    {entry.title}
                </li>
            ))}
        </ul>
    );
}
```

> **Key:** React ใช้ระบุ item ไหนเปลี่ยน/เพิ่ม/ลบ  
> ใช้ **unique ID** (จาก database) — **ห้ามใช้ index array**

## 8. RAG Diary — Components จริง

```jsx
// App.jsx
import { useState, useEffect } from 'react';
import Header from './Header';
import Sidebar from './Sidebar';
import ChatWindow from './ChatWindow';
import './App.css';

function App() {
    const [entries, setEntries] = useState([]);
    const [selectedEntry, setSelectedEntry] = useState(null);
    const [messages, setMessages] = useState([]);

    useEffect(() => {
        fetch('/api/entries')
            .then(res => res.json())
            .then(data => setEntries(data));
    }, []);

    function handleSelectEntry(entry) {
        setSelectedEntry(entry);
        fetch(`/api/entries/${entry.id}/chat`)
            .then(res => res.json())
            .then(data => setMessages(data));
    }

    return (
        <div className="app-layout">
            <Header />
            <Sidebar entries={entries} onSelect={handleSelectEntry} />
            <main>
                {selectedEntry ? (
                    <ChatWindow entry={selectedEntry} messages={messages} setMessages={setMessages} />
                ) : (
                    <div className="empty-state">
                        <h2>เลือก Entry เพื่อเริ่มแชท</h2>
                    </div>
                )}
            </main>
        </div>
    );
}
```

## 9. แบบฝึกหัด

1. สร้าง `SearchBar` component ที่รับ `onSearch` เป็น prop
2. ใช้ `useState` เก็บ search text
3. เมื่อพิมพ์ → filter entries (client-side)

---

## 🔗 Links

- ก่อนหน้า: [[Frontend/02 JavaScript DOM|JavaScript DOM]]
- ต่อไป: [[Frontend/04 Frontend-Backend Integration|Frontend-Backend Integration]]
- กลับไป: [[_Curriculum]]
