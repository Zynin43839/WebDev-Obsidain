---
created: 2026-07-02
type: note
topic: frontend
status: seedling
---

# Frontend Fundamentals — HTML, CSS, JavaScript DOM, React

> **เปรียบเทียบ:** การสร้างเว็บไซต์ = **การสร้างบ้าน**  
> HTML = โครงสร้าง เสาเข็ม กำแพง  
> CSS = การตกแต่ง ทาสี จัดเฟอร์นิเจอร์  
> JavaScript = ระบบไฟฟ้า ประปา กลไกต่างๆ  
> React = บล็อกเลโก้สำเร็จรูปที่ประกอบกันเป็นบ้านได้เร็วขึ้น

---

## 🔹 Part 1: HTML — โครงสร้างของเว็บ

### HTML คืออะไร?

**HTML (HyperText Markup Language)** = ภาษาที่ใช้บอกเบราว์เซอร์ว่า **หน้านี้มีอะไรบ้าง**

- ไม่ใช่ภาษาโปรแกรม — มันคือ **Markup Language** (ภาษาสำหรับ "ติดป้าย" เนื้อหา)
- ใช้ **Tags** (แท็ก) เพื่อบอกว่า "ส่วนนี้คือหัวข้อ" "ส่วนนี้คือย่อหน้า" "ส่วนนี้คือรูปภาพ"

### โครงสร้างพื้นฐานของ HTML

```html
<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>RAG Diary</title>
</head>
<body>
    <header>
        <h1>My RAG Diary</h1>
    </header>
    <main>
        <p>ยินดีต้อนรับ!</p>
    </main>
</body>
</html>
```

| ส่วน | ทำอะไร | เปรียบ |
|------|--------|--------|
| `<!DOCTYPE html>` | บอกเบราว์เซอร์ว่าใช้ HTML5 | ป้ายทะเบียนบ้าน |
| `<html>` | รากของทุกอย่าง | ตัวบ้านทั้งหลัง |
| `<head>` | ข้อมูลที่ไม่แสดงบนหน้า (meta, title, link CSS) | แบบบ้าน, ทะเบียน |
| `<body>` | เนื้อหาที่ผู้ใช้เห็น | ของในบ้านที่คุณมองเห็น |
| `<meta charset="UTF-8">` | รองรับภาษาไทย, emoji | สายไฟที่รองรับทุกภาษา |

### Semantic Tags — ทำไมต้องใช้?

**Divitis = โรคที่ทุกอย่างเป็น `<div>` หมด**

ในอดีต — นักพัฒนาสร้างทุกอย่างด้วย `<div>`:
```html
<!-- ❌ แบบเก่า - Divitis -->
<div class="header">
    <div class="title">My RAG Diary</div>
</div>
<div class="sidebar">
    <div class="entry">Entry 1</div>
</div>
<div class="main-content">
    <div class="chat">...</div>
</div>
```

ปัญหาคือ:
- โค้ดอ่านยาก — ไม่รู้ว่ากล่องไหนคืออะไร
- Screen reader (โปรแกรมอ่านหน้าจอคนตาบอด) ทำงานไม่ได้
- SEO (Search Engine Optimization) แย่ — Google ไม่เข้าใจโครงสร้าง

**Semantic Tags = แท็กที่บอกความหมายในตัว**

```html
<!-- ✅ แบบใหม่ - Semantic -->
<header>
    <h1>My RAG Diary</h1>
</header>
<aside>
    <article>Entry 1</article>
    <article>Entry 2</article>
</aside>
<main>
    <section id="chat-window">
        <p>แชทกับ AI...</p>
    </section>
</main>
```

| Semantic Tag | ใช้ทำอะไร | แทน `<div>` ที่ |
|-------------|-----------|----------------|
| `<header>` | หัวเว็บไซต์ หรือหัวของ section | `<div class="header">` |
| `<nav>` | เมนูนำทาง | `<div class="nav">` |
| `<main>` | เนื้อหาหลักของหน้า (มีได้ 1 อัน) | `<div class="main">` |
| `<aside>` | เนื้อหาข้างเคียง (sidebar) | `<div class="sidebar">` |
| `<section>` | กลุ่มเนื้อหาที่เกี่ยวข้องกัน | `<div class="section">` |
| `<article>` | เนื้อหาอิสระ (โพสต์, entry) | `<div class="post">` |
| `<footer>` | ท้ายเว็บไซต์ | `<div class="footer">` |

