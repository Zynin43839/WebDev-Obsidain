# Think Like a Programmer: An Introduction to Creative Problem Solving

> **Author:** V. Anton Spraul

---

## บทนำ — แก่นของหนังสือเล่มนี้คืออะไร

หนังสือเล่มนี้สอนให้คิดแบบโปรแกรมเมอร์ (Think Like a Programmer) ไม่ใช่สอนภาษาใดภาษาหนึ่งเฉพาะทาง แต่สอน **วิธีคิดและแก้ปัญหาเชิงโปรแกรมมิ่ง** (Problem Solving) อย่างเป็นระบบ เนื้อหาใช้ภาษา C++ เป็นสื่อการสอน แต่แนวคิดใช้ได้กับทุกภาษา เหมาะสำหรับโปรแกรมเมอร์ที่ต้องการพัฒนาทักษะคิดวิเคราะห์และแก้ปัญหาที่ซับซ้อน

---

## บทที่ 1 — กลยุทธ์การแก้ปัญหา (Strategies for Problem Solving)

### นิยาม "การแก้ปัญหา" ของโปรแกรมเมอร์

การแก้ปัญหา (Problem Solving) คือ **การเขียนโปรแกรมใหม่ที่ทำงานตามที่กำหนด และทำตามข้อจำกัดทั้งหมด** (Constraints) ข้อจำกัดอาจเป็นภาษาที่ใช้, แพลตฟอร์ม, ประสิทธิภาพ, หรือหน่วยความจำ โปรแกรมเมอร์มือใหม่มักโฟกัสแค่ "ให้มันทำงานได้" แต่ลืม **ข้อจำกัด** — สิ่งนี้ผู้เขียนเรียกว่า **Kobayashi Maru** (จาก Star Trek) คือการแก้ปัญหาแบบ "หลีกเลี่ยงข้อจำกัด" แทนที่จะแก้ให้ครบ

### แบบฝึกหัดคลาสสิกที่ใช้สอนวิธีคิด

**1. ปัญหาสุนัขจิ้งจอก ห่าน และข้าวโพด (Fox, Goose, Corn)**
- เกษตรกรต้องข้ามแม่น้ำ โดยบรรทุกได้ทีละอย่าง สุนัจจิ้งจอกห้ามอยู่กับห่าน ห่านห้ามอยู่กับข้าวโพด
- **บทเรียน**: การนิยามปัญหาอย่างเป็นทางORMAL (Formal Restatement) ช่วยให้เห็นทางออกที่มองไม่เห็น — เช่น การ **ย้อนกลับ** ของสิ่งที่ข้ามไปแล้ว
- ถ้าเราไม่รู้จัก "การกระทำที่เป็นไปได้ทั้งหมด" (Operations) ก็แก้ปัญหาไม่ได้
- **การคิดเกี่ยวกับปัญหา** อาจมีค่าเท่ากับหรือมากกว่าการคิดเกี่ยวกับวิธีแก้

**2. ปริศนาสไลด์ไทล์ (Sliding Tile Puzzle)**
- ปัญหาไม่ใช่การมองไม่เห็น operation แต่คือ **ความยาวของสาย的操作** ที่ต้องทำต่อกัน
- ผู้เขียนใช้เทคนิคที่เรียกว่า **Train** — การหมุนวน (Rotate) ของไทล์ตามวงจร ทำให้ย้ายกลุ่มไทล์的同时คงลำดับ
- **บทเรียน**: แม้มองไม่เห็นทางแก้ทั้งหมด ก็ต้องมีแผน (Plan) อย่าใช้แบบสุ่ม (Trial and Error)
- **การลดขนาดปัญหา** (Reduction) — ลองทำปัญหาขนาดเล็กก่อน (2×3) แล้วขยายไป 3×3 หรือ 4×4

**3. ซูโดคุ (Sudoku)**
- กฎบังคับทำให้ปัญหาง่ายขึ้น — เพราะตัดทางเลือกออก
- **บทเรียน**: ให้เริ่มจากส่วนที่ถูกจำกัดมากที่สุด (Most Constrained Variable) ก่อน

**4. ปริศนา Quarrasi Lock (ตัวปลอม)**
- ปัญหาหน้าตาซับซ้อน แต่จริงๆ คือ "ปัญหาสุนัขจิ้งจอก ห่าน และข้าวโพด" ที่ปลอมตัวมา
- **บทเรียน**: การ **มองหาความคล้ายคลึง** (Analogy) ระหว่างปัญหาที่แก้แล้วกับปัญหาใหม่เป็นทักษะสำคัญที่สุดของโปรแกรมเมอร์

