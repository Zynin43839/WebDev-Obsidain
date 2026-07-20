---
created: 2026-07-02
type: lesson
series: frontend
number: 01
topic: css
status: published
---

# CSS — ทำให้เว็บสวยงาม

> **เปรียบ:** HTML = โครงกระดูก, CSS = เสื้อผ้า+เครื่องสำอาง  
> ถ้า HTML คือบ้าน โครงสร้างครบ CSS คือการทาสี จัดเฟอร์นิเจอร์

---

## 1. CSS คืออะไร?

**CSS (Cascading Style Sheets)** = ภาษาที่บอกว่า HTML **ควรมีหน้าตาเป็นยังไง**
- สี, ขนาด, ระยะห่าง, ตำแหน่ง, อนิเมชั่น
- ทำให้เว็บ responsive (ปรับตามขนาดหน้าจอ)
- แยกหน้าตาออกจากเนื้อหา — เปลี่ยน CSS ก็เปลี่ยนลุคทั้งเว็บ

## 2. 3 วิธีเขียน CSS

```html
<!-- 1. Inline ❌ ไม่ควรใช้ -->
<p style="color: red;">ข้อความแดง</p>

<!-- 2. Internal ⚠️ ใช้ได้ในไฟล์เดี่ยว -->
<head>
    <style>
        p { color: red; }
    </style>
</head>

<!-- 3. External ✅ วิธีที่ถูกต้อง -->
<head>
    <link rel="stylesheet" href="style.css">
</head>
```

## 3. Selectors — เลือกว่าจะตกแต่งอันไหน

```css
/* Tag selector — เลือกทุก <p> */
p { color: blue; }

/* Class selector — เลือกทุก .chat-bubble */
.chat-bubble { background: #f0f0f0; }

/* ID selector — เลือก #chat-window */
#chat-window { border: 1px solid #ccc; }

/* Descendant — <p> ใน <aside> */
aside p { font-size: 14px; }

/* Multiple — h1, h2, h3 พร้อมกัน */
h1, h2, h3 { font-family: 'Sarabun', sans-serif; }
```

> **Specificity:** ค่าความจำเพาะ — ID > Class > Tag  
> ถ้าเท่ากัน — อันที่เขียนทีหลังมีผล

## 4. Box Model — ทุกอย่างคือกล่อง

```
┌──────────────────────────────────┐
│         MARGIN (ระยะห่างนอก)      │
│  ┌────────────────────────────┐  │
│  │       BORDER (เส้นขอบ)      │  │
│  │  ┌──────────────────────┐  │  │
│  │  │    PADDING (ระยะใน)    │  │  │
│  │  │  ┌────────────────┐  │  │  │
│  │  │  │    CONTENT      │  │  │  │
│  │  │  │   (เนื้อหา)      │  │  │  │
│  │  │  └────────────────┘  │  │  │
│  │  └──────────────────────┘  │  │
│  └────────────────────────────┘  │
└──────────────────────────────────┘
```

### box-sizing — ปัญหาโลกแตก

```css
/* ปกติ width = ขนาดของ content เท่านั้น */
.box {
    width: 200px;
    padding: 20px;        /* +40px */
    border: 2px solid;    /* +4px */
    /* จริง = 200 + 40 + 4 = 244px */  /* งงไหม? */
}

/* ใช้ border-box แก้ */
.box {
    box-sizing: border-box;  /* width รวม padding+border แล้ว */
    width: 200px;
    padding: 20px;
    border: 2px solid;
    /* จริง = 200px */  /* ตรงเป๊ะ */
}
```

> **📌 ทุกโปรเจค:** ใช้ `*, *::before, *::after { box-sizing: border-box; }` เสมอ

## 5. Flexbox — จัดเรียง 1 มิติ

ใช้ตอน: **เรียงของในแถวหรือคอลัมน์, จัดกึ่งกลาง, จัดระยะห่างเท่ากัน**

