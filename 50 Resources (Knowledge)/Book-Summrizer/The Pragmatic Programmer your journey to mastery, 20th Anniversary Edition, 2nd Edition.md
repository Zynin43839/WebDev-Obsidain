# The Pragmatic Programmer: Your Journey to Mastery, 20th Anniversary Edition, 2nd Edition

> **Author:** David Thomas, Andrew Hunt

## ภาพรวมของหนังสือ
The Pragmatic Programmer เป็นหนังสือคลาสสิกสำหรับนักพัฒนาซอฟต์แวร์ที่เน้นการปฏิบัติจริงมากกว่าทฤษฎี แบ่งเป็น 8 บท ครอบคลุม 43 หัวข้อ ตั้งแต่ปรัชญาไปจนถึงเทคนิคขั้นสูง

---

## บทที่ 1: A Pragmatic Philosophy

### Topic 1: The Pragmatic Programmer
นักพัฒนาควรเป็น "generalizing specialists" — เชี่ยวชาญหลายด้านแต่ลึกซึ้งบาง领域 ใช้หลัก DRY (Don't Repeat Yourself) อย่าทำซ้ำในสิ่งที่ทำได้โดยอัตโนมัติ อย่ายึดเทคโนโลยีเดียว ต้องยืดหยุ่นและพร้อมเรียนรู้สิ่งใหม่ สร้าง portfolio ความรู้ส่วนตัวเหมือนนักลงทุน — กระจายความเสี่ยง

### Topic 2: A Pragmatic Philosophy
**Stone Soup** — เริ่มจากสิ่งเล็กๆ แล้วขยายผล **Good-Enough Software** — ซอฟต์แวร์ที่สมบูรณ์แบบมากเกินไปอาจไม่คุ้มค่า **Provide Options, Don't Make Lame Excuses** — ให้ทางเลือกแทนข้ออ้าง **Don't Live with Broken Windows** — แก้ปัญหาเล็กก่อนลุกลาม **Be a Catalyst for Change** — เป็นตัวเร่งการเปลี่ยนแปลง **Remember the Big Picture** — อย่าจมรายละเอียด **Make Quality a Requirements Issue** — คุณภาพต้องเป็นข้อกำหนดตั้งแต่แรก **Invest Regularly** — ลงทุนในความรู้สม่ำเสมอ **Critically Analyze** — วิเคราะห์ข้อมูลอย่างมีวิจารณญาณ

### Topic 3: The Basic Tools
เลือกเครื่องมือที่เหมาะสมกับงาน **Broken Windows** — อย่าปล่อยปัญหาเล็กกลายเป็นปัญหาใหญ่ **Boiled Frog** — ระวังการเปลี่ยนแปลงค่อยๆ ที่ไม่ทันสังเกต **Single Version of the Truth** — มีข้อมูลเดียวที่เชื่อถือได้ **Don't Outrun Your Headlights** — อย่าก้าวเร็วเกินไปจนตามไม่ทัน

### Topic 4: Pragmatic Paranoia
**Design by Contract** — ระบุสิ่งที่คาดหวังอย่างชัดเจน **Dead Programs Tell No Lies** — โปรแกรมที่ตายมักไม่โกหก ตรวจสอบสาเหตุให้เจอ **Assertive Programming** — ใช้ assertion ตรวจสอบเงื่อนไข **How to Balance Resources** — จัดสรรทรัพยากรอย่างสมดุล **Don't Pry Open the Window** — อย่าเปิดเผยสิ่งที่ไม่จำเป็น

### Topic 5: DRY — Don't Repeat Yourself
**The Evils of Duplication** — การทำซ้ำทำให้เกิดปัญหาหลายประการ **The Single Source of Truth** — มีแหล่งข้อมูลเดียว **Implement, Don't Repeat** — ใช้โค้ดเดิมแทนเขียนซ้ำ **Parameterize from the Outside** — กำหนด parameter จากภายนอก **You Can't Repeat Yourself...Or Can You?** — การทำซ้ำบางครั้งจำเป็น

### Topic 6: Tracer Bullets
ใช้ "tracer bullets" (โค้ดสำรวจ) เพื่อทดสอบทิศทาง กระสุนสำรวจไม่เหมือนกระสุนจริง — โค้ดสำรวจเป็นสิ่งที่ทิ้งได้ เน้นเป้าหมายเดียว และสร้าง skeleton โครงสร้างพื้นฐานให้แอปพลิเคชัน

### Topic 7: Prototypes and Post-it Notes
สร้างต้นแบบ (prototype) เพื่อเรียนรู้ สื่อสาร และสำรวจ — ไม่ใช่เพื่อสร้างผลิตภัณฑ์จริง prototype เป็นเครื่องมือทดสอบความคิดก่อนลงมือทำจริง

---

## บทที่ 2: A Pragmatic Approach

### Topic 8: Orthogonality
ลดการเชื่อมโยงระหว่างส่วนต่างๆ ของระบบ — ผลคือแก้ไขส่วนเดียวไม่กระทบส่วนอื่น ครอบคลุมทั้งตารางเวลาทีม การเพิ่มผลผลิต การเขียนโค้ด testing, debugging, tuning ทั้งหมดต้องรักษาความเป็น orthogonality

### Topic 9: Because It's There
การเขียนโค้ดเป็น "construction" (การก่อสร้าง) ไม่ใช่แค่พิมพ์โค้ด — ต้องคิดเหมือนช่าง ทำความเข้าใจโครงสร้างก่อนลงมือ ไม่ใช่ทำเพราะเทคโนโลยีมีอยู่

### Topic 10: How to Balance Resources
การจัดสรรทรัพยากร — รู้ประเภททรัพยากร (เวลา, คน, เครื่องมือ, ข้อมูล) และปัญหาที่เกิดขึ้นในการพัฒนาซอฟต์แวร์ ต้องวางแผนจัดสรรอย่างสมดุล

### Topic 11: Don't Live with Broken Windows
แก้ปัญหาก่อนที่ปัญหาจะแก้คุณ — อย่าขุดหลุมฝังศพล่วงหน้า อย่าปล่อย technical debt สะสม เพราะยิ่งแก้ยากขึ้นเรื่อยๆ

### Topic 12: The Topic of The Topic
อย่าเป็นทาสของเครื่องมือ — เครื่องมือช่วยได้แต่ต้องเข้าใจหลักการก่อน การตัดสินใจเลือกเครื่องมือต้องมีเหตุผล ไม่ใช่ตามกระแส

### Topic 13: Pragmatic Teams
ทีมแบบนักปฏิบัติมีจริยธรรมที่เข้มแข็ง ทำงานเป็นทีม มีมาตรฐานสูง และเลือกใช้เครื่องมือที่เหมาะสมกับงาน ต้องมีวินัยร่วมกัน

---

## บทที่ 3: The Basic Tools

### Topic 14: The Power of Plain Text
ข้อความธรรมดา (plain text) มีพลังมาก — ไม่ผูกกับ platform ใด ง่ายต่อการค้นหา, version control, และประมวลผล อยู่ในโลก plain text ได้ตลอดเวลา

### Topic 15: Shell Games
เรียนรู้ใช้ shell (command line) ให้ชำนาญ — เชลล์ของคุณเองช่วยทำงานซ้ำๆ ได้เร็ว สร้าง power apps ที่เชื่อมต่อกันผ่าน shell

### Topic 16: Power Editing
ต้องรู้จัก editor ที่ใช้จริงๆ — macros, utilities ช่วยเร่งงาน โครงสร้าง editor ต้องเข้าใจเพื่อใช้ศักยภาพเต็มที่

### Topic 17: Version Control
version control ไม่ใช่แค่เก็บไฟล์ — เป็น "conductor" (วาทยกร) ที่ควบคุมทุกอย่าง ใช้ใน日常工作 แตกแขนง (branching) อย่างมีระบบ

### Topic 18: Debugging
จิตใจของการ debug สำคัญกว่าเทคนิค — ต้อง冷静 ค้นหาสาเหตุจริง ไม่ใช่เดา กลยุทธ์ debug มีหลายแบบ ต้องเลือกให้เหมาะกับปัญหา

### Topic 19: Text Manipulation
การจัดการข้อความ (data munging) — สร้างภาษาขนาดเล็ก (small language) ในโลกจริงเพื่อทำงานซ้ำๆ หาความแตกต่างระหว่างข้อมูล

---

## บทที่ 4: Pragmatic Paranoia

### Topic 20: Design by Contract
ออกแบบตามสัญญา — ระบุ precondition, postcondition, และ invariant ให้ชัดเจน เขียนสัญญาให้ compiler อ่านได้ ป้องกันข้อผิดพลาดตั้งแต่ต้น

### Topic 21: Dead Programs Tell No Lies
โปรแกรมที่ crash มักจะบอกความจริง — วิธีทำให้โปรแกรม crash ได้อย่าง controlled (fail fast) ดีกว่าปล่อยทำงานผิดเงื่อนไข ใช้ exception อย่างถูกวิธี

### Topic 22: Assertive Programming
เขียน assertion ที่ดี — ตรวจสอบ invariant และ precondition เสมอ อย่าเชื่อข้อมูลจากภายนอกโดยไม่ตรวจสอบ assertion ช่วยจับ bug ก่อน production

### Topic 23: How to Balance Resources
จัดสรรทรัพยากร (memory, file handles, connections) อย่างสมดุล รู้ประเภททรัพยากรและปัญหาที่เกิด สร้างสมดุลเพื่อไม่ให้เกิด resource leak

### Topic 24: Don't Pry Open the Window
อย่าเปิดเผยสิ่งที่ไม่จำเป็น — พูด "ไม่" เมื่อจำเป็น พูด "ใช่" เมื่อมั่นใจ รักษา encapsulation และ abstraction ของระบบ

---

## บทที่ 5: DRY — Don't Repeat Yourself

### Topic 25: The Evils of Duplication
การทำซ้ำเป็นรากเหตุของปัญหาซอฟต์แวร์ — เมื่อแก้ที่เดียวต้องแก้ทุกที่ ใช้ single source of truth เพื่อหลีกเลี่ยงปัญหานี้

### Topic 26: How to Balance Resources
เขียนสัญญา (contract) ให้ชัดเจน จัดสรรทรัพยากรอย่างสมดุล รู้ว่าเมื่อไหร่ควร allocate, deallocate, และ reuse

### Topic 27: How to Balance Resources
สร้างสมดุลทรัพยากรในทุกมิติ — เวลา, คน, เครื่องมือ — ไม่ใช่แค่ memory แต่รวมถึง effort และ attention ของทีมด้วย

---

## บทที่ 6: While You Are Coding

### Topic 28: Design by Contract
ระหว่างเขียนโค้ด — ออกแบบตามสัญญาทุก function, ทุก module ระบุ input/output ให้ชัดเจน ป้องกัน bug ก่อนเกิด

### Topic 29: Dead Programs Tell No Lies
เขียนโค้ดให้ fail fast — ถ้ามีอะไรผิดปกติให้ crash ทันที อย่าปล่อยให้โปรแกรมทำงานต่อในสภาพที่ไม่ถูกต้อง ใช้ exception อย่างชาญฉลาด

### Topic 30: Assertive Programming
ใส่ assertion ในทุกจุดที่มี invariant — เขียน assertion ที่มีความหมาย ไม่ใช่แค่ assert True ตรวจสอบ edge case และ boundary เสมอ

### Topic 31: How to Balance Resources
จัดสรรและคืนทรัพยากรอย่างสมดุล — ใช้ pattern เช่น RAII หรือ try-finally ป้องกัน resource leak รู้ประเภททรัพยากรที่ใช้

### Topic 32: Don't Pry Open the Window
รักษา abstraction boundary — อย่าเข้าถึง internal state ของ module อื่นโดยตรง พูด "ไม่" กับการละเมิด encapsulation

### Topic 33: The Topic of The Topic
อย่าเป็นทาสของเครื่องมือ — ใช้เครื่องมือเป็น แต่ต้องเข้าใจหลักการเบื้องหลัง การตัดสินใจต้องมีเหตุผล

### Topic 34: Pragmatic Teams
ทำงานเป็นทีมอย่างมีจริยธรรม — มีมาตรฐานสูง รักษาคุณภาพโค้ดร่วมกัน บันทึกเกี่ยวกับเครื่องมือที่ทีมใช้

### Topic 35: The Power of Plain Text
ใช้ plain text เป็น format หลัก — ง่ายต่อการ refactor, search, และ maintain อยู่ในโลก plain text ได้ทุก platform

### Topic 36: Shell Games
ใช้ shell ให้เป็นประโยชน์ — เขียน script สำหรับงานซ้ำๆ เชื่อมต่อเครื่องมือต่างๆ เข้าด้วยกันผ่าน command line

### Topic 37: Power Editing
Master editor ที่ใช้ — เรียนรู้ macros, shortcuts, และ utilities โครงสร้าง editor ที่ดีช่วยเร่งงานพัฒนาอย่างมาก

### Topic 38: Version Control
version control เป็นหัวใจของทุกโปรเจกต์ — ใช้ branching strategy ที่เหมาะสม ตรวจสอบโค้ดก่อน merge ทุกครั้ง

---

## บทที่ 7: Before the Project

### Topic 39: Pragmatic Teams
ก่อนเริ่มโครงการ — สร้างทีมที่มีจริยธรรม ทำงานร่วมกัน มีมาตรฐานสูง และเลือกเครื่องมือที่เหมาะสม

### Topic 40: Coconuts Don't Cut It
อย่าใช้วิธีแก้ปัญหาที่ไม่เหมาะ — เปรียบเหมือน "coconut" ที่ไม่เหมาะกับทุกงาน ต้องเลือกวิธีแก้ปัญหาให้ตรงจุด

---

## บทที่ 8: Pragmatic Projects

### Topic 41: Pragmatic Teams
ทีมที่ดีมีจริยธรรมเข้มแข็ง ทำงานเป็นระบบ มีวินัย และรักษาคุณภาพร่วมกัน

### Topic 42: Coconuts Don't Cut It
เลือกเครื่องมือและวิธีแก้ปัญหาให้ตรงจุด — อย่าใช้ coconuts (วิธีที่ไม่เหมาะ) มาตัดทุกอย่าง ต้องเข้าใจปัญหาก่อนเลือกวิธีแก้

### Topic 43: Pragmatic Teams
ปิดท้ายด้วยทีมที่แข็งแกร่ง — มีจริยธรรม มีมาตรฐาน รักษาคุณภาพ และพร้อมปรับปรุงอยู่เสมอ

---

## บทสรุป
The Pragmatic Programmer เน้นการปฏิบัติจริงมากกว่าทฤษฎี หลักสำคัญ: DRY (Don't Repeat Yourself) อย่าทำซ้ำ, Orthogonality ลดการเชื่อมโยง, Tracer Bullets ใช้โค้ดสำรวจ, Design by Contract ออกแบบตามสัญญา, Debugging แก้ไขข้อผิดพลาดอย่างมีระบบ, Pragmatic Teams ทีมที่มีจริยธรรมและประสิทธิภาพ เหมาะสำหรับนักพัฒนาทุกระดับ

---