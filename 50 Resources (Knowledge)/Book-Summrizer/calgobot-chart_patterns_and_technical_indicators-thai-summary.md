# Chart Patterns and Technical Indicators — สรุปภาษาไทย

> สรุปจากหนังสือ **Chart Patterns and Technical Indicators** (cAlgoBot) — ครอบคลุมทุก chart pattern (รูปแบบกราฟ) ที่สำคัญ ตั้งแต่ pattern แบบต่อเนื่อง (continuation) ไปจนถึง pattern แบบกลับตัว (reversal) รวมถึง oscillator (ตัวชี้วัดการสั่นไหว) หลักๆ 4 ตัว

---

## ส่วนที่ 1: Triangle Patterns (รูปแบบสามเหลี่ยม)

### 1. Ascending Triangle (สามเหลี่ยมขาขึ้น) — Bullish Continuation

**ลักษณะ:** มี trendline บน (resistance) แบบ flat (ระดับเดิม) และ trendline ล่าง (support) ค่อยๆ สูงขึ้น เกิดเป็นมุมฉาก

**สิ่งที่ต้องรู้:**
- **Price Action (พฤติกรรมราคา):** แสดงว่าฝั่ง buyer แข็งแกร่งกว่า seller เพราะราคา bounce ขึ้นมา碰 resistance เดิมซ้ำๆ แต่จุดต่ำสุดค่อยๆ สูงขึ้น (rising lows)
- **Breakout (การ breakout):** ควรเกิด breakout ก่อนที่ราคาจะถึง apex (จุดบรรจบของ trendline) — ช่วงที่เหมาะสมคือ 2/3 ถึง 3/4 ของแนวนอนของ pattern
- **Volume (ปริมาณซื้อขาย):** ควรสูงตอน bounce ขึ้น และเบาตอน dips ลง — เมื่อ breakout เกิด volume ต้อง surge (พุ่งขึ้น)
- **วัดเป้าหมายราคา:** วัดความสูงของ triangle จากฐานถึงด้านบน แล้วเอาไปบวกกับราคาที่ breakout (สำหรับ upside breakout)
- **False Breakout (false move):** ถ้า breakout ไม่มี volume surge ให้สงสัย — รอสัก 2-3 วันเพื่อยืนยัน เพราะ false move มักแก้ตัวเองภายใน 1 สัปดาห์
- **Smart Money Story:** ฝั่ง "smart money" (นักลงทุนที่ซื้อถูกตั้งแต่แรก) จะขายที่ราคาสูง สร้าง resistance ซ้ำๆ — แต่ถ้า buyer แกร่งพอ supply จะหมด และ breakout เกิดขึ้น

**ความน่าเชื่อถือ:** Ascending triangle เป็น pattern ที่เชื่อถือได้มากที่สุดตัวหนึ่ง เพราะ supply และ demand ชัดเจน

---

### 2. Descending Triangle (สามเหลี่ยมขาลง) — Bearish Continuation

**ลักษณะ:** mirror ของ ascending triangle — trendline ล่าง (support) แบบ flat และ trendline บน (resistance) ค่อยๆ ต่ำลง

**สิ่งที่ต้องรู้:**
- **Price Action:** แสดงว่า seller แข็งแกร่งกว่า buyer — จุดสูงสุดค่อยๆ ต่ำลง (lower highs) แต่ราคาหยุดที่ระดับเดิมซ้ำๆ
- **Volume:** สูงตอน dips, เบาตอน bounces — ตรงข้ามกับ ascending triangle
- **Forecasting (การทำนาย):** โดยทั่วไปถือว่า bearish — แต่ Bulkowski เตือนว่าแค่ 55% ของ descending triangle ที่เกิดขึ้นจริงจะเป็นขาลง ถ้ารอ breakout ยืนยันแล้ว อัตราสำเร็จเพิ่มเป็น 96%
- **Smart Money Story:** เมื่อหุ้นขาลงจากข่าวร้าย smart money จะซื้อที่ราคาที่觉得自己ว่า fair value สร้าง bottom ซ้ำๆ — แต่ถ้า demand หมด stock ก็จะ breakout ลง
- **วัดเป้าหมาย:** เหมือน ascending — วัดความสูงจากฐานแล้วหักลบจากราคา breakout