```css
.chat-container {
    display: flex;
    flex-direction: column;   /* column = แนวตั้ง, row = แนวนอน */
    gap: 8px;
}

/* จัด bubble ผู้ใช้ชิดขวา, AI ชิดซ้าย */
.bubble-user {
    align-self: flex-end;
    background: #007bff;
    color: white;
}

.bubble-ai {
    align-self: flex-start;
    background: #f0f0f0;
}
```

| Property | ความหมาย |
|----------|----------|
| `display: flex` | เปิด Flexbox |
| `flex-direction` | `row` (แนวนอน) / `column` (แนวตั้ง) |
| `justify-content` | จัดตามแกนหลัก (`center`, `space-between`) |
| `align-items` | จัดตามแกนรอง (`center`, `stretch`) |
| `gap` | ระยะห่างระหว่าง item |
| `align-self` | จัดตำแหน่งเฉพาะตัวนี้ |
| `flex: 1` | ให้ขยายเต็มที่ |

## 6. CSS Grid — จัดเรียง 2 มิติ

ใช้ตอน: **layout ใหญ่ของหน้า, ตาราง, dashboard**

```css
.app-layout {
    display: grid;
    grid-template-columns: 250px 1fr;  /* sidebar + main */
    grid-template-rows: auto 1fr auto; /* header + content + footer */
    grid-template-areas:
        "header header"
        "sidebar main"
        "footer footer";
    height: 100vh;
}

header { grid-area: header; }
aside  { grid-area: sidebar; }
main   { grid-area: main; }
footer { grid-area: footer; }
```

### Flexbox vs Grid

| Flexbox | Grid |
|---------|------|
| 1 มิติ (แถว หรือ คอลัมน์) | 2 มิติ (แถว และ คอลัมน์) |
| เนื้อหากำหนดขนาด | layout กำหนดขนาด |
| Chat bubbles, navbar, centering | หน้า layout, dashboard |
| "เรียงของในแถวนี้ให้สวย" | "แบ่งหน้าจอเป็นโซนแบบนี้" |

## 7. Responsive Design

**Responsive = เว็บปรับตัวตามขนาดหน้าจอ โดยไม่ต้องสร้างแยก**

3 เสาหลัก:
1. **Fluid Grids** — ใช้ % แทน px
2. **Flexible Images** — รูปไม่ล้นจอ
3. **Media Queries** — เปลี่ยน CSS ตาม ขนาดหน้าจอ

```css
/* พื้นฐาน: จอใหญ่ (desktop) */
.app-layout {
    display: grid;
    grid-template-columns: 250px 1fr;
}

/* จอ tablet: ซ่อน sidebar */
@media (max-width: 768px) {
    .app-layout {
        grid-template-columns: 1fr;
    }
    aside { display: none; }
}

/* จอมือถือ: ลดขนาดตัวอักษร */
@media (max-width: 480px) {
    header h1 { font-size: 18px; }
}
```

## 8. RAG Diary CSS — ตัวอย่าง

```css
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

body {
    font-family: 'Sarabun', sans-serif;
    background: #f5f5f5;
    color: #333;
}

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

header {
    grid-area: header;
    background: #2c3e50;
    color: white;
    padding: 16px 24px;
}

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
.entry-item:hover { background: #e8f4fd; }

main {
    grid-area: main;
    display: flex;
    flex-direction: column;
    padding: 16px;
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

footer {
    grid-area: footer;
    background: #2c3e50;
    color: white;
    text-align: center;
    padding: 12px;
    font-size: 12px;
}

/* Mobile */
@media (max-width: 768px) {
    .app-layout {
        grid-template-columns: 1fr;
        grid-template-areas: "header" "main" "footer";
    }
    aside { display: none; }
}
```

## 9. แบบฝึกหัด

ทำให้ sidebar ของ RAG Diary responsive:
- บนมือถือ: ซ่อน sidebar → แสดงปุ่ม hamburger
- ใช้ media query + CSS transition

---

## 🔗 Links

- ก่อนหน้า: [[Frontend/00 HTML|HTML]]
- ต่อไป: [[Frontend/02 JavaScript DOM|JavaScript DOM]]
- กลับไป: [[_Curriculum]]