> **ข้อควรจำ:** Semantic Tags ช่วยให้โค้ดสื่อความหมาย — ทั้งกับมนุษย์, โปรแกรม, และ Search Engine

### แบบฝึกหัด HTML — RAG Diary Structure

ลองเขียนโครงสร้าง HTML สำหรับ RAG Diary:

```html
<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>RAG Diary</title>
</head>
<body>
    <!-- Header: ชื่อแอป -->
    <header>
        <h1>📔 RAG Diary</h1>
        <p>ไดอารี่ที่คุยกับ AI ได้</p>
    </header>

    <!-- Sidebar: รายการไดอารี่เก่า -->
    <aside>
        <h2>Entries</h2>
        <ul>
            <li>2026-07-01: เรียน TypeScript</li>
            <li>2026-06-30: เรียน Trading</li>
            <li>2026-06-29: เรียน REST API</li>
        </ul>
    </aside>

    <!-- Main: หน้าต่างแชท -->
    <main>
        <section id="chat-messages">
            <p><strong>คุณ:</strong> จำได้ไหมว่าเมื่อวานเราเรียนอะไร?</p>
            <p><strong>AI:</strong> เมื่อวานคุณเรียน Trading ครับ</p>
        </section>
        
        <section id="chat-input">
            <input type="text" placeholder="พิมพ์ข้อความ...">
            <button>ส่ง</button>
        </section>
    </main>

    <!-- Footer -->
    <footer>
        <p>Powered by RAG + Supabase</p>
    </footer>
</body>
</html>
```

---

## 🔹 Part 2: CSS — ทำให้สวยงามและเป็นระเบียบ

### CSS คืออะไร?

**CSS (Cascading Style Sheets)** = ภาษาที่ใช้บอกว่า **HTML ควรมีหน้าตาเป็นยังไง**

- สี, ขนาด, ระยะห่าง, ตำแหน่ง, อนิเมชั่น
- ทำให้เว็บ responsive (ปรับตามขนาดหน้าจอ)
- แยกหน้าตาออกจากเนื้อหา — เปลี่ยน CSS ก็เปลี่ยนลุคทั้งเว็บ

### 3 วิธีเขียน CSS

**1. Inline** — เขียนตรงในแท็ก (ห้ามใช้ ถ้าไม่จำเป็น)
```html
<p style="color: red; font-size: 16px;">ข้อความแดง</p>
```

**2. Internal** — เขียนใน `<style>` ใน `<head>`
```html
<head>
    <style>
        p { color: red; }
    </style>
</head>
```

**3. External ✅** — แยกไฟล์ `.css` (วิธีที่ถูกต้อง)
```html
<head>
    <link rel="stylesheet" href="style.css">
</head>
```

### CSS Selectors — เลือกว่าจะตกแต่งอันไหน

```css
/* Tag selector — เลือกทุก <p> */
p { color: blue; }

/* Class selector — เลือกทุก element ที่มี class="chat-bubble" */
.chat-bubble { background: #f0f0f0; }

/* ID selector — เลือก element ที่มี id="chat-window" */
#chat-window { border: 1px solid #ccc; }

/* Descendant selector — เลือก <p> ที่อยู่ใน <aside> */
aside p { font-size: 14px; }

/* Multiple selector — เลือกหลายอย่างพร้อมกัน */
h1, h2, h3 { font-family: 'Sarabun', sans-serif; }
```

> **หลักการสำคัญ:** Specificity = ค่าความจำเพาะ  
> ID > Class > Tag  
> ถ้า specificity เท่ากัน — อันที่เขียนทีหลังมีผล

### Box Model — ทุกอย่างคือกล่อง

CSS มองทุก element เป็นกล่อง 4 ชั้น:

```
┌──────────────────────────────────┐
│         MARGIN (ระยะห่างนอก)      │
│  ┌────────────────────────────┐  │
│  │       BORDER (เส้นขอบ)      │  │
│  │  ┌──────────────────────┐  │  │
│  │  │    PADDING (ระยะใน)    │  │  │
│  │  │  ┌────────────────┐  │  │  │
│  │  │  │   CONTENT       │  │  │  │
│  │  │  │   (เนื้อหา)      │  │  │  │
│  │  │  └────────────────┘  │  │  │
│  │  └──────────────────────┘  │  │
│  └────────────────────────────┘  │
└──────────────────────────────────┘
```

