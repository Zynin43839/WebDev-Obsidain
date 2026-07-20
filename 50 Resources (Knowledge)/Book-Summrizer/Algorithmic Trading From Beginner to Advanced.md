# Algorithmic Trading From Beginner to Advanced
> **Author:** Mega Backtrader Strategy Pack (ไม่ระบุชื่อผู้เขียนเดี่ยว)

---

หนังสือเล่มนี้ครอบคลุมการสร้างระบบเทรดอัตโนมัติ (algorithmic trading) ตั้งแต่พื้นฐานจนถึงกลยุทธ์ระดับสูง โดยใช้ Python และ Backtrader เป็นแกนหลักทั้งหมด 16 บท

---

## บทที่ 1 — พื้นฐานการเทรดและ finance concepts

- การซื้อขาย (trading) คือการซื้อ-ขายสินทรัพย์เพื่อทำกำไรจากราคา ตลาดมีหลายประเภท เช่น หุ้น (stocks), forex, สินค้าโภคภัณฑ์ (commodities), คริปโตเคอเรนซี (cryptocurrencies), พันธบัตร (bonds), และดัชนี (indices)
- ผู้เล่นในตลาด (market participants) แบ่งเป็น retail traders, institutional investors, market makers และ high-frequency traders (HFTs)
- ข้อมูล OHLCV (Open, High, Low, Close, Volume) คือโครงสร้างพื้นฐานของราคาทุกแท่งเทียน
- ผลตอบแทน (returns) วัดได้สองแบบ: simple return = (P_t - P_{t-1}) / P_{t-1} และ logarithmic return = ln(P_t / P_{t-1}) ซึ่งแบบ log มีความน่าสนใจกว่าเพราะบวกได้ตามเวลา (time-additive)
- ความผันผวน (volatility) วัดด้วย standard deviation (σ) ของผลตอบแทน ยิ่งสูงยิ่งเสี่ยง
- Value at Risk (VaR) ประมาณความสูญเสียสูงสุดที่อาจเกิดขึ้น เช่น VaR 95% หมายถึง loss ที่แย่ที่สุด 5% ของกรณีทั้งหมด
- ตัวชี้วัดperformance ที่สำคัญ: Win Rate, Sharpe Ratio (ผลตอบแทน risk-adjusted), Sortino Ratio (ลงโทษเฉพาะ downside), Max Drawdown (loss สูงสุดจากจุดสูงสุด), Profit Factor
- Order types หลัก: Market Order (ทันที), Limit Order (ราคาที่กำหนด), Stop Loss, Stop-Limit, Bracket Order (entry + SL + TP พร้อมกัน)
- กลยุทธ์หลัก 4 ประเภท: Trend Following, Mean Reversion, Breakout, Momentum
- Timeframes ตั้งแต่ 1m-15m (scalping) ไปจนถึง Weekly-Monthly (investing)

---

## บทที่ 2 — Python Quick Start สำหรับเทรด

- Python เหมาะสำหรับ algorithmic trading เพราะ syntax อ่านง่าย มี libraries ด้านการเงินนับพัน
- ตั้งค่าสภาพแวดล้อมได้ 2 ทาง: Spyder IDE (offline, ผ่าน Anaconda) หรือ Google Colab (online, ไม่ต้องติดตั้ง)
- พื้นฐาน Python ที่ต้องรู้: variables (เก็บข้อมูล), lists (เก็บ序列), dictionaries (key-value pairs), loops (ทำซ้ำ), functions (กลุ่มคำสั่งที่ใช้ซ้ำ)
- ติดตั้ง libraries ด้วย pip: backtrader, pandas, numpy, matplotlib, yfinance, ta-lib
- ดาวน์โหลดข้อมูลตลาดจริงด้วย yfinance: `yf.download("BTC-USD", period="1y", interval="1d")`
- คำนวณ indicators ด้วย TA-Lib: RSI, MACD, Bollinger Bands, ATR
- วาดกราฟด้วย matplotlib: Price + SMA, Price + Bollinger Bands, Subplots (Price + RSI)
- เตรียมข้อมูลให้ Backtrader: rename columns เป็น lowercase, ตั้ง datetime index

---

## บทที่ 3 — เริ่มต้นกับ Backtrader

