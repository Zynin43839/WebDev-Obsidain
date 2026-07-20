# Data Structures and Algorithms in Python
> **Author:** Michael T. Goodrich, Roberto Tamassia, Michael H. Goldwasser

หนังสือเรียน data structures และ algorithms ภาษา Python 770 หน้า เนื้อหาครอบคลุมตั้งแต่พื้นฐานภาษา Python ไปจนถึงโครงสร้างข้อมูลขั้นสูงและ algo สำหรับ graph/text processing เหมาะสำหรับนักศึกษา CS ระดับปริญญาตรี-โท ที่ต้องการเข้าใจวิธีออกแบบและวิเคราะห์โครงสร้างข้อมูลอย่างลึกซึ้ง

---

## บทที่ 1 — Python Primer (Python พื้นฐาน)

บทนี้ปูพื้นฐานภาษา Python ทั้งหมดที่จะใช้ในเล่ม:

- **Python Interpreter**: เป็น interpreted language ทำงานผ่าน interpreter, ไฟล์ .py เรียกใช้ด้วย `python filename.py` หรือ interactive mode ด้วย `python`
- **Objects ใน Python**: ทุกอย่างเป็น object, identifier (ตัวแปร) อ้างถึง object ใน memory Python เป็น **dynamically typed** คือไม่ต้องประกาศ type ล่วงหน้า, identifier สามารถ assign ใหม่เป็น object ชนิดใดก็ได้, การ assign `a = b` ทำให้ a เป็น **alias** ของ b (ชี้ object เดียวกัน)
- **Built-in Classes หลัก**:
  - `bool` (immutable) — True/False
  - `int` (immutable) — จำนวนเต็มไม่จำกัดขนาด, รองรับ base 2/8/16 (0b, 0o, 0x)
  - `float` (immutable) — ทศนิยม fixed-precision (เทียบเท่า double ใน C/Java)
  - `list` (mutable) — sequence อ้างถิง elements, รองรับ dynamic resize, zero-indexed
  - `tuple` (immutable) — sequence แบบ immutable, ใช้ `()` คั่น, tuple ตัวเดียวต้องใส่ comma เช่น `(17,)`
  - `str` (immutable) — string Unicode, รองรับ single/double/triple quotes, escape characters (`\n`, `\t`, `\\`)
  - `set` / `frozenset` — set แบบไม่เรียงลำดับ, ไม่รับ duplicates, ต้องเป็น immutable elements เท่านั้น, ใช้ hash table สำหรับ lookup เร็ว
  - `dict` — key-value mapping, key ต้องเป็น hashable (immutable), ใช้ curly braces `{}`
- **Operators**: logical (`and`, `or`, `not` แบบ short-circuit), equality (`is` เช็ค identity vs `==` เช็ค equivalence), comparison, arithmetic (`/` true division, `//` floor division, `%` modulo — รับประกัน `q*m + r == n`), bitwise, sequence operators (indexing, slicing `s[start:stop:step]`, concatenation `+`, repetition `*`, `in`/`not in`), set operators (`|` union, `&` intersection, `-` difference, `^` symmetric difference)
- **Control Flow**: `if`/`elif`/`else`, `while` loop, `for element in iterable` (เหมือน for-each), `break`/`continue`, `range()` สำหรับ index-based loop
- **Functions**: ประกาศด้วย `def`, Python ไม่มี type declaration, parameter passing ใช้ assignment semantics (เหมือน alias), mutable parameter ทำให้ function แก้ไข object เดิมได้, **default parameter values**, **keyword arguments** (ส่ง parameter ตามชื่อ), functions เป็น **first-class objects** (assign, pass as parameter, return ได้)
- **I/O**: `print()` รองรับ `sep`/`end`/`file` keywords, `input()` อ่านจาก console, `open()` เปิดไฟล์ (`r`/`w`/`a`) คืน file proxy, รองรับ `read()`/`write()`/`readlines()`/`writelines()`
- **Exception Handling**: ใช้ `try`/`except`/`finally`, Python มี exception hierarchy (Exception > ValueError, TypeError, IndexError, KeyError, etc.), `raise` สำหรับโยน exception, สองหลักการคือ "look before you leap" (เช็คก่อน) vs "Easier to Ask for Forgiveness than Permission" (EAFP — ลองทำก่อนแล้วจับ error)
- **Iterators และ Generators**: iterable สร้าง iterator ได้ด้วย `iter()`, iterator ใช้ `next()` จน `StopIteration`, **generator** ใช้ `yield` แทน `return` — lazy evaluation ไม่ต้องเก็บผลลัพธ์ทั้งหมดในหน่วยความจำ, ตัวอย่าง: generator หา factors ของตัวเลข, Fibonacci infinite series
- **Comprehension Syntax**: list comprehension `[expr for x in iterable if cond]`, set comprehension `{expr for x in iterable}`, dict comprehension `{k:v for x in iterable}`, generator comprehension `(expr for x in iterable)`
- **Packing/Unpacking**: `a, b, c = 1, 2, 3` (simultaneous assignment), ใช้สลับค่า `j, k = k, j`
- **Scopes/Namespaces**: global scope vs local scope, namespace เป็น dict ที่ map identifier → value, ใช้ `dir()`/`vars()` ตรวจสอบ, **first-class objects** ทำให้ functions/classes assign ให้ identifier ได้
- **Modules**: สร้าง module ด้วยไฟล์ .py, import ด้วย `from module import name` หรือ `import module`, `if __name__ == "__main__":` สำหรับ unit test ใน module

