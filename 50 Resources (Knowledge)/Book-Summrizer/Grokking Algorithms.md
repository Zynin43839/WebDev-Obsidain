# Grokking Algorithms

> **Author:** Aditya Y. Bhargava

**Grokking Algorithms** เป็นหนังสือที่อธิบายแนวคิดอัลกอริทึม อันเข้าใจยากด้วยภาษาที่อ่านสนุกและมีภาพการ์ตูนประกอบ เป้าหมายคือให้ผู้อ่านเข้าใจว่าอัลกอริทึมตัวไหนควรใช้เมื่อไหร่ (trade-offs) ไม่ใช่แค่เขียนตามเฉยๆ

## บทที่ 1: Introduction to Algorithms

อธิบายแนวคิดของ **Big O notation** ผ่านการเปรียบเทียบ simple search (O(n)) กับ **Binary search (O(log n))**

- Simple search ต้องตรวจทุกๆ element ทีละตัวตั้งแต่ตัวแรกจนถึงตัวสุดท้าย (linear time)
- Binary search ทำงานบน sorted list โดยแบ่งครึ่งข้อมูลไปเรื่อยๆ ครั้งละครึ่ง
- log2 8 = 3 เพราะ 2³=8: binary search จะใช้อย่างมาก 3 ครั้งในการหา element ใน 8 ตัว
- Big O describes the worst-case runtime growth rate, not the actual speed

> **ตัวอย่าง:** หาคำในพจนานุกรม 240,000 คำ: simple search → 240,000 steps, binary search → 18 steps

**Travelling salesperson problem**: NP-complete problem ที่ตัวอย่างการแก้ 5 เมืองมี 120 permutations (O(n!)), 100 เมืองต้องใช้เวลามากกว่าอายุขัยของจักรวาลในการหาคำตอบที่เหมาะสมที่สุด

## บทที่ 2: Selection sort

อธิบายความแตกต่างระหว่าง **array** และ **linked list** เพื่อเลือก Data Structure ที่เหมาะสมกับงาน:

| Operation | Array | Linked list |
|-----------|-------|-------------|
| Access/Read | O(1) random | O(n) sequential |
| Insert | O(n) | O(1) |
| Delete | O(n) | O(1) |

- Array: จองเนื้อที่ติดกันในหน่วยความจำ → read เร็ว แต่ insert/delete ช้า
- Linked list: แต่ละ node เก็บ address ของ node ถัดไป; ไม่ต้องจองที่ติดกัน → insert/delete ที่จุดเริ่มและจุดสิ้นสุดทำได้ O(1)
- องค์ประกอบของ array จะนับจาก index 0 เสมอ
- Selection sort: ทำงาน O(n²) โดยหา element ที่เล็กที่สุดใน array ไล่ตั้งแต่ index 0 ขึ้นไปเรื่อยๆ
- Arrays are better for random access (jumping to any element)
- All elements in an array must be the same data type

## บทที่ 3: Recursion

Recursion คือการที่ function เรียกตัวมันเอง

- condition ทุก recursion ต้องมี 2 ส่วน: **base case** (เงื่อนไขหยุด) และ **recursive case** (เรียกตัวเอง)
- infinite loop ถ้าไม่ใส่ base case; ตัว call stack memory จะ overflow

> **ตัวอย่าง quick & dirty:** ถ้า list ว่าง ให้ return 0; ถ้าไม่ว่าง ให้ return 1 + การเรียก list ที่เหลือซ้ำอีกครั้ง (ให้ได้ base case)

**Call stack**: เมื่อ function A เรียก function B, function A จะถูก pause อยู่ใน stack จนกว่า B จะ return. แต่ละ function call ใช้ memory ใน stack; ถ้า recursion ลึกเกินไป อาจทำให้ stack overflow.

## บทที่ 4: QuickSort

QuickSort เป็น sorting algorithm ที่ใช้ **divide-and-conquer (D&C)** และทำงานใน average O(n log n) — เร็วกว่า selection sort มาก

