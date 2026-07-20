# Clean Code: A Handbook of Agile Software Craftsmanship

> **Author:** Robert C. Martin

---

## บทนำ: โค้ดที่ดี (Clean Code)

หนังสือเล่มนี้เขียนโดย Robert C. Martin หรือที่รู้จักกันในชื่อ "Uncle Bob" เป็นคู่มือสำหรับนักพัฒนาซอฟต์แวร์ที่ต้องการเขียนโค้ดที่มีคุณภาพ สะอาดตา และง่ายต่อการดูแลรักษา

Bob อธิบายว่าโค้ดที่ดีไม่ใช่แค่โค้ดที่ทำงานได้ แต่ต้องอ่านง่าย เข้าใจง่าย และเปลี่ยนแปลงได้ง่ายด้วย เขาเปรียบเทียบโค้ดเหมือนวรรณกรรม — ต้องเขียนให้คนอื่นอ่านเข้าใจ

หนังสือเล่มนี้รวมประสบการณ์จากนักพัฒนาชั้นนำหลายคน เช่น Jeff Langr, Michael Feathers, và Ron Jeffries ซึ่งแต่ละคนมีมุมมองต่อ Clean Code ที่แตกต่างกัน

Bob ย้ำว่าทีมที่ดีต้องมีมาตรฐานโค้ดเดียวกัน ทุกคนต้องรักษามาตรฐานนั้น โค้ดที่สกปรกจะทำให้โครงการล้มเหลวในระยะยาว

**สิ่งที่ได้เรียนรู้:**
- โค้ดที่ดีต้องอ่านง่าย เข้าใจง่าย
- มาตรฐานโค้ดต้องสอดคล้องกันทั้งทีม
- โค้ดสกปรกทำให้โครงการล้มเหลว
- การเขียนโค้ดเป็นทักษะที่ฝึกฝนได้

---

## บทที่ 2: ชื่อที่มีความหมาย (Meaningful Names)

### ใช้ชื่อที่เปิดเผยความตั้งใจ

ชื่อต้องบอกว่าทำไมถึงมีตัวแปรนี้อยู่ ตัวอย่างเช่น `d` ไม่ดี แต่ `elapsedTimeInDays` ดี เพราะบอกว่าเป็นระยะเวลา

### หลีกเลี่ยงข้อมูลหลอก

อย่าใช้ชื่อที่ทำให้เข้าใจผิด เช่น `accountList` ถ้ามันไม่ใช่ List ให้ใช้ `accounts` แทน

### ทำให้ชื่อเป็นการออกเสียงได้

ชื่อที่ออกเสียงง่ายจะสื่อสารได้ดีกว่า เช่น `hw` สู้ `hypotenuse` ไม่ได้

### ทำให้ชื่อเป็นการค้นหาได้

ชื่อที่ยาวและชัดเจนจะค้นหาใน IDE ได้ง่ายกว่า

### หลีกเลี่ยงการเข้ารหัส

อย่าใส่ข้อมูลประเภทในชื่อ เช่น `String name` ไม่ต้องเขียนเป็น `String nameString`

### หลีกเลี่ยงการตั้งชื่อแบบ�ีความหมายสองนัย

ชื่อต้องมีความหมายเดียวเท่านั้น เช่น `get()` ไม่ชัดเจน ต้องเจาะจงกว่านั้น

### ใช้คำศัพท์ที่สอดคล้องกัน

อย่าใช้คำที่มีความหมายเดียวกันสลับกัน เช่น ถ้าเรียก `fetch` แล้ว อย่าเรียก `retrieve` ในที่อื่น

### อย่าใช้คำสแลงหรือคำเฉพาะทาง

ชื่อต้องเป็นภาษาที่ทุกคนในทีมเข้าใจ อย่าใช้คำเฉพาะทางหรือคำสแลง

### ตั้งชื่อจากDOMAIN ของปัญหา

ใช้คำศัพท์จาก domain ของปัญหาที่กำลังแก้ จะสื่อสารได้ดีกว่า