---

## บทที่ 2 — Object-Oriented Programming (OOP)

- **OOP Design Goals**: robustness (ถูกต้อง + จัดการ error), adaptability/evolvability (เปลี่ยนแปลงได้), reusability (ใช้ซ้ำได้)
- **OOP Principles**: modularity (แยก component), abstraction (ลดความซับซ้อน เน้น interface ไม่ใช่ implementation — ได้ concept **Abstract Data Types** หรือ ADT), encapsulation (ซ่อน internal details, Python ใช้ convention `_name` สำหรับ nonpublic)
- **Design Patterns** ที่พบในเล่ม:
  - Algorithm: Recursion, Amortization, Divide-and-conquer, Prune-and-search, Brute force, Dynamic programming, Greedy method
  - Software engineering: Iterator, Adapter, Position, Composition, Template method, Locator, Factory method
- **Software Development**: Design (CRC cards — Class/Responsibility/Collaborator, UML diagrams), Pseudo-code, Coding Style (PEP 8: 4-space indent, CamelCase สำหรับ class, snake_case สำหรับ function/variable), Documentation (docstrings ด้วย `""" """`), Testing (method coverage, statement coverage, edge cases, top-down vs bottom-up testing, `unittest` module), Debugging (breakpoints, `pdb`)
- **Class Definitions**:
  - `__init__()` constructor — ตั้งค่า instance variables ด้วย `self.name = value`
  - `self` identifier — อ้างถึง instance ปัจจุบันที่ method ถูกเรียก
  - Accessor methods (get balance) vs Mutator methods (charge, make payment)
  - **Operator Overloading**: ใช้ special methods เช่น `__add__` สำหรับ `+`, `__len__` สำหรับ `len()`, `__getitem__` สำหรับ indexing `[]`, `__eq__` สำหรับ `==`, `__str__` สำหรับ `str()` ฯลฯ — ทำให้ object ของ user-defined class ใช้ operators ได้เหมือน built-in types
  - **Polymorphism**: method รับ parameter ได้หลาย type (duck typing — ถ้ามี behavior ที่ต้องการก็ใช้ได้เลย)
  - **Example**: CreditCard class (customer, bank, account, limit, balance), charge() ตรวจสอบ credit limit, make_payment() ลด balance
  - **Example**: Vector class สำหรับมิติสูง — ใช้ `__add__` บวก vectors ทีละ element, `__getitem__`/`__setitem__` สำหรับ indexing
- **Iterators**: class ที่มี `__next__()` + `__iter__()` ทำหน้าที่ iterator, ถ้า class มี `__len__` + `__getitem__` จะได้ iterator อัตโนมัติ
- **Example**: Range class จำลอง built-in `range()` — lazy evaluation, ไม่สร้าง list, คำนวณ length ด้วย formula `max(0, (stop - start + step - 1) // step)`
- **Inheritance**: subclass สืบทอดจาก superclass (is-a relationship), **override** methods เพื่อ specialize, **extend** ด้วย methods ใหม่, `super().__init__()` เรียก constructor ของ parent, protected members (`_name`) accessible ใน subclass แต่ไม่ public
  - **Example**: PredatoryCreditCard สืบทอดจาก CreditCard — เพิ่ม fee $5 เมื่อ charge ปฏิเสธ, process_month() คิดดอกเบี้ย APR แบบ compound
  - **Numeric Progressions hierarchy**: Progression (base) → ArithmeticProgression, GeometricProgression, FibonacciProgression — ใช้ template method pattern (advance() เป็น abstract method ให้ subclass override)
