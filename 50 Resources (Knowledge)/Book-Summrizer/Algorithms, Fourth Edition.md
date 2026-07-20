# Algorithms, Fourth Edition
> **Author:** Robert Sedgewick, Kevin Wayne

หนังสือเล่มนี้เป็นตำรา algorithm ระดับตำนาน 969 หน้า เขียนโดย Robert Sedgewick (Princeton University) และ Kevin Wayne เน้นการเรียน algorithm ผ่านโค้ด Java จริงๆ ไม่ใช่แค่ theory ล้วนๆ เหมาะสำหรับคนที่อยากเข้าใจว่า algorithm ทำงานยังไงในทางปฏิบัติ

---

## ภาพรวมหนังสือ (Overview)

หนังสือแบ่งเป็น 6 บทหลัก:

| บท | หัวข้อหลัก | เนื้อหา |
|:---|:----------|:--------|
| 1 | Fundamentals | พื้นฐานการเขียนโปรแกรม, Data Abstraction, Data Structure พื้นฐาน, การวิเคราะห์ algorithm, Union-Find |
| 2 | Sorting | เรียงลำดับข้อมูล — จาก Elementary Sort ถึง Mergesort, Quicksort, Priority Queue |
| 3 | Searching | ค้นหาข้อมูล — Binary Search Tree, Balanced BST, Hash Table |
| 4 | Graphs | กราฟ — Undirected/Directed Graph, Shortest Path, Minimum Spanning Tree |
| 5 | Strings | ประมวลผลข้อความ — String Sort, Trie, Pattern Matching, Data Compression |
| 6 | Context | หัวข้อขั้นสูง — Event-based Simulation, B-Tree, Suffix Array, Max Flow, NP-Completeness |

**เหตุผลที่ต้องเรียน algorithm**: ถ้าข้อมูลมีล้านตัว แค่เลือก algorithm ที่ดีก็ทำให้โปรแกรมเร็วขึ้นล้านเท่าได้ ต่างจากการซื้อเครื่องใหม่ที่เร็วขึ้นแค่ 10-100 เท่า

---

## Chapter 1: Fundamentals (พื้นฐาน)

### 1.1 Basic Programming Model (แบบจำลองการเขียนโปรแกรมพื้นฐาน)

ใช้ Java subset เล็กๆ เขียนโค้ดที่อ่านง่าย รันได้จริง

**องค์ประกอบหลักของโปรแกรม Java:**

- **Primitive Data Types** — ชนิดข้อมูลพื้นฐาน: `int` (จำนวนเต็ม 32-bit), `double` (ทศนิยม 64-bit), `boolean` (จริง/เท็จ), `char` (ตัวอักษร 16-bit)
- **Statements** — คำสั่ง 6 ชนิด: declaration (ประกาศตัวแปร), assignment (กำหนดค่า), conditional (if/else), loop (while/for), call (เรียก method), return (ส่งค่ากลับ)
- **Arrays** — เก็บข้อมูลหลายตัวชนิดเดียวกัน  index เริ่มจาก 0 ถึง N-1; ต้อง declare → create (`new`) → initialize; มี bounds checking อัตโนมัติ ถ้า index ผิดจะ抛 `ArrayOutOfBoundsException`
- **Static Methods** — function ที่ encapsulate โค้ดซ้ำๆ ใช้ `public static` ประกาศ; arguments passing เป็น pass by value (แต่ array จะเป็น alias)
- **Strings** — ข้อความใน Java ไม่ใช่ primitive type แต่มี built-in support มากมาย เช่น concatenation (`+`), `Integer.parseInt()`, `Double.parseDouble()`
- **Input/Output** — ผ่าน `StdIn`, `StdOut`, `StdDraw`; รองรับ redirection (`>`, `<`) และ piping (`|`)

**Binary Search** — ตัวอย่าง algorithm แรกของหนังสือ: ค้นหาค่าใน array ที่เรียงลำดับแล้วโดยแบ่งครึ่งไปเรื่อยๆ ใช้เวลา O(log N) ต่างจาก brute-force ที่ใช้ O(N)

