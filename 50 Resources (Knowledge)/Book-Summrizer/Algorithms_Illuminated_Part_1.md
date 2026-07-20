# Algorithms Illuminated Part 1: The Basics

> **Author:** Tim Roughgarden

# Algorithms Illuminated Part 1 — สรุปภาษาไทย

## ภาพรวม
หนังสือเล่มนี้คือ Part 1 ของซีรีส์ Algorithms Illuminated โดย Tim Roughgarden (Stanford/Coursera) ครอบคลุมพื้นฐานทั้งหมดของ Algorithm Analysis ตั้งแต่ Asymptotic Analysis ไปจนถึง Divide-and-Conquer ครอบคลุม 6 บทเรียน + ปัญหาฝึกหัด มี 216 หน้า เหมาะสำหรับผู้เริ่มต้นที่ต้องการเข้าใจวิธีคิดและพิสูจน์ความถูกต้องของ algorithm

---

## Chapter 1: Introduction (หน้า 1-32)

### 1.1 Why Study Algorithms?
เหตุผลที่ต้องเรียน Algorithm:
- **Algorithm คือชุดคำสั่งที่แก้ปัญหา** — ทุกอย่างที่คอมพิวเตอร์ทำล้วนเป็น algorithm
- **Performance สำคัญ** — algorithm ที่ดีสามารถทำงานเร็วกว่าหลายล้านเท่าเมื่อเทียบกับวิธีง่ายๆ (brute force)
- **Machine ไม่ได้ช่วยเรื่อง algorithm ที่ไม่ดี** — เครื่องเร็วแค่ไหนก็ช่วย algorithm ที่ออกแบบแย่ไม่ได้ (ตัวอย่าง: MergeSort vs InsertionSort — บนข้อมูล 10 ล้านตัว, InsertionSort ใช้เวลา 3 ชั่วโมง, MergeSort ใช้เวลา 20 วินาที)
- **Algorithm คือ theory ที่ separate จาก technology** — algorithm ที่ดีจะยังดีอยู่แม้ hardware เปลี่ยนไป
- **Algorithm คือภาษาสากล** — ใช้ได้กับทุก domain ตั้งแต่ AI ไปจนถึง data science

### 1.2 Integer Multiplication — วิธีโรงเรียน (Schoolbook)
ปัญหา: คูณสองเลข integer ที่มี n หลัก

**วิธีโรงเรียน (Schoolbook Multiplication)**
- คูณทีละหลัก: แต่ละหลักของ first number คูณกับทุกหลักของ second number
- มี n × n = n² การคูณย่อย
- **Running time: O(n²)** — เรียก naive approach

ตัวอย่าง: n=6 หลัก ต้องมี 36 ครั้งการคูณย่อย (plus 5 ครั้งสำหรับ addition)

### 1.3 Karatsuba Multiplication
วิธีที่ฉลาดกว่าโดยใช้ **Divide-and-Conquer** idea:

**แนวคิดหลัก:**
ถ้ามีเลขสองหลัก x และ y แต่ละตัวมี n หลัก:
- แยก x = 10^(n/2) · a + b (เช่น x = 3141 → a=31, b=41)
- แยก y = 10^(n/2) · c + d
- คูณได้: xy = ac · 10^n + (ad+bc) · 10^(n/2) + bd

**วิธีง่ายๆ** ต้องคูณ 4 ครั้ง: ac, ad, bc, bd

**Karatsuba Trick:** คำนวณ only 3 คูณ:
- P1 = ac
- P2 = bd
- P3 = (a+b)(c+d) = ac + ad + bc + bd
- ad + bc = P3 - P1 - P2
- ผลลัพธ์: P1 · 10^n + (P3 - P1 - P2) · 10^(n/2) + P2

**Recursive formula:** T(n) = 3T(n/2) + O(n)
- แทนที่จะแยกเป็น 4 ปัญหาย่อย เหลือแค่ 3
- **Result: O(n^(log₂3)) ≈ O(n^1.585)** — เร็วกว่า O(n²) อย่างมากเมื่อ n ใหญ่

**Why?** เพราะลดจำนวน subproblems จาก 4 เหลือ 3 โดยแลกกับ addition/subtraction เล็กน้อย (linear work)