### กฎการแก้ปัญหาทั่วไป 7 ข้อ

1. **Always Have a Plan** — ต้องมีแผนเสมอ แม้แผนอาจต้องเปลี่ยนระหว่างทาง การวางแผนช่วยให้เห็น progress และมี intermediate goals
2. **Restate the Problem** — นิยามปัญหาใหม่ด้วยมุมมองต่างๆ อาจพบทางออกที่ซ่อนอยู่
3. **Divide the Problem** — แบ่งปัญหาเป็นส่วนย่อยๆ แต่ละส่วนง่ายกว่าเดิมมาก (ไม่ใช่แค่ครึ่งเดียว)
4. **Start with What You Know** — เริ่มจากส่วนที่ทำได้ก่อน ทำ partial solution แล้วต่อยอด
5. **Reduce the Problem** — ลด scope ของปัญหา (เช่น 3D → 2D → 1D) แล้วค่อยๆ ขยายกลับ
6. **Look for Analogies** — หาปัญหาที่เคยแก้แล้วที่คล้ายกัน แล้วใช้แนวทางเดียวกัน
7. **Experiment** — ทดสอบสมมติฐานอย่างเป็นระบบ ไม่ใช่เดาสุ่ม
8. **Don't Get Frustrated** — อย่าหงุดหงิด ถ้าติดให้พัก ทำอย่างอื่น แล้วค่อยกลับมา

---

## บทที่ 2 — ปริศนาบริสุทธิ์ (Pure Puzzles)

เนื้อหาในบทนี้ใช้ C++ พื้นฐานเท่านั้น (if, for, while, switch, functions) เพื่อโฟกัสที่วิธีคิดมากกว่า syntax

### 2.1 Output Patterns — สร้างรูปทรงด้วย Loop

**ปัญหา Half of a Square**: สร้างรูปครึ่งจัตุรัส `#####` ลดหลั่นลงมา โดยใช้แค่ `cout << "#"` กับ `cout << "\n"`

- **เทคนิค Reduction**: ลดจากครึ่งจัตุรัส → เต็มจัตุรัส → แค่ 1 บรรทัด แล้วค่อยๆ ขยายกลับ
- ใช้ nested loop: วงนอกคือ row, วงในคือ number of hash marks
- หา formula ผ่าน **การทดลอง** — เช่น `6 - row` สำหรับลดจำนวน # แต่ละแถว
- ใช้ **Absolute Value** (`abs(4 - row)`) เพื่อสร้างรูปสามเหลี่ยมหมุน

**บทเรียน**: แม้ปัญหาดูง่าย การใช้ reduction technique ช่วยให้แก้ได้ทุกขนาด

### 2.2 Input Processing — ประมวลผล input ทีละตัวอักษร

**ปัญหา Luhn Checksum Validation**:
- สูตร Luhn: คูณ 2 กับทุกตัวเลขที่ตำแหน่งคู่ (นับจากขวา), บวก digits ทั้งหมด, ถ้าหาร 10 ลงตัว = ถูกต้อง
- **เทคนิค**: แยกปัญหาเป็น subproblems แล้วแก้ทีละตัว:
  1. แปลง char digit → integer (ใช้ `digit - '0'`)
  2. คูณ 2 แล้วบวก digits ถ้า > 9 (ใช้ `1 + doubled % 10`)
  3. อ่านทีละตัวอักษรจาก cin (ใช้ `cin.get()`)
  4. รู้ว่าถึงจุดจบยังไง (เช็ค `\n` หรือ comma)
  5. ไม่รู้ความยาวล่วงหน้า → คำนวณทั้ง odd และ even checksum แล้วเลือกตอนจบ
- **บทเรียน**: การใช้ **analogy** กับ "Positive or Negative" problem ช่วยให้เห็นว่า track 2 ค่าพร้อมกันแล้วเลือกทีหลังทำได้

### 2.3 Tracking State — ถอดรหัสข้อความ

**ปัญหา Decode a Message**:
- ข้อความเข้ารหัสเป็น integer คั่นด้วย comma, 3 modes (UPPERCASE/LOWERCASE/PUNCTUATION)
- เมื่อ modulo ได้ 0 → สลับ mode
- **เทคนิค**: แยกย่อยเป็น subproblems เล็กๆ → อ่าน integer ทีละตัวจาก stream, แปลง number → letter (`number + 'A' - 1`), จัดการ mode ด้วย enumeration
- ประกอบชิ้นส่วนทั้งหมดเข้าด้วยกัน — **code reuse** จาก subproblems ที่เขียนไว้