**Recursion** — method เรียกตัวเอง มี 3 กฎทอง:
1. ต้องมี base case (หยุดเมื่อเงื่อนไขถึง)
2. ต้อง converge ไปหา base case (ปัญหาย่อยต้องเล็กลงเรื่อยๆ)
3. ห้ามมี subproblem ที่ overlap กัน

**Modular Programming** — แยกโค้ดเป็น module ย่อยๆ ให้ import มาใช้ซ้ำได้; แต่ละ module ควรมี `main()` method สำหรับ unit test

**API (Application Programming Interface)** — สัญญาณระหว่าง client และ implementation: client ไม่ต้องรู้ว่า implement ยังไง แค่รู้ว่า method ทำอะไร

---

### 1.2 Data Abstraction (การสร้าง Abstraction จากข้อมูล)

**Abstract Data Type (ADT)** คือ data type ที่ซ่อน representation ไว้ข้างใน ให้ client ใช้งานผ่าน API อย่างเดียว

**Object-Oriented Programming ย่อๆ:**
- **Object** = entity ที่เก็บค่า data type มี 3 คุณสมบัติ: state (ค่า), identity (ที่อยู่ในหน่วยความจำ), behavior (method ที่ทำได้)
- **Constructor** — เรียก `new` เพื่อสร้าง object, allocate memory, initialize ค่า
- **Instance Methods** — เรียกผ่าน object name เช่น `heads.increment()` ต่างจาก static method ที่เรียกผ่าน class name เช่น `Math.sqrt()`
- **Aliasing** — เมื่อ assign ตัวแปร reference สองตัวไป object เดียวกัน การเปลี่ยนค่าตัวใดตัวหนึ่งจะกระทบอีกตัวด้วย (common bug!)
- **Pass by Value** — Java ส่ง reference ค่าให้ method แต่ไม่ได้ส่ง object จริงๆ; method เปลี่ยน object ได้แต่เปลี่ยน reference ต้นทางไม่ได้

**ตัวอย่าง ADT:**
- **Counter** — ตัวนับชื่อ + จำนวน increment
- **Point2D, Interval1D, Interval2D** — วัตถุเรขาคณิต (ใช้ Monte Carlo คำนวณพื้นที่สี่เหลี่ยม)
- **Date, Transaction** — วันที่และธุรกรรม (ข้อมูลเชิงธุรกิจ)
- **String** — มี method หลายสิบตัว เช่น `charAt()`, `indexOf()`, `substring()`, `compareTo()`

**Inherited Methods** — Java บังคับให้ทุก data type มี `toString()`, `equals()`, `compareTo()`, `hashCode()` ถ้า implement ดีๆ จะใช้กับ algorithm ทั่วไปได้

---

### 1.3 Bags, Queues, and Stacks (ถุง, คิว, และสแต็ก)

**3 Collection ADT พื้นฐาน:**

| ADT | หลักการ | เหมือน |
|:----|:---------|:-------|
| **Bag** (ถุง) | ไม่มีลำดับ, ใส่ได้ตลอด, ไม่ลบ | ตะกร้าเก็บของ |
| **Queue** (คิว) | FIFO — ใครมาก่อนได้ก่อน | ต่อแถวซื้อของ |
| **Stack** (สแต็ก) | LIFO — ใครมาทีหลังได้ก่อน | จานซ้อน |

**Stack** — ใช้ใน recursion (call stack), undo, expression parsing; มี 2 operations: `push` (ใส่), `pop` (เอาออก)

**Queue** — ใช้ใน buffering, print spooling, BFS; FIFO โดยธรรมชาติ

**Linked List** — node แต่ละตัวชี้ไป node ถัดไป; ต่างจาก array ที่เก็บต่อเนื่องในหน่วยความจำ; เพิ่ม/ลบ ต้น list ทำได้ O(1) แต่เข้าถึง index 任意 ต้องเดินทีละตัว

**resizing array** — array ที่ขยายตัวอัตโนมัติเมื่อเต็ม ( amortized O(1) ต่อ insert)

**Generics** — ใช้ `<Item>` เพื่อให้ collection ใช้กับข้อมูลชนิดใดก็ได้ (ไม่ใช่แค่ int หรือ String)

---