### 1.4 MergeSort: The Algorithm
**MergeSort** ใช้ Divide-and-Conquer:
1. **Divide:** แบ่ง array ครึ่งหนึ่ง (-recursively)
2. **Conquer:** เรียงลำดับแต่ละ half (recursively)
3. **Combine:** ผสาน (merge) สอง sorted array กลับเข้าด้วยกัน

**Merge Algorithm:**
- ใช้ two pointers i, j ชี้ที่ beginning ของแต่ละ sorted array
- เปรียบเทียบและเลือก element ที่เล็กกว่าใส่ output array
- ทำงานใน **O(n)** time — linear pass เท่านั้น

**Recursive formulation:**
- T(n) = 2T(n/2) + O(n) — 2 recursive calls + merge
- Base case: n=1 → O(1)

### 1.5 MergeSort: The Analysis

**วิธีวิเคราะห์: Recursion Tree Method**
- สร้าง recursion tree แต่ละ level มี cost O(n)
- Depth ของ tree = log₂(n) + 1 levels
- **Total cost = O(n log n)**

**Guiding Principles สำหรับการวิเคราะห์:**
1. **Ignore constant factors** — O(2n) = O(n)
2. **Ignore lower-order terms** — O(n² + n) = O(n²)
3. **Best, Worst, Average case** — merge sort มี performance เหมือนกันในทุก case (ถ้า input size เท่ากัน)

**Why MergeSort ดีกว่า InsertionSort:**
- InsertionSort: O(n²) — ทำงานช้าลงเร็วมากเมื่อ n ใหญ่
- MergeSort: O(n log n) — ยังทำงานเร็วอยู่แม้ n ใหญ่มาก
- ตัวอย่างจริง: n = 10,000,000 → InsertionSort ~3 ชั่วโมง, MergeSort ~20 วินาที

**Bottom-up MergeSort:** ไม่ใช้ recursion แต่ใช้ iteration — เริ่มจาก subarray ขนาด 1 แล้ว merge ขึ้นไปทีละ step

---

## Chapter 2: Asymptotic Analysis (หน้า 33-50)

### Big-O, Big-Omega, Big-Theta

**Big-O (Upper Bound):**
- f(n) = O(g(n)) ถ้ามีค่าคงที่ c > 0 และ n₀ ที่ f(n) ≤ c · g(n) สำหรับทุก n ≥ n₀
- **ความหมาย:** f ไม่เติบโตเร็วกว่า g (อย่างน้อยก็ไม่แย่กว่า g เท่าไหร่)

**Big-Omega (Lower Bound):**
- f(n) = Ω(g(n)) ถ้ามีค่าคงที่ c > 0 และ n₀ ที่ f(n) ≥ c · g(n) สำหรับทุก n ≥ n₀
- **ความหมาย:** f เติบโตไม่ช้ากว่า g (อย่างน้อยก็เร็วเท่า g เท่าไหร่)

**Big-Theta (Tight Bound):**
- f(n) = Θ(g(n)) ถ้า f(n) = O(g(n)) ทั้ง O และ Ω
- **ความหมาย:** f เติบโตในอัตราเดียวกับ g (มี upper และ lower bound เหมือนกัน)

**ตัวอย่าง:**
- 2n² + 3n + 5 = O(n²) — ทั้ง O, Ω, Θ(n²)
- 2^(n+10) = O(2^n) — constant factor 2^10 ถูก ignore
- log(n¹⁰) = Θ(log n) — power 10 ถูก ignore (เพราะ log(n^10) = 10 · log n)

### วิธีพิสูจน์ Big-O
วิธีที่นิยมที่สุดคือ **การคำนวณ limit:**
- f(n) = O(g(n)) ถ้า lim(n→∞) f(n)/g(n) = 0
- f(n) = Ω(g(n)) ถ้า lim(n→∞) f(n)/g(n) = ∞
- f(n) = Θ(g(n)) ถ้า lim(n→∞) f(n)/g(n) = c (ค่าคงที่ ≠ 0)

**常见 mistake:** อย่ามองแค่ highest-order term — ต้องดู coefficient ด้วย

---

## Chapter 3: Divide-and-Conquer (หน้า 51-100)

### 3.1 Counting Inversions (นับ Inversions)

**ปัญหา:** _given array A นับจำนวน inversion pairs (i,j) ที่ i < j แต่ A[i] > A[j]_

**Brute Force:** ตรวจสอบทุกคู่ → O(n²)

