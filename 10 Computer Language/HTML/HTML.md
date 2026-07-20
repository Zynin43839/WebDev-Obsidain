---
created:
  "{ date }":
language: HTML
module: HTML Fundamental
source: https://www.freecodecamp.org/learn/responsive-web-design-v9/#lab-debug-pet-adoption-page
---

## ทฤษฎี

- <div> = <div className="flex gap-2"> — just layout, no meaning
-  <section> = <section>Daily Review</section> — a meaningful section of a page
- <article> = A single Concept note — self-contained, makes sense on its own
Let me also think about the technical difference they're really after:
1. <div> = Generic container. Zero semantic meaning. Use when you just need a box for styling/layout.
2. <section> = Thematic grouping. Has a heading usually. Like chapters in a book.
3. <article> = Self-contained composition. Can be independently distributed or reused. Like a blog post, news article, or comment.
The key insight: the difference is in how much context you need to understand the content.

พอแล้ว — มาสรุป:
Tag	ความหมาย	Screen reader อ่านว่า
<div>	ไม่มี	ข้ามไป (ไม่บอกอะไร)
<section>	"ส่วนนี้คือ... (หัวข้อ)"	"Heading: Morning Routine, Section start"
<article>	"อันนี้ยืนได้เอง"	"Article: HTTP Protocol, เข้าเนื้อหาเลย"
Key takeaway: สามอันนี้มองเห็นเหมือนกัน (เป็นแค่กล่อง) แต่คนตาบอดที่ใช้ screen reader จะได้ยินต่างกัน — <section> บอก "ส่วนใหม่แล้วนะ" ส่วน <article> บอก "เนื้อหาชิ้นใหม่แล้วนะ"

## ทำไมถึงเป็นแบบนี้

[เหตุผลเบื้องหลัง — ถ้าไม่มี concept นี้ จะเกิดอะไร?]

## เชื่อมกับที่รู้แล้ว

[อันนี้เหมือนหรือต่างจากอะไรที่เคยเรียนมา?]

## Aha Moment

[อะไรที่เพิ่งเข้าใจตอนนี้?]