- **Abstract Base Classes (ABC)**: ใช้ `ABCMeta` + `@abstractmethod` บังคับให้ subclass implement methods ที่จำเป็น, `collections.Sequence` ABC ให้ count(), index(), `__contains__()` อัตโนมัติถ้ามี `__len__` + `__getitem__`
- **Namespaces ใน OOP**: instance namespace (เก็บ data members ของแต่ละ instance), class namespace (เก็บ methods ที่ share กัน), class data members (static variables), **Name Resolution**: lookup ใน instance → class → superclasses ตามลำดับ (**MRO — Method Resolution Order**)
- **Shallow vs Deep Copy**: shallow copy คัดลอก reference เดียวกัน, deep copy คัดลอก object ซ้อนกันทั้งหมด

---

## บทที่ 3 — Algorithm Analysis (การวิเคราะห์ Algo)

- **Experimental Studies**: วัด runtime จริงแต่ไม่น่าเชื่อถือเพราะขึ้นอยู่กับ hardware/OS/input — ต้องใช้ theoretical analysis
- **Seven Functions ที่ใช้บ่อย**: `1` (constant), `log n` (logarithmic), `n` (linear), `n log n` (linearithmic), `n²` (quadratic), `2ⁿ` (exponential), `n!` (factorial) — เรียงจากเร็วไปช้า
- **Asymptotic Analysis — Big-O Notation**: `f(n) = O(g(n))` หมายถึง f ไม่เติบโตเร็วก่า g เมื่อ n ใหญ่พอ, `Ω` (lower bound), `Θ` (tight bound) — ใช้เปรียบเทียบ performance ของ algorithms โดยไม่ขึ้นกับ hardware
- **Comparative Analysis**: เปรียบเทียบ algorithms 2 ตัวโดยดูว่าตัวไหนทำงานน้อยกว่าเมื่อ n ใหญ่
- **Practical guideline**: ถ้า algo ต่างกันแค่ constant factor ให้เลือกตัวที่โค้ดสะอาดกว่า, ถ้าต่างกันในระดับ function (เช่น O(n) vs O(n²)) ให้เลือกตัวที่ growth rate ต่ำกว่า
- **Simple Justification Techniques**: proof by example, proof by contradiction (reductio ad absurdum), mathematical induction (พิสูจน์ base case แล้ว step inductive), loop invariants (ข้อเท็จจริงที่จริงทุกรอบของ loop)

---

## บทที่ 4 — Recursion (เวียนย้อน)

- **Recursive function**: function ที่เรียกตัวเอง, ต้องมี **base case** (หยุด) และ **recursive case** (ลดปัญหาลง)
- **Examples**: factorial (`n! = n × (n-1)!`), drawing English ruler (recursive dashes), binary search (แบ่งครึ่งทีละครั้ง → O(log n)), file system traversal
- **Analyzing Recursive Algorithms**: ใช้ **recurrence relations** เช่น `T(n) = T(n-1) + O(1)` → `O(n)`, `T(n) = T(n/2) + O(1)` → `O(log n)`, `T(n) = 2T(n/2) + O(n)` → `O(n log n)`
- **Recursion Run Amok**: ถ้าไม่มี base case หรือ recursive case ไม่ลดลง → infinite recursion → stack overflow, Python จำกัด max recursion depth (~1000) ด้วย `sys.getrecursionlimit()`
- **Types**: linear recursion (recursive call เดียว), binary recursion (2 calls), multiple recursion (k calls)
- **Designing Recursive Algorithms**: นิยามปัญหาให้เล็กลงเรื่อยๆ, base case จัดการกรณีง่าย, ไว้ใจ recursive call ว่าจะทำงานถูกต้อง (trust the recursion)
- **Tail Recursion**: recursive call เป็น statement สุดท้าย — Python ไม่ optimise tail recursion เหมือน Scala/ML, ต้อง rewrite เป็น loop ถ้าต้องการ efficiency