**Divide-and-Conquer approach:**
1. **Split** array ออกเป็น 2 ครึ่ง (Left, Right)
2. **Count** inversions ใน Left (recursively)
3. **Count** inversions ใน Right (recursively)
4. **Count split inversions** — inversions ที่ element หนึ่งอยู่ใน Left อีกตัวอยู่ใน Right

**Split Inversions:** ใช้ **merge sort variant** — ระหว่าง merge, ถ้า element จาก Right ถูกใส่ก่อน element จาก Left → element จาก Left ที่เหลือทั้งหมดสร้าง split inversions กับ element ตัวนั้น

**Result: O(n log n)** — เร็วกว่า brute force อย่างมาก

### 3.2 Strassen Matrix Multiplication

**ปัญหา:** คูณเมทริกซ์ขนาด n×n

**Naive approach:** n³ operations (for each element คำนวณ dot product ของ row × column)

**Divide-and-Conquer approach:**
- แบ่งแต่ละเมทริกซ์ออกเป็น 4 submatrices (quadrants)
- คูณ 8 ครั้ง recursive + addition 4 ครั้ง
- T(n) = 8T(n/2) + O(n²) → O(n³) — ไม่ดีกว่า naive!

**Strassen's Trick:** คำนวณ only 7 products (instead of 8)
- สร้าง 7 intermediate matrices (P1-P7) ด้วย addition/subtraction เฉพาะ
- คำนวณ P1-P7 ด้วย multiplication 7 ครั้ง
- สร้าง result quadrants จาก P1-P7

**Result:** T(n) = 7T(n/2) + O(n²) → O(n^(log₂7)) ≈ O(n^2.807)

**Note:** Strassen ไม่ได้ practically ดีกว่ามากในปัจจุบัน (cache effects, numerical stability) แต่แสดงให้เห็นว่า Divide-and-Conquer สามารถลดได้มากกว่า n³

### 3.3 Closest Pair of Points

**ปัญหา:** หาคู่จุดที่อยู่ใกล้กันที่สุดใน 2D plane

**Brute Force:** ตรวจสอบทุกคู่ → O(n²)

**Divide-and-Conquer:**
1. **Sort** จุดทั้งหมดตาม x-coordinate
2. **Split** ออกเป็น 2 ซีก (Left, Right)
3. **Recursive:** หา closest pair ใน Left, Right
4. **Combine:** หา closest pair ที่ straddle the divider

**Key Insight — Strip Method:**
- ถ้า closest pair ข้าม divider → ทั้งสองจุดต้องอยู่ภายใน distance δ (min จาก recursive calls) จาก line กลาง
- **Sort** จุดใน strip ตาม y-coordinate
- **Check** แต่ละจุดกับจุดถัดไป (max 7 จุดถัดไป) — **Lemma:** ต้อง check อย่างมาก 7 จุดถัดไปเท่านั้น

**Result:** T(n) = 2T(n/2) + O(n log n) → O(n log²n)
- .optimized: pre-sort by y แล้ว maintain → O(n log n)

---

## Chapter 4: The Master Method (หน้า 101-136)

### วิธีแก้ Recurrences

**Master Method** แก้ recurrence รูปแบบ: T(n) = aT(n/b) + O(n^d)

โดยที่:
- a = number of recursive calls (subproblems)
- b = factor ที่ input ถูกแบ่ง (subproblem size = n/b)
- d = exponent ของ work done outside recursion (O(n^d))

**Three Cases:**

**Case 1: d < log_b(a)**
- Recursive calls มากเกินไป — work ที่ Leaves มากกว่า Root
- **T(n) = O(n^(log_b(a)))**

**Case 2: d = log_b(a)**
- Work สมดุล — work ทุก level เท่ากัน
- **T(n) = O(n^d · log n)** = O(n^d · log₂n)

**Case 3: d > log_b(a)**
- Root ทำงานมากที่สุด — work ที่ Root มากกว่า Leaves
- **T(n) = O(n^d)**

### ตัวอย่างในหนังสือ

**MergeSort:** T(n) = 2T(n/2) + O(n)
- a=2, b=2, d=1
- log₂(2) = 1 = d → **Case 2**
- **Result: O(n log n)** ✓

**Binary Search:** T(n) = T(n/2) + O(1)
- a=1, b=2, d=0
- log₂(1) = 0 = d → **Case 2**
- **Result: O(log n)** ✓