---

### 3. Symmetrical Triangle (สามเหลี่ยมสมมาตร) — Neutral / Continuation

**ลักษณะ:** trendline บนและล่างเบนเข้าหากัน (converging) — ราคาแกว่งในช่วงแคบลงเรื่อยๆ เหมือนสปริงที่ถูกบีบ

**สิ่งที่ต้องรู้:**
- **Price Action:** เป็น neutral pattern — ราคาค่อยๆ แคบลง (lower highs + higher lows) แสดงว่า trader กำลังหา consensus (ความเห็นตรงกัน) เรื่องราคา
- **Breakout:** -direction ไม่แน่นอน — ต้องรอ breakout ก่อนค่อยตัดสินใจ ห้ามเดา
- **Volume:** ควรค่อยๆ ลดลงตาม pattern แล้ว surge ตอน breakout
- **Symmetrical triangle เกิดหลัง:** ราคา moved strongly ไปก่อน — แล้ว momentum แผ่วลง จนเกิด consolidation แล้ว breakout ด้วยข่าวใหม่
- **False Breakout:** Triangle เป็น pattern ที่ susceptible กับ false breakout มาก — เพราะ volume เบาลงเรื่อยๆ เลยต้องการ activity นิดเดียวก็หลุด trendline ได้

**ความน่าเชื่อถือ:** หลัง breakout เกิดแล้ว ถือว่าค่อนข้าง reliable

---

## ส่วนที่ 2: Continuation Patterns อื่นๆ

### 4. Rectangle (สี่เหลี่ยมผืนผ้า) — Continuation

**ลักษณะ:** ราคาแกว่งระหว่าง resistance ด้านบนและ support ด้านล่างแบบ horizontal (ระดับเดิม) — ดูเหมือน double top แต่ rectangle เป็น continuation ไม่ใช่ reversal

**สิ่งที่ต้องรู้:**
- **Duration (ระยะเวลา):** ปกติ 4-6 สัปดาห์ ถ้าต่ำกว่า 3 สัปดาห์จะเป็น flag ( ธง) แทน ถ้า 3 เดือนจะ reliable มาก ถ้า 6 เดือนจะ breakout แรงกว่า target
- **4 Points สำคัญ:** ต้องมีอย่างน้อย 2 reaction high (จุดสูง) และ 2 reaction low (จุดต่ำ) — ราคาแกว่งไปมาระหว่าง 2 เส้นนี้
- **Volume:** ไม่มี pattern ชัดเจน — บางทีเบาลง บางทีแกว่ง ให้ดูว่า rally หรือ dip ไหน volume สูงกว่า เป็นเบาะแสว่า breakout จะไปทางไหน
- **Breakout Confirmation:** ต้อง breakout แบบ closing price (ราคาปิด) — บาง trader ใช้ filter: +3% จากราคา, 3 วัน, หรือ volume expansion
- **Return to Breakout:** หลัง breakout มักมีการ pullback กลับมาทดสอบ breakout level — broken support กลายเป็น resistance และ vice versa

---

### 5. Rising Wedge (ลิ่มขาขึ้น)

**ใน downtrend (ขาลง) — Bearish Continuation:**
- ราคา bounce ขึ้น形成 higher highs + higher lows แต่ volume ค่อยๆ ลดลง
- ทุก rally อ่อนแอกว่าครั้งก่อน — แสดงว่า "ขาขึ้น" นี้ไม่จริง (distribution กำลังเกิด)
- Breakout ลงมักเกิดบน high volume → ราคาตกแรง

**ใน uptrend (ขาขึ้น) — Bearish Reversal:**
- ราคาดูเหมือนจะดี (higher highs/lows) แต่ volume ค่อยๆ ลดลง — smart money กำลัง distribute (ขาย) หุ้นให้ retail investor
- Wall Street analyst ยัง bullish แต่จริงๆ แล้วมืออาชีพกำลังขาย
- Breakout ลงมาแรงเพราะ supply/demand ผิดสมดุล