---

## บทที่ 5 — Array-Based Sequences

- **Low-level Arrays**: memory เป็น contiguous block, element access `O(1)` ด้วย offset calculation
- **Referential Arrays**: Python list เก็บ references (pointers) ไปยัง objects ไม่ใช่ object เอง — ทำให้ list รับ element ได้หลาย type
- **Dynamic Arrays**: เมื่อ list เต็มจะ allocate memory ใหม่ขนาด ~2× (overallocation) แล้ว copy ทั้งหมด → **amortized O(1)** ต่อ append
  - Amortized analysis: append n ตัว = O(n) total → เฉลี่ย O(1) ต่อตัว
- **Python's List internals**: `list` จริงๆ คือ dynamic array of pointers, `list.append()` amortized O(1), `list.insert(i, x)` O(n) เพราะต้อง shift
- **Efficiency**: `list` — append O(1)*, index O(1), insert O(n), `tuple` — เบากว่า list (immutable → internal optimization), `str` — immutable, concatenation `s + t` สร้าง string ใหม่ O(n) (ใช้ join() แทน loop concat)
- **Sorting Sequences**: insertion sort สำหรับ sorted list (insert ทีละตัว)
- **Multidimensional Data Sets**: ใช้ list of lists จำลอง matrix เช่น `[[0]*n for _ in range(m)]`

---

## บทที่ 6 — Stacks, Queues, and Deques

- **Stack (Stack ADT)**: LIFO — push (เพิ่ม) / pop (เอาออก) / top (ดูตัวบน) / is_empty
  - Array-based implementation: ใช้ Python list `append()`/`pop()` → O(1) ทั้งคู่
  - ประยุกต์: กลับข้อมูล, จับคู่วงเล็บ/HTML tags, undo operations
- **Queue (Queue ADT)**: FIFO — enqueue (เพิ่มท้าย) / dequeue (เอาออกหน้า) / front
  - Array-based: ต้องใช้ **circular array** (index % capacity) ไม่เช่นนั้น dequeue ต้อง shift ทั้ง array → O(n)
  - Circular array: ใช้ front index + size, เมื่อถึงท้าย array แล้ววนกลับไป index 0
- **Deque (Double-Ended Queue)**: เพิ่ม/ลบ ได้ทั้งหน้าและหลัง
  - Circular array implementation: O(1) สำหรับ add_first/add_last/remove_first/remove_last
  - Python `collections.deque` — doubly-linked list implementation, O(1) ทุก operations
- **Adapter Pattern**: ใช้ Stack adapter ที่ wrap list, แสดงให้เห็นว่า interface ต่างกันแม้ implementation จะเหมือนกัน

---

## บทที่ 7 — Linked Lists

- **Singly Linked List**: แต่ละ node เก็บ data + reference ไป node ถัดไป, head pointer ชี้ตัวแรก, tail pointer ชี้ตัวสุดท้าย
  - **prepend** O(1), **append** O(1) ถ้ามี tail, **insert/remove ตรงกลาง** O(1) ถ้ามี node reference (ไม่ต้อง search)
  - ใช้ sentinel node (dummy header/tail) เพื่อลด edge cases
  - ใช้สร้าง Stack (push = add_first) และ Queue (add_last + remove_first)
- **Circularly Linked List**: tail.next = head, ใช้สำหรับ round-robin scheduler (หมุนวนระหว่าง processes)
- **Doubly Linked List**: แต่ละ node มี prev + next references, ลบ/insert ได้ O(1) ถ้ามี node reference, ต้องระวัง **memory leak** — ต้องunlink node ให้ถูกต้อง
- **Positional List ADT**: abstraction ที่เก็บ "position" (pointer ไป node) แทน index — ไม่ต้อง关心 shifting, insert/remove ได้ O(1) เมื่อ biết position
  - Position object ซ่อน internal node structure จากผู้ใช้
- **Case Study: Access Frequencies**: ใช้ sorted list (insertion sort) vs **Move-to-Front heuristic** (ย้าย element ที่เข้าถึงบ่อยไปหน้าสุด) — MTF heuristic ให้ performance ดีกว่าสำหรับ access pattern ที่ไม่ uniform

---

## บทที่ 8 — Trees (ต้นไม้)