**Karatsuba Multiplication:** T(n) = 3T(n/2) + O(n)
- a=3, b=2, d=1
- log₂(3) ≈ 1.585 > 1 = d → **Case 1**
- **Result: O(n^(log₂3)) ≈ O(n^1.585)** ✓

**Strassen:** T(n) = 7T(n/2) + O(n²)
- a=7, b=2, d=2
- log₂(7) ≈ 2.807 > 2 = d → **Case 1**
- **Result: O(n^(log₂7)) ≈ O(n^2.807)** ✓

### Geometric Series Case (Case 3 เฉพาะ)
ถ้า work ที่ Root มากกว่า Leaves → ผลลัพธ์ถูกกำหนดโดย Root เสมอ
- T(n) = 2T(n/2) + O(n²) → **O(n²)** (Case 3)
- ความหมาย: ในแต่ละ level งานเพิ่มขึ้นเป็น 2 เท่า แต่ size ลดลงแค่ครึ่ง → level บนสุด dominates

---

## Chapter 5: QuickSort (หน้า 137-178)

### 5.1 Overview
QuickSort คือ **practically ดีที่สุด** sorting algorithm:
- Average case: O(n log n)
- Worst case: O(n²)
- In-place (ไม่ต้องใช้ extra memory)
- Cache-friendly
-常数 factor เล็ก

### 5.2 Partition Subroutine
**Partition** คือหัวใจของ QuickSort:
1. เลือก pivot element (จาก array)
2. **Reorder** array ให้ elements < pivot อยู่ซ้าย, elements > pivot อยู่ขวา
3. **Place pivot** ในตำแหน่งสุดท้ายของ "left region"
4. Return index ของ pivot

**Partition Algorithm (Lomuto scheme):**
- ใช้ pointer i ติดตาม boundary ของ left region
- ไล่ j จาก left ไป right
- ถ้า A[j] ≤ pivot → swap A[i+1] กับ A[j], increment i
- สุดท้าย swap pivot กับ A[i+1]
- **Works in O(n)** time — single pass

**Hoare scheme** (ดีกว่าเล็กน้อย): ใช้ two pointers จากสองข้างเข้าหากัน

### 5.3 Choosing a Good Pivot
**Pivot สำคัญมาก:**
- **Bad pivot (min/max):** ทำให้เกิด unbalanced partition → O(n²)
- **Good pivot (median):** ทำให้เกิด balanced partition → O(n log n)

**Randomized QuickSort:**
- เลือก pivot **randomly** จาก array
- ไม่มี input ที่ทำให้ worst case เสมอ (ในทาง probability)
- Expected running time: **O(n log n)**

### 5.4 Analysis of QuickSort

**Analysis ผ่าน Indicator Random Variables:**
- กำหนด X_ij = 1 ถ้า element i และ j เทียบกันใน partition
- ความน่าจะเป็นที่ i และ j เทียบกัน = 2/(j-i+1)
- Expected comparisons = Σ Σ 2/(j-i+1) = O(n log n)

**Lower Bound: Ω(n log n) สำหรับ Comparison-Based Sorting**
- **Decision Tree Model:** comparison-based sorting algorithm คือ decision tree
- Tree มี n! leaves (หนึ่งตัวต่อ permutation)
- Binary tree ที่มี n! leaves ต้องมี depth ≥ log₂(n!) = Ω(n log n)
- **ความหมาย:** ไม่มี comparison-based sorting algorithm  nào ที่เร็วกว่า O(n log n) ใน worst case

**MergeSort vs QuickSort:**
- MergeSort: O(n log n) guaranteed, ต้องใช้ O(n) extra space
- QuickSort: O(n log n) average, O(n²) worst case, in-place
- **Practically QuickSort ดีกว่า** เนื่องจาก cache performance และ constant factors

---

## Chapter 6: Linear-Time Selection (หน้า 179-216)

### 6.1 The Selection Problem
**ปัญหา:** หา k-th smallest element ใน array (k-th order statistic)

**Applications:**
- Median finding (k = n/2)
- Percentiles, quantiles
- Sorting (k ตัวแรก)

### 6.2 QuickSelect (Randomized)
**QuickSelect** ใช้ Partition idea เหมือน QuickSort:
1. Partition array (เหมือน QuickSort)
2. ถ้า pivot อยู่ที่ index k → return
3. ถ้า k < pivot index → recurse ซ้าย
4. ถ้า k > pivot index → recurse ขวา

