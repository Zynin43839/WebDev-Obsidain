# Designing for Performance

> **Author:** Lara Callender Hogan

---

## บทนำ: ทำไม Performance ถึงสำคัญ

ความเร็วเว็บ (Page Speed) คือหัวใจของ User Experience ตรงๆ เลย ผู้ใช้ expects ให้เว็บโหลดภายใน 2 วินาที ถ้าเกิน 3 วินาที จะมีคนปิดเว็บทิ้งมากถึง 40% (Abandonment Rate) ออกแบบสวยแค่ไหนก็ไม่ช่วย ถ้าโหลดช้า

**ผลกระทบต่อธุรกิจ**: 75% ของผู้ใช้จะไม่ซื้อของถ้าเว็บค้างหรือโหลดช้า (Akamai study) 88% จะไม่กลับมาใช้เว็บเดิมอีก (Gomez study) แม้แค่หน่วง 400ms ก็ทำให้คนค้นหาน้อยลง 0.76% ภายใน 3 สัปดาห์ (Google study) ทุก миллисекунดมีราคา

**SEO Impact**: Google ใช้ Page Speed เป็น ranking factor เว็บเร็วกว่า = อันดับสูงกว่า = traffic มากกว่า

---

## Chapter 1: Performance คือ User Experience

### ผลกระทบต่อ Mobile Users
- Mobile Traffic กำลังเติบโตเรื่อยๆ ที่ Etsy ถึง 50% ของ traffic ทั้งหมด
- คนใช้มือถือ expects ข้อมูลทันที ไม่รอ ต้องการข้อมูลเร็วทุกช่วงเวลา
- Mobile Network มี Latency สูงกว่า Desktop มาก ต้องสร้าง Radio Channel ก่อนส่งข้อมูล ใช้เวลา 1-2 วินาทีสำหรับ 3G
- WiFi บนมือถือยังช้ากว่า Laptop เพราะเสาอากาศเล็กกว่าและประหยัดไฟ

### ผลกระทบของ Mobile Hardware
- Smartphone มีเสาอากาศสั้น ลด Output Power เพื่อประหยัดไฟ ทำให้ WiFi ช้ากว่า Desktop
- การ Optimize รูปเป็น JPEG 92% quality ช่วยประหยัดพลังงานได้ 20-30% บนมือถือ

### บทบาทของ Designer
- การตัดสินใจตอน Design Phase มี Impact ต่อ Performance มาก เช่น รูปแบบ ขนาด สี โครงสร้าง HTML/CSS
- ความหน่วง (Latency) ที่ผู้ใช้รู้สึก: ต่ำกว่า 100ms รู้สึกทันที, 100-300ms รู้สึกได้, 300-1000ms รู้สึกรอ, มากกว่า 1000ms จะเปลี่ยนหน้าทันที

---

## Chapter 2: พื้นฐานของ Page Speed

### วิธีที่ Browsers Render Content
- Browsers ส่ง Request ไปที่ Server → ทำ DNS Lookup → รับ Response กลับมา → Render หน้าเว็บ
- Time to First Byte (TTFB) คือเวลาที่ Browser ได้รับ Byte แรกจาก Server เป็นตัววัดความเร็ว Backend
- Browsers ทำงานแบบ Parallel (หลาย Connection พร้อมกัน) ได้สูงสุด 6-8 Connection

### Requests และ Connections
- จำนวน Request ยิ่งน้อย ยิ่งเร็ว เพราะ Request แต่ละตัวต้องผ่าน DNS Lookup + Connection + Download
- Waterfall Chart แสดง Timeline ของ Request แต่ละตัว ช่วยวิเคราะห์ Bottleneck
- Persistent Connection ช่วยให้ Browser ใช้ Connection เดิมซ้ำ ไม่ต้องเปิดใหม่ทุกครั้ง

### Page Weight
- ขนาดไฟล์ (HTML, CSS, JS, Images) โดยตรงส่งผลต่อเวลาโหลด ยิ่งเล็กยิ่งดี
- ใช้ YSlow ตรวจสอบ Page Weight ของแต่ละ Resource Type