- Backtrader มี 4 องค์ประกอบหลัก: Cerebro (engine), Strategy (logic class), Orders (buy/sell), Analyzers (performance metrics)
- Strategy class มี lifecycle สำคัญ: `__init__` (ตั้ง indicators, รันครั้งเดียว), `next` (ตัดสินใจทุกแท่งเทียน)
- เริ่มจาก Barebones (ไม่เทรด) → SMA crossover (ซื้อเมื่อ fast cross above slow) → เพิ่ม logging → เพิ่ม commission/slippage
- Order types ใน Backtrader: Market, Limit, Stop, StopLimit, Bracket (entry + SL + TP)
- ATR-based risk management: วัดความเสี่ยงเป็น % ของ equity, ขนาด position แปรผกผันกับ ATR ( volatility สูง = ขนาดเล็ก)
- Analyzers สำคัญ: SharpeRatio, DrawDown, TimeReturn, TradeAnalyzer
- Rolling backtest: ทดสอบ strategy บนหน้าต่างเวลาหลายช่วงเพื่อวัดความเสถียร across market regimes

---

## บทที่ 4 — Regime-Filtered Trend

- กลยุทธ์นี้เทรดเฉพาะตอนตลาดอยู่ในโหมด trending เท่านั้น โดยใช้ regime classifier หลายตัวชี้วัดร่วมกัน
- 3 signals สำหรับ classify regime: ADX > threshold (trend strength), Bollinger Band Width (expansion), Normalized ATR (volatility)
- ใช้ voting system: ถ้า >= 2/3 signals = trending, 0/3 = ranging, อื่นๆ = unknown
- Smooth regime changes ด้วย confirmation window (K bars) เพื่อป้องกัน flip-flopping
- Position sizing แปรผันตาม confidence: trending = full size (20-80% equity), ranging = 0, uncertain = minimal
- Trailing stop ปรับตาม regime: trending ใช้ trailing กว้าง (3× ATR), ranging ใช้แคบ (1× ATR)
- Entry: SMA crossover เท่านั้นเมื่อ regime = trending
- Exit: ทั้ง trailing stop และ signal flip (cross กลับ) หรือ regime เปลี่ยน

---

## บทที่ 5 — Relative Momentum Acceleration (Thrust Oscillator)