**ตัวอย่างโค้ดที่ดี:**

```java
// ไม่ดี
int d; // elapsed time in days

// ดี
int elapsedTimeInDays;
```

---

## บทที่ 3: ฟังก์ชัน (Functions)

### ทำสิ่งเดียว

ฟังก์ชันต้องทำสิ่งเดียว ทำให้ดี ทำให้สำเร็จ ถ้าฟังก์ชันทำหลายอย่าง ให้แยกออกมาเป็นฟังก์ชันย่อย

### ทำให้เล็ก

ฟังก์ชันควรสั้น ที่ Bob ตั้งกฎว่าไม่เกิน 20 บรรทัด แต่ละฟังก์ชันควรอ่านได้โดยไม่ต้องเลื่อนหน้าจอ

### ระดับนามธรรมเดียว

แต่ละฟังก์ชันควรทำงานในระดับนามธรรมเดียว อย่าผสมระดับสูงและระดับต่ำในฟังก์ชันเดียวกัน

### ปิดท้ายด้วยระดับนามธรรม

ลำดับบรรทัดในฟังก์ชันควรเป็นเหมือนเรื่องเล่า — เริ่มจากระดับสูงแล้วลงไประดับต่ำ

### ใช้คำสั่งและคืนค่า

ฟังก์ชันควรสั่งงานหรือคืนค่า ไม่ควรทำทั้งสองอย่าง ถ้าคืนค่าแล้วไม่ควรมี side effect

### หลีกเลี่ยง Parameters หลายตัว

ฟังก์ชันที่มี parameters หลายตัวมักจะทำหลายอย่าง ให้แยกออกมาเป็นฟังก์ชันย่อย

### ไม่มี side effects

ฟังก์ชันไม่ควรเปลี่ยนแปลงค่าของตัวแปรภายนอกโดยไม่ตั้งใจ

### ใช้ Command-Query Separation

แยกคำสั่ง (สั่งงาน) ออกจาก Query (ถามคำถาม) อย่าทำทั้งสองอย่างในฟังก์ชันเดียว

**ตัวอย่างโค้ดที่ดี:**

```java
// ไม่ดี
public void process(String name, int age, boolean isMember) {
    // ทำหลายอย่างในฟังก์ชันเดียว
}

// ดี
public void validateMembership(Member member) {
    // ตรวจสอบความถูกต้อง
}

public void registerMember(Member member) {
    // ลงทะเบียนสมาชิก
}
```

---

## บทที่ 4: ความคิดเห็น (Comments)

### อย่าคอมเมนต์โค้ดที่ไม่ดี

ถ้าโค้ดต้องคอมเมนต์อธิบาย ให้เขียนโค้ดใหม่ให้ดีกว่าเดิม อย่าใช้คอมเมนต์มาแก้ปัญหา

### ใช้โค้ดแทนคอมเมนต์

โค้ดที่ดีจะอธิบายตัวเอง อย่าคอมเมนต์สิ่งที่โค้ดบอกอยู่แล้ว

### คอมเมนต์เมื่อจำเป็นจริงๆ

คอมเมนต์ควรใช้เฉพาะเมื่อจำเป็น เช่น เตือนเรื่อง API, อธิบายตรรกะที่ซับซ้อน, หรือบันทึกการตัดสินใจ

### คอมเมนต์ที่ดี

คอมเมนต์ที่ดีคือคอมเมนต์ที่อธิบายว่าทำไมถึงเขียนโค้ดแบบนี้ ไม่ใช่อธิบายว่าทำอะไร

### อย่าคอมเมนต์สิ่งที่ล้าสมัย

ถ้าโค้ดเปลี่ยนแล้ว ให้ลบคอมเมนต์เก่าทิ้ง อย่าปล่อยให้ล้าสมัย

**ตัวอย่างโค้ดที่ดี:**

```java
// ไม่ดี
int count; // ตัวนับ

// ดี
int numberOfUnprocessedOrders; // ชื่อตัวแปรบอกอยู่แล้ว
```