### 1.4 Analysis of Algorithms (การวิเคราะห์ Algorithm)

**Why analyze?** — ไม่ใช่แค่รู้ว่า program ทำงานได้ แต่ต้องรู้ว่าใช้เวลากี่หน่วย, ความจำเท่าไหร่

**Observation → Hypothesis → Theory → Empirical Analysis:**

- **Growth Rate** — สิ่งที่สำคัญที่สุดคือ order of growth เช่น O(N), O(N²), O(N log N); ค่าสัมประสิทธิ์ (constant) ไม่สำคัญเท่า N ที่ใหญ่ขึ้น
- **Tilde Approximation** (~) — ละ term ที่เล็กกว่า dominant term เช่น 3N³ + 5N² ~ N³
- **Big-O (O)** — upper bound ของ growth rate
- **Binary Search** — O(log N) ต่างจาก linear search O(N) อย่างสิ้นเชิง
- **Mergesort** — O(N log N) ดีกว่า insertion sort O(N²) เมื่อ N ใหญ่ (เช่น N=100000 ต่างกัน 5,000 เท่า)

**Amortized Analysis** — การวิเคราะห์ค่าเฉลี่ยของการ operation หลายครั้ง เช่น array resizing มี amortized cost O(1) แม้บางครั้งจะ resize ทีละ O(N)

**Probabilistic Analysis** — วิเคราะห์ algorithm ที่มี random elements เช่น randomized QuickSort มี expected time O(N log N) เสมอ

**หัวข้อสำคัญ:**
- ความสัมพันธ์ระหว่าง space กับ time
- วิธี profiling (วัดเวลาจริง), การ experiment
- วิธีพิสูจน์ correctness ของ algorithm
- วิธีวิเคราะห์ recursive algorithm (recurrence relation)

---

### 1.5 Case Study: Union-Find (กรณีศึกษา: การรวมกลุ่ม)

**Problem**: มี N object ต้องรู้ว่า two objects เชื่อมต่อกันหรือไม่ และ merge สองกลุ่มเข้าด้วยกัน

**Use Case** — เครือข่าย, social network, image processing (เชื่อม pixel ที่ใกล้กัน)

**API:**
- `UF(int N)` — สร้างด้วย N object
- `union(int p, int q)` — เชื่อม p กับ q
- `connected(int p, int q)` — คืน true ถ้า p กับ q อยู่กลุ่มเดียวกัน
- `find(int p)` — หา id ของกลุ่มที่ p อยู่

**วิวัฒนาการ: Quick-Find → Quick-Union → Weighted Quick-Union → Weighted Quick-Union with Path Compression**

| อัลกอริทึม | Union | Find | หมายเหตุ |
|:-----------|:-----:|:----:|:--------|
| Quick-Find | O(N) | O(1) | union ช้ามาก |
| Quick-Union | O(N) | O(N) worst | find ช้าถ้า tree สูง |
| Weighted Quick-Union | O(log N) | O(log N) | union โดยเปรียบเทียบขนาด |
| Weighted + Path Compression | ~O(1) amortized | ~O(1) amortized | flat tree เกือบทุกครั้ง |

**Path Compression** — ทุกครั้งที่ find, ทำให้ node ทุกตัวบนเส้นทางชี้ตรงไป root (flatten tree); union กับ path compression ทำให้ operation เกือบ O(1) ตลอด

---

## Chapter 2: Sorting (การเรียงลำดับ)

### 2.1 Elementary Sorts (การเรียงลำดับพื้นฐาน)

**Rules of the Game:**
- เรียง array ของ Java objects ที่ implement `Comparable`
- `v < w` ถ้า `v.compareTo(w) < 0`
- Sorting in-place: ไม่ต้องใช้ memory เพิ่ม (swap ข้อมูลใน array เดิม)
- Stability: element ที่เท่ากันจะรักษาลำดับเดิมหรือไม่

**3 Algoritms พื้นฐาน:**

| Algorithm | Time | Space | Stable? | ใช้ตอนไหน |
|:----------|:----:|:-----:|:-------:|:-----------|
| **Selection Sort** | O(N²) | O(1) | ไม่ | N เล็กมาก |
| **Insertion Sort** | O(N²) | O(1) | ใช่ | N เล็ก หรือเกือบ sorted |
| **Shell Sort** | ~O(N^{3/2}) | O(1) | ไม่ | N ปานกลาง, code สั้น |