### Perceived Performance
- ความรู้สึกของผู้ใช้ว่าเว็บเร็วแค่ไหน สำคัญเท่ากับเวลาโหลดจริง
- Critical Rendering Path คือ Process ที่ Browser สร้าง DOM Tree → CSSOM → Render Tree → แสดงผล
- Jank คืออาการกระตุกเวลา Scroll เกิดจาก Browser Render ช้ากว่า 60 FPS ตรวจจับด้วย Chrome DevTools Timeline

### ปัจจัยภายนอกที่ส่งผลต่อ Page Speed
- **ภูมิศาสตร์ (Geography)**: ผู้ใช้อยู่ไกล Server ยิ่งช้า ใช้ CDN ช่วยได้
- **Network**: Bandwidth จำกัด ทำให้ดาวน์โหลดช้า โดยเฉพาะ Mobile Network
- **Browser**: Browser ต่างกัน Render ต่างกัน เช่น Progressive JPEG ไม่รองรับทุก Browser

---

## Chapter 3: การ Optimize รูปภาพ

### การเลือกรูปแบบไฟล์ (Image Format)

#### JPEG
- เหมาะสำหรับรูปถ่าย (Photographs) ที่มีสีจำนวนมาก
- เป็น Lossy Format คือบีบอัดแล้วข้อมูลบางส่วนหายไป
- คุณภาพยิ่งต่ำ ยิ่งเห็น Artifacting (ภาพแตก)
- **Baseline JPEG**: โหลดจากบนลงล่างทีละแถว
- **Progressive JPEG**: โหลดทั้งภาพแบบเบลอๆ แล้วค่อยๆ ชัด รู้สึกเร็วกว่า แต่กิน CPU มากกว่า
- ลด Noise ช่วยลดขนาดไฟล์ได้มาก

#### GIF
- เหมาะสำหรับ Animation (เช่น Loading Spinner)
- รองรับ Transparency แต่จำกัดสีได้สูงสุด 256 สี
- Dithering ทำให้ภาพดูเนียนขึ้นแต่เพิ่มขนาดไฟล์
- GIF บีบอัดแบบ Horizontal Redundancy ดังนั้น Vertical Pattern จะทำให้ไฟล์ใหญ่ขึ้น
- ถ้าใช้เป็น Animation เล็กๆ พิจารณาใช้ CSS3 Animation แทน

#### PNG
- **PNG-8**: จำกัด 256 สี เหมาะสำหรับรูป Icon, โลโก้ ที่มีสีน้อย
- **PNG-24**: รองรับ Partial Transparency (โปร่งแสงบางส่วน) แต่ไฟล์ใหญ่กว่า PNG-8 มาก
- PNG บีบอัดทั้ง Horizontal และ Vertical Pattern จึงเล็กกว่า GIF
- ถ้าไม่ต้องการ Transparency ใช้ JPEG แทนจะเล็กกว่ามาก

#### การบีบอัดเพิ่มเติม (Additional Compression)
- ใช้ ImageOptim หรือ Smush.it บีบอัดแบบ Lossless หลัง Export จาก Photoshop
- บีบอัดอัตโนมัติใน Build Process เพื่อให้ทุกรูปที่ Upload ผ่านการ Optimize เสมอ

### การลดจำนวน Image Requests

#### CSS Sprites
- รวมรูปเล็กๆ หลายรูปเป็นรูปเดียว ลด HTTP Request ได้มาก
- ใช้ CSS `background-position` เลือกแสดงส่วนต่างๆ ของ Sprite
- Trade-off: ไฟล์ Sprite ใหญ่ขึ้น แต่ Request น้อยลง ภาพรวมเร็วขึ้น

#### CSS3 Gradients แทนรูป
- ใช้ CSS3 สร้าง Gradient, Shape, Pattern แทนรูปจริง ลด Image Request
- รองรับ Transparency, เปลี่ยนสีง่าย, ขนาดไฟล์เล็ก

#### Data URIs (Base64 Encoding)
- แปลงรูปเป็นข้อความ Base64 ฝังใน CSS หรือ HTML ตรงๆ
- ประหยัด HTTP Request แต่ไม่สามารถ Cache แยกได้ และ CSS จะใหญ่ขึ้น

