# Domain Driven Design - Tackling Complexity in the Heart of Software

> **Author:** Eric Evans

Domain Driven Design (DDD) คือแนวทางการออกแบบซอฟต์แวร์ที่เน้นการสร้าง Model ให้สอดคล้องกับธุรกิจจริง เพื่อจัดการกับความซับซ้อนของซอฟต์แวร์

## บทที่ 1: สารบัญและบทนำ

DDD ให้ความสำคัญกับ Domain Model เป็นหัวใจของการออกแบบ โมเดลไม่ใช่แค่แผนภาพ แต่เป็นความรู้ที่ทีมมีร่วมกัน

## บทที่ 2: การสื่อสารและภาษา (Communication and Language)

### Ubiquitous Language
Ubiquitous Language คือภาษาเดียวกันที่ทั้งทีมใช้ร่วมกัน ไม่ว่าจะเป็น developer, domain expert, หรือ business analyst

### ทำไมต้อง Ubiquitous Language?
- การแปลภาษาทำให้ความหมายผิดเพี้ยน
- ทีมที่ไม่มีภาษาเดียวกันจะมีปัญหาในการสื่อสาร
- โมเดลควรเป็นแกนหลักของภาษาที่ใช้ในโครงการ

### วิธีใช้ Ubiquitous Language
1. ใช้โมเดลเป็นแกนหลักของภาษา
2. ใช้ภาษาเดียวกันในทุกการสื่อสาร
3. เปลี่ยนชื่อ class และ method ให้ตรงกับโมเดล
4. Domain expert ควรใช้ภาษานี้ในการเขียน requirement

### ตัวอย่าง การสนทนา
Scenario 1 (ไม่ดี): ใช้คำศัพท์เทคนิค和技术ทั่วไป เช่น "ลบ rows ใน shipment table"
Scenario 2 (ดี): ใช้คำศัพท์จากโมเดล เช่น "Route Specification", "Itinerary"

### Modeling Out Loud
การพูดถึงโมเดลดังๆ ช่วยให้เข้าใจได้ดีขึ้น การทดลองภาษาใหม่ๆ ช่วยพัฒนาโมเดล

### Documents and Diagrams
- แผนภาพ UML ช่วยในการสื่อสารแต่ไม่ควรครอบคลุมทุกอย่าง
- เอกสารควร complement code ไม่ใช่ทำซ้ำ
- เอกสารควร alive และ update ตลอดเวลา

## บทที่ 3: Binding Model and Implementation

### Model-Driven Design
Model-Driven Design คือการออกแบบที่ผูกโมเดลเข้ากับ code อย่างแน่นหนา

### ปัญหาของ Analysis Model vs Design Model
- โมเดลที่แยกจาก implementation มักจะไม่ได้ใช้จริง
- Developer มักจะสร้าง design ใหม่แทนที่จะใช้ analysis model
- ทำให้ insight ที่ได้จากการวิเคราะห์หายไป

### หลักการ Model-Driven Design
1. ทุก class ใน design ควรตรงกับโมเดล
2. Code ควรสื่อความหมายของโมเดลได้
3. โมเดลควรเป็นได้ทั้ง analysis และ design

## บทที่ 4: Layered Architecture

### ทำไมต้อง Layered Architecture?
- แยกความรับผิดชอบชัดเจน
- ทำให้โค้ด maintenance ง่ายขึ้น
- ป้องกันไม่ให้ UI layer ส่งผลต่อ domain logic

### Layers ที่แนะนำ
1. **User Interface** - การแสดงผลและการรับ input
2. **Application** - การจัดการ workflow
3. **Domain** -  бизнес logic และ rules
4. **Infrastructure** - Database, messaging, etc.

### หลักการสำคัญ
- Domain layer ไม่ควรรู้เรื่อง UI หรือ Infrastructure
- ใช้ Dependency Injection เพื่อเชื่อมต่อ layers
- Domain layer ควรเป็น pure business logic

## บทที่ 5: A Model Expressed in Software

### Entity vs Value Object

**Entity** คือ object ที่มี identity เฉพาะตัว แม้ว่าข้อมูลจะเหมือนกันแต่ถ้า identity ต่างกัน ก็เป็นคนละตัว
- ตัวอย่าง: Customer, Order, Product (ที่มี serial number)

**Value Object** คือ object ที่ไม่มี identity เฉพาะตัว เปรียบเทียบจากข้อมูล
- ตัวอย่าง: Money, DateRange, Address

### หลักการเลือกใช้
- ใช้ Entity เมื่อต้อง track state เปลี่ยนแปลง
- ใช้ Value Object เมื่อข้อมูลสำคัญกว่า identity
- Value Object ควร immutable