- **General Tree**: node มี children ได้หลายคน, root (ราก), leaf (ใบ), parent/child/sibling relationships
  - **Depth** ของ node = ระยะห่างจาก root, **Height** ของ tree = depth สูงสุด (สูง = วัดจาก bottom)
- **Binary Tree**: แต่ละ node มี left child + right child อย่างมาก 2 คน
  - Properties: ถ้ามี `n` nodes → height ≥ `⌊log n⌋`, จำนวน叶子 ≤ internal nodes + 1
- **Implementations**: linked structure (node class มี element/left/right), array-based (index i → left=2i+1, right=2i+2 — ดีสำหรับ complete binary tree เช่น heap), linked structure สำหรับ general tree (children เป็น list)
- **Tree Traversals**:
  - **Preorder**: visit root → recurse children (ใช้ copy tree, แสดง structure)
  - **Postorder**: recurse children → visit root (ใช้ calculate disk space, evaluate expression)
  - **Breadth-First (Level-order)**: ใช้ queue, เยี่ยมทีละ level จากบนลงล่าง
  - **Inorder** (binary tree เท่านั้น): recurse left → visit root → recurse right — ได้ sorted order สำหรับ BST
  - **Euler Tour**: traversal ที่ visit แต่ละ node 2 ครั้ง (ก่อน-หลัง visit children) — flexible pattern สำหรับ many tree computations
- **Expression Tree**: binary tree ที่叶子เป็น operands, internal nodes เป็น operators — evaluate ด้วย postorder traversal

---

## บทที่ 9 — Priority Queues (คิวลำดับความสำคัญ)

- **Priority Queue ADT**: คล้าย queue แต่ dequeue คืน element ที่มี **priority สูงสุด** (ไม่ใช่ FIFO)
  - Operations: insert (enqueue), remove_min/remove_max, min/max, is_empty
- **Implementations เปรียบเทียบ**:
  - Unsorted list: insert O(1), remove_min O(n) — ช้าตอนหา min
  - Sorted list: insert O(n) (ต้อง shift), remove_min O(1) — ช้าตอน insert
  - **Heap**: insert O(log n), remove_min O(log n) — **สมดุลที่สุด**
- **Binary Heap**: complete binary tree ที่มี **heap property** — parent ≤ children (min-heap) หรือ parent ≥ children (max-heap)
  - Array-based implementation: node i → left child 2i+1, right child 2i+2, parent ⌊(i-1)/2⌋
  - **insert (upheap)**: เพิ่มท้ายแล้ว bubble up → O(log n)
  - **remove_min (downheap)**: เอา root ออก, ย้ายตัวสุดท้ายมาแทนแล้ว bubble down → O(log n)
  - **Bottom-up heap construction**: สร้าง heap จาก unordered array O(n) — เริ่มจาก internal nodes แล้ว downheap ทีละตัว
  - Python `heapq` module — min-heap implementation
- **Sorting with Priority Queue**:
  - Selection-Sort: หา min ทีละรอบ → O(n²)
  - Insertion-Sort: insert ทีละตัวเข้า sorted list → O(n²) worst, O(n) best
  - **Heap-Sort**: ใส่ทุกตัวเข้า heap แล้ว dequeue ทีละตัว → O(n log n), in-place ได้
- **Adaptable Priority Queue**: รองรับ update priority ของ element ที่มีอยู่แล้ว — ใช้ locator pattern เพื่อ track position ของ element ใน heap

---

## บทที่ 10 — Maps, Hash Tables, and Skip Lists

- **Map ADT**: key-value pairs, operations: get(k), put(k,v), remove(k), __contains__(k), __len__() — Python `dict` คือ concrete implementation
  - ตัวอย่าง: นับ word frequency ด้วย dict
- **Unsorted List Map**: insert O(1), search O(n) — ช้า lookup
- **Hash Tables**: ใช้ **hash function** แปลง key เป็น bucket index → average O(1) สำหรับ lookup/insert/remove
  - **Hash Functions**: ต้อง deterministic, uniform distribution, เช่น `hash(key) % capacity`
  - Python object hash ได้ด้วย `hash()`, string hash ด้วย polynomial rolling hash
  - **Collision Handling**:
    - **Chaining** (separate chaining): bucket เก็บ linked list ของ entries ที่ collision
    - **Open Addressing**: หา bucket ว่างถัดไป — linear probing (O(1) แต่ clustering), quadratic probing, double hashing
  - **Load Factor** `α = n/N` (entries / capacity): ถ้า α สูงเกิน → rehash (resize array ใหม่ + คำนวณ hash ใหม่)
  - **Amortized O(1)** สำหรับ dict operations เมื่อ load factor ต่ำ