- **Selection Sort** — หา min ทีละรอบ แล้ว swap ไปตำแหน่งถูกต้อง; เขียนง่ายสุด แต่ช้าสุด (data movement น้อยสุด = N swaps)
- **Insertion Sort** — หยิบ element ทีละตัว สอดเข้าไปในตำแหน่งที่ถูกต้อง; ดีมากถ้าข้อมูลเกือบ sorted (nearly sorted = O(N))
- **Shell Sort** — เป็น generalization ของ insertion sort ที่ swap ไกลๆ ก่อน (h-sorting) แล้วค่อย缩小 h; code สั้น ประสิทธิภาพดีพอใช้สำหรับ N ปานกลาง

---

### 2.2 Mergesort (การเรียงลำดับแบบผสาน)

**Core idea**: แบ่ง array เป็นสองส่วน เรียงส่วนละอัน (recursively) แล้ว merge กลับเข้าด้วยกัน

- **Time**: O(N log N) เสมอ ทั้ง best, average, worst case
- **Space**: O(N) — ต้องใช้ auxiliary array ชั่วคราว
- **Stable**: ใช่ (ถ้า implement ถูกต้อง)

**Optimizations:**
- ถ้า array เล็ก (≤ 7-15 ตัว) ใช้ insertion sort จะเร็วกว่า (merge overhead ไม่คุ้ม)
- **Skip merge** ถ้า a[mid] ≤ a[mid+1] (ข้อมูลครึ่งล่างน้อยกว่าครึ่งบนอยู่แล้ว)
- **Bottom-up mergesort** — ไม่ต้องใช้ recursion; merge ทีละคู่ (pass 1: merge pairs of 1, pass 2: merge pairs of 2, ...)
- Copy to auxiliary array สลับกันไปมา ระหว่าง aux กับ original array (eliminate copy time)

**Inverse Count** — จำนวน inversion pairs ใน array; mergesort นับ inversion ได้พร้อมกับ sort

---

### 2.3 Quicksort (การเรียงลำดับแบบรวดเร็ว)

**Core idea**: เลือก pivot แล้ว partition array ให้ element เล็กกว่า pivot อยู่ซ้าย ใหญ่กว่าอยู่ขวา; เรียงซ้ายและขวา (recursively)

- **Time**: O(N log N) average, O(N²) worst case (ถ้า pivot แย่)
- **Space**: O(log N) — เรียงในที่ (in-place)
- **Stable**: ไม่ (partition เปลี่ยนลำดับ)

**Why Quicksort?** — ในทางปฏิบัติเร็วกว่า Mergesort เพราะ cache-friendly (ทำงานใน array เดิม) และ constant factors เล็กกว่า

**Optimizations สำคัญ:**
- **Shuffle ก่อน sort** — ป้องกัน worst case จากข้อมูลที่เรียงแล้ว
- **3-way partitioning (Dutch National Flag)** — จัด element เป็น 3 กลุ่ม: เล็กกว่า pivot, เท่ากับ pivot, ใหญ่กว่า pivot; ดีมากเมื่อมี duplicate จำนวนมาก
- **Cutoff to insertion sort** — เหมือน mergesort

**Duplicate Keys** — 3-way partitioning จัดการ duplicate ได้ดี (ไม่ partition ซ้ำซ้อน)

---

### 2.4 Priority Queues (คิวลำดับความสำคัญ)

**Abstract Data Type**:
- ใส่ข้อมูล (insert)
- เอา element ที่มีค่ามากที่สุด/น้อยที่สุดออก (remove max/min)

**Binary Heap** — โครงสร้างข้อมูล tree แบบ complete binary tree ที่ใช้ array เก็บ:
- parent ของ index `k` อยู่ที่ `k/2`
- children ของ index `k` อยู่ที่ `2k` และ `2k+1`
- **Max-Heap**: parent ≥ children
- Insert: swim ขึ้น (O(log N))
- Remove max: sink ลง (O(log N))