### Service
Service คือ operation ที่ไม่ควรอยู่ใน Entity หรือ Value Object
- ไม่มี state
- ทำงานกับ domain object หลายตัว
- ตัวอย่าง: TransferService, PricingService

## บทที่ 6: The Structure of Systems (Modules)

### ทำไมต้อง Modules?
- จัดระเบียบ code ให้เข้าใจง่าย
- ลดความซับซ้อน
- ทำให้ code reuse ง่ายขึ้น

### หลักการ modular design
1. **Low Coupling** - ลดการเชื่อมต่อระหว่าง modules
2. **High Cohesion** - รวมสิ่งที่เกี่ยวข้องกันเข้าด้วยกัน
3. **Publish Interface** - แสดงเฉพาะสิ่งที่จำเป็น

### Patterns ที่เกี่ยวข้อง
- **Package by Feature** - จัดกลุ่มตามฟีเจอร์
- **Hexagonal Architecture** - แยก core domain จาก外部依赖

## บทที่ 7: Factories

### ทำไมต้อง Factory?
- ซ่อน complexity ของการสร้าง object
- ให้ flexibility ในการเลือกวิธีสร้าง
- ทำให้ code สะอาดขึ้น

### Factory Patterns
1. **Factory Method** - method ใน class ที่สร้าง instance ของตัวเอง
2. **Abstract Factory** - สร้าง family ของ object ที่เกี่ยวข้องกัน
3. **Builder** - สร้าง object ที่ซับซ้อน step by step

### หลักการใช้ Factory
- Factory ควรสร้าง object ที่พร้อมใช้งาน
- ไม่ควรให้ factory มี business logic มากเกินไป
- ควรให้ factory return type ที่ abstract ที่สุด

## บทที่ 8: Repositories

### ทำไมต้อง Repository?
- ซ่อน complexity ของ data access
- ให้ interface ที่สะอาดสำหรับ domain layer
- ทำให้ domain logic เป็นอิสระจาก database

### Repository Pattern
```python
class CustomerRepository:
    def find_by_id(self, customer_id: str) -> Customer:
        pass
    
    def save(self, customer: Customer):
        pass
```

### หลักการใช้ Repository
1. Repository ควร interface ที่ชัดเจน
2. ไม่ควร expose database-specific details
3. ใช้ Aggregate Root ในการ save/find
4. Repository 1 ตัวต่อ 1 Aggregate

## บทที่ 9: Making Implicit Concepts Explicit

### ทำไมต้อง Make Implicit Concepts Explicit?
- ความรู้ที่ซ่อนอยู่ทำให้โค้ดซับซ้อน
- การทำให้ชัดเจนช่วยให้เข้าใจง่าย
- ลด bug ที่เกิดจากความเข้าใจผิด

### วิธี Make Implicit Concepts Explicit
1. **Named Value Object** - แทน string ด้วย Value Object
2. **Domain Event** - บันทึกสิ่งที่เกิดขึ้นใน domain
3. **Policy/Strategy** - แยก business rule ออกมา
4. **Specification** - กำหนดเงื่อนไขเป็น object

## บทที่ 10: Refactoring to Deep Model

### ทำไมต้อง Refactoring?
- โมเดลเดิมอาจไม่ตรงกับ domain จริง
- ความรู้ใหม่ทำให้โมเดลต้องพัฒนา
- Code ที่ซับซ้อนต้อง refactor อยู่เสมอ

### วิธี Refactoring
1. **Look for Hidden Concepts** - ค้นหาสิ่งที่ซ่อนอยู่
2. **Distill Domain Logic** - แยก domain logic ออกมา
3. **Eliminate Side Effects** - ลบ side effect ที่ไม่จำเป็น
4. **Make Model Expressive** - ทำให้โมเดลสื่อความหมาย

## บทที่ 11: Supple Design

### คุณสมบัติของ Supple Design
1. **Intention-Revealing Interfaces** - interface บอกเจตนาชัดเจน
2. **Side-Effect-Free Functions** - function ไม่มี side effect
3. **Assertions** - กำหนดเงื่อนไขไว้ชัดเจน
4. **Conceptual Contour** - ออกแบบให้ตรงกับ domain concept
5. **Standalone Classes** - class ที่เป็นอิสระ
6. **Closure of Operations** - operation ที่ปิดตัวเอง

### วิธีทำให้ Design Supple
1. ใช้ Value Object แทน primitive
2. แยก business logic ออกมาเป็น Service
3. ใช้ Specification pattern สำหรับเงื่อนไข
4. ออกแบบ interface ให้เข้าใจง่าย

## บทที่ 12: Relating Design Patterns to the Model

### Patterns ที่เกี่ยวข้องกับ DDD

**Strategy Pattern** - สำหรับ business rule ที่เปลี่ยนแปลงได้
**Composite Pattern** - สำหรับ structure ที่ซ้ำกัน
**Facade Pattern** - สำหรับ interface ที่ซับซ้อน
**Mediator Pattern** - สำหรับ coordination ระหว่าง objects