```css
.chat-bubble {
    /* Content */
    width: 300px;
    height: auto;
    
    /* Padding — ระยะระหว่าง content กับ border */
    padding: 12px 16px;        /* บน-ล่าง 12px, ซ้าย-ขวา 16px */
    
    /* Border */
    border: 1px solid #ddd;
    border-radius: 8px;         /* มุมโค้ง */
    
    /* Margin — ระยะระหว่างกล่องนี้กับกล่องอื่น */
    margin-bottom: 8px;
}
```

**ทำไม Box Model ถึงสำคัญ?**  
การเข้าใจ Box Model = รู้ว่าทำไม element ถึงมีขนาดไม่ตรงกับที่ตั้ง width ไว้

```css
/* ปกติ width = ขนาดของ content เท่านั้น */
.box {
    width: 200px;            /* content = 200px */
    padding: 20px;           /* +40px (ซ้าย+ขวา) */
    border: 2px solid black; /* +4px */
    /* รวม = 200 + 40 + 4 = 244px */  ← งงไหม?
}

/* ใช้ box-sizing: border-box แก้ */
.box {
    box-sizing: border-box;  /* width รวม padding+border แล้ว */
    width: 200px;            /* content = 200 - 40 - 4 = 156px */
    padding: 20px;
    border: 2px solid black;
    /* รวม = 156 + 40 + 4 = 200px */  ← ตรงตามที่ตั้ง
}
```

> **คำแนะนำ:** ใช้ `*, *::before, *::after { box-sizing: border-box; }` ทุกโปรเจค

### Flexbox — จัดเรียง 1 มิติ

**Flexbox = เครื่องมือจัดเรียง element ในแถวหรือคอลัมน์**

กรณีใช้: เรียงของในแนวเดียวกัน, จัดกึ่งกลาง, จัดระยะห่างเท่ากัน

```css
/* เซ็ต container เป็น flex */
.chat-container {
    display: flex;
    flex-direction: column;   /* column = เรียงแนวตั้ง, row = แนวนอน */
    gap: 8px;                 /* ระยะห่างระหว่าง item */
}

/* จัด chat bubble ของผู้ใช้ให้ชิดขวา */
.bubble-user {
    align-self: flex-end;     /* ชิดขวา */
    background: #007bff;
    color: white;
}

/* จัด chat bubble ของ AI ให้ชิดซ้าย */
.bubble-ai {
    align-self: flex-start;   /* ชิดซ้าย */
    background: #f0f0f0;
}
```

| Property | ค่า | ความหมาย |
|----------|-----|----------|
| `display: flex` | — | เปิดใช้งาน Flexbox |
| `flex-direction` | `row` / `column` | แกนหลัก (แนวนอน/แนวตั้ง) |
| `justify-content` | `center` / `space-between` / `flex-end` | จัดตามแกนหลัก |
| `align-items` | `center` / `stretch` / `flex-start` | จัดตามแกนรอง |
| `flex-wrap` | `wrap` | ขึ้นบรรทัดใหม่เมื่อเกิน |
| `gap` | `8px` | ระยะห่างระหว่าง item |
| `flex` | `1` | สัดส่วนการขยายตัว |
| `align-self` | `flex-end` / `center` | จัดตำแหน่งเฉพาะตัวนี้ |

### CSS Grid — จัดเรียง 2 มิติ

**Grid = จัดเรียงแบบตาราง — ทั้งแถวและคอลัมน์**

กรณีใช้: layout ใหญ่ของหน้า (sidebar + main), ตารางรูปภาพ

```css
/* Layout หลักของ RAG Diary */
.app-layout {
    display: grid;
    grid-template-columns: 250px 1fr;  /* sidebar 250px, main ที่เหลือ */
    grid-template-rows: auto 1fr auto; /* header, content, footer */
    grid-template-areas:
        "header header"
        "sidebar main"
        "footer footer";
    height: 100vh;                     /* เต็มจอ */
}

header { grid-area: header; }
aside  { grid-area: sidebar; }
main   { grid-area: main; }
footer { grid-area: footer; }
```

**Flexbox vs Grid — ใช้ตอนไหน?**

| Flexbox | Grid |
|---------|------|
| 1 มิติ (แถว หรือ คอลัมน์) | 2 มิติ (แถว และ คอลัมน์) |
| เนื้อหาเป็นตัวกำหนดขนาด | layout เป็นตัวกำหนดขนาด |
| Chat bubbles, navbar, centering | หน้า layout ทั้งหมด, dashboard |
| "เรียงของในแถวนี้ให้สวย" | "แบ่งหน้าจอเป็นโซนแบบนี้" |