- **Sorted Maps**: เก็บ key เรียงลำดับ — ใช้ **binary search** O(log n) สำหรับ lookup, ตัวอย่าง: sorted search table
- **Skip Lists**: โครงสร้างข้อมูลแบบ概率性 (probabilistic) ที่ใช้ linked lists ซ้อนกันหลาย level — search/insert/remove average O(log n) เหมือน balanced BST แต่โค้ดง่ายกว่า
  - แต่ละ level เป็น subset ของ level ต่ำกว่า, "promote" elements สุ่มขึ้น level สูง
- **Sets, Multisets, Multimaps**: Set ADT (add, remove, __contains__, union, intersection), Python set ใช้ hash table, Multiset (bag) นับจำนวน, Multimap map ที่ key ซ้ำได้

---

## บทที่ 11 — Search Trees (ต้นไม้ค้นหา)

- **Binary Search Tree (BST)**: binary tree ที่ left subtree < root < right subtree → in-order traversal ได้ sorted order
  - Search: O(h) h = height (best O(log n), worst O(n) ถ้า skewed)
  - Insert: ค้นหา position แล้วเพิ่ม leaf → O(h)
  - Delete: 3 กรณี — leaf (ลบเลย), 1 child (replace ด้วย child), 2 children (swap กับ in-order successor แล้วลบ)
  - **Worst case O(n)** เมื่อ insert เรียงลำดับ → tree เหมือน linked list
- **Balanced Search Trees**: รักษา height = O(log n) เสมอ
  - Framework: rotations (single/double rotation) สำหรับ rebalance
- **AVL Trees**: BST ที่ **balance factor** (height(left) - height(right)) ของทุก node อยู่ใน [-1, 0, 1]
  - Insert/Delete: อาจ imbalance → แก้ด้วย single/double rotation → O(log n)
  - Rotation types: LL, RR, LR, RL
- **Splay Trees**: BST ที่搬移 (splay) node ที่เข้าถึงล่าสุดขึ้น root ทุกครั้ง — **amortized O(log n)** (ไม่ guarantee per-operation)
  - Splay operations: zig, zig-zig, zig-zag
- **(2,4) Trees**: multiway search tree (แต่ละ node มี 1-3 keys, 2-4 children) — perfect balance
  - Insert: ใส่ leaf แล้ว split ถ้า node เต็ม (4 keys → split เป็น 2 nodes + promote key ขึ้น parent)
  - Delete: แล้ว/merge ถ้า node มี key น้อยเกินไป
  - Red-Black Tree คือ binary tree equivalent ของ (2,4) tree
- **Red-Black Tree**: BST ที่ node มีสีแดง/ดำ, rules: root=black, red node มีลูก black, ทุก path มี black nodes เท่ากัน → height ≤ 2 log(n+1)
  - Insert/Delete ใช้ recoloring + rotations → O(log n)
  - เป็นตัวเลือกยอดนิยมสำหรับsorted containers ใน standard libraries

---

## บทที่ 12 — Sorting and Selection

- **Merge-Sort**: divide-and-conquer — แบ่ง array ครึ่ง แล้ว merge 2 sorted halves → **O(n log n)** เสมอ, แต่ต้องใช้ O(n) memory เสริม
  - เปรียบเทียบ: เสถียร (stable sort), parallelizable
- **Quick-Sort**: divide-and-conquer — เลือก pivot, partition (elements < pivot อยู่ซ้าย, > pivot อยู่ขวา) แล้ว recurse
  - **Worst case O(n²)** (เช่น sorted array + pivot เป็นตัวแรก), **average O(n log n)**
  - **Randomized Quick-Sort**: เลือก pivot สุ่ม → expected O(n log n) เสมอ
  - Optimizations: small subarray ใช้ insertion sort, median-of-three pivot, in-place partition
