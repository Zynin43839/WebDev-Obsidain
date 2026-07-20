---
created: 2026-07-02
type: lesson
series: frontend
number: 02
topic: javascript-dom
status: published
---

# JavaScript DOM — ทำให้เว็บมีชีวิต

> **เปรียบ:** HTML = โครงกระดูก, CSS = หน้าตา, **JavaScript DOM = ระบบประสาท**  
> สั่งให้ element เคลื่อนไหว, เปลี่ยน, โต้ตอบกับผู้ใช้

---

## 1. DOM คืออะไร?

**DOM (Document Object Model)** = ตัวแทนของ HTML ในหน่วยความจำของเบราว์เซอร์ ในรูปแบบ **ต้นไม้ (Tree)**

```
HTML ที่เขียน:                    DOM Tree ในเบราว์เซอร์:
<body>                           document
  <header>                         ├── <body>
    <h1>RAG Diary</h1>             │     ├── <header>
  </header>                        │     │     └── <h1> ── "RAG Diary"
  <main>                           │     └── <main>
    <p>Hello</p>                   │           └── <p> ── "Hello"
  </main>                          │
</body>                            └── ...
```

**Flow:** HTML text → DOM Tree → JavaScript แก้ DOM → เบราว์เซอร์ re-render

> **เชื่อมกับ:** [[Fundamental/03 Async Await & Event Loop]] — DOM events ใช้ Event Loop

## 2. การเลือก Elements (Querying)

```javascript
// เลือกอันเดียว (อันแรกที่เจอ)
const header = document.querySelector('header');
const chatWindow = document.querySelector('#chat-window');
const firstBubble = document.querySelector('.chat-bubble');

// เลือกทั้งหมด (ได้ NodeList)
const allBubbles = document.querySelectorAll('.chat-bubble');

// วิธีอื่นๆ (แต่ querySelector ครอบคลุมหมด)
document.getElementById('chat-window');
document.getElementsByClassName('chat-bubble');
document.getElementsByTagName('p');
```

## 3. การแก้ไข Content

```javascript
// เปลี่ยนข้อความ
const title = document.querySelector('h1');
title.textContent = 'My RAG Diary v2';

// เปลี่ยน CSS
const bubble = document.querySelector('.bubble-user');
bubble.style.backgroundColor = '#28a745';

// เปลี่ยน Class
bubble.classList.add('active');
bubble.classList.remove('inactive');
bubble.classList.toggle('highlighted');  // สลับ
```

## 4. สร้างและเพิ่ม Elements

```javascript
// สร้าง element ใหม่
const newBubble = document.createElement('div');
newBubble.className = 'bubble-user';
newBubble.textContent = 'สวัสดี AI!';

// เพิ่มเข้าไปใน DOM
const chatMessages = document.querySelector('#chat-messages');
chatMessages.appendChild(newBubble);    // ต่อท้าย
chatMessages.prepend(newBubble);        // ไว้หน้า
// chatMessages.insertBefore(newBubble, referenceNode);  // แทรก

// ลบ
newBubble.remove();
```

## 5. Events — การโต้ตอบกับผู้ใช้

```javascript
const sendBtn = document.querySelector('#send-btn');
const input = document.querySelector('#chat-input');
const chatMessages = document.querySelector('#chat-messages');

// ดักจับ click
sendBtn.addEventListener('click', () => {
    const text = input.value.trim();
    if (!text) return;
    
    const bubble = document.createElement('div');
    bubble.className = 'bubble-user';
    bubble.textContent = text;
    chatMessages.appendChild(bubble);
    
    input.value = '';
    chatMessages.scrollTop = chatMessages.scrollHeight;
});

// ดักจับ Enter
input.addEventListener('keypress', (event) => {
    if (event.key === 'Enter') {
        sendBtn.click();
    }
});
```

| Event | เกิดเมื่อ |
|-------|---------|
| `click` | คลิก |
| `dblclick` | ดับเบิ้ลคลิก |
| `mouseover` / `mouseout` | ชี้ / เอาออก |
| `keypress` / `keydown` / `keyup` | กดคีย์บอร์ด |
| `submit` | ส่งฟอร์ม |
| `change` | เปลี่ยนค่า input |
| `input` | พิมพ์ใน input (ทุกตัวอักษร) |
| `scroll` | เลื่อนหน้า |