### วิธีเลือก Pattern
1. เข้าใจ domain ก่อนแล้วค่อยเลือก pattern
2. ไม่ใช้ pattern แค่เพราะมี pattern
3. Pattern ควรช่วยให้โมเดลชัดเจนขึ้น

## บทที่ 13: Bounded Contexts

### ทำไมต้อง Bounded Context?
- ระบบขนาดใหญ่มีหลาย domain ย่อย
- แต่ละ domain ย่อยมีโมเดลต่างกัน
- ป้องกันไม่ให้โมเดลสับสน

### วิธีกำหนด Bounded Context
1. ดูจากทีมที่รับผิดชอบ
2. ดูจาก business capability
3. ดูจาก subdomain

### Context Mapping
- **Shared Kernel** - ใช้โมเดลร่วมกัน
- **Customer-Supplier** - ลูกค้า-supplier relationship
- **Conformist** - ใช้โมเดลของอีกฝ่าย
- **Anti-Corruption Layer** - แปลงระหว่างโมเดล

## บทที่ 14: Maintaining Model Integrity

### ปัญหาของ Model Integrity
- โมเดลเดิมไม่ตรงกับ domain จริง
- หลายทีมใช้โมเดลต่างกัน
- โมเดลเก่าขัดขวางการพัฒนาใหม่

### วิธีรักษา Model Integrity
1. **Continuous Refactoring** - refactor อยู่เสมอ
2. **Model Validation** - ทดสอบโมเดลกับ domain expert
3. **Context Isolation** - แยก context ให้ชัดเจน
4. **Published Model** - ประกาศโมเดลให้ทุกคนรู้

## บทที่ 15: Distillation

### ทำไมต้อง Distill?
- โมเดลใหญ่เกินไปทำให้เข้าใจยาก
- บางส่วนสำคัญกว่าส่วนอื่น
- ต้องแยก core domain ออกมา

### วิธี Distill
1. **Identify Core Domain** - ค้นหาส่วนที่สำคัญที่สุด
2. **Extract Generic Subdomain** - แยก subdomain ทั่วไป
3. **Focus on Core** - ให้ทรัพยากรกับ core domain
4. **Simplify Other Parts** - ใช้ solution สำเร็จรูปกับ subdomain อื่น

## บทที่ 16: Large-Scale Structures

### ทำไมต้อง Large-Scale Structure?
- ระบบขนาดใหญ่ต้องมีโครงสร้างชัดเจน
- ช่วยให้ทีมเข้าใจระบบโดยรวม
- ป้องกันไม่ให้ระบบสับสน

### Patterns สำหรับ Large-Scale Structure
1. **Evolving Order** - โครงสร้างที่พัฒนาไปเรื่อยๆ
2. **System Metaphor** - ใช้เปรียบเทียบเพื่อเข้าใจระบบ
3. **Responsibility Layers** - แยกชั้นความรับผิดชอบ

## บทที่ 17: Bringing the Strategy Together

### วิธีนำ DDD ไปใช้จริง
1. **Start with Domain Model** - เริ่มจากโมเดล
2. **Iterate and Refine** - ทำซ้ำและปรับปรุง
3. **Involve Domain Experts** - ให้ domain expert มีส่วนร่วม
4. **Use Ubiquitous Language** - ใช้ภาษาเดียวกันทั้งทีม

### ปัญหาที่พบบ่อย
- ทีมไม่เข้าใจ DDD
- Domain expert ไม่ร่วมมือ
- โมเดลไม่ตรงกับ domain จริง
- Code ซับซ้อนเกินไป

### คำแนะนำ
1. เริ่มจาก small project ก่อน
2. ฝึกฝนอย่างสม่ำเสมอ
3. ให้ domain expert มีส่วนร่วมตั้งแต่ต้น
4. อย่ากลัวที่จะ refactor

## บทสรุป

DDD เป็นแนวทางที่ช่วยให้ซอฟต์แวร์ตรงกับธุรกิจจริง ความสำเร็จของ DDD ขึ้นอยู่กับ:

1. **Ubiquitous Language** - ภาษาเดียวกันทั้งทีม
2. **Model-Driven Design** - โมเดลเป็นแกนหลัก
3. **Continuous Refactoring** - พัฒนาอยู่เสมอ
4. **Domain Expert Involvement** - domain expert มีส่วนร่วม

DDD ไม่ใช่ silver bullet แต่เป็นแนวทางที่ช่วยจัดการกับความซับซ้อนของซอฟต์แวร์ได้อย่างมีประสิทธิภาพ

---
✅ เสร็จแล้ว: Domain Driven Design - Tackling Complexity in the Heart of Software.md