#### SVG (Scalable Vector Graphics)
- ใช้ XML นิยามรูปร่าง สี เส้น ขนาดไฟล์เล็กมาก
- รองรับ Retina Display ได้ดีเพราะเป็น Vector ขยายได้ไม่แตก
- Inline SVG ลด Request แต่ไม่ Cache ได้, External SVG ใช้ `<img>` หรือ CSS ได้
- ใช้ SVGO, Scour ลบ unnecessary code จาก Export

### การวางแผนและ Iterating
- กำหนด Style Guide สำหรับรูปภาพ เพื่อให้ทีมใช้รูปแบบเดียวกัน
- ตรวจสอบรูปภาพเป็นประจำ ลบอันที่ไม่ใช้แล้ว บีบอัดอันใหม่
- Mentor คนอื่นในทีมเรื่องการ Optimize รูปภาพ

---

## Chapter 4: การ Optimize Markup และ Styles

### การทำความสะอาด HTML
- ลบ Inline Styles ที่ควรอยู่ใน Stylesheet
- ลบ Element ที่ไม่จำเป็น (Divitis = div เยอะเกินไปจนไม่มีความหมาย)
- ใช้ Semantic HTML5 Elements เช่น `<header>`, `<article>`, `<nav>` แทน `<div>` เปล่าๆ

### Semantics
- ตั้งชื่อ Class/ID ที่สื่อความหมาย เช่น `.login`, `.sidebar` แทน `.left`, `.blue`
- ทำให้ CSS อ่านง่าย ดูแลง่าย ใช้ซ้ำได้
- ช่วย Accessibility (ผู้พิการสายตา) เพราะ Screen Reader อ่าน HTML ได้ดีกว่า

### Frameworks และ Grids
- Bootstrap, HTML5 Boilerplate ช่วยเริ่มต้นได้เร็ว แต่มี Code ที่ไม่ใช้เยอะ
- ต้อง Clean Up หลังใช้ Framework เสมอ ลบส่วนที่ไม่ได้ใช้ออก

### การทำความสะอาด CSS
- **ลบ Unused Styles**: ใช้ Chrome DevTools Audits หรือ Dust-Me Selectors หา CSS ที่ไม่ได้ใช้
- **Combine and Condense Styles**: รวม CSS ที่เหมือนกันเข้าด้วยกัน ลดขนาดไฟล์
- **CSS Shorthand**: ใช้ `background: #fff url(...) no-repeat` แทนเขียนหลายบรรทัด
- **Remove Specificity**: ใช้ Selector ที่เบากว่า ลดการใช้ `!important`
- **Clean Stylesheet Images**: ตรวจสอบรูปใน CSS ลบอันเก่า ใช้ CSS3/SVG แทน

### การ Optimize Web Fonts
- ใช้ Character Subsetting โหลดแค่ตัวอักษรที่ใช้จริง
- ใช้ `@font-face` แค่ Format WOFF (รองรับ Browser สมัยใหม่)
- ใช้ Fallback Font กรณี Web Font ไม่โหลด
- จำกัด Font Weight ให้น้อยที่สุด ทุก Weight เพิ่ม Request + Page Weight
- ใช้ Media Query โหลด Web Font เฉพาะ Large Screen

### CSS และ JavaScript Loading
- **CSS**: โหลดใน `<head>` เสมอ เพราะ CSS Block Rendering
- **JavaScript**: โหลดท้ายหน้า + ใช้ `async` attribute เพื่อไม่ Block DOM
- หลีกเลี่ยง `@import` เพราะทำให้ CSS โหลดช้าลง
- ลด CSS เหลือ 30KB ต่อ Stylesheet เป็นเป้าหมาย

### Minification และ gzip
- **Minification**: ลบ Space, Semicolon, Unnecessary Characters จาก CSS/JS ลดขนาดไฟล์ 15%+
- **gzip**: บีบอัดไฟล์ Text (HTML, CSS, JS, Font) ที่ Server-side ช่วยลดขนาดได้มาก