### Responsive Design — รองรับทุกหน้าจอ

**Responsive Design = เว็บปรับตัวตามขนาดหน้าจอ โดยไม่ต้องสร้างแยก**

3 เสาหลักของ Responsive Design:
1. **Fluid Grids** — ใช้ % แทน px
2. **Flexible Images** — รูปไม่ล้นจอ
3. **Media Queries** — เปลี่ยน CSS ตาม ขนาดหน้าจอ

**Media Queries — เปลี่ยน CSS ตาม breakpoint:**

```css
/* พื้นฐาน: จอใหญ่ (desktop) */
.app-layout {
    display: grid;
    grid-template-columns: 250px 1fr;
}

/* จอขนาดกลาง: tablet */
@media (max-width: 768px) {
    .app-layout {
        grid-template-columns: 1fr;  /* sidebar หาย, main กว้างเต็ม */
    }
    aside {
        display: none;  /* ซ่อน sidebar */
    }
}

/* จอเล็ก: มือถือ */
@media (max-width: 480px) {
    header h1 { font-size: 18px; }
    .chat-bubble { font-size: 14px; }
}
```

> **หลักการ:** Mobile-first = เริ่มจากจอเล็กก่อน แล้วค่อยเพิ่มของสำหรับจอใหญ่  
> หรือ Desktop-first = เริ่มจากจอใหญ่ แล้วค่อยซ่อนสำหรับจอเล็ก

### แบบฝึกหัด CSS — จัด RAG Diary

```css
/* Global Reset */
*, *::before, *::after {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}

body {
    font-family: 'Sarabun', sans-serif;
    background: #f5f5f5;
    color: #333;
}

/* Layout */
.app-layout {
    display: grid;
    grid-template-columns: 280px 1fr;
    grid-template-rows: auto 1fr auto;
    grid-template-areas:
        "header header"
        "sidebar main"
        "footer footer";
    min-height: 100vh;
}

/* Header */
header {
    grid-area: header;
    background: #2c3e50;
    color: white;
    padding: 16px 24px;
}

/* Sidebar */
aside {
    grid-area: sidebar;
    background: white;
    padding: 16px;
    border-right: 1px solid #ddd;
}

.entry-item {
    padding: 12px;
    margin-bottom: 4px;
    border-radius: 6px;
    cursor: pointer;
    transition: background 0.2s;
}

.entry-item:hover {
    background: #e8f4fd;
}

/* Main chat area */
main {
    grid-area: main;
    display: flex;
    flex-direction: column;
    padding: 16px;
}

#chat-messages {
    flex: 1;
    overflow-y: auto;
    display: flex;
    flex-direction: column;
    gap: 8px;
}

.bubble-user {
    align-self: flex-end;
    background: #007bff;
    color: white;
    padding: 10px 16px;
    border-radius: 16px 16px 4px 16px;
    max-width: 70%;
}

.bubble-ai {
    align-self: flex-start;
    background: #e9ecef;
    padding: 10px 16px;
    border-radius: 16px 16px 16px 4px;
    max-width: 70%;
}

/* Footer */
footer {
    grid-area: footer;
    background: #2c3e50;
    color: white;
    text-align: center;
    padding: 12px;
    font-size: 12px;
}

/* Responsive */
@media (max-width: 768px) {
    .app-layout {
        grid-template-columns: 1fr;
        grid-template-areas:
            "header"
            "main"
            "footer";
    }
    aside {
        display: none;
    }
}
```

---

## 🔹 Part 3: JavaScript DOM — ทำให้เว็บมีชีวิต

### DOM คืออะไร?

**DOM (Document Object Model)** = ตัวแทนของ HTML ในหน่วยความจำของเบราว์เซอร์ ในรูปแบบ **ต้นไม้ (Tree)**

เมื่อเบราว์เซอร์โหลด HTML:
1. อ่าน HTML เป็น text
2. แปลงเป็น **DOM Tree** (โครงสร้างต้นไม้ในหน่วยความจำ)
3. JavaScript สามารถเข้าไปแก้ไข DOM Tree ได้ตลอดเวลา
4. ทุกครั้งที่ DOM เปลี่ยน → หน้าจอเปลี่ยนตาม