---

## บทที่ 5: การจัดรูปแบบ (Formatting)

### การจัดรูปแบบสำคัญ

การจัดรูปแบบไม่ใช่เรื่องเล็กน้อย เป็นส่วนหนึ่งของการสื่อสาร โค้ดที่จัดรูปแบบดีอ่านง่ายกว่ามาก

### ความสอดคล้อง

ทั้งทีมต้องใช้มาตรฐานการจัดรูปแบบเดียวกัน ไม่ว่าจะเป็นเรื่อง indentation, ระยะห่าง, หรือ line breaks

### กฎ 30 บรรทัด

ฟังก์ชันที่ยาวเกิน 30 บรรทัดมักจะมีปัญหาเรื่องความซับซ้อน

### อย่าปล่อยให้บรรทัดยาวเกินไป

บรรทัดที่ยาวเกิน 120 ตัวอักษรจะอ่านยาก ให้แบ่งเป็นหลายบรรทัด

### จัดกลุ่ม related code

โค้ดที่เกี่ยวข้องกันควรอยู่ใกล้กัน จัดกลุ่มอย่างเป็นระเบียบ

### ตัวคั่น函数

ใช้ blank lines คั่นระหว่างฟังก์ชัน เพื่อให้มองเห็นขอบเขตชัดเจน

### ตัวคั่น within functions

ใช้ blank lines คั่นระหว่าง logical breaks ภายในฟังก์ชันเดียวกัน

---

## บทที่ 6: วัตถุและโครงสร้างข้อมูล (Objects and Data Structures)

### ความแตกต่างระหว่าง Objects และ Data Structures

Objects ซ่อนข้อมูลไว้ภายในและเปิดเผย行为 (behavior) ในขณะที่ Data Structures เปิดเผยข้อมูลทั้งหมดและไม่มี behavior

### Data Abstraction

อย่าเปิดเผย internal structure ของ data ให้ผู้ใช้รู้ ให้ซ่อนไว้แล้วเปิดเผยแค่ interface

### Law of Demeter

วัตถุ不应该รู้จัก internals ของวัตถุอื่น อย่าเรียก method ผ่าน method chaining

### Data Transfer Objects (DTOs)

DTOs เป็น data structure ที่ใช้โอนย้ายข้อมูลระหว่าง layer ต่างๆ ไม่มี behavior

### Active Record

Active Record เป็น pattern ที่รวม data structure และ database access เข้าด้วยกัน

### วัตถุvs Data Structures

วัตถุง่ายต่อการเพิ่ม behavior ใหม่ แต่ยากต่อการเพิ่ม data structure ใหม่ Data Structures ง่ายต่อการเพิ่ม structure ใหม่ แต่ยากต่อการเพิ่ม behavior ใหม่

**ตัวอย่างโค้ด:**

```java
// Data Structure (เปิดเผยข้อมูลทั้งหมด)
public class Point {
    public double x;
    public double y;
}

// Object (ซ่อนข้อมูล)
public class Circle {
    private Point center;
    private double radius;
    
    public double area() {
        return Math.PI * radius * radius;
    }
}
```

---

## บทที่ 7: การจัดการข้อผิดพลาด (Error Handling)

### อย่าซ่อนข้อผิดพลาด

อย่า catch exception แล้วไม่ทำอะไร ให้ log หรือ propagate ออกไป

### ให้ message ที่มีความหมาย

ข้อความ error ต้องบอกว่าเกิดอะไรขึ้นและทำไมถึงเกิด

### อย่า return null

อย่าคืนค่า null ให้ใช้ empty objects หรือ special values แทน

### อย่าส่งต่อ null

อย่าส่ง null ไปให้ฟังก์ชันอื่น ให้ตรวจสอบก่อนเรียก

### อย่าใช้ checked exceptions

checked exceptions ทำให้โค้ดยุ่งยาก ให้ใช้ unchecked exceptions หรือ error codes

### Use exceptions instead of return codes

ใช้ exceptions แทน error codes เพราะ exceptions บังคับให้จัดการ