**Heap Sort** — build max-heap แล้ว swap root กับ element สุดท้าย แล้ว sink; O(N log N) in-place แต่ slow iver quicksort ในทางปฏิบัติ (ไม่ cache-friendly)

**Multiway Merge** — ใช้ priority queue merge หลาย sorted sequence เข้าด้วยกัน

**Event-Driven Simulation** — ใช้ priority queue เก็บ events เรียงตามเวลา; ตัวอย่าง: จำลองการชนของอนุภาค (particle collision)

---

### 2.5 Sorting Applications (การประยุกต์ใช้การเรียงลำดับ)

- **Stability สำคัญมาก** — หลาย application ต้องการรักษาลำดับเดิม เช่น เรียงข้อมูลหลาย column ทีละคอลัมน์
- **Selection** — หา k-th smallest element ( QuickSelect, O(N) average)
- **Duplicate** — นับจำนวน unique keys
- **Frequency** — นับความถี่ของแต่ละ key
- **Sorting custom objects** — ใช้ Comparable interface หรือ Comparator

---

## Chapter 3: Searching (การค้นหา)

### 3.1 Symbol Tables (ตารางสัญลักษณ์)

**Abstract Data Type**: เก็บ key-value pairs; operations:
- `put(key, val)` — ใส่/อัพเดท
- `get(key)` — คืนค่าที่ match key
- `delete(key)` — ลบ
- `contains(key)` — ตรวจสอบ
- `size()`, `keys()` — ข้อมูลเพิ่มเติม

**API Design**: ใช้ convention ของ Java — `get(key)` คืน `null` ถ้า key ไม่มี, `put(key, val)` ลบ key ถ้า val เป็น null

**Sequential Search (Linked List)** — ใส่/ค้นหา ทีละตัว; O(N) ทุก operation

**Binary Search (Sorted Array)** — ค้นหา O(log N) แต่ insert ช้า (ต้อง shift) — เหมาะกับข้อมูลที่แทบไม่เปลี่ยน

---

### 3.2 Binary Search Trees (BST)

**Structure**: Binary tree ที่มี property: ค่าใน left subtree < node < ค่าใน right subtree

**Operations:**
- Search: เทียบ key กับ root แล้วไปซ้ายหรือขวา; O(log N) average, O(N) worst case
- Insert: ค้นหาจนเจอ null แล้วใส่ตรงนั้น
- Inorder traversal: visit left → root → right = ได้ข้อมูลเรียงลำดับ

**Problems:**
- ถ้า insert ข้อมูลเรียงลำดับ (sorted) จะได้ tree ที่ link เหมือน linked list → O(N) ทุก operation
- ไม่มี balance guarantee

---

### 3.3 Balanced Search Trees (Binary Search Tree สมดุล)

**2-3 Tree** — tree ที่มี 2 ชนิด node:
- **2-node**: มี key 1 ตัว, children 2 ตัว
- **3-node**: มี key 2 ตัว, children 3 ตัว
- Tree สมดุลเสมอ: path ทุกเส้นยาวเท่ากัน

**Red-Black BST** — implementation ของ 2-3 tree ที่ใช้ node สีแดง/ดำ:
- Red link = เชื่อม 3-node (left-leaning)
- Black link = เชื่อม 2-node
- Properties: root เป็นสีดำ, red link ชี้ซ้ายเท่านั้น, ไม่มี two red links ติดกัน, path จาก root มี black links เท่ากันทุกเส้น
- **Insertion**: ใช้ rotations + color flips; O(log N) เสมอ

**B-Trees** — tree ที่แตก node ได้หลาย children; ออกแบบมาให้契合 disk I/O (minimize disk reads)

---

### 3.4 Hash Tables (ตารางแฮช)

**Core Idea**: ใช้ hash function เปลี่ยน key เป็น index ใน array (_buckets)

**Hash Function**: 
- `hashCode()` → convert key เป็น integer
- Compression function: `hash(key) % M` (M = ขนาด array)
- ต้องการ: uniform distribution, deterministic, efficient compute