**เป้าหมายราคา:** วัดความสูงของ wedge แล้วหักลบจากราคา breakout

---

### 6. Falling Wedge (ลิ่มขาลง) — Bullish Reversal/Continuation

**ลักษณะ:** ราคา making lower highs + lower lows แต่ range ค่อยๆ แคบลง (cone shape ลง)

**สิ่งที่ต้องรู้:**
- **Volume:** ต้อง surge ตอน breakout — ถ้าไม่มี volume expansion breakout จะไม่น่าเชื่อถือ
- ** reversal vs continuation:** ใน downtrend เป็น reversal (กลับตัวขึ้น) เสมอ ใน uptrend อาจเป็น continuation
- **Deception:** ดูเหมือนราคาจะลงต่อ แต่จริงๆ selling pressure กำลังหมด — ตัวที่ short หุ้นอยู่จะ panic เมื่อ breakout ขึ้น
- **Implication จำกัด:** Falling wedge มักเป็นแค่จุดเริ่มต้นของ reversal ที่ใหญ่กว่า — target จึงไม่ไกลมาก

---

### 7. Cup and Handle (ถ้วยและมือจับ) — Bullish Continuation

**ลักษณะ:** ราคา forming U-shape (cup) แล้ว pullback เล็กน้อย (handle) ก่อน breakout ขึ้น

**สิ่งที่ต้องรู้:**
- **Duration:** Cup ใช้เวลา 1-6 เดือน, Handle ใช้ 1-4 สัปดาห์ — ทั้งหมด 9-16 สัปดาห์
- **Depth:** Cup ควร retracement ไม่เกิน 1/3 ของ rally ก่อนหน้า (สูงสุด 2/3 ใน extreme case)
- **Handle:** ควร pullback ไม่เกิน 1/3 ของ cup's advance — handle ยิ่งเล็ก ยิ่ง bullish
- **Volume:** ต้อง surge ตอน breakout จาก handle — ยืนยันว่า selling หมดแล้ว
- **เป้าหมาย:** วัดจากขวา cup ถึงก้น cup แล้วบวกเข้ากับ breakout level
- **Difference จาก Double Top:** Cup and Handle เหมือน double top แต่ sell ไม่ accelerate หลัง top ที่สอง — กลับ consolidate แล้ว breakout ขึ้น

---

## ส่วนที่ 3: Reversal Patterns (รูปแบบกลับตัว)

### 8. Double Bottom (ก้นสองยอด) — Bullish Reversal

**ลักษณะ:** ราคา forming 2 lows (จุดต่ำสุด) ที่ระดับใกล้เคียงกัน — mirror ของ double top

**สิ่งที่ต้องรู้:**
- **Prior Trend:** ต้องมี downtrend ก่อน — ถ้า downtrend สั้น double bottom อาจไม่ hold
- **Time between Bottoms:** ยิ่งห่างกันมาก ยิ่ง可靠 — ควรมีอย่างน้อย 1 เดือน
- **Increase from First Low:** ราคา bounce ขึ้นระหว่าง bottom ที่ 1 และ 2 ควรประมาณ 10-20%
- **Volume:** สูงสุดตอน bottom ที่ 1 (value investor ลงสนาม) — เบาลงตอน bottom ที่ 2 แล้ว surge ตอน breakout
- **Decisive Breakout:** ต้อง breakout ผ่าน "reaction high" (จุดสูงระหว่าง 2 bottoms) ด้วย high volume
- **Pullback:** Bulkowski ประมาณว่า 68% ของ double bottom มี pullback กลับมาทดสอบ breakout level
- **Failure Rate:** 64% ถ้าไม่รอ breakout (สูงมาก!) — ลดเหลือ 3% ถ้ารอ breakout ยืนยัน ✅

**Smart Money Story:** Value investor ค่อยๆ accumulate (สะสม) หุ้นตอนราคาต่ำ — short seller ค่อยๆ cover — breakout เกิดเมื่อ supply หมด

---

### 9. Double Top (ยอดสองยอด) — Bearish Reversal