**บทเรียน**: ทุกบรรทัดใน final program มาจาก subproblems ที่แยกเขียน+test ไว้ก่อนหน้า ไม่ต้องเขียนทั้งหมดในครั้งเดียว

---

## บทที่ 3 — แก้ปัญหาด้วย Array (Solving Problems with Arrays)

### พื้นฐาน Array Operations

- **Store**: กำหนดค่าให้ element แต่ละตัว
- **Copy**: คัดลอก array ทั้งหมด (เพื่อ preserve ต้นฉบับ หรือ shuffle ลำดับ)
- **Retrieval & Search**: ดึงค่าจาก index ที่รู้ หรือ Sequential Search หาค่าที่ต้องการ
- **Criterion-Based Search**: หาค่าที่ตรงเงื่อนไข (เช่น ค่าสูงสุด — "King of the Hill" technique)
- **Sort**: `qsort` สำหรับเร็ว, Insertion Sort สำหรับเข้าใจง่ายและแก้ไขได้
- **Compute Statistics**: หาค่าเฉลี่ย, median, mode ฯลฯ

### ตัวอย่าง — Finding the Mode

**ปัญหา**: หาค่าที่ปรากฏบ่อยที่สุดใน array

- **แนวทางแรก**: Sort array ให้ค่าเดียวกันอยู่ติดกัน แล้วนับทีละกลุ่ม — ใช้ `qsort` กับ counting logic
- **Refactoring**: สร้าง **Histogram Array** นับความถี่ของแต่ละค่า แล้วหา max ใน histogram — ประสิทธิภาพดีกว่า (Linear time vs Sort = O(n log n))
- **บทเรียน**: โค้ดที่ทำงานได้แล้วอาจไม่ดีที่สุด — **Refactoring** คือการปรับปรุงวิธีทำงานโดยไม่เปลี่ยนผลลัพธ์

### Array of Fixed Data — Lookup Table

- ใช้ `const array` เป็น lookup table แทน switch statement ยาวๆ
- เช่น ตารางภาษี license: สร้าง array ของ threshold + array ของค่า แล้วใช้ loop หา category
- **บทเรียน**: Const array ช่วยลด control statements และ scale ได้ดีเมื่อมีข้อมูลเพิ่ม

### Non-scalar Arrays & Multidimensional Arrays

- Array ของ struct/class: ใช้ `.member` access เหมือน scalar
- 2D Array: ใช้ nested loop; พิจารณา **orientation** ของข้อมูล (rows vs columns)
- บางครั้งควร split 2D array เป็น array ของ struct แทน — ช่วยให้ code อ่านง่ายและใช้ function ได้สะดวก

### เมื่อไหร่ควรใช้ Array?

- รู้ขนาด max ล่วงหน้า หรือมี estimate ที่เชื่อถือได้
- ต้องการ **random access** (เข้าถึง element 任意 ตัวโดยตรง)
- ถ้าไม่ต้องการ random access → พิจารณา linked list หรือ vector
- ถ้าข้อมูลน้อย (< 10 ตัว) → ไม่ต้องคิดมาก ใช้ array ไป

---

## บทที่ 4 — แก้ปัญหาด้วย Pointer และ Dynamic Memory

### Pointers — ทำไมต้องเรียน?

- ช่วยให้สร้างข้อมูลขนาด runtime (ไม่ต้องกำหนดล่วงหน้า)
- สร้าง data structure ที่ resize ได้ (linked list, dynamic array)
- ใช้ shared memory ผ่าน reference parameters ลดการ copy
- แม้ภาษาสูงจะซ่อน pointer ไว้ แต่ต้องเข้าใจเพื่อ debug ปัญหาที่ซับซ้อน

### Stack vs Heap

- **Stack**: Memory จัดเรียงเป็น stack, จัดการอัตโนมัติตาม function call (activation record)
- **Heap**: Memory แบบ "กระจาย", จัดการด้วย `new`/`delete` เอง, เกิด **memory fragmentation** ได้
- **Memory Leaks**: ถูก `new` แต่ไม่ถูก `delete` — memory หายไปตลอดกาล
- **Dangling Pointers**: pointer ชี้ไป memory ที่ถูก delete แล้ว → ใช้งานไม่ได้
- **Stack Overflow**: เรียก function ซ้ำๆ มากเกินไปจน stack เต็ม

### ตัวอย่าง — Variable-Length String

