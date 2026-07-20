# สรุป: Building Winning Trading Systems with TradeStation — The Bollinger Bandit Trading Strategy

> **Source:** Building Winning Trading Systems with TradeStation, Chapter 6
> **ผู้เขียน:** George Pruitt

---

## ภาพรวมของระบบ Bollinger Bandit

ระบบ Bollinger Bandit เป็น **channel breakout system** ระยะยาว ที่ใช้ Bollinger Bands (created by John Bollinger, 1960s) เป็นแกนหลัก แทนที่จะเทรด mean reversion (ซื้อเมื่อแตะ lower band, ขายเมื่อแตะ upper band) ระบบใช้ **breakout** — ซื้อเมื่อราคาทะลุ upper band, ขายชอร์ตเมื่อราคาหลุด lower band

### หลักการ Bollinger Bands
- **Middle line**: Simple Moving Average (SMA) ของราคาปิด
- **Upper band**: SMA + n × standard deviation
- **Lower band**: SMA − n × standard deviation
- 2 standard deviation ≈ 95% confidence level — ราคาอยู่ในช่วงนี้ 95% ของเวลา

---

## Entry Signal

### เงื่อนไขเข้า Long ( BUY )
1. ราคาปิดวันนี้ > ราคาปิด 30 วันก่อน (Rate of Change > 0) — **trend filter**
2. ราคาตลาดวันนี้ >= Upper Band (SMA50 + 1.25 × StdDev50)
3. เข้า BUY ที่ราคา stop order ของวันถัดไป

### เงื่อนไขเข้า Short ( SELL )
1. ราคาปิดวันนี้ < ราคาปิด 30 วันก่อน (Rate of Change < 0)
2. ราคาตลาดวันนี้ <= Lower Band (SMA50 − 1.25 × StdDev50)
3. เข้า SELL ที่ราคา stop order ของวันถัดไป

### Trend Filter
- ใช้ Rate of Change (ROC) 30 วันเป็นตัวกรอง — ต้องอยู่ในแนวโน้มที่ถูกต้องก่อนเข้าเทรด
- ป้องกันการเข้า Long ในตลาดขาลง และ Short ในตลาดขาขึ้น

---

## Exit Strategy — Trailing Stop แบบ Dynamic

จุดเด่นของ Bollinger Bandit คือ **trailing stop ที่ปรับตามเวลา** ที่ถือครอง position:

### กลไกการออก
1. **เมื่อเปิด position ใหม่**: protective stop ตั้งที่ SMA50 (liqLength = 50 วัน)
2. **ทุกวันที่ถือ position**: ลด liqLength ลง 1 วัน → moving average สั้นลงเรื่อยๆ → ตามราคาใกล้ขึ้น → ออกง่ายขึ้น
3. **Floor ที่ 10 วัน**: เมื่อ liqLength ลดถึง 10 จะไม่ลดอีก (ป้องกัน whipsaw)
4. **Exit condition**:
   - Long: ออกเมื่อ SMA(liqDays) < Upper Band
   - Short: ออกเมื่อ SMA(liqDays) > Lower Band
5. **เมื่อถูก stop out**: reset liqLength กลับเป็น 50

### เหตุผลที่ใช้ Trailing Stop แบบนี้
- observation จาก King Keltner system: ให้กำไรก้อนใหญ่กลับคืนมากเมื่อรอ exit ที่ SMA ปกติ
- Trailing stop ทำให้ risk ลดลงตามเวลาที่ถือ position — เพราะ shorter-term MA ตามราคาได้ใกล้กว่า long-term MA

---

## Pseudocode สรุป

```
liqDay = 50 (เริ่มต้น)
upBand = SMA(Close, 50) + StdDev(Close, 50) × 1.25
dnBand = SMA(Close, 50) - StdDev(Close, 50) × 1.25
rocCalc = Close วันนี้ - Close 30 วันก่อน

ถ้า rocCalc > 0 และ market >= upBand → BUY ที่ upBand
ถ้า rocCalc < 0 และ market <= dnBand → SELL ที่ dnBand

ถ้า long และ SMA(liqDays) < upBand → ออก Long
ถ้า short และ SMA(liqDays) > dnBand → ออก Short

ทุกวันที่ไม่ถูก stop: liqDays = liqDays - 1 (min = 10)
ถูก stop out: liqDays = 50 (reset)
```

---

## TradeStation Program 要点

- ใช้ฟังก์ชัน `BollingerBand()` — ส่ง 3 parameters: (1) price series, (2) sample size, (3) number of deviations (ใช้ negative สำหรับ lower band)
- ใช้ `MaxList()` สำหรับหาค่า max ใน list
- liqDays เป็น counter variable ที่จัดการ exit

---

## ผลการทดสอบ (1982 – 2002)

| Market | Net Profit | Max Drawdown | Trades | Win% | Max Cons. Losses |
|--------|-----------|-------------|--------|------|-----------------|
| **Japanese Yen** | **$121,937** | $(21,462) | 180 | 37.2% | 8 |
| **Natural Gas** | **$85,897** | $(21,737) | 113 | 44.3% | 6 |
| Swiss Franc | $76,312 | $(9,987) | 188 | 41.0% | 5 |
| Deutsch Mark | $51,075 | $(13,812) | 186 | 41.4% | 6 |
| US Bonds | $48,381 | $(15,343) | 204 | 36.3% | 6 |
| Crude Oil | $47,242 | $(17,522) | 170 | 41.8% | 8 |
| British Pound | $38,750 | $(43,612) | 194 | 33.5% | 20 |
| **Total** | **$537,821** | — | 3,092 | — | — |

**ตลาดที่ขาดทุน**: Corn (-$5K), Live Cattle (-$17K), Soybeans (-$16K), Wheat (-$20K)

---

## บทเรียนสำคัญ

1. **Breakout ดีกว่า Mean Reversion สำหรับ Bollinger Bands** — ทดสอบแล้วว่าใช้ upper band เป็น resistance ให้ผลลัพธ์แย่ แต่ใช้เป็น breakout signal ได้ผลดีกว่ามาก
2. **Correlation กับ King Keltner สูง** — ระบบ Bollinger Bandit และ King Keltner ทำกำไรจากตลาดเดียวกัน ใช้ด้วยกันจะไม่ดีเพราะ hedge กันเอง
3. **Win rate ต่ำ แต่ profit สูง** — ส่วนใหญ่มี win% แค่ 30-45% แต่ profit factor สูงเพราะ win ต่อ win ใหญ่กว่า loss
4. **Trailing stop ช่วยลด drawdown ได้บ้าง** — แต่ profit increase ไม่มาก หลักการสำคัญคือ risk ควรลดลงเมื่อถือ position นานขึ้น
5. **ระบบเหมาะกับ commodity currencies และ bonds** — ตลาดที่มี trend ชัดเจน ไม่เหมาะกับ market ที่ choppy