**ลักษณะ:** ราคา forming 2 peaks (จุดสูงสุด) ที่ระดับใกล้เคียงกัน — มักเรียกว่า "M pattern"

**สิ่งที่ต้องรู้:**
- **Uptrend Required:** ต้องมี uptrend ที่แข็งแกร่งพอ — uptrend สั้นอาจไม่พอ
- **Time between Tops:** ควรมีอย่างน้อย 1 เดือน — ยิ่งห่างกัน ยิ่ง可靠
- **Decline from First Top:** Bulkowski ให้ weight มากกับข้อนี้ — decline ควรประมาณ 20% ยิ่งลึกยิ่งดี
- **Volume:** สูงสุดตอน peak ที่ 1 — เบากว่าตอน peak ที่ 2 — แล้ว surge ตอน breakout ลง
- **Decisive Breakout:** ต้อง breakout ลงผ่าน reaction low — รอ high volume confirm
- **Pullback:** มัก pullback กลับมา — ยิ่ง breakout volume สูง ยิ่งมีโอกาส pullback
- **Failure Rate:** 65% ถ้าไม่รอ breakout — ลดเหลือ 17% ถ้ารอ breakout ✅

**Distribution Story:** Smart money ขายหุ้นเข้ากับข่าวดี (earnings, analyst upgrade) — สร้าง top ซ้ำๆ — แล้ว breakout ลงเมื่อ supply > demand

---

### 10. Head & Shoulders Bottom (หัวไหล่กลับหัว) — Bullish Reversal

**ลักษณะ:** 3 declines — left shoulder, head (ต่ำสุด), right shoulder — มี neckline (เส้นเชื่อม 2 highs)

**สิ่งที่ต้องรู้:**
- **Symmetry:** Left และ right shoulder ควรมีระยะเวลาใกล้เคียงกัน — ถ้า right shoulder ยาวเกินไปให้ระวัง
- **Volume Sequence:** สูงสุดตอน left shoulder → ต่ำสุดตอน right shoulder → surge ตอน breakout ผ่าน neckline
- **Neckline:** เส้นเชื่อม 2 reaction high — มักจะเบนลง (downward slope) แต่ upward slope ก็มี
- **Duration:** Bottoming pattern มักใช้เวลานานกว่า top — ราคาแกว่งน้อยกว่า
- **Decisive Neckline Break:** Pattern ไม่ complete จนกว่าราคา breakout ผ่าน neckline ด้วย high volume
- **After Breakout:** ราคาอาจ pullback มาทดสอบ neckline (broken resistance → become support)

---

### 11. Head & Shoulders Top — Bearish Reversal

**ลักษณะ:** 3 rallies — left shoulder, head (สูงสุด), right shoulder — มี neckline

**สิ่งที่ต้องรู้:**
- **Symmetry:** ซ้ายและขวาควรมีระยะเวลาใกล้เคียงกัน — สำคัญมากสำหรับ reliability
- **Volume:** ควรสูงสุดตอน left shoulder → ลดลงเรื่อยๆ → ต่ำสุดตอน right shoulder → surge ตอน breakout ลง
- **Neckline:** เชื่อม 2 reaction low — ทิศทาง slope ทำนายความแรงของการ decline:
  - Upward slope = bullish มากกว่า (decline ไม่แรง)
  - Downward slope = bearish มากกว่า (decline แรง)
- **Drooping Shoulder:** ถ้า right shoulder ต่ำกว่า left มาก = สัญญาณ technical แย่มาก
- **Duration:** อย่างน้อย 3 เดือน ถึง breakout — ถ้าถึง 6 เดือน = pattern ใหญ่มาก
- **False Pattern:** ถ้า neckline ไม่ breakout → ราคาอาจขึ้นต่อ — pattern fail

**Smart Money Story:** Distribution ซ้ำๆ — smart money ขายเข้ากับข่าวดี — analyst ยัง bullish แต่ supply กำลังล้น

---

### 12. Rounding Bottom (ฐานกลม) — Long-term Bullish Reversal

**ลักษณะ:** ราคา forming semi-circle (ครึ่งวงกลม) — คล้าย head & shoulders bottom แต่ไม่มี shoulder ชัดเจน