**Average case: O(n)** — เร็วกว่า sorting ทั้งหมด (ไม่ต้อง sort ทุกตัว)
**Worst case: O(n²)** — ถ้า pivot แย่ทุกครั้ง

### 6.3 DSelect — Deterministic Linear-Time Selection
**DSelect** รับประกัน O(n) ในทุก case โดยใช้ pivot ที่ดี:

**Median-of-Medians Algorithm:**
1. แบ่ง array เป็น groups ขนาด 5 (last group อาจมี < 5)
2. หา median ของแต่ละ group (using sorting/inspection — O(1) per group)
3. **Recurse** เพื่อหา median ของ medians (n/5 elements)
4. ใช้ median-of-medians เป็น pivot
5. Partition และ recurse เหมือน QuickSelect

**Why groups of 5?**
- ถ้ากลุ่มใหญ่กว่า 5 (เช่น 7) → จำนวน recursive calls เพิ่ม → constant factor แย่
- ถ้ากลุ่มเล็กกว่า 5 (เช่น 3) → guaranteed split ไม่ดีพอ (60-40 แทน 70-30)
- **5 เป็นจุดสมดุลที่ดีที่สุด**

**Guaranteed Split:**
- median-of-medians เป็น pivot → guarantee ว่าอย่างน้อย 30% ของ elements น้อยกว่า pivot และ 30% มากกว่า
- ทำให้ recursive call ลดลงอย่างน้อย 30% ทุกครั้ง

**Running Time:**
- T(n) = T(n/5) + T(7n/10) + O(n)
- T(n/5) = สำหรับหา median-of-medians
- T(7n/10) = สำหรับ recurse บน partition ที่ใหญ่สุด (worst case 70%)
- **Result: O(n)** — linear!

### Comparison
| Algorithm | Average | Worst | Space | Notes |
|-----------|---------|-------|-------|-------|
| QuickSelect | O(n) | O(n²) | O(1) | Fast in practice |
| DSelect | O(n) | O(n) | O(1) | Deterministic, higher constant |
| Sort + Index | O(n log n) | O(n log n) | O(n) | Simple but slower |

---

## Problem Set Summary

### Problem Set 1 (Chapter 1-2)
- Karatsuba variations: เปลี่ยน number of subproblems → find new recurrence
- Analyze algorithms using Big-O notation
- Count inversions in specific inputs

### Problem Set 2 (Chapter 3-4)
- MergeSort with 3-way split: T(n) = 3T(n/3) + O(n) → O(n log₃n) = O(n log n)
- Polynomial multiplication via Divide-and-Conquer
- Closest Pair variations

### Problem Set 3 (Chapter 4)
- Master Method applications:
  - T(n) = 9T(n/3) + O(n) → Case 1: O(n²)
  - T(n) = 3T(n/9) + √n → Case 2: O(√n · log n)
  - T(n) = T(2n/3) + O(1) → Case 2: O(log n)
  - T(n) = 3T(n/4) + n log n → Case 3: O(n log n)
  - T(n) = 2T(n/2) + n log n → Case 2: O(n log²n)

### Problem Set 4-6 (Chapter 5-6)
- QuickSort variations and pivot strategies
- Lower bound proofs
- Selection algorithm analysis

---

## สรุป Key Takeaways

1. **Algorithm Analysis สำคัญ** — ไม่ใช่แค่เข้าใจว่า algorithm ทำงานอย่างไร แต่ต้องวิเคราะห์ได้ว่าเร็วแค่ไหน
2. **Divide-and-Conquer** เป็น paradigm ที่ทรงพลัง — ลดปัญหาใหญ่เป็นปัญหาย่อย (MergeSort, QuickSort, Karatsuba, Strassen)
3. **Master Method** เครื่องมือวิเคราะห์ recurrence ที่ง่ายและ powerful — จำแค่ 3 cases
4. **Big-O notation** คือภาษากลางสำหรับเปรียบเทียบ algorithm — ต้องเข้าใจ O, Ω, Θ
5. **QuickSort** 是 practically fastest sorting algorithm — O(n log n) average, in-place
6. **Linear-Time Selection** — หา median ใน O(n) ได้จริงด้วย median-of-medians
7. **Lower bound** — comparison-based sorting ต้อง Ω(n log n) อย่างน้อย

---
