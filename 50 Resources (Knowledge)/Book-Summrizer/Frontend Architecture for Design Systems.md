# Frontend Architecture for Design Systems

> **Author:** Micah Godbolt

---

## Part I — ที่มาของ Frontend Architecture

### จุดเริ่มต้น (Origins)
เว็บเริ่มจากยุค 90 ที่เรียบง่าย ต่อมา Webmaster ต้องแยกสายงานเพราะเว็บซับซ้อนขึ้น จนเกิดเป็นสาย frontend, backend, content strategy และอื่น Content Strategy (กลยุทธ์เนื้อหา) เริ่มเป็นที่รู้จักจาก Kristina Halvorson ปี 2008

### กำเนิด Responsive Web Design
iPhone เปิดตัวปี 2007 บังคับให้เว็บต้องรองรับหลายหน้าจอ m.dot sites แก้ปัญหาได้บางส่วน แต่ต้องรักษา 2 เว็บแยกกัน ปี 2010 Ethan Marcotte เขียนบทความ Responsive Web Design (RWD) บน A List Apart ใช้ Fluid Grids, Flexible Images, Media Queries แก้ปัญหาทั้งหมด

### กำเนิด Frontend Architect
ผู้เขียนรู้สึกว่า Frontend Development ถูกมองเป็นงานตกแต่งท้ายสุดเสมอ จึง提出แนวคิด Frontend Architect คือผู้ออกแบบ architecture ของ frontend ตั้งแต่ต้น project เหมือน Software Architect ของ backend คำนี้ถูกประกาศเป็นครั้งแรกที่ CSS Dev Conf ปี 2014

## Chapter 1 — วินัยของ Frontend Architecture

Frontend Architecture คือเครื่องมือและกระบวนการที่ improve คุณภาพ code และสร้าง workflow ที่มีประสิทธิภาพ มีหน้าที่หลัก 3 ข้อ:

- **Design (ออกแบบ)**: สร้าง design system philosophy ให้ frontend ทั้งทีมทำตาม
- **Planning (วางแผน)**: จัด workflow จาก dev สู่ production (version control, build tools, tests)
- **Oversight (ดูแล)**: ปรับกระบวนการต่อเนื่องเมื่อ project เปลี่ยน

### การทำให้เกิด Architectural Process
ในอดีต developer มักถูกดึงเข้า project หลัง design เสร็จแล้ว ทำให้ frontend เป็น "afterthought" Frontend Architect ต้อง fight ให้เข้า project ตั้งแต่ต้น ต้องมีตัวอย่าง project สำเร็จมายืนยันความถูกต้อง (chicken-or-egg problem)

## Chapter 2 — Alpha Project (Red Hat)

ผู้เขียนได้รับโอกาส architect design system ให้ Red Hat โดยมีเวลาวาง foundation อย่างเต็มที่ 4 โฟกัสหลัก:

1. **Modular Content**: ใช้ Atomic Design สร้าง component เล็กๆ แทน band หลายร้อยแบบ
2. **Comprehensive Testing**: ทดสอบ design system ด้วย coverage เท่ากับ application level
3. **Streamlined Processes**: ทำ feature branch เล็กลงเท่ากับ component ขนาด
4. **Exhaustive Documentation**: เอกสารครบถ้วนสำหรับทุก role (frontend, backend, designer, marketing)

## Chapter 3 — เสาหลัก 4 ของ Frontend Architecture

1. **Code Pillar (โค้ด)**: HTML, CSS, JavaScript คุณภาพสูง
2. **Process Pillar (กระบวนการ)**: Workflow จาก idea สู่ production
3. **Testing Pillar (ทดสอบ)**: ป้องกัน regression ทุกรูปแบบ
4. **Documentation Pillar (เอกสาร)**: Blueprint ของ design system

## Part II — Code Pillar

## Chapter 4 — HTML

### อดีตของ Markup
- **Procedural Markup**: CMS สร้าง HTML อัตโนมัติ แต่ developer ควบคุมไม่ได้ → div soup (10 ชั้นซ้อนกัน)
- **Static Markup**: Developer เขียน HTML เอง ควบคุมได้ 100% แต่เกิด specificity nightmare ต้องเขียน descendant selector ยาวๆ

### Modular Markup
ทางออกคือ modular markup ใช้ class-based styling ไม่依赖 element tags ใช้ BEM naming convention เพื่อความชัดเจน เช่น `nav__item--secondary` ทุก element มี class เฉพาะตัว

### Design System คืออะไร
ไม่ใช่ "หน้าเว็บ" แต่เป็นระบบ visual language ที่programmatically สร้างหน้าเว็บได้ทุกรูปแบบ แตก components เล็กที่สุดแล้ว組み合わせใหม่