- **Lower Bound for Sorting**: comparison-based sorting มี lower bound **Ω(n log n)** — ไม่มี algo เร็วกว่านี้ใน worst case (พิสูจน์ด้วย decision tree model)
- **Linear-Time Sorting**: เมื่อ input มี constraints เช่น integers ในช่วงจำกัด
  - **Bucket-Sort**: ใส่ elements เข้า buckets ตามค่า → O(n+k) k=range ของค่า
  - **Radix-Sort**: เรียงทีละ digit จาก digit ต่ำสุด → O(d(n+k)) d=จำนวน digits
- **Python's Built-In Sort**: **Timsort** — hybrid ของ merge sort + insertion sort, O(n log n), stable, ใช้ `sorted()` หรือ `.sort()` พร้อม key function
- **Selection**: หา k-thsmallest element
  - **Quick-Select**: เหมือน Quick-Sort แต่ recurse ฝั่งเดียว → **average O(n)**, worst O(n²)
  - Median-of-medians: deterministic O(n) แต่ constant สูง

---

## บทที่ 13 — Text Processing

- **Pattern Matching**: หา substring (pattern) ใน text
  - **Brute Force**: O(nm) n=text length, m=pattern length — ลองทุก position
  - **Boyer-Moore**: skip characters โดยใช้ **bad character heuristic** + **good suffix heuristic** — average O(n/m) สำหรับ random text
  - **Knuth-Morris-Pratt (KMP)**: ใช้ **failure function** (prefix table) เพื่อไม่ต้องย้อนกลับ — O(n+m)
- **Dynamic Programming (DP)**: แก้ปัญหาโดย breaking down เป็น subproblems, เก็บผลลัพธ์ (memoization) เพื่อไม่คำนวณซ้ำ
  - **Matrix Chain-Product**: หาลำดับที่ optimal สำหรับคูณ matrix หลายตัว — O(n³)
  - **DNA/Text Sequence Alignment**: หา longest common subsequence ด้วย DP — O(nm)
- **Text Compression — Huffman Coding**: encode characters ด้วย variable-length codes — characters ที่พบบ่อยได้ code สั้น
  - ใช้ **greedy method**: เริ่มจาก leaf nodes แล้ว merge nodes ที่มี weight ต่ำสุด
  - ได้ prefix-free code (ไม่มี code เป็น prefix ของอีก code) — optimal compression
- **Tries (Prefix Trees)**: tree ที่แต่ละ level แทน characters ของ string
  - Standard Trie: แต่ละ edge แทน character, path จาก root → leaf เป็น word
  - **Compressed Trie**: merge nodes ที่มีลูกคนเดียว → ลด memory
  - **Suffix Trie**: เก็บ suffixes ทั้งหมดของ string — ใช้ substring search O(m)
  - **Search Engine Indexing**: ใช้ inverted index (word → document list)

---

## บทที่ 14 — Graph Algorithms (อัลกอริทึมกราฟ)

- **Graph Concepts**: V = vertices (nodes), E = edges (connections), directed/undirected, weighted/unweighted, degree (จำนวน edges ที่เชื่อม)
  - **Graph ADT**: incident_edges(v), opposite(v, e), are_adjacent(u, v)
- **Graph Representations**:
  - **Edge List**: list ของ edges → O(V+E) space, ค้นหา edge ช้า O(E)
  - **Adjacency List**: per-vertex list of neighbors → O(V+E) space, ค้นหา edge O(degree)
  - **Adjacency Map**: per-vertex dict of neighbors → O(V+E) space, edge lookup O(1)
  - **Adjacency Matrix**: V×V matrix → O(V²) space, edge lookup O(1) แต่เปลือง memory
- **Graph Traversals**:
  - **DFS (Depth-First Search)**: ใช้ stack (หรือ recursion), สำรวจลึกก่อน → O(V+E)
    - ประยุกต์: ตรวจ cycle, topological ordering, connected components, maze solving
  - **BFS (Breadth-First Search)**: ใช้ queue, สำรวจ level by level → O(V+E)
    - ประยุกต์: shortest path (unweighted graph), connected components
- **Transitive Closure**: หาว่า vertices ใดบ้างที่เชื่อมถึงกัน (Reachability) — ใช้ DFS/BFS จากทุก vertex หรือ Floyd-Warshall O(V³)
- **DAG (Directed Acyclic Graphs)**: กราฟมีทิศทางไม่มี cycle
  - **Topological Ordering**: linear ordering ที่ถ้ามี edge u→v → u มาก่อน v — ใช้ DFS-based algorithm, ใช้กับ task scheduling, dependency resolution
