
### **📊 What's HTML?**
```markdown
---
created: 06/04/2569
type: HTML-Concept
category: front-end-web-dev
difficulty: beginner
tags: [html, semantic-html, web-development, basics]
prerequisites: [[basic-programming]]
related: [[css-fundamentals], [web-accessibility]]
status: Learning 
priority: High
source: freecodecamp-html-module
```
---
# HTML

## 📋 Definition
HyperText Markup Language(HTML) = เป็นเหมือนสารบัญของเว็บไซต์ที่บันทึกโครงสร้างและ Content ของเว็บ
HTML Element = เป็นตัวบ่งบอก Heading ของเว็บไซต์ เพื่อบ่งบอกโครงสร้างว่าบรรทัดไหนเป็นโครงสร้างอะไร เช่น h1, h2, p
Attribute = เป็นตัวขยาย element เช่น href, a, src เป็นต้น ทำให้สามารถเข้าใจได้ว่าข้อมูลที่ต้องการ คืออะไร โดยบาง Attribute ก็ต้องการข้อมูลแบบเฉพาะเจาะจง
	Link element = เป็นเหมือนสะพานที่ทำหน้าที่เชื่อมไฟล์หนึ่งกับอีกไฟล์หนึ่ง เช่น เชื่อมกับ Style.css, .ico, .png, font
CSS = ภาษาที่ใช้สำหรับตกแต่งและเพิ่มลูกเล่นให้กับหน้าเว็บ
HTML boilerplate = เป็นโครงสร้างเฉพาะที่มี Element จำเป็นสำหรับการสร้างเว็บ
UTF-8 Character Encoding = ใช้เพื่อการรองรับภาษารวมถึง Emoji ต่างๆ ทำให้ภาษา/สัญลักษณ์ไม่เพี้ยน และทำให้ทำงานด้วยภาษที่ต้องการได้ในทุกประเทศ
<main> = 

## ````
## 🎯 Purpose & Use Cases
- **วัตถุประสงค์:** สร้างโครงสร้างและเนื้อหาของเว็บไซต์
- **กรณีใช้งาน:** 
  - สร้างหน้าเว็บแบบ static
  - โครงสร้างเนื้อหาบทความ
  - Form รับข้อมูลผู้ใช้
  - Embed multimedia (วีดีโอ, audio)

## ⚡ Key Features
- **Semantic Structure:** ใช้ tags ที่มีความหมาย
- **Accessibility:** รองรับ screen readers
- **SEO Friendly:** Google เข้าใจโครงสร้าง
- **Cross-platform:** ทำงานบนทุก browser

## 🛠️ Implementation
```html
<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>หน้าเว็บของฉัน</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <header>
        <h1>สวัสดีชาวโลก!</h1>
    </header>
    <main>
        <p>นี่คือเว็บแรกของฉัน</p>
    </main>
</body>
</html>
````