### จัดการ exceptions ในระดับที่เหมาะสม

อย่าจัดการ exception ในระดับต่ำเกินไป ให้จัดการในระดับที่เข้าใจ context ของปัญหา

**ตัวอย่างโค้ด:**

```java
// ไม่ดี
try {
    // process
} catch (Exception e) {
    // ซ่อน error
}

// ดี
try {
    // process
} catch (InsufficientFundsException e) {
    logger.error("Transfer failed: {}", e.getMessage());
    throw new TransferException("Transfer failed", e);
}
```

---

## บทที่ 8: ขอบเขต (Boundaries)

### รู้จักขอบเขต

โค้ดต้องรู้ว่าขอบเขตของ system อยู่ตรงไหน ไม่ควรรู้ internals ของ system อื่น

### ใช้ interfaces แทน classes

ให้โปรแกรมขึ้นกับ interfaces ไม่ใช่ implementations เพื่อความยืดหยุ่น

### อย่าใช้ third-party มากเกินไป

ยิ่งใช้ third-party มากเท่าไหร่ ยิ่งเสี่ยงต่อการเปลี่ยนแปลง

### Boundary testing

ทดสอบ boundary ระหว่าง system ต่างๆ เพื่อให้แน่ใจว่าทำงานร่วมกันได้ถูกต้อง

### สร้าง boundaries ของตัวเอง

สร้าง interfaces ภายในของคุณเองเพื่อแยก third-party code ออกจาก business logic

**ตัวอย่างโค้ด:**

```java
// ไม่ดี - ขึ้นกับ implementation
ArrayList<String> list = new ArrayList<>();

// ดี - ขึ้นกับ interface
List<String> list = new ArrayList<>();
```

---

## บทที่ 9: ทดสอบหน่วย (Unit Tests)

### 3 Laws of TDD

1. ต้องเขียน failing unit test ก่อน
2. ต้องเขียน minimal code ที่ทำให้ test ผ่าน
3. ต้อง refactoring อยู่เสมอ

### Testing是 Design

การทดสอบช่วยออกแบบโค้ด ถ้าทดสอบยาก แสดงว่าออกแบบไม่ดี

### FIRST Rules

- Fast: ทดสอบเร็ว
- Independent: ทดสอบอิสระจากกัน
- Repeatable: ทดสอบซ้ำได้
- Self-Validating: ทดสอบตัวเองได้
- Timely: เขียนทดสอบทันเวลา

### ทดสอบ Boundaries

ทดสอบboundary ระหว่าง components ต่างๆ

### ทดสอบ clean code

clean code ต้องทดสอบได้ง่าย ถ้าทดสอบยาก แสดงว่าโค้ดไม่สะอาด

**ตัวอย่างโค้ด:**

```java
@Test
public void shouldTransferFunds() {
    Account from = new Account(1000);
    Account to = new Account(0);
    
    from.transfer(to, 500);
    
    assertEquals(500, from.getBalance());
    assertEquals(500, to.getBalance());
}
```

---

## บทที่ 10: คลาส (Classes)

### Single Responsibility Principle

คลาสต้องมีResponsibility เดียว ทำสิ่งเดียวให้ดี

### Cohesion

คลาสที่ดีมี methods ที่ใช้ data เดียวกัน methods ควรใช้ instance variables เดียวกัน

### หลีกเลี่ยง data hugging

อย่าใส่ data มากเกินไปในคลาสเดียว ให้แยกตาม responsibility

### ขนาดคลาส

คลาสที่ดีควรสั้น ไม่ควรมี methods มากเกินไป

### หลีกเลี่ยง inheritance

 inheritance มีปัญหาเรื่อง tight coupling ให้ใช้ composition แทน

### ใช้ composition แทน inheritance

composition ยืดหยุ่นกว่า inheritance เพราะเปลี่ยน behavior ได้ runtime

**ตัวอย่างโค้ด:**