**สิ่งที่ต้องรู้:**
- **Duration:** ยาวมาก — เหมาะกับ weekly chart
- **Volume Pattern:** สูงตอน decline → ต่ำสุดตอน bottom → ค่อยๆ สูงขึ้นตอน advance — volume ควร "track" รูปทรงของ rounding bottom
- **Breakout:** Confirm เมื่อ breakout ผ่าน reaction high (จุดเริ่มต้นของ decline)
- **Symmetry:** ด้านซ้ายและขวาไม่ต้องเท่ากัน แต่ capturing "essence" ของ pattern สำคัญกว่า

---

### 13. Rounding Top (ยอดกลม) — Bearish Reversal

**ลักษณะ:** mirror ของ rounding bottom — ราคา forming dome shape

**สิ่งที่ต้องรู้:**
- **Volume:** ควรลดลงทุกครั้งที่ rally — แสดงว่า distribution กำลังเกิด
- **Multiple Shoulders:** ต่างจาก head & shoulders top — rounding top มักมี "shoulders" หลายตัว
- **Deception:** ข่าวดีเรื่อยๆ แต่ราคากลับไม่ขึ้น — smart money กำลัง distribute

---

### 14. Triple Bottom (ฐานสามยอด) — Bullish Reversal

**ลักษณะ:** 3 lows ที่ระดับใกล้เคียงกัน — เหมือน double bottom แต่ซ้ำ 3 ครั้ง

**สิ่งที่ต้องรู้:**
- **Duration:** ประมาณ 4 เดือน — เป็น pattern ที่ใช้เวลานาน
- **Volume:** ควรลดลงเรื่อยๆ แล้ว surge ตอน breakout — การที่ volume สูงที่ bottom แสดงว่า accumulation
- **Pullback:** Bulkowski ประมาณว่า 70% มี pullback กลับมาทดสอบ breakout level
- **Breakout:** ต้อง breakout ผ่าน peak ที่เกิดระหว่าง bottom #2 และ #3

---

### 15. Triple Top (ยอดสามยอด) — Bearish Reversal

**ลักษณะ:** 3 peaks ที่ระดับใกล้เคียงกัน — เหมือน double top แต่ซ้ำ 3 ครั้ง

**สิ่งที่ต้องรู้:**
- **Distribution 3 รอบ:** Smart money ขาย 3 ครั้ง — ทุกครั้ง buyer ผิดหวังซ้ำซาก
- **Volume:** ควรลดลงตอน rally ไปหา top #2 และ #3 — ยืนยันว่า distribution เกิด
- **Breakout:** ต้อง breakout ลงผ่าน lows ระหว่าง top #2 และ #3
- **Hybrid Variation:** บางครั้งมี "fourth peak" ก่อน breakout เกิด — ยังเป็น valid pattern

---

### 16. One Day Reversal (กลับตัววันเดียว)

**ลักษณะ:** ราคากapping up (เปิดสูง) ทำ new high ด้วย volume สูงมาก แต่ปิดต่ำกว่า — ทั้งวันเป็นขาลง

**สิ่งที่ต้องรู้:**
- **เกิดเพราะ:** Large investor ต้องการ liquidity ปิด long position — ขายเข้ากับข่าวดี (วันที่ liquidity สูงสุด)
- **Volume:** ต้อง accelerate ตอนราคาลง
- **Close สำคัญ:** ต้องปิดใกล้ session low — ถ้าปิดกลางวัน = ไม่ค่อยน่าเชื่อถือ
- **เป็นจุดเริ่มต้น:** ทุก major reversal pattern เริ่มจาก one day reversal

---

### 17. Island Reversal (กลับตัวแบบเกาะ)

**ลักษณะ:** ราคากapping ขึ้น → trade หลายวันในช่วงใหม่ → gapping ลง → ราคากลับมาช่วงเดิม — ช่วงที่ trade อยู่ดูเหมือน "เกาะ" ที่ถูก gap ทั้งบนและล่าง