**Collision Resolution:**
- **Separate Chaining** — bucket เก็บ linked list; ใส่ element ที่ hash เข้า list เดียวกัน; load factor λ = N/M
- **Linear Probing** — ถ้า bucket เต็ม ลองถัดไปเรื่อยๆ; ดีเพราะ cache-friendly; ต้อง unload (λ < 0.75)

**Performance:**
- Average: O(1) สำหรับทุก operation (ถ้า hash function ดี และ load factor ต่ำพอ)
- Worst case: O(N) ถ้า hash function แย่มาก

**Hashing Strategy ที่ดี:**
- ใช้ prime numbers สำหรับ table size
- Separate chaining + prime size มักดีกว่า linear probing
- ใช้ randomized hashing เพื่อป้องกัน DoS attack

---

### 3.5 Searching Applications (การประยุกต์ใช้การค้นหา)

- **Dictionary** — lookup word แล้วบอกความหมาย
- **File Index** — index ทุกคำในทุกไฟล์ (inverted index)
- **Sparse Vector** — เก็บเฉพาะ element ที่ไม่เป็น 0
- **Hash Table ในภาษา Java** — `java.util.HashMap` ใช้ separate chaining
- **BST สำหรับ ordered data** — Red-Black tree ใช้ใน `java.util.TreeMap`

---

## Chapter 4: Graphs (กราฟ)

**Graph** = จุด (vertices) เชื่อมด้วยเส้น (edges) — model ปัญหาหลายอย่าง: เครือข่าย, social network, map, dependency

### 4.1 Undirected Graphs (กราฟไม่มีทิศทาง)

**Graph API:**
- `V()` — จำนวน vertices
- `E()` — จำนวน edges
- `addEdge(v, w)` — เพิ่ม edge
- `adj(v)` — adjacency list ของ v

**Representations:**
- **Adjacency Matrix** — 2D array; ใช้ memory O(V²) — ดีสำหรับ dense graph
- **Adjacency List** — array ของ lists; ใช้ memory O(V + E) — ดีสำหรับ sparse graph (ใช้ในหนังสือ)

**Search Algorithms:**
- **DFS (Depth-First Search)** — ไปให้ลึกสุดก่อน แล้วย้อนกลับ; ใช้ stack (explicit หรือ call stack); ใช้ cycle detection, connected components, path finding
- **BFS (Breadth-First Search)** — ไปทีละ level; ใช้ queue; หา shortest path (จำนวน edges) จาก source ถึง destination

**Connected Components** — ใช้ DFS หา component ทั้งหมด; two vertices อยู่ component เดียวกัน ↔ มี path เชื่อม

**Cycle Detection** — ใช้ DFS, ถ้าเจอ back edge (edge ไปหา ancestor) = มี cycle

**Bipartite Test** — ใช้ BFS/DFS สองสี; ถ้าสองสีไม่ชนกัน = bipartite (2-colorable)

---

### 4.2 Directed Graphs (กราฟมีทิศทาง)

**Digraph** — edge มีทิศทาง (A → B ≠ B → A)

**DFS on Digraphs:**
- **Pre/Post order** — บันทึกเวลาเข้า/ออกจาก vertex
- **Topological Sort** — เรียง vertices ให้ทุก edge ชี้ไปข้างหน้า; เหมาะสำหรับ task scheduling, course prerequisites; DFS-based: reverse postorder
- **Cycle Detection** — ถ้ามี back edge = มี cycle; digraph ที่ไม่มี cycle = DAG (Directed Acyclic Graph)
- **Strongly Connected Components (SCC)** — Kosaraju's algorithm: ใช้ DFS สองรอบ; สำคัญสำหรับ web page analysis, dependency resolution

---

### 4.3 Shortest Paths (เส้นทางสั้นที่สุด)

**Problem**: หาเส้นทางที่ weight รวมน้อยที่สุด จาก source ถึงทุก vertex

| Algorithm | Weight | Neg Cycle? | Time |
|:----------|:------:|:----------:|:----:|
| **BFS** | Unweighted | N/A | O(V + E) |
| **Dijkstra** | Non-negative | ไม่รองรับ | O((V + E) log V) |
| **Bellman-Ford** | Any | ตรวจจับได้ | O(VE) |
| **Topological Sort** | DAG | N/A | O(V + E) |