```java
// ไม่ดี - คลาสใหญ่เกินไป
public class UserManager {
    // methods 100+ methods
}

// ดี - แยก responsibility
public class UserValidator {
    // ตรวจสอบความถูกต้อง
}

public class UserPersistence {
    // จัดการข้อมูล
}
```

---

## บทที่ 11: ระบบ (Systems)

### ความใส่ใจในรายละเอียด

ระบบต้องดูแลเรื่อง dependency injection, service locator, และ architecture

### Separation of Concerns

แยก concerns ต่างๆ ออกจากระบบ อย่าผสมกัน

### Dependency Injection

ใช้ dependency injection เพื่อลด coupling ระหว่าง components

### Aspect Oriented Programming

ใช้ AOP เพื่อจัดการ cross-cutting concerns เช่น logging, security

### Testability

ระบบที่ออกแบบดีต้องทดสอบได้ง่าย

### หลีกเลี่ยง God Object

อย่ามีคลาสที่รู้จักทุกอย่าง ให้แยก responsibilities

### สร้าง boundaries ที่ชัดเจน

กำหนด boundaries ระหว่าง components อย่างชัดเจน

---

## บทที่ 12: การเกิดขึ้น (Emergence)

### Emergent Design

Emergent Design คือการออกแบบที่เกิดขึ้นจากการ refactoring อยู่เสมอ

### 4 Rules of Simple Design

1. ผ่าน tests
2. ไม่มี code duplication
3. แสดงความตั้งใจ
4. จำนวน methods/classes น้อยที่สุด

### Refactoring

Refactoring คือการเปลี่ยนแปลงโครงสร้างโค้ดโดยไม่เปลี่ยน behavior

### ทดสอบก่อน refactor

ก่อน refactor ต้องมี tests ที่ครอบคลุม เพื่อให้มั่นใจว่า behavior ไม่เปลี่ยน

### Small changes

refactor ทีละเล็กทีละน้อย อย่าเปลี่ยนแปลงใหญ่เกินไป

**ตัวอย่างโค้ด:**

```java
// Refactoring from long method
public void processOrder(Order order) {
    validateOrder(order);
    calculateTotal(order);
    applyDiscount(order);
    saveOrder(order);
    notifyCustomer(order);
}
```

---

## บทที่ 13: ขอบเขต (Boundaries) - ตอนจบ

### ข้อควรระวัง

-boundaries ต้องได้รับการปกป้อง ต้องมี tests ครอบคลุม

### ทดสอบ third-party code

third-party code ต้องทดสอบผ่าน boundaries ของเรา

### สร้าง adapter

ใช้ adapter pattern เพื่อแยก third-party code ออกจาก business logic

### อย่า test implementation details

ทดสอบ behavior ไม่ใช่ implementation

---

## บทที่ 14: JUnit (JUnit Internals)

### ภายใน JUnit

JUnit ใช้ reflection และ dynamic proxies เพื่อสร้าง tests

### ความสำคัญของ testing framework

framework ที่ดีช่วยให้เขียน tests ได้ง่าย

### ใช้ assertion ที่ชัดเจน

assertions ต้องบอกว่าทำไม test ถึง fail

### ทดสอบ edge cases

ทดสอบทั้ง normal cases และ edge cases

---

## บทที่ 15: การปรับโครงสร้าง (Refactoring - SerialDate)

### ตัวอย่างจริง

Bob แสดงวิธี refactoring โค้ดจริงจาก проект SerialDate

### ปัญหาที่พบ

โค้ดเก่ามี methods ยาว, duplication, และ logic ซับซ้อน

### วิธี refactoring

1. _extract method_ — แยก method ยาวเป็น method สั้น
2. _rename_ — ตั้งชื่อใหม่ให้ชัดเจน
3. _move method_ — ย้าย method ไปอยู่ในคลาสที่เหมาะสม
4. _replace temp with query_ — แทน temp variable ด้วย method call

### ผลลัพธ์

โค้ดสั้นลง, อ่านง่ายขึ้น, และทดสอบได้ง่ายขึ้น

---