```
HTML ที่เขียน:                    DOM Tree ในเบราว์เซอร์:
                              document
                                │
<body>                        <body>
  <header>                     ├── <header>
    <h1>RAG Diary</h1>         │     └── <h1> ── "RAG Diary"
  </header>                    │
  <main>                       ├── <main>
    <p>Hello</p>               │     └── <p> ── "Hello"
  </main>                      │
</body>                        └── (ต้นไม้ย่อยต่อไป)
```

### การเลือก Elements (Querying)

ก่อนจะแก้ไขอะไร — ต้องเลือกก่อนว่าจะแก้ไขอันไหน:

```javascript
// เลือกอันเดียว (อันแรกที่เจอ)
const header = document.querySelector('header');
const chatWindow = document.querySelector('#chat-window');
const firstBubble = document.querySelector('.chat-bubble');

// เลือกทั้งหมด (ได้ Array-like NodeList)
const allBubbles = document.querySelectorAll('.chat-bubble');
const allButtons = document.querySelectorAll('button');

// วิธีอื่นๆ
document.getElementById('chat-window');        // เลือกจาก ID
document.getElementsByClassName('chat-bubble'); // เลือกจาก class
document.getElementsByTagName('p');             // เลือกจาก tag
```

### การแก้ไข Content

```javascript
// เปลี่ยนข้อความ
const title = document.querySelector('h1');
title.textContent = 'My RAG Diary v2';

// เปลี่ยน HTML ข้างใน
const chatMessages = document.querySelector('#chat-messages');
chatMessages.innerHTML = '<p>Hello AI!</p>';  // ระวัง XSS!

// เปลี่ยน CSS
const bubble = document.querySelector('.bubble-user');
bubble.style.backgroundColor = '#28a745';
bubble.style.display = 'block';
```

### การสร้างและเพิ่ม Elements

```javascript
// สร้าง element ใหม่ในหน่วยความจำ
const newBubble = document.createElement('div');
newBubble.className = 'bubble-user';
newBubble.textContent = 'สวัสดี AI!';

// เพิ่มเข้าไปใน DOM
const chatMessages = document.querySelector('#chat-messages');
chatMessages.appendChild(newBubble);  // ต่อท้าย
// หรือ chatMessages.prepend(newBubble);  // ไว้หน้า
// หรือ chatMessages.insertBefore(newBubble, referenceNode);  // แทรก

// ลบ element
newBubble.remove();
// หรือ chatMessages.removeChild(newBubble);
```

### Events — การโต้ตอบกับผู้ใช้

**Event = สิ่งที่ผู้ใช้ทำบนหน้าเว็บ** เช่น คลิก, พิมพ์, เลื่อน

```javascript
const sendButton = document.querySelector('#send-btn');
const input = document.querySelector('#chat-input');
const chatMessages = document.querySelector('#chat-messages');

// ดักจับ click
sendButton.addEventListener('click', function() {
    const text = input.value;
    if (text.trim() === '') return;
    
    // สร้าง bubble ของผู้ใช้
    const bubble = document.createElement('div');
    bubble.className = 'bubble-user';
    bubble.textContent = text;
    chatMessages.appendChild(bubble);
    
    // เคลียร์ Input
    input.value = '';
    
    // เลื่อนลงล่างสุด
    chatMessages.scrollTop = chatMessages.scrollHeight;
});

// ดักจับ Enter (กด Enter = ส่ง)
input.addEventListener('keypress', function(event) {
    if (event.key === 'Enter') {
        sendButton.click();  // เรียก click ของปุ่มส่ง
    }
});
```

| Event | เกิดเมื่อ |
|-------|---------|
| `click` | ผู้ใช้คลิก |
| `dblclick` | ดับเบิ้ลคลิก |
| `mouseover` / `mouseout` | เอาเมาส์ชี้ / เอาออก |
| `keypress` / `keydown` / `keyup` | กดคีย์บอร์ด |
| `submit` | ส่งฟอร์ม |
| `change` | เปลี่ยนค่า input |
| `input` | พิมพ์ใน input (ทุกตัวอักษร) |
| `scroll` | เลื่อนหน้า |
| `load` | โหลดเสร็จ |

### Event Propagation — Bubbling & Capturing

เมื่อคุณคลิก element ที่ซ้อนกัน — event จะเดินทางเป็น 3 เฟส:

```
document
  └── <body>
        └── <main>
              └── <div class="bubble-user">  ← คลิกที่นี้
              
1. Capturing (จากบนลงล่าง): document → body → main → div
2. Target: ที่ div
3. Bubbling (จากล่างขึ้นบน): div → main → body → document
```