- ใช้ dynamic array (`char*`) เป็น string representation
- ใช้ null terminator (`\0`) กำกับจุดจบของ string (เหมือน C string ดั้งเดิม)
- **Append**: สร้าง array ใหม่ขนาดใหญ่ขึ้น, copy เดิม + เพิ่มตัวใหม่ + null terminator, delete array เดิม
- **Concatenate**: เหมือน append แต่เพิ่ม string ทั้งก้อน
- **เทคนิค Solving by Sample Case**: เขียนตัวอย่าง input/output บนกระดาษก่อนเขียนโค้ด ช่วยติดตาม pointer/memory state ได้ชัดเจน

### ตัวอย่าง — Linked List

- **Node struct**: มี data + pointer ไป node ถัดไป (self-referencing struct)
- **Head Pointer**: pointer ชี้ไป node ตัวแรก เป็นตัวแทนของ list ทั้งก้อน
- **Add Node**: เพิ่มที่หน้า (beginning) ดีกว่าท้าย (end) เพราะ O(1) vs O(n)
- **List Traversal**: ใช้ while loop จนกว่า pointer จะเป็น NULL
- **Special Cases**: ต้องเช็ค empty list (NULL head) เสมอ — ไม่งั้น divide by zero

**บทเรียน**: ใช้ diagram (แผนภาพ) ช่วยคิดเสมอ — วาด before/after state ของ memory แล้วเขียนโค้ดตาม diagram

---

## บทที่ 5 — แก้ปัญหาด้วย Class (Object-Oriented Programming)

### Class Fundamentals

- **Class**: Blueprint สำหรับสร้าง object ที่มี data members + methods
- **Access Specifiers**: `public` (เข้าถึงได้ทุกที่), `private` (เข้าถึงได้เฉพาะใน class), `protected` (เข้าถึงได้ใน subclass ด้วย)
- **Constructor**: Method ที่ชื่อเดียวกับ class, เรียกอัตโนมัติตอนสร้าง object
- **Encapsulation**: ซ่อน implementation ไว้ภายใน class, ให้ interface ที่ชัดเจนกับ client code

### ทำไมใช้ Class?

- จัดกลุ่ม data + function ที่เกี่ยวข้องกันอย่างมีความหมาย
- ป้องกันปัญหาจาก pointer โดยการ wrap ไว้ใน class (เช่น destructor จัดการ memory อัตโนมัติ)
- ใช้ inheritance (subclass) เพื่อขยายความสามารถโดยไม่แก้ code เดิม
- **บทเรียน**: อย่าใช้ class ทุกอย่าง — ใช้เฉพาะเมื่อมี data + behavior ที่เกี่ยวข้องกันจริงๆ

---

## สรุปภาพรวมของหนังสือ

| หัวข้อ | แก่นความรู้ |
|---|---|
| **วิธีคิด** | Plan → Restate → Divide → Reduce → Analogy → Experiment |
| **Pattern & Loop** | ลดปัญหาเป็นชิ้นเล็กๆ แล้วต่อเข้าด้วยกัน |
| **Input Processing** | อ่านทีละตัว, แปลง char→int, จัดการ stream ไม่รู้จบ |
| **Array** | Store, Search, Sort, Histogram, Lookup Table |
| **Pointer & Memory** | Stack vs Heap, Diagram ช่วยคิด, ป้องกัน memory leak |
| **Linked List** | Dynamic structure, O(1) insert, Sequential access only |
| **Class** | Encapsulation, Access control, When to use (and when not to) |

### หลักการสำคัญที่ต้องจำ

1. **มีแผนเสมอ** — แม้แผนจะเปลี่ยน แต่การวางแผนคือกระบวนการที่ให้คุณค่า
2. **ลดปัญหาเป็นชิ้นเล็ก** — ยิ่งเล็กยิ่งง่าย อย่ากลัวที่จะใช้ many small steps
3. **วาด diagram ก่อนเขียนโค้ด** — โดยเฉพาะกับ pointer/memory manipulation
4. **เริ่มจากส่วนที่รู้** — ทำ partial solution ก่อน แล้วขยาย
5. **Look for Analogies** — ปัญหาที่ดูใหม่อาจเป็นปัญหาเก่าที่ปลอมตัวมา
6. **Refactor ได้เสมอ** — โค้ดที่ทำงานได้แล้วอาจดีกว่าเดิมได้
7. **อย่าหงุดหงิด** — ความหงุดหงิดทำให้คิดไม่ออก ถ้าติดให้พักก่อน

---

*หนังสือเล่มนี้สอนวิธีคิดมากกว่าสอน syntax — เป็น foundation ที่โปรแกรมเมอร์ทุกคนควรอ่านเพื่อพัฒนาทักษะ Problem Solving อย่างเป็นระบบ*