## Chapter 5 — CSS

### ปัญหาอดีต — Specificity Wars
ใช้ `#sidebar h2` แล้วต้อง override ด้วย selector ที่ยาวกว่า → cascade battle ไม่จบสิ้น location dependence ทำให้ย้าย component แล้ว style ผิด

### วิธีแก้ — Modular CSS
- **OOCSS**: แยก container จาก content, แยก structure จาก skin
- **SMACSS**: แบ่ง Base, Layout, Module, State, Theme
- **BEM**: Block__Element--Modifier ทุก class มีความหมายชัดเจน

### หลักการสำคัญ
- **Single Responsibility**: 1 class = 1 จุดประสงค์
- **Single Source of Truth**: ทุก style ของ component อยู่ในไฟล์เดียวกัน
- **Component Modifiers**: ใช้ modifier class แทน context-based override

## Chapter 6 — JavaScript

### การเลือก Framework
ไม่ต้องใช้ framework ทุกครั้ง เริ่มจากไม่ใช้อะไรเลย แล้วเพิ่มเฉพาะที่จำเป็น Lean approach ดีกว่า bundling ทุกอย่างตั้งแต่ต้น

### การเขียน JavaScript ให้สะอาด
- ใช้ linting tool เช่น JSHint
- เขียน reusable functions แทน jQuery chain ยาวๆ
- แยก logic เป็นฟังก์ชันเล็กๆ ที่ตั้งชื่อชัดเจน

## Chapter 7 — Red Hat Code

### ปัญหาเดิม
Redhat.com ตั้งต้นไม่มี modular thinking เลย CSS 500KB, Bootstrap dependency, location-dependent selectors ยาวเหยียด

### Road Runner Rules
กฎ的设计 system ของ Red Hat:
- Layout ห้าม impose padding/style บน children
- Component ต้อง touch ทั้ง 4 ด้านของ parent (margin ล่างขวาลบออก)
- ทุก element มี class เฉพาะตัว 1 ตัว
- Element ห้ามมี top margin
- JavaScript ไม่ binding กับ element class — ใช้ data attributes

### Opt-in Modifiers & Contexts
ใช้ `data-rh-theme="dark"` แทน class modifier component ต้อง opt-in เองว่าจะใช้ modifier ตัวไหน ไม่ใช่ถูกบังคับจาก parent

### Semantic Grids
ใช้ data attribute ตั้ง layout บน parent element เช่น `data-rh-layout="gallery5"` → ลูกทุกตัวจะจัด 5 คอลัมน์อัตโนมัติ

## Part III — Process Pillar

## Chapter 8 — Workflow

### วงจรเก่า vs ใหม่
- เก่า: Designer ส่ง PSD → Developer ทำ markup ให้ match → ปัญหาเยอะ
- ใหม่: Requirements → Prototyping → Development → QA โดยทุกฝ่ายร่วมตั้งแต่ต้น

### Frontend Workflow
1. **Provisioning**: ติดตั้งเครื่องมือให้ new developer
2. **Spinning Up Local**: clone repo + ตั้ง local dev environment
3. **Story Writing**: เขียน user stories โฟกัสที่ component ไม่ใช่หน้า
4. **Development**: feature branch → code → merge request
5. **Distribution**: deploy สู่ production (CI server, tagged releases)

## Chapter 9 — Task Runners

### วิวัฒนาการ
Compass (Ruby) → Grunt/Gulp (Node.js) → ครอบคลุมทุก build step

### Grunt vs Gulp
- **Grunt**: configuration-based, module มากกว่า
- **Gulp**: pipe-based, เร็วกว่า, code chaining

### สิ่งที่ Task Runner ทำได้
Compile Sass, concat JS, autoprefix, image optimization, lint, visual regression test, style guide build, live reload, deploy

## Chapter 10 — Red Hat Process

### Schema-Driven Design System
ใช้ JSON Schema อธิบาย data model ของทุก component:
- Schema กำหนด field ที่ต้องการ, data type, required fields
- ใช้ `oneOf` เลือก schema ทางเลือก (เช่น 2up หรือ 3up logos)
- Twig template รับ JSON data → render HTML เดียวกันเสมอ

### ข้อดีของ Schema-Driven
- Content model ชัดเจนตั้งแต่ต้น
- ตรวจสอบ data ก่อน render
- UI editor สร้างจาก schema อัตโนมัติ
- ไม่ต้อง guess ว่า CMS จะ output อะไร

## Part IV — Testing Pillar

## Chapter 11 — Unit Testing