### Caching Assets
- ตั้ง Expires หรือ Cache-Control: max-age (สูงสุด 1 ปี) สำหรับ Static Assets
- ใช้ Last-Modified หรือ ETag สำหรับ Version Check
- ทุกไฟล์ Static (CSS, JS, Image, Font) ควร Cache ได้

---

## Chapter 5: Responsive Web Design

### ปัญหาของ Responsive Design
- Responsive Sites มักส่งรูปภาพเดียวกันให้ทุกหน้าจอ ทำให้ Mobile User ต้องดาวน์โหลดไฟล์ใหญ่เกินจำเป็น
- 72% ของเว็บ Responsive ส่ง Page Weight เท่ากันทุกหน้าจอ (Guy Podjarny study)

### Deliberately Loading Content

#### Images ใน Responsive Design
- รูปภาพควรแสดงในขนาดที่ Display จริง ไม่ใช่โหลดไฟล์ใหญ่แล้วย่อ
- **RESS (Responsive Web Design with Server-side Components)**: Server เลือกรูปให้ตาม Device
- **CSS Media Queries**: บอก Browser ว่าดาวน์โหลดรูปไหนตาม Screen Size
- **`<picture>` Element**: บอก Browser เลือกรูปตาม Media Query + Device Pixel Ratio (1x, 2x)
- **`srcset` + `sizes`**: ให้ Browser เลือกรูปที่เหมาะสมที่สุดจากหลายขนาด
- `display: none` ไม่ได้หยุด Browser จากการดาวน์โหลดรูป ต้องซ่อน Parent Element แทน

#### Fonts ใน Responsive Design
- ใช้ Media Query โหลด Web Font เฉพาะหน้าจอใหญ่ ประหยัด Request บน Mobile

### วิธีเข้าถึง Responsive Design
- **Project Documentation**: กำหนด Performance Budget ทุก Breakpoint เช่น Page Weight ≤ 300KB สำหรับ ≤640px
- **Mobile First**: เริ่มออกแบบจากหน้าจอเล็กสุดก่อน ค่อยเพิ่ม Feature สำหรับจอใหญ่
- **Measure Everything**: วัด Performance ทุก Breakpoint ด้วย Chrome DevTools Emulation หรือ WebPagetest

---

## Chapter 6: การวัดและปรับปรุง Performance

### Browser Tools

#### YSlow
- Plug-in สำหรับ Firefox, Chrome, Opera, Safari
- ให้ Grade (A-F) สำหรับ Performance ของเว็บ
- แนะนำวิธีปรับปรุง เช่น Load Order, Compression, Caching
- ใช้ Components Tab ดูขนาดไฟล์แต่ละ Type

#### Chrome DevTools
- Web Page Performance Audit: แนะนำวิธีปรับปรุง
- Network Tab: แสดง Waterfall ของ Request ทุกตัว
- FPS Meter: ตรวจจับ Jank (อาการกระตุกตอน Scroll)
- ใช้ Timeline View บันทึก Frame Rate ระหว่างใช้งาน

### Synthetic Testing

#### WebPagetest
- ทดสอบจาก Location/Browser/Connection ต่างๆ ทั่วโลก
- แสดง Waterfall Chart, Speed Index, Filmstrip View
- แนะนำ Performance Review พร้อม Grade
- รัน 5 Runs แล้วเลือก Median Result
- เปรียบเทียบ 2 URL ด้วย Visual Comparison Tool

### Real User Monitoring (RUM)
- วัดเวลาโหลดจริงจากผู้ใช้จริง ต่างจาก Synthetic Testing ที่เป็นการทดสอบจำลอง
- เครื่องมือ: Google Analytics, mPulse, Glimpse
- วัด Median Load Time, 95th Percentile แยกตาม Geography, Network Type
- 95th Percentile คือ 5% ที่ช้าสุด สำคัญเพราะยังเป็นส่วนหนึ่งของผู้ใช้

### Changes over Time
- ตรวจสอบ Performance เป็นประจำรายสัปดาห์/รายไตรมาส
- ติดตาม Page Weight, Load Time, Speed Index เปรียบเทียบ Quarter to Quarter
- บันทึกเหตุผลที่ Performance เปลี่ยน (Redesign, New Script, New Image)
- ติดตาม Performance ของคู่แข่งด้วย
- สร้าง Dashboard และ Alert อัตโนมัติเมื่อ Performance เปลี่ยน