## บทที่ 16: การปรับโครงสร้าง (Refactoring - Payroll)

### กรณีศึกษา Payroll

Bob แสดงวิธี refactoring ระบบ Payroll ที่มี complexity สูง

### ปัญหาหลัก

โค้ดเดิมมี switch statements ยาว, data structures ซับซ้อน, และ tightly coupled

### วิธีแก้ไข

1. _Replace conditional with polymorphism_ — ใช้ inheritance แทน switch
2. _Extract class_ — แยกคลาสจาก code ที่ทำหลายอย่าง
3. _Introduce parameter object_ — รวม parameters ที่เกี่ยวข้องกัน
4. _Replace magic numbers with named constants_ — แทนตัวเลขด้วยชื่อ

### ผลลัพธ์

โค้ดยืดหยุ่นขึ้น, เพิ่ม feature ได้ง่ายขึ้น, และ maintain ได้ง่ายขึ้น

---

## บทที่ 17: กลิ่นโค้ดและโค้ดไม่ดี (Code Smells and Bad Code)

### Code Smells

Code Smells คือสัญญาณเตือนว่าโค้ดมีปัญหา ไม่ใช่ error แต่เป็นสัญญาณว่าต้อง refactoring

### Bloaters

- Long Method: method ยาวเกินไป
- Large Class: คลาสใหญ่เกินไป
- Long Parameter List: parameters มากเกินไป
- Data Clumps: data กลุ่มเดียวกันปรากฏหลายที่

### Object-Orientation Abusers

- Switch Statements: ใช้ switch บ่อยเกินไป
- Parallel Inheritance Hierarchies: inheritance คู่ขนาน
- Alternative Classes with Different Interfaces: คลาสที่ทำเหมือนกันแต่ interface ต่างกัน

### Change Preventers

- Divergent Change: คลาสที่ต้องเปลี่ยนแปลงหลายเหตุผล
- Shotgun Surgery: เปลี่ยนแปลงทีเดียวหลายที่
- Feature Envy: method ที่ใช้ data จากคลาสอื่นมากกว่าของตัวเอง

### Dispensables

- Comments: comments ที่ไม่จำเป็น
- Duplicate Code: code ซ้ำกัน
- Lazy Class: คลาสที่ไม่ได้ทำอะไร
- Data Class: คลาสที่มีแค่ data ไม่มี behavior

### Couplers

- Feature Envy: method ที่envy methods ของคลาสอื่น
- Inappropriate Intimacy: คลาสที่รู้จัก internals ของกันและกัน
- Message Chains: method chaining ยาวเกินไป
- Middle Man: คลาสที่ทำหน้าที่แค่ส่งต่อ

---

## บทที่ 18: สถาปัตยกรรม (Architecture)

### สถาปัตยกรรม的重要性

สถาปัตยกรรมที่ดีทำให้ระบบ maintain ได้ง่าย, test ได้ง่าย, และเปลี่ยนแปลงได้ง่าย

### Clean Architecture

Bob แนะนำ Clean Architecture ที่มี layers ชัดเจน:
1. **Entities**: business objects
2. **Use Cases**: application-specific business rules
3. **Interface Adapters**: แปลงข้อมูลระหว่าง layers
4. **Frameworks and Drivers**: เครื่องมือและ frameworks

### Dependency Rule

dependencies ต้องชี้เข้าด้านในเท่านั้น ชั้นนอกต้องขึ้นกับชั้นใน ไม่ใช่กลับกัน

### The Endgame

สถาปัตยกรรมที่ดีทำให้ระบบยืดหยุ่น เปลี่ยนแปลงได้ง่าย โดยไม่กระทบ core business logic

---

## บทที่ 19: นักพัฒนาฉลาด vs โง่ (Smart vs Stupid Developers)

### ความแตกต่าง

นักพัฒนาฉลาดเขียนโค้ดที่คนอื่นอ่านเข้าใจ นักพัฒนาโง่เขียนโค้ดที่ตัวเองเข้าใจคนเดียว

