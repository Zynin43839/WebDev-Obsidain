---
created: 2026-07-02
type: lesson
series: frontend
number: 00
topic: html
status: published
---

# HTML — โครงสร้างของเว็บ

> **เปรียบ:** HTML = เสาเข็ม + โครงสร้างบ้าน  
> CSS = ทาสี ตกแต่ง  
> JavaScript = ไฟฟ้า ประปา  
> React = บล็อกเลโก้ประกอบบ้าน

---

## 1. HTML คืออะไร?

**HTML (HyperText Markup Language)** = ภาษาที่บอกเบราว์เซอร์ว่า **หน้านี้มีอะไรบ้าง**

- ไม่ใช่ภาษาโปรแกรม — มันคือ **Markup Language** (ภาษาสำหรับ "ติดป้าย" เนื้อหา)
- ใช้ **Tags** เพื่อบอกว่า "ส่วนนี้คือหัวข้อ" "ส่วนนี้คือย่อหน้า" "ส่วนนี้คือรูปภาพ"

## 2. โครงสร้างพื้นฐาน

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

## 3. Semantic Tags

**Divitis = โรคที่ทุกอย่างเป็น `<div>` หมด**

```html
<!-- ❌ Divitis - อ่านยาก, SEO แย่, screen reader ใช้ไม่ได้ -->
<div class="header">
    <div class="title">My RAG Diary</div>
</div>
<div class="sidebar">
    <div class="entry">Entry 1</div>
</div>

<!-- ✅ Semantic - อ่านรู้เรื่อง, SEO ดี, accessibility ดี -->
<header>
    <h1>My RAG Diary</h1>
</header>
<aside>
    <article>Entry 1</article>
</aside>
```

| Tag | ใช้ทำอะไร | แทน div อะไร |
|-----|-----------|--------------|
| `<header>` | หัวเว็บหรือหัว section | `<div class="header">` |
| `<nav>` | เมนูนำทาง | `<div class="nav">` |
| `<main>` | เนื้อหาหลัก (มีได้ 1 อัน) | `<div class="main">` |
| `<aside>` | เนื้อหาข้างเคียง (sidebar) | `<div class="sidebar">` |
| `<section>` | กลุ่มเนื้อหาที่เกี่ยวข้อง | `<div class="section">` |
| `<article>` | เนื้อหาอิสระ (โพสต์, entry) | `<div class="post">` |
| `<footer>` | ท้ายเว็บ | `<div class="footer">` |

## 4. Forms — การรับข้อมูลจากผู้ใช้

```html
<!-- Login form สำหรับ RAG Diary -->
<form action="/api/auth/login" method="POST">
    <div>
        <label for="email">อีเมล:</label>
        <input type="email" id="email" name="email" required>
    </div>
    <div>
        <label for="password">รหัสผ่าน:</label>
        <input type="password" id="password" name="password" required minlength="6">
    </div>
    <button type="submit">เข้าสู่ระบบ</button>
</form>

<!-- Input types ที่พบบ่อย -->
<input type="text">     <!-- ข้อความทั่วไป -->
<input type="email">    <!-- อีเมล (ตรวจสอบ @ อัตโนมัติ) -->
<input type="password"> <!-- ซ่อนตัวอักษร -->
<input type="number">   <!-- เฉพาะตัวเลข -->
<input type="date">     <!-- เลือกวันที่ -->
<input type="file">     <!-- อัปโหลดไฟล์ -->
<textarea></textarea>   <!-- ข้อความหลายบรรทัด -->
<select><option>...</select>  <!-- dropdown -->
```

## 5. RAG Diary HTML Structure

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
        <h1>📔 RAG Diary</h1>
        <p>ไดอารี่ที่คุยกับ AI ได้</p>
    </header>

    <aside>
        <h2>Entries</h2>
        <ul>
            <li>2026-07-01: เรียน TypeScript</li>
            <li>2026-06-30: เรียน Trading</li>
        </ul>
    </aside>

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

    <footer>
        <p>Powered by RAG + Supabase</p>
    </footer>
</body>
</html>
```

## 6. แบบฝึกหัด

สร้างโครงสร้างหน้า **Login** สำหรับ RAG Diary:
- `<header>` ชื่อแอป
- `<main>` มี form (email + password + login button)
- `<footer>` copyright

---

## 🔗 Links

- ต่อไป: [[Frontend/01 CSS|CSS]]
- กลับไป: [[_Curriculum]]