---

## Chapter 7: การชั่งน้ำหนัก Aesthetics vs Performance

### Finding the Balance
- Performance กับ Aesthetics ไม่ใช่ศัตรูกัน แต่ต้องชั่งน้ำหนักร่วมกัน
- ต้องชั่งน้ำหนัก 3 มิติ: Performance Difference, Aesthetics Difference, Operational Cost
- Operational Cost คือเวลาที่เสียไป ความยากในการ Maintenance ความรู้ทีม

### Performance Budget
- กำหนดงบ Performance ตั้งแต่แรก เช่น Total Page Load Time ≤ 2s, Page Weight ≤ 800KB
- ถ้าต้องเพิ่ม Feature ที่หนัก ก็ลด Feature อื่นเพื่อชดเชย
- วัดเป้าหมายด้วย WebPagetest, Real User Monitoring

### A/B Testing
- ทดสอบสอง Version พร้อมกัน วัดว่าผู้ใช้ตอบสนองอย่างไร
- บางครั้ง Font Weight เพิ่มขึ้นก็ไม่ได้แย่ ถ้าผู้ใช้ชอบ
- YouTube "Feather" Experiment: หน้าเบาลง แต่คนใช้ได้มากขึ้นเพราะเร็วขึ้น

### Workflow
- ใส่ Performance เป็นส่วนหนึ่งของ Workflow ประจำวัน
- Automate Image Compression ใช้ Image Resizing Service
- มี Style Guide ให้คนในทีมใช้ Pattern เดียวกัน

---

## Chapter 8: การเปลี่ยนวัฒนธรรมองค์กร

### Performance Cops vs Champions
- อย่าให้คนเดียวแบกรับ Performance ทั้งหมด เพราะจะ Burnout
- Performance Champion ควรทำ: สอน, สร้างเครื่องมือ, ตั้ง Baseline, เฉลิมฉลองชัยชนะ

### Upward Management
- โน้มน้าวผู้บริหารด้วย Business Metrics (Conversion Rate, Revenue, Bounce Rate)
- แสดง WebPagetest Filmstrip เปรียบเทียบเว็บเรากับคู่แข่งให้ผู้บริหาร "รู้สึก" ความต่าง
- รัน Quick Win Experiment เช่น บีบอัดรูป แล้ววัด Impact ต่อ Business

### การ Education และ Empower ทีม
- จัด Lunch-and-Learn สอนเรื่อง Performance ให้ Designer/Developer
- แสดง Performance Data บน Toolbar ภายใน (เช่นที่ Etsy ทำ)
- สร้าง "Performance Heroes Dashboard" เฉลิมฉลองคนที่ช่วยปรับปรุง Performance
- ทำ Ticket List ขนาดเล็ก ให้คนในทีมหยิบไปทำได้ง่าย

---

## สรุปรวม

หนังสือเล่มนี้ครอบคลุมทุกด้านของการ Optimize Performance สำหรับเว็บ:

1. **Performance คือ UX**: ความเร็วส่งผลโดยตรงต่อความไว้วางใจ ยอดขาย และ SEO
2. **Mobile First**: Mobile Network มี Latency สูง ต้อง Optimize เป็นพิเศษ
3. **Image Optimization**: คือ Win ที่ใหญ่ที่สุด เลือกรูปแบบ บีบอัด ลด Request
4. **Clean Markup**: HTML/CSS ที่สะอาด = ไฟล์เล็ก + ดูแลง่าย
5. **Responsive Done Right**: อย่าส่งไฟล์ใหญ่ให้หน้าจอเล็ก ใช้ `<picture>`, `srcset`
6. **Measure Everything**: ใช้ YSlow, Chrome DevTools, WebPagetest, RUM
7. **Performance Budget**: กำหนดงบตั้งแต่แรก แล้วชั่งน้ำหนักระหว่าง Beauty กับ Speed
8. **Culture Change**: Performance ไม่ใช่แค่ Technical Problem แต่เป็น Cultural Problem

---