### The Boy Scout Rule

 leaving the campground cleaner than you found it — ทิ้งโค้ดไว้ให้ดีกว่าที่เจอ

### Professionalism

ความเป็นมืออาชีพหมายถึงการเขียนโค้ดที่ดี, ทดสอบ, และ maintain ได้

### ความรับผิดชอบ

นักพัฒนามืออาชีพรับผิดชอบต่อโค้ดของตัวเอง ไม่ใช่โยนให้คนอื่น

---

## บทที่ 20: โค้ดแบบเลนส์กล้อง (Telescoping Code)

### ปัญหา

Telescoping code คือโค้ดที่มี parameters หลายตัวสลับกันไปมา ทำให้ยากต่อการอ่านและใช้งาน

### Builder Pattern

ใช้ Builder pattern เพื่อสร้าง objects ที่มี parameters หลายตัวอย่างชัดเจน

### Telescoping Constructor

หลีกเลี่ยง telescoping constructor ที่มี parameters หลายตัวสลับกัน

### ตัวอย่าง

```java
// ไม่ดี - Telescoping Constructor
public User(String name, int age) { ... }
public User(String name, int age, String email) { ... }
public User(String name, int age, String email, String phone) { ... }

// ดี - Builder Pattern
User user = new UserBuilder()
    .setName("John")
    .setAge(30)
    .setEmail("john@example.com")
    .build();
```

---

## บทสรุป

Clean Code เป็นคู่มือที่สำคัญสำหรับนักพัฒนาซอฟต์แวร์ทุกคน ให้แนวทางในการเขียนโค้ดที่มีคุณภาพ สะอาดตา และง่ายต่อการดูแลรักษา

**หลักการสำคัญ:**
1. **Meaningful Names**: ตั้งชื่อที่สื่อความหมายชัดเจน
2. **Small Functions**: ฟังก์ชันควรสั้น ทำสิ่งเดียว
3. **Comments**: ใช้เมื่อจำเป็นจริงๆ
4. **Formatting**: จัดรูปแบบให้อ่านง่าย
5. **Error Handling**: จัดการข้อผิดพลาดอย่างเหมาะสม
6. **Unit Tests**: ทดสอบอยู่เสมอ
7. **Classes**: คลาสเล็กๆ ที่มี responsibility เดียว
8. **Systems**: แยก concerns อย่างชัดเจน
9. **Emergent Design**: refactoring อยู่เสมอ
10. **Architecture**: ออกแบบระบบอย่างมีชั้นเชิง

**สิ่งที่ได้เรียนรู้จากหนังสือเล่มนี้:**
- โค้ดที่ดีไม่ใช่แค่โค้ดที่ทำงานได้ แต่ต้องอ่านง่าย เข้าใจง่าย เปลี่ยนแปลงได้ง่าย
- การเขียน clean code เป็นทักษะที่ฝึกฝนได้ ต้อง practice อยู่เสมอ
- ทีมต้องมีมาตรฐานเดียวกัน ทุกคนต้องรักษามาตรฐานนั้น
- Clean code ทำให้โครงการสำเร็จในระยะยาว
- อย่ากลัวที่จะ refactoring โค้ดอยู่เสมอ
- Testing เป็นส่วนหนึ่งของการเขียนโค้ด ไม่ใช่ทางเลือก

**การนำไปใช้:**
- เริ่มจากการตั้งชื่อที่ดี
- เขียนฟังก์ชันสั้นๆ ทำสิ่งเดียว
- หลีกเลี่ยง code smells
- ทดสอบอยู่เสมอ
- Refactoring อยู่เสมอ
- ออกแบบระบบอย่างมีชั้นเชิง

หนังสือเล่มนี้เป็นคู่มือที่ควรอ่านซ้ำแล้วซ้ำอีก เพราะ clean code ไม่ใช่แค่แนวคิด แต่เป็นวิธีปฏิบัติจริงในการเขียนซอฟต์แวร์คุณภาพ

---

✅ เสร็จแล้ว: Clean Code A Handbook of Agile Software Craftsmanship