- **Dijkstra** — ใช้ priority queue; เลือก vertex ที่ distance สั้นสุดที่ยังไม่ visited; ห้ามมี negative edges
- **Bellman-Ford** — ทำ relaxation V-1 รอบ; ตรวจ negative cycle ได้; ช้ากว่า Dijkstra
- **Shortest Path Tree** — tree ของ shortest paths จาก source

---

### 4.4 Minimum Spanning Trees (ต้นไม้พ้นน้ำทั้งหมด)

**MST** = subgraph ที่เชื่อมทุก vertex เข้าด้วยกันโดย weight รวมน้อยที่สุด (ไม่มี cycle)

**2 Algorithms หลัก:**

| Algorithm | Approach | Time | ใช้ตอนไหน |
|:----------|:---------|:----:|:-----------|
| **Kruskal** | เรียง edges แล้วเพิ่มทีละตัว (ถ้าไม่ทำ cycle) | O(E log E) | Sparse graph |
| **Prim** | เริ่มจาก vertex เดียว แล้วขยาย frontier | O(E log V) | Dense graph |

- **Kruskal** — ใช้ Union-Find ตรวจ cycle; sort edges ทีเดียว
- **Prim** — เหมือน Dijkstra แต่ใช้ weight แทน distance; priority queue เก็บ edges บน frontier
- **Proof of Correctness** — Cut Property: ทุก cut มี MST edge ข้าม cut ที่มี weight น้อยสุด

---

## Chapter 5: Strings (ข้อความและข้อความ)

### 5.1 String Sorts (การเรียงลำดับข้อความ)

**Key Insight**: string comparison ไม่เหมือน number comparison — length ต่างกัน, character ต่างกัน

**Algorithms:**
- **Key-Indexed Counting** — เรียงตัวอักษรทีละตัว (MSD); O(N) สำหรับ alphabet ขนาดเล็ก
- **LSD (Least Significant Digit) Radix Sort** — เรียงจากตัวอักษรขวาไปซ้าย; เหมาะกับ string ยาวเท่ากัน (เช่น license plates)
- **MSD (Most Significant Digit) Radix Sort** — เรียงจากซ้ายไปขวา; เหมาะกับ string ยาวไม่เท่ากัน (trie-like)
- **3-Way String Quicksort** — partition 3 ทาง แล้ว recurse; ดีสำหรับ string ที่มี prefix ซ้ำกันเยอะ

---

### 5.2 Tries (โครงสร้างข้อมูลแบบ Trie)

**Trie** = radix tree; เก็บ string โดยแต่ละ edge แทน character

**Properties:**
- Root ไม่มีค่า (empty)
- Node แต่ละตัวเก็บ character
- String แต่ละตัวอยู่บน path จาก root ถึง leaf (หรือ internal node ที่ mark ไว้)
- **No wasted space**: prefix ที่ซ้ำกันจะถูก sharing

**Variants:**
- **R-Way Trie** — มี children R ตัว (R = ขนาด alphabet)
- **Ternary Search Trie (TST)** — node มี 3 children: <, =, >; เก็บ space น้อยกว่า R-way
- **Ternary Search Trie with R-Way leaves** — combine ข้อดีของทั้งสอง

---

### 5.3 Substring Search (การค้นหา Pattern)

**Problem**: หา pattern ยาว M ใน text ยาว N

| Algorithm | Preprocessing | Matching | Time |
|:----------|:-------------:|:--------:|:----:|
| **Brute Force** | None | 逐 character | O(MN) |
| **KMP** | Build DFA from pattern | O(N) | O(M + N) |
| **Boyer-Moore** | Build skip table | Right-to-left | O(MN) best, O(N) avg |
| **Rabin-Karp** | Compute hash | Roll hash | O(M + N) average |

- **KMP (Knuth-Morris-Pratt)** — สร้าง DFA จาก pattern; ไม่ต้อง backtrack text pointer
- **Boyer-Moore** — เปรียบเทียบจากขวาไปซ้าย; skip characters ทีละหลายตัว; ในทางปฏิบัติเร็วกว่า KMP
- **Rabin-Karp** — ใช้ rolling hash เปรียบเทียบ hash values; ถ้า hash match ค่อยตรวจ character