```javascript
// โดยปกติ addEventListener ฟังตอน Bubbling phase
// ถ้าอยากฟังตอน Capturing — เพิ่ม true เป็นตัวที่ 3

document.querySelector('main').addEventListener('click', function(e) {
    console.log('main ถูกคลิก (bubbling)');
});

document.querySelector('main').addEventListener('click', function(e) {
    console.log('main ถูกคลิก (capturing)');
}, true);

// e.stopPropagation() = หยุดไม่ให้ event bubbling ต่อ
// e.preventDefault() = ยกเลิกพฤติกรรมปกติ (เช่น ไม่ให้ submit form)
```

### CRUD Operations กับ DOM (สำหรับ RAG Diary)

```javascript
// === CREATE: เพิ่ม entry ใหม่ ===
function addEntry(title, date, content) {
    const list = document.querySelector('aside ul');
    
    const li = document.createElement('li');
    li.className = 'entry-item';
    li.innerHTML = `<strong>${date}</strong>: ${title}`;
    
    li.addEventListener('click', () => loadEntry(li));
    list.prepend(li);
}

// === READ: โหลด entry list ===
async function loadEntryList() {
    const response = await fetch('/api/entries');
    const entries = await response.json();
    
    const list = document.querySelector('aside ul');
    list.innerHTML = '';  // ลบของเก่า
    
    entries.forEach(entry => {
        addEntry(entry.title, entry.date, entry.content);
    });
}

// === UPDATE: แก้ไข entry ===
function updateEntry(entryId, newContent) {
    const bubble = document.querySelector(`[data-entry-id="${entryId}"]`);
    if (bubble) {
        bubble.textContent = newContent;
    }
}

// === DELETE: ลบ entry ===
function deleteEntry(entryId) {
    const listItem = document.querySelector(`[data-entry-id="${entryId}"]`);
    if (listItem) {
        listItem.remove();
    }
}
```

### แบบฝึกหัด JS DOM — Chat Widget อย่างง่าย

```html
<!DOCTYPE html>
<html>
<head>
    <title>Simple Chat</title>
    <style>
        /* CSS ที่เขียนไปแล้วข้างบน */
    </style>
</head>
<body>
    <div class="chat-container" style="width: 400px; margin: 20px auto;">
        <div id="messages" style="height: 300px; overflow-y: auto; border: 1px solid #ddd; padding: 8px; display: flex; flex-direction: column; gap: 8px;">
            <div style="align-self: flex-start; background: #e9ecef; padding: 8px 12px; border-radius: 12px;">
                สวัสดี! ถามอะไรก็ได้นะ
            </div>
        </div>
        <div style="display: flex; gap: 8px; margin-top: 8px;">
            <input id="input" type="text" style="flex: 1; padding: 8px; border: 1px solid #ddd; border-radius: 4px;" placeholder="พิมพ์ข้อความ...">
            <button id="send" style="padding: 8px 16px; background: #007bff; color: white; border: none; border-radius: 4px; cursor: pointer;">ส่ง</button>
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
</body>
</html>
```

---

## 🔹 Part 4: React — สร้าง UI แบบ Component

### ทำไมต้อง React? — ปัญหาของ Vanilla JS

การจัดการ DOM ด้วย JavaScript ตรงๆ มีปัญหา:

```javascript
// ปัญหา: ถ้า state (ข้อมูล) เปลี่ยน → ต้องแก้ DOM เองทุกครั้ง
let messages = [];

function addMessage(text) {
    messages.push(text);           // อัปเดต data
    renderMessages();              // ต้องเรียก render เอง
}

function renderMessages() {
    const container = document.querySelector('#messages');
    container.innerHTML = '';      // ลบของเก่าทิ้ง
    messages.forEach(msg => {
        const div = document.createElement('div');
        div.textContent = msg;
        container.appendChild(div);
    });
}
```

เมื่อแอปใหญ่ขึ้น → มี state หลายตัว → ต้องคอย sync data กับ DOM → **ยุ่งเหยิง**

React แก้โดย:
1. **Declarative** — บอก React ว่าอยากได้ UI แบบไหน → React จัดการ DOM ให้
2. **Component-based** — แบ่ง UI เป็นชิ้นเล็กๆ
3. **Re-render อัตโนมัติ** — เมื่อ state เปลี่ยน → UI เปลี่ยนตาม

### Components — หัวใจของ React

**Component = ฟังก์ชันที่ return JSX (HTML ใน JavaScript)**