### หลักการ
- แยกฟังก์ชันเป็น unit เล็กที่สุด "Do one thing, do it well"
- Test-Driven Development (TDD): เขียน test ก่อน code
- ครอบคลุมทุก function สำคัญ

### ครอบคลุมแค่ไหน
- เริ่มจากส่วนที่ง่ายก่อนเพื่อสร้าง momentum
- เพิ่ม coverage สำหรับ critical parts เรื่อยๆ
- ทุก user story ต้องมี test coverage task

## Chapter 12 — Performance Testing

### Performance Budget
ตั้ง target สำหรับ:
- **Page Weight**: น้ำหนักรวม (ภาพ 61% ของ total)
- **HTTP Requests**: จำนวน request ต่อหน้า
- **Timing Metrics**: TTFB, Time to Start Render, Speed Index
- **Hybrid Metrics**: PageSpeed Score

### การตั้ง Budget
- ดูคู่แข่ง: เร็วกว่า 20%
- ดูค่าเฉลี่ยอุตสาหกรรม: HTTPArchive
- ใช้ Grunt PageSpeed, Grunt Perfbudget ทดสอบอัตโนมัติ

## Chapter 13 — Visual Regression Testing

### ปัญหาที่แก้ได้
- Developer A แก้ code แล้วไม่รู้ว่ากระทบ component อื่น
- Design ไม่ consistent ระหว่าง PSD files
- Decision maker เปลี่ยนใจบ่อย

### เครื่องมือ
- **Wraith**: page-based diffing
- **BackstopJS**: component-based diffing
- **PhantomCSS**: headless browser + screenshot compare

## Chapter 14 — Red Hat Testing

### PhantomCSS Workflow
1. PhantomJS เปิด browser
2. CasperJS ไปหน้า style guide
3. จับ screenshot ของ component
4. เปรียบเทียบกับ baseline
5. รายงาน PASS/FAIL

### การจัดการ
- Baselines เก็บใน component folder ตัวเอง
- แยก test per component (ไม่ lump รวมกัน)
- ทดสอบ portable ได้หลายที่

## Part V — Documentation Pillar

## Chapter 15 — Style Guides

### Hologram
- ใส่ documentation ใน CSS comment `/*doc ... */`
- สร้าง static site จาก inline docs
- ใช้ Markdown + code examples

### SassDoc
- เอกสารอัตโนมัติสำหรับ Sass variables, mixins, functions
- แสดง dependency graph อัตโนมัติ
- ใช้ annotation: `@type`, `@access`, `@param`, `@return`

## Chapter 16 — Pattern Libraries

### Atomic Design (Brad Frost)
- **Atoms**: heading, image, form elements
- **Molecules**: search form, media block
- **Organisms**: blog article, comment section
- **Templates**: layout ของ multiple organisms
- **Pages**: template + real content data

### Pattern Lab
- Static site generator สำหรับ design system
- ใช้ Mustache templates + JSON data
- สร้าง browsable component library
- ทีมทุก role ใช้เป็น common language

## Chapter 17 — Red Hat Documentation

### วิวัฒนาการ 5 ขั้น
1. **Static Style Guide**: Hologram + inline docs
2. **Reinvented Pattern Lab**: Twig templates + JSON data → render ซ้ำได้
3. **Split Pattern Library**:แยก repos ออกจาก style guide
4. **Unified Rendering Engine**: Twig เป็น rendering engine เดียว (Drupal + Style Guide)
5. **Automate Pattern Creation**: JSON Schema → auto-import เป็น CMS entities

### บทเรียนสำคัญ
Schema-driven design ทำให้ frontend ควบคุมทั้ง collection, storage, rendering, และ styling ของ content ใน CMS

## Chapter 18 — Conclusion

### บทเรียนหลัก
- Architecture ต้อง iterate อยู่เสมอ ไม่มีครั้งเดียวจบ
- อย่ากลัวที่จะเปลี่ยนเครื่องมือเมื่อ project ต้องการ
- Frontend Architect ต้องเป็น "student for life" เรียนรู้เครื่องมือใหม่ตลอด
- อย่าอายที่จะขอความช่วยเหลือ แบ่งปันความรู้ และเขียนบันทึก

### คำแนะนำสำหรับ Frontend Architect มือใหม่
- อย่า placing ความหวังทั้งหมดใน solution เดียว
- Iterate มากเท่าที่จำเป็น แต่ iterate เสมอ
- ถ้าคุณมีประสบการณ์ อย่าหยุดทดลองสิ่งใหม่
- อย่ากลัวที่จะลุกขึ้นบนเวทีแล้ว encouraging คนอื่น
- **最重要: เขียนมันทั้งหมดลงในหนังสือ**

---