---

### 5.4 Regular Expressions (นิพจน์ทั่วไป)

**RE** = pattern สำหรับ match string; สำคัญมากใน text processing

**NFA (Non-deterministic Finite Automaton):**
- สร้าง NFA จาก RE
- Match text กับ NFA: ถ้าถึง accept state = match
- **epsilon transitions** — ไป state ถัดไปได้โดยไม่ consumes character
- Time: O(MN) — M = ความยาว RE, N = ความยาว text

**GREP** — command line tool สำหรับค้นหา pattern ในไฟล์; ใช้ NFA-based matching

---

### 5.5 Data Compression (การบีบอัดข้อมูล)

**Goal**: ลดขนาดข้อมูลโดยไม่สูญเสียข้อมูล (lossless)

**Algorithms:**
- **Huffman Coding** — สร้าง prefix code โดยใช้ binary tree; character ที่พบบ่อยได้ code สั้น; ต้องส่ง tree พร้อม data
- **LZW (Lempel-Ziv-Welch)** — dictionary-based; แทน string ซ้ำด้วย index; ไม่ต้องส่ง dictionary
- **Arithmetic Coding** — encode ทั้ง message เป็น number เดียวระหว่าง 0-1; compression ratio ดีสุดแต่ซับซ้อน
- **Garbage Collection** — ข้อมูลที่ไม่ใช้แล้วถูกลบอัตโนมัติ

---

## Chapter 6: Context (หัวข้อขั้นสูง)

บทนี้เชื่อม algorithm กับสาขาอื่นๆ:

- **Event-Based Simulation** — จำลองระบบแบบ time-driven; ใช้ priority queue จัด events
- **B-Trees** — disk-based search tree; ใช้ใน database index; minimize disk I/O
- **Suffix Arrays and Suffix Trees** — ค้นหา substring ได้เร็วมาก; ใช้ใน text search, bioinformatics
- **Network Flow (Max Flow)** — หา flow สูงสุดใน network; Ford-Fulkerson, Edmonds-Karp; ประยุกต์ได้หลายปัญหา (matching, partitioning)
- **String Search in Large Text** — suffix array approach
- **NP-Completeness** — ปัญหาที่ยังไม่รู้ algorithm แบบ polynomial; reduction: ถ้าปัญหา A ลดเป็น B ได้ = B ยากเท่า A; examples: Traveling Salesman, Hamiltonian Cycle, SAT
- **Reduction** — เทคนิคสำคัญ: ถ้าแก้ปัญหาหนึ่งได้ แปลว่าแก้อีกปัญหาหนึ่งที่ยากกว่าได้ด้วย

---

## สรุปสิ่งที่หนังสือเล่มนี้สอน

1. **Algorithm สำคัญกว่า hardware** — เลือก algorithm ดีเร็วกว่าซื้อเครื่องใหม่ 100 เท่า
2. **Data Structure ≠ Algorithm แต่เชื่อมกัน** — BST, Hash Table, Graph ล้วนเป็น container สำหรับ algorithm
3. **Analysis of Algorithms** — ไม่ใช่แค่ "รันได้" แต่ต้องรู้ว่า "เร็วแค่ไหน" — Big-O, amortized, worst case
4. **Practical implementations** — หนังสือเน้น Java code จริง รันได้จริง ไม่ใช่แค่ pseudocode
5. **Modular programming** — แยก code เป็น module ย่อยๆ ให้ import ใช้ซ้ำได้
6. **Abstract Data Types** — ซ่อน implementation ไว้ behind API; เปลี่ยน algorithm ได้โดยไม่กระทบ client
7. **Trade-offs everywhere** — time vs space, simplicity vs performance, worst case vs average case
8. **Recursion เป็นเครื่องมือหลัก** — BST, QuickSort, Mergesort, Graph search ล้วนใช้ recursion
9. **Graph algorithms ครอบจักรวาล** — social network, routing, scheduling, ทุกอย่าง model เป็นกราฟได้
10. **String processing สำคัญมาก** — web search, text mining, compression ล้วนพึ่ง string algorithms

---