- **Shortest Paths**:
  - **Dijkstra's Algorithm**: หา shortest path จาก source vertex เดียว — **O((V+E) log V)** ด้วย priority queue (min-heap)
  - ใช้ได้กับ weighted graph ที่ weights ไม่เป็นลบ
  - ใช้ greedy: เลือก vertex ที่ distance น้อยสุดที่ยังไม่ visited
- **Minimum Spanning Trees (MST)**: เชื่อม vertices ทั้งหมดด้วย edges ที่มี total weight ต่ำสุด (ไม่มี cycle)
  - **Prim-Jarník Algorithm**: เริ่มจาก vertex เดียว แล้วเพิ่ม edge ที่ lightweight ที่สุดที่เชื่อม tree กับ non-tree → O((V+E) log V)
  - **Kruskal's Algorithm**: เรียง edges ตาม weight แล้วเพิ่มทีละตัวถ้าไม่ทำให้เกิด cycle → O(E log E)
  - **Union-Find (Disjoint Sets)**: data structure สำหรับเช็คว่า 2 vertices อยู่ใน same component — union-by-rank + path compression → nearly O(1) per operation

---

## บทที่ 15 — Memory Management and B-Trees

- **Memory Management**: วิธีที่ OS จัดการ memory ให้ program
  - **Memory Allocation**: stack-based (automatic, เร็ว, limited size) vs heap-based (dynamic, manual/free)
  - **Garbage Collection**: Python ใช้ **reference counting** + **cycle detector** — นับ references ถ้า = 0 ก็ free memory, แต่ cycle ทำให้ reference count ไม่เป็น 0 → ต้อง cycle detector เพิ่ม
  - **Python memory overhead**: object แต่ละตัวมี header (type, reference count) ~28 bytes → integer เล็กใน Python ใช้ memory มากกว่าใน C
- **Memory Hierarchies**: CPU cache (L1/L2/L3) > main memory (RAM) > disk — cache เร็วกว่าแต่เล็กกว่า
  - **Caching Strategies**: FIFO, LRU (Least Recently Used — ใช้ OrderedDict หรือ doubly-linked list + hash map), LFU
  - Cache miss มีค่า cost สูง — algorithm ที่ดีควร minimizes cache misses
- **External Searching — B-Trees**: โครงสร้างข้อมูลสำหรับข้อมูลที่ใหญ่กว่า memory (เก็บบน disk)
  - **(a,b) Tree**: multiway search tree, แต่ละ node มี a ถึง b children, ทุก leaf อยู่ level เดียวกัน
  - **B-Tree**: (2, n) tree ที่ n = order (จำนวน keys สูงสุดต่อ node), node เต็ม → split
  - B-Tree เป็น basis ของ database indexes (MySQL InnoDB, SQLite) — lookup/insert/delete O(log n)
- **External-Memory Sorting**: sorting ข้อมูลที่ใหญ่กว่า RAM
  - **Multiway Merging**: แบ่งข้อมูลเป็น chunks ที่ fit ใน memory, sort แต่ละ chunk แล้ว merge ด้วย k-way merge

---

## Appendix A — Character Strings in Python

- Python str เป็น immutable sequence ของ Unicode characters
- String operations: concatenation (`+` — O(n), ไม่ efficient สำหรับ many concatenations ให้ใช้ `"".join(list)`), repetition (`*`), slicing, searching (`in`, `find`, `count`), formatting (`%`, `.format()`, f-strings)
- String methods ที่สำคัญ: `split()`, `strip()`, `replace()`, `upper()`, `lower()`, `startswith()`, `endswith()`

## Appendix B — Useful Mathematical Facts

- Sum formulas: arithmetic series, geometric series, harmonic numbers
- Logarithm properties: `log_a(b) = log_c(b) / log_c(a)`, `log(ab) = log(a) + log(b)`
- Floor/ceiling: `⌊x⌋` (round down), `⌈x⌉` (round up)
- Permutations/Combinations: P(n,k) = n!/(n-k)!, C(n,k) = n!/(k!(n-k)!)
- Probabilities: สำหรับ birthday paradox, expected values, linearity of expectation

---

*สรุปจากหนังสือ Data Structures and Algorithms in Python (770 หน้า) — ครอบคลุม 15 บท + 2 Appendices ครบถ้วน*