```jsx
// Component แบบฟังก์ชัน
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

**การนำ Component มาใช้:**

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

> **ข้อแตกต่าง:** Component = ฟังก์ชัน → เรียกซ้ำได้, ส่ง Props ต่างกัน → UI ต่างกัน

### Props — ส่งข้อมูลจากแม่ไปลูก

**Props = Properties = ข้อมูลที่ส่งจาก Component แม่ไป Component ลูก**

```jsx
// แม่ส่ง props
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

// ลูกรับ props
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
| ฟังก์ชัน arguments | ส่งค่าเข้าไป |
| HTML attributes | `<img src="..." alt="...">` |
| ของที่แม่ส่งให้ลูก | ลูกใช้ได้แต่เปลี่ยนไม่ได้ |

### State (useState) — ความจำของ Component

**State = ข้อมูลที่ Component จำไว้ — เมื่อเปลี่ยน → UI re-render อัตโนมัติ**

```jsx
import { useState } from 'react';

function ChatWindow() {
    // useState คืนค่า: [ตัวแปร, ฟังก์ชันเปลี่ยนค่า]
    const [messages, setMessages] = useState([]);
    const [input, setInput] = useState('');
    
    function handleSend() {
        if (!input.trim()) return;
        
        // เพิ่มข้อความใหม่
        setMessages([...messages, { 
            text: input, 
            isUser: true 
        }]);
        setInput('');
        
        // TODO: ส่งให้ API แล้วค่อยเพิ่มข้อความตอบ
    }
    
    return (
        <div>
            <div className="messages">
                {messages.map((msg, i) => (
                    <ChatBubble key={i} text={msg.text} isUser={msg.isUser} />
                ))}
            </div>
            <div className="input-area">
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

**กฏของ State:**
1. **ห้ามเปลี่ยน state โดยตรง** — ต้องใช้ setter function เสมอ
   ```jsx
   // ❌ ผิด
   messages.push(newMsg);
   
   // ✅ ถูก
   setMessages([...messages, newMsg]);
   ```
2. **State change → Component re-render** — React เรียกฟังก์ชัน Component ใหม่
3. **State อยู่ภายใน Component** — Component อื่นเข้าถึงไม่ได้

### useEffect — side effects

**useEffect = ทำอะไรบางอย่างหลังจาก Component แสดงผลเสร็จแล้ว**

เหมาะสำหรับ:
- ดึงข้อมูลจาก API
- จับเวลา (setInterval)
- Subscriptions
- อัปเดต document title

```jsx
import { useState, useEffect } from 'react';

function EntryList() {
    const [entries, setEntries] = useState([]);
    const [loading, setLoading] = useState(true);
    
    // useEffect ที่ dependencies [] = รันครั้งเดียว (ตอน mount)
    useEffect(() => {
        async function fetchEntries() {
            try {
                const res = await fetch('/api/entries');
                const data = await res.json();
                setEntries(data);
            } catch (err) {
                console.error('Failed to load entries:', err);
            } finally {
                setLoading(false);
            }
        }
        
        fetchEntries();
    }, []);  // [] = รันแค่ครั้งแรก
    
    if (loading) return <div>Loading...</div>;
    
    return (
        <ul>
            {entries.map(entry => (
                <li key={entry.id}>{entry.title}</li>
            ))}
        </ul>
    );
}
```

**Dependencies Array — ควบคุมว่า useEffect จะรันตอนไหน:**

| `[]` | รันครั้งเดียว (mount) |
|------|---------------------|
| `[count]` | รันทุกครั้งที่ `count` เปลี่ยน |
| ไม่ใส่ | รันทุกครั้งที่ re-render |
| Return function | Cleanup — รันก่อน unmount |

```jsx
// useEffect กับ Cleanup
useEffect(() => {
    const timer = setInterval(() => {
        console.log('tick');
    }, 1000);
    
    // Cleanup — รันเมื่อ Component ถูกทำลาย
    return () => {
        clearInterval(timer);
    };
}, []);
```

### Conditional Rendering — แสดง/ซ่อน ตามเงื่อนไข

```jsx
function ChatMessage({ message }) {
    if (!message) {
        return <p>ไม่มีข้อความ</p>;  // Early return
    }
    
    return (
        <div className={`bubble ${message.isUser ? 'user' : 'ai'}`}>
            {message.text}
            
            {/* Ternary — แสดง badge */}
            {message.isUser && <span className="badge">👤</span>}
            
            {/* Ternary — แสดง/ซ่อน */}
            {message.isLoading ? (
                <span className="loading">กำลังพิมพ์...</span>
            ) : null}
        </div>
    );
}
```

### Lists and Keys

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

> **Key คืออะไร?** — React ใช้ key ระบุว่า item ไหนเปลี่ยน, เพิ่ม, หรือลบ  
> ควรใช้ **unique ID** (จาก database) — ห้ามใช้ index array (เด็ดขาด!)

### RAG Diary Components — ตัวอย่างสมบูรณ์

```jsx
// App.jsx — Component หลัก
import { useState, useEffect } from 'react';
import Header from './Header';
import Sidebar from './Sidebar';
import ChatWindow from './ChatWindow';
import './App.css';