**สิ่งที่ต้องรู้:**
- **News-driven:** มักเกิดจากข่าวขัดแย้งในช่วงสั้นๆ — breakout ด้วยข่าวดี แล้ว fail ด้วยข่าวร้าย
- **Volume:** ต้อง surge ทั้งตอน breakout และตอน fail
- **Impact:** เป็น "trend killer" — มักทำลาย trend แล้วตามด้วย pattern ขนาดใหญ่

---

### 18. Diamond Top (เพชร) — Bearish Reversal

**ลักษณะ:** เหมือน head & shoulders แต่ neckline โค้ง (broadening) → ราคากลับมา narrow → breakout ลง

**สิ่งที่ต้องรู้:**
- **Large Pattern:** Diamond เป็น pattern ขนาดใหญ่มาก — technical implication มักจะแรงมาก
- **Complex:** Volume มักบอก accumulation แต่จริงๆ เป็น distribution — ต้องดู price action ประกอบ
- **Wait for Completion:** ต้องรอ breakout ลงก่อนค่อย short — ห้ามเข้าเร็ว

---

### 19. Broadening Top (หูช้าง) — Bearish Reversal

**ลักษณะ:** ราคา making higher highs + lower lows — ขยายออกเรื่อยๆ เหมือนหูช้าง (megaphone) — ตรงข้ามกับ triangle ที่แคบลง

**สิ่งที่ต้องรู้:**
- **Increasing Volatility:** ยิ่งเวลาผ่านไป ยิ่ง volatile — ตรงกันข้ามกับ pattern อื่นๆ
- **Volume Increases:** Volume สูงขึ้นตามราคา — ดูเหมือน bullish แต่ rally สั้นมาก
- **Only Tops:** Broadening top เกิดเฉพาะตอนบน (topping) — เพราะเกิดจาก expectation ที่ไม่ realistic ของ bullish investor
- **Target ใหญ่มาก:** Pattern ใหญ่ → technical implication ใหญ่

---

## ส่วนที่ 4: Technical Indicators (ตัวชี้วัดเทคนิค)

### 20. DMI (Directional Movement Indicator)

**คืออะไร:** พัฒนาโดย Welles Wilder — ใช้บอกว่าหุ้นกำลัง "trending" หรือไม่

**วิธีใช้:**
- **+DI line:** วัดการเคลื่อนไหวขาขึ้น
- **-DI line:** วัดการเคลื่อนไหวขาลง
- **Buy Signal:** +DI ตัดขึ้นเหนือ -DI
- **Sell Signal:** +DI ตัดลงต่ำกว่า -DI
- **Extreme Point Rule:** เมื่อเกิด crossover ให้ใช้ extreme price (high สำหรับ short, low สำหรับ long) เป็นจุด reverse

**ใช้เป็น:** System เดี่ยว หรือ filter สำหรับ trend-following system

---

### 21. MACD (Moving Average Convergence/Divergence)

**คืออะไร:** Momentum oscillator (ตัวชี้วัดโมเมนตัม) — แสดงความสัมพันธ์ระหว่าง 2 moving averages

**วิธีคำนวณ:** EMA 26 วัน - EMA 12 วัน = MACD line, signal line = EMA 9 วัน ของ MACD

**3 วิธีใช้หลัก:**

1. **Crossovers (จุดตัด):**
   - Buy: MACD ตัดขึ้นเหนือ signal line
   - Sell: MACD ตัดลงต่ำกว่า signal line
   - Buy/Sell: ตอน MACD ข้าม/ตกผ่าน zero line

2. **Overbought/Oversold:**
   - ถ้า MACD ห่างจาก moving average มากเกินไป → ราคา likely overextended → จะกลับมาสู่ระดับปกติ

3. **Divergence (ความแตกต่าง):**
   - **Bearish divergence:** MACD ทำ new lows แต่ราคาไม่ → สัญญาณขาลง
   - **Bullish divergence:** MACD ทำ new highs แต่ราคาไม่ → สัญญาณขาขึ้น
   - สำคัญที่สุดเมื่อเกิดที่ overbought/oversold levels

**MACD Histogram:** แสดงความแตกต่างระหว่าง MACD กับ signal line — signal เร็วกว่า MACD ปกติ