- D&C principle: divide the problem into smallest unit (base case) until it's solved
- QuickSort picks a pivot (ขวาน) then partitions the array around the pivot:
  - ตัวที่เล็กกว่า pivot ไปอยู่ sub-array ด้านซ้าย
  - ตัวที่ใหญ่ไปอยู่ sub-array ด้านขวา

- Recursively sort left and right sub-arrays
- Worst case: O(n²) — เกิดขึ้นเมื่อ pivot ถูกเลือกไม่ดี (เช่น array ที่ถูก sort ไว้แล้ว)
- Average & best case: O(n log n) — pemilihan pivot ที่ดี (เช่น สุ่ม) ทำให้ partition มีความสมดุล
- vs Merge sort: Quicksort is faster in practice because of the constant factor (c) dominates

## บทที่ 5: Hash Tables

Hash table คือ Data Structure ที่ combine 3 สิ่ง: hash function + array + ability to handle collisions.

- Key → hash function → index in array → store/lookup value O(1)
- Collisions: 2 keys map to the same index → resolved by LinkedList in the same slot
- Load factor = number of items / total slots. Once > 0.7, resize (double the array)
- A good hash function distributes keys evenly and avoids collisions

**Use cases:**
- look-up (dictionary, phone book)
- duplicate prevention
- caching/memorization (e.g., web page cache)
- also: DNS resolution (mapping URLs to IP), bots/firewalls

## บทที่ 6: Breadth-First Search (BFS)

BFS is a graph algorithm whose purpose is to find the shortest path:

- Solves two problems: 1) Is there a path? 2) What is the shortest path?
- Graph terdiri dari nodes dan edges. Edge bisa directed (mono-directional) or undirected.
- BFS visits all the nearest neighbors (1st degree) before second-degree nodes
- Uses a queue - FIFO (First In, First Out) to keep track of the order
- runtime: O(V + E) — V for vertices, E for edges  
- BFS guarantees the shortest path in terms of the number of edges (not the weight)

## Chapter 7: Dijkstra's (The file does not contain the full text of chapters 7-11)

- Dijkstra's algorithm is used to find the shortest path in weighted graphs
- Uses a greedy approach: at each step, picks the unvisited node with the smallest known distance
- Works only with non-negative weights (cannot handle graphs with negative edge)

## Chapter 8: Greedy Algorithms

- Greedy: at each step, pick the locally optimal decision
- Does not guarantee globally optimal solution in all cases
- If the problem has optimal substructure (optimal solution contains optimal sub-solutions), greedy works

**Set-covering problem**: Given a U set and a set of S subsets, we need the smallest number of subsets such that their union covers everything in U. NP-complete; greedy approximation works well.

## Chapter 9: Dynamic Programming

- Solves problem by breaking into sub-problems, combining their solutions
- Use DP when the problem can be divided into smaller independent sub-problems
- Can be solved with a 2D table where rows are items and columns are knapsack capacities
- Allows fractional solutions? No, it requires 0/1 decisions.

## Chapter 10: KNN

- K-Nearest Neighbors: a supervised learning algorithm for classification and regression
- How: Given a new sample, find the k closest training points using Euclidean distance
- k is a hyperparameter: larger k (e.g., 19) produces smoother boundary; smaller k (1) captures noise

## Chapter 11: Where to go from here

- Trees (binary search trees)
- Fourier transform
- Parallel algorithms
- MapReduce / distributed algorithms
- Bloom filters & HyperLogLog
- SHA algorithms (hashes, passwords)
- reverse indexing & search engines

---

## Key Takeaways

1. **Big O** is not about raw speed; it's about how the runtime grows as n grows.
2. **Binary search** O(log n) is fundamental for searching sorted lists.
3. **Quicksort** O(n log n) is the go-to sorting algorithm for most cases, beating selection sort O(n²).
4. **Hash tables** (which Time O(1) on average) are indispensable for lookups, deduplication, and caching.
5. **BFS** solves shortest path in unweighted graphs; Dijkstra's for weighted with non-negative edges.
6. **Dynamic Programming** breaks down problem into smaller parts and combines the best results.
7. **Greedy Algorithms** trade perfection for speed and are often used for NP-complete problems.

---