function App() {
    const [entries, setEntries] = useState([]);
    const [selectedEntry, setSelectedEntry] = useState(null);
    const [messages, setMessages] = useState([]);

    // โหลด entry list ตอนเริ่ม
    useEffect(() => {
        fetch('/api/entries')
            .then(res => res.json())
            .then(data => setEntries(data));
    }, []);

    function handleSelectEntry(entry) {
        setSelectedEntry(entry);
        // โหลดประวัติแชทของ entry นั้น
        fetch(`/api/entries/${entry.id}/chat`)
            .then(res => res.json())
            .then(data => setMessages(data));
    }

    return (
        <div className="app-layout">
            <Header />
            <Sidebar 
                entries={entries}
                onSelect={handleSelectEntry}
            />
            <main>
                {selectedEntry ? (
                    <ChatWindow 
                        entry={selectedEntry}
                        messages={messages}
                        setMessages={setMessages}
                    />
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

---

## 🔹 สรุป: ความสัมพันธ์ของ HTML ↔ CSS ↔ JS ↔ React

```
HTML = โครงสร้าง           <--- รู้เท่านี้ก็สร้างเว็บได้ (แต่โหลน)
CSS  = ความสวยงาม          <--- เพิ่ม CSS = เว็บสวยขึ้น
JS   = ความมีชีวิต          <--- เพิ่ม JS = โต้ตอบได้
React = จัดระเบียบ          <--- เพิ่ม React = สร้างแอปใหญ่ได้เป็นระบบ
```

| Layer | ภาษา | หน้าที่ |
|-------|------|--------|
| **Structure** | HTML | บอกว่า "มีอะไรบ้าง" |
| **Presentation** | CSS | บอกว่า "หน้าตาเป็นยังไง" |
| **Behavior** | JavaScript | บอกว่า "โต้ตอบยังไง" |
| **Framework** | React | บอกว่า "จัดระเบียบยังไงให้เป็นระบบ" |

### Flow การทำงานของ Frontend

```
User พิมพ์ข้อความ → กดส่ง
         ↓
[1] Event Listener (JS/React) รับ click/keypress
         ↓
[2] อ่านค่า input → สร้าง object ข้อความ
         ↓
[3] เรียก API: fetch('/api/chat', { method: 'POST', body: ... })
         ↓
[4] รอ response → ได้ข้อความตอบกลับจาก AI
         ↓
[5] อัปเดต State → React re-render DOM
         ↓
[6] เบราว์เซอร์วาด UI ใหม่ → ผู้ใช้เห็นข้อความตอบ
```

---

## 🔹 แบบฝึกหัดท้ายบท

1. **HTML:** สร้างโครงสร้างหน้า login สำหรับ RAG Diary (มี form: email, password, login button)
2. **CSS:** ทำให้ sidebar ของ RAG Diary responsive — บนมือถือให้ซ่อน sidebar แล้วแสดงปุ่ม hamburger
3. **DOM JS:** เขียนฟังก์ชัน `loadChatHistory(entryId)` ที่ดึงข้อความจาก API แล้วแสดงใน `#chat-messages`
4. **React:** แปลง ChatBubble Components (จาก JS DOM) เป็น React Component ที่รับ `text` และ `isUser` เป็น Props และใช้ `useState` จัดการ input

---

## 🔹 Links

- ก่อนหน้า: [[TypeScript Basics]]
- ต่อไป: [[REST API JSON]]
- กลับไปที่ [[_Curriculum]] หรือ [[Database Design & SQL Deep Dive]]

---

*"Frontend = สิ่งที่ผู้ใช้เห็น ถ้ามันสวยและใช้งานง่าย = ผู้ใช้ happy"*