---

### 22. Stochastic Momentum Index (SMI)

**คืออะไร:** พัฒนาโดย William Blau — เหมือน Stochastic Oscillator แต่แสดงว่าราคาปิดอยู่ห่างจาก midpoint ของ high/low range เท่าไหร่

**วิธีใช้:**
- Buy: SMI ตกต่ำกว่า -40 แล้วกลับขึ้นเหนือ
- Sell: SMI ขึ้นสูงกว่า +40 แล้วตกลงมาต่ำกว่า
- Buy: SMI ขึ้นเหนือ moving average
- Sell: SMI ตกต่ำกว่า moving average
- **Range:** -100 ถึง +100

**Slow Stochastic:** Smoothed version — ใช้ 5-day MA ของ 12-day stochastic
- Oversold: < 20
- Overbought: > 80

**ข้อแนะนำ:** ใช้ Stochastic ได้ดีใน trading range (sideways market) — ไม่เหมาะกับ trending market ที่ไม่มี correction

---

### 23. RSI (Relative Strength Index)

**คืออะไร:** Oscillator (ตัวชี้วัดสั่นไหว) ที่เปรียบเทียบ magnitude ของ gains กับ losses — คิดค้นโดย Welles Wilder

**วิธีคำนวณ:** `100 - [100 / {1 + (U/D)}]` โดย U = average upward change, D = average downward change

**วิธีใช้หลัก:**
- **Overbought/Oversold:**
  - Overbought: RSI ≥ 70 → สัญญาณขาลง (ถ้า RSI ตกต่ำกว่า 70)
  - Oversold: RSI ≤ 30 → สัญญาณขาขึ้น (ถ้า RSI ขึ้นเหนือ 30)
- **Divergence:** ราคาทำ new high แต่ RSI ไม่ตาม → reversal กำลังมา
- **Failure Swing:** RSI ไม่สามารถข้าม peak ก่อนหน้าได้ → confirmation ของ reversal

---

## กฎเหล็กของ Chart Patterns (ทุก pattern)

1. **รอ Breakout ยืนยันเสมอ** — ห้ามเข้าก่อน breakout เกิด
2. **Volume ต้อง confirm** — Breakout ที่ไม่มี volume surge มักเป็น false breakout
3. **False Breakout Rule:** ถ้าราคา breakout แล้วกลับเข้ามาใน pattern ภายในไม่กี่วัน → pattern ใช้ไม่ได้
4. **Return to Breakout Level:** Broken support → become resistance, broken resistance → become support — ราคามัก pullback กลับมาทดสอบ level นี้
5. **Target เป็นแค่ Guidepost** — ทุก target เป็นแค่เป้าหมาย ไม่ใช่ guarantee
6. **Symmetry สำคัญ** — Pattern ที่สมมาตรมัก reliable กว่า

---

## สรุป Pattern Classification

| Pattern | Type | Bias |
|---------|------|------|
| Ascending Triangle | Continuation | Bullish |
| Descending Triangle | Continuation | Bearish |
| Symmetrical Triangle | Continuation/Reversal | Neutral |
| Rectangle | Continuation | Neutral |
| Rising Wedge (downtrend) | Continuation | Bearish |
| Rising Wedge (uptrend) | Reversal | Bearish |
| Falling Wedge | Reversal/Continuation | Bullish |
| Cup and Handle | Continuation | Bullish |
| Double Bottom | Reversal | Bullish |
| Double Top | Reversal | Bearish |
| H&S Bottom | Reversal | Bullish |
| H&S Top | Reversal | Bearish |
| Rounding Bottom | Reversal | Bullish |
| Rounding Top | Reversal | Bearish |
| Triple Bottom | Reversal | Bullish |
| Triple Top | Reversal | Bearish |
| One Day Reversal | Reversal | Depends |
| Island Reversal | Reversal | Depends |
| Diamond Top | Reversal | Bearish |
| Broadening Top | Reversal | Bearish |

---

*สรุปจาก: Chart Patterns and Technical Indicators — ทุก chapter, 15 chunks*