## 6. Event Propagation — Bubbling & Capturing

เมื่อคลิก element ที่ซ้อนกัน — event เดินทาง 3 เฟส:

```
document
  └── <body>
        └── <main>
              └── <div class="bubble">  ← คลิกที่นี้

1. Capturing: document → body → main → div
2. Target: ที่ div
3. Bubbling: div → main → body → document
```

```javascript
// ปกติฟังตอน Bubbling
document.querySelector('main').addEventListener('click', () => {
    console.log('main bubbing');
});

// ฟังตอน Capturing — เพิ่ม true เป็นตัวที่ 3
document.querySelector('main').addEventListener('click', () => {
    console.log('main capturing');
}, true);

// e.stopPropagation() = หยุด bubbling
// e.preventDefault() = ยกเลิกพฤติกรรมปกติ
```

## 7. CRUD Operations กับ DOM

```javascript
// CREATE
function addEntry(title, date) {
    const li = document.createElement('li');
    li.className = 'entry-item';
    li.innerHTML = `<strong>${date}</strong>: ${title}`;
    li.addEventListener('click', () => loadEntry(li));
    document.querySelector('aside ul').prepend(li);
}

// READ
async function loadEntryList() {
    const res = await fetch('/api/entries');
    const entries = await res.json();
    const list = document.querySelector('aside ul');
    list.innerHTML = '';
    entries.forEach(entry => addEntry(entry.title, entry.date));
}

// UPDATE
function updateEntry(entryId, newContent) {
    const bubble = document.querySelector(`[data-entry-id="${entryId}"]`);
    if (bubble) bubble.textContent = newContent;
}

// DELETE
function deleteEntry(entryId) {
    const item = document.querySelector(`[data-entry-id="${entryId}"]`);
    if (item) item.remove();
}
```

## 8. Mini Project — Chat Widget

```html
<div class="chat-container" style="width:400px;margin:20px auto;">
    <div id="messages" style="height:300px;overflow-y:auto;border:1px solid #ddd;padding:8px;display:flex;flex-direction:column;gap:8px;">
        <div style="align-self:flex-start;background:#e9ecef;padding:8px 12px;border-radius:12px;">
            สวัสดี! ถามอะไรก็ได้นะ
        </div>
    </div>
    <div style="display:flex;gap:8px;margin-top:8px;">
        <input id="input" type="text" style="flex:1;padding:8px;" placeholder="พิมพ์ข้อความ...">
        <button id="send" style="padding:8px 16px;background:#007bff;color:white;border:none;border-radius:4px;cursor:pointer;">ส่ง</button>
    </div>
</div>

<script>
    const input = document.getElementById('input');
    const sendBtn = document.getElementById('send');
    const messages = document.getElementById('messages');

    function addMessage(text, isUser) {
        const bubble = document.createElement('div');
        bubble.textContent = text;
        bubble.style.cssText = `
            align-self: ${isUser ? 'flex-end' : 'flex-start'};
            background: ${isUser ? '#007bff' : '#e9ecef'};
            color: ${isUser ? 'white' : 'black'};
            padding: 8px 12px;
            border-radius: 12px;
            max-width: 80%;
        `;
        messages.appendChild(bubble);
        messages.scrollTop = messages.scrollHeight;
    }

    sendBtn.addEventListener('click', () => {
        const text = input.value.trim();
        if (!text) return;
        addMessage(text, true);
        input.value = '';
    });

    input.addEventListener('keypress', (e) => {
        if (e.key === 'Enter') sendBtn.click();
    });
</script>
```

## 9. แบบฝึกหัด

เขียนฟังก์ชัน `loadChatHistory(entryId)` ที่:
1. เรียก `fetch('/api/entries/' + entryId + '/chat')`
2. นำข้อความที่ได้มาแสดงใน `#chat-messages`
3. จัดการ error (แสดงข้อความ error ถ้า fetch ล้มเหลว)

> **Hint:** ใช้ async/await + try/catch (จาก [[Fundamental/03 Async Await & Event Loop]])

---

## 🔗 Links

- ก่อนหน้า: [[Frontend/01 CSS|CSS]]
- ต่อไป: [[Frontend/03 React Basics|React Basics]]
- กลับไป: [[_Curriculum]]