- หา "การเร่งตัวของ momentum" โดยเปรียบเทียบ fast EMA กับ KAMA (Kaufman's Adaptive Moving Average)
- Thrust oscillator = (EMA_fast - KAMA) / (KAMA + ε) ค่าบวกหมายถึง momentum เร่งตัวขึ้น, ค่าลบหมายถึงลดลง
- KAMA ปรับตัวอัตโนมัติ: วิ่งเร็วใน trending, ช้าใน choppy ตลาด
- Bollinger Bands บน oscillator (ไม่ใช่บนราคา): เมื่อ oscillator ทะลุ band หมายถึง momentum ผิดปกติ (statistical breakout)
- Entry: ซื้อเมื่อ oscillator > upper band, ขายเมื่อ oscillator < lower band
- Risk management: ATR trailing stop ยึดจาก highest/lowest ราคาตั้งแต่เข้า position

---

## บทที่ 6 — Ichimoku Cloud Strategy

- Ichimoku Kinko Hyo มี 5 components: Tenkan-sen (conversion), Kijun-sen (base), Senkou Span A/B (cloud), Chikou Span (lagging)
- Entry conditions ต้องผ่าน 3 filters พร้อมกัน: (1) Price breakout ออกจาก cloud, (2) Tenkan/Kijun cross ยืนยัน momentum, (3) Chikou span ยืนยัน breadth (อยู่เหนือราคาเก่าสำหรับ long)
- Cloud ปรับตัวตาม volatility; การ breakout ออกจาก cloud เป็น regime signal ที่แรงกว่า simple MA cross
- Exit: StopTrail order ที่ trail percent 2% ติดตามราคา
- Rolling backtest พร้อม ATR threshold filter: ต้องมี volatility พอที่จะ support trend ก่อนเข้าเทรด

---

## บทที่ 7 — Keltner Channel Breakout

- Keltner Channel = EMA centerline ± ATR × multiplier: ตรวจจับ range expansion (ราคาทะลุ band)
- Entry ใช้昨日 close (ไม่ใช่ today): หลีก look-ahead bias — ถ้า昨日 close > upper band = long,昨日 close < lower band = short
- Exit: เมื่อราคากลับผ่าน EMA midline (mean reversion exit)
- Custom indicator class `KeltnerChannel` สร้าง 3 lines: mid, top, bot
- EquityTracker analyzer ติดตาม account value ตลอด backtest
- Rolling backtest บน DOGE-USD แสดงผลว่า upside แรงมากในบางปี แต่ drawdown ก็หนักเช่นกัน

---

## บทที่ 8 — Hybrid Keltner + RSI Breakout

- ใช้ Keltner Channel ตรวจจับ breakout + RSI เป็น confirmation filter เพื่อคัดกรอง signal ที่อ่อน
- Long: price > upper band AND RSI > rsi_low (30) — ป้องกันไม่ซื้อ spike ที่อ่อนที่สุด
- Short: price < lower band AND RSI < rsi_high (70) — ป้องกันไม่ขาย dip ที่อ่อนที่สุด
- Exit ด้วย manual ATR trailing stop: trailing_stop_price = close ± ATR × mult
- Rolling backtest บน ADA-USD: mean return 134%, Sharpe 0.79, แต่ std สูง 170%

---

## บทที่ 9 — ML-Enhanced ADX Strategy (Random Forest)

- ผสม traditional ADX trend filter กับ Machine Learning (Random Forest classifier) เพื่อเลือกเฉพาะ trades ที่มั่นใจสูง
- Features 15 ตัวต่อ bar: ADX, +DI, -DI, DI spread, 5/10-day returns, MA gaps, ATR%, BB width/position, volume ratio, RSI, recent range
- Labels: 3 classes — +1 (up >1%), -1 (down >1%), 0 (flat ภายใน ±1%)
- Retrain model ทุก 30 bars (rolling online learning) ด้วย StandardScaler + RandomForest(n_estimators=50, max_depth=10)
- Entry gate: ADX > threshold AND ML prediction = ±1 AND confidence > 0.6
- Exit: broker-managed StopTrail ด้วย ATR × atr_multiplier

---

## บทที่ 10 — Momentum Ignition Strategy

- กลยุทธ์นี้หาช่วง consolidation (ราคาสงบ) แล้วเข้าเทรดตอน momentum "จุดระเบิด" (momentum ignition)
- Consolidation filter: normalised std dev ของราคา < threshold (เช่น < 10% ของราคา)
- Momentum ignition: ROC (Rate of Change) ทะลุ statistical bands (μ ± kσ ของ ROC เอง)
- Trend filter: ต้อง alignment กับ SMA — uptrend = ซื้อ, downtrend = ขาย
- ต้องผ่านเงื่อนไข 3 ชั้น: quiet market → momentum breakout → trend alignment
- Exit: ATR trailing stop

---

## บทที่ 11 — OBV Market Regime Breakout

- ใช้ On-Balance Volume (OBV) ตรวจจับ accumulation/distribution turns โดยดู OBV cross up/down กับ SMA ของ OBV
- 5 filters พร้อมกัน: (1) OBV crossover, (2) ADX > threshold (trending), (3) Volume > SMA(volume), (4) RSI ไม่ overbought/oversold, (5) Price breakout ทะลุ N-bar high/low
- OBV: บวก volume เมื่อราคาขึ้น, ลบเมื่อราคาลง — วัดแรงซื้อ/ขายสุทธิ
- Exit: broker-managed StopTrail (3% trail)
- Rolling backtest บน ETH-USD: explosive upside บางปี (618%) แต่ drawdown หนักเช่นกัน (-30%)

---

## บทที่ 12 — OBV Momentum Strategy

- ใช้ OBV crossover กับ SMA เป็นสัญญาณหลัก พร้อม RSI filter และ volume participation filter
- Long: OBV cross up SMA + RSI < 70 + volume > avg
- Short: OBV cross down SMA + RSI > 30 + volume > avg
- Exit: broker-managed StopTrail 2%
- แตกต่างจาก Ch.11 ตรงที่ไม่มี ADX gate และ price breakout — เน้น OBV momentum ล้วนๆ
- Rolling backtest บน ETH-USD: mean 145%, Sharpe 0.70, แต่ risk สูง (std 206%)

---

## บทที่ 13 — Ornstein-Uhlenbeck (OU) Mean Reversion

- โมเดล log-price เป็น Ornstein-Uhlenbeck process: dX = θ(μ - X)dt + σdW — ราคาแกว่งรอบค่าสมดุล μ
- ประมาณ OU parameters จาก data: θ = mean-reversion speed, μ = equilibrium, σ = diffusion
- คำนวณ z-score = (X_t - μ) / eq_std วัดระยะห่างจากค่าสมดุล
- Long: z < -threshold AND price > SMA (期望 reversion ขึ้น), Short: z > threshold AND price < SMA
- Exit: กลับเข้า near mean (z = 0)
- Rolling backtest บน BTC-USD 3-month windows: เหมาะกับสินทรัพย์ mean-reverting เช่น forex

---

## บทที่ 14 — Quantile Channel Strategy

- ใช้ Quantile Regression สร้าง 3 เส้นบน rolling window: upper (q=0.8), median (q=0.5), lower (q=0.2)
- Quantile loss (pinball loss) เป็น objective function สำหรับ optimize regression
- Entry: breakout ทะลุ channel โดยมี confirmation buffer (เช่น 1%)
- Stop loss: ใช้เส้นตรงข้ามของ channel เป็น dynamic stop (long → stop ที่ lower band)
- Exit: ราคากลับเข้า channel ใกล้ trend line (mean reversion exit)
- Rolling backtest บน ETH-USD: mean 56%, Sharpe 1.20 — risk-adjusted ดีที่สุดในกลุ่ม breakout strategies

---

## บทที่ 15 — Rolling & Walk-Forward Testing

- Rolling backtest แบ่งประวัติเป็นหน้าต่างเวลา (เช่น 12 เดือน) แล้วทดสอบ strategy แต่ละช่วง
- คำตอบที่ได้: "มันทำงานสม่ำเสมอ across regimes ไหม?" ดู dispersion, best/worst periods, rolling Sharpe
- `run_rolling_backtest()` สร้าง DataFrame ที่มี start, end, return_pct, final_value ต่อ window
- `report_stats()` คำนวณ equity curve จาก compounding window returns + metrics: mean/median/std, win rate, max DD, Sharpe
- Plot helpers หลายแบบ: period bar chart, cumulative returns, rolling Sharpe, return distribution, overlay charts
- Walk-forward testing ต่างจาก standard backtest ตรงที่ test บน out-of-sample data เท่านั้น

---

## บทที่ 16 — Stress Tests

- Standard backtesting มีจุดบอด: ไม่รู้เหตุการณ์ที่ไม่เคยเกิดขึ้น, underestimates tail risk, สมมุติว่า market structure ไม่เปลี่ยน
- Stress testing = shock ตัวแปรหลักเพื่อดูว่า strategy ทนทานแค่ไหน
- 5 ประเภท shock: (1) Volatility shock — ขยาย daily returns ด้วย multiplier, (2) Correlation shock — บังคับให้สินทรัพย์ขยับพร้อมกัน, (3) Flash crash — inject วันที่ราคาดิ่งหนักแล้ว rebound, (4) Cost stress — เพิ่ม commission สูง, (5) Parameter sensitivity — sweep ค่า parameters เพื่อดู stability
- `CustomPerformanceAnalyzer` คำนวณ Sharpe, total return, max drawdown, trade count
- `TrendFollowingStrategy` (SMA crossover) เป็น baseline strategy สำหรับทดสอบ
- `DiversifiedTrendStrategy` ทดสอบ multi-asset portfolio (SPY/GLD/TLT/VTI)
- สาธิต pipeline ทั้งหมด: download data → baseline → shock tests → parameter sweep → historical crisis → comparison plots

---

## สรุปรวม

- หนังสือ覆盖 กลยุทธ์ 5 ประเภทหลัก: Adaptive Filters, Mean Reversion, Momentum & Trend, Volatility, ML & Advanced
- ทุก strategy ใช้ Backtrader framework — มี codde พร้อมรันทุกบท
- Rolling backtest framework ช่วย evalute ความเสถียร across time
- Stress testing framework ช่วยวัด robustness ต่อ extreme scenarios
- จุดเด่น: มี code จริงทุก strategy, มี rolling backtest ทุกบท, มี risk management (trailing stops, ATR sizing) ในทุก strategy
- จุดอ่อน: ไม่มีผลตอบแทนเปรียบเทียบระหว่าง strategies, ไม่มี transaction cost model ที่ละเอียด, ไม่มี regime-aware ทุก strategy
---