# Setup Guide — ScalpGold Sniper EA

## Requirements

| Requirement | Specification |
|---|---|
| Platform | MetaTrader 5 (MT5) |
| Symbol | XAUUSD (Gold vs US Dollar) |
| Timeframe | M5 |
| Account type | ECN or STP, hedging mode |
| Typical spread | ≤ 20 points on XAUUSD |
| Minimum deposit | $500 recommended (higher = more stable lot sizing) |
| VPS | Strongly recommended for 24/5 uptime |

---

## Installation

1. Purchase the EA from [MQL5 Market](https://www.mql5.com/en/market/product/168064) (search: *Gold Scalper M5 LWMA Stochastic Sniper*)
2. Open MetaTrader 5
3. Navigate to **File → Open Data Folder → MQL5 → Experts**
4. The `.ex5` file will appear automatically after purchase and platform restart
5. Open a **XAUUSD M5** chart
6. Drag the EA from the Navigator panel onto the chart
7. Enable **Allow Automated Trading** in the EA settings dialog
8. Press **OK**

The dashboard will appear in the top-left corner of the chart immediately.

---

## Visual Companion Indicator (Optional)

The `ScalpGold_Visual` indicator paints signals, LWMA, and Bollinger Bands directly on the chart.

1. Place `ScalpGold_Visual.ex5` in **MQL5 → Indicators** folder
2. Attach to the same XAUUSD M5 chart
3. Match input parameters to your EA settings

**What you'll see:**
- 🔵 Blue line — LWMA(20)
- ⚫ Dotted grey lines — Bollinger Bands
- 🟢 Green arrow — BUY signal
- 🔴 Red arrow — SELL signal
- ⭐ Gold star — High-confidence signal (BB+RSI confirmed)

---

## Recommended Parameters (Starting Point)

```
LWMA Period         = 20
Stochastic K/D/Slow = 14 / 3 / 3
Oversold Level      = 30
Overbought Level    = 70
BB Period           = 20
BB Deviation        = 2.0
RSI Period          = 14
RSI Oversold        = 35
RSI Overbought      = 65
M15 LWMA Period     = 20
Use M15 Trend       = true
ATR Period          = 14
SL Multiplier       = 1.5
TP Multiplier       = 2.5
Trail Activation    = 1.0
Trail Distance      = 0.75
Risk per Trade %    = 0.75
Max Daily DD %      = 3.0
Max Positions       = 2
Max Consec. Losses  = 5
Min Bars Between    = 5
Session Start GMT   = 8
Session End GMT     = 17
Max Spread Points   = 35
```

---

## Testing Protocol (Before Live Trading)

### Phase 1 — Strategy Tester Backtest
1. Open MT5 Strategy Tester (Ctrl+R)
2. Select: Expert = ScalpGold_EA, Symbol = XAUUSD, Timeframe = M5
3. Date range: 2020-01-01 to 2024-12-31
4. Modelling: Every tick based on real ticks (if available) or 1-minute OHLC
5. Deposit: $10,000 (realistic lot sizing baseline)
6. Enable: Use date · Optimization = Disabled
7. Run and review: equity curve, profit factor (target >1.5), max DD (target <20%)

### Phase 2 — Demo Account (Minimum 4 Weeks)
- Use a demo account with realistic spreads (not fixed-spread demo)
- Monitor: daily trade frequency (expect 2–5 trades/day), signal quality, spread filter activity
- Do not modify parameters during this phase

### Phase 3 — Live with Minimum Risk
- Start with 0.25–0.5% risk per trade for the first month
- Scale to full 0.75% after 3 months of consistent results

---

## Dashboard Reference

The on-chart dashboard shows real-time status:

```
┌── ScalpGold EA v5.0 ──────────┐
│ XAUUSD | M5
│ Session : ACTIVE / CLOSED
│ Spread  : 18.3 pts
│ ATR(14) : 2.14
├────────────────────────────────┤
│ Trend   : YES / NO
│ Stoch   : YES / NO
│ BB+RSI  : YES / NO
├────────────────────────────────┤
│ Trades  : 12
│ Wins    : 8
│ ConsecL : 1 / 5
│ Day DD  : 0.82%
└────────────────────────────────┘
```

- **Session: ACTIVE** — within 08:00–17:00 GMT, weekday
- **Trend/Stoch/BB+RSI** — current bar signal conditions
- **ConsecL** — current consecutive losses / halt threshold
- **Day DD** — drawdown from today's starting equity

---

## Common Questions

**Why only M5 on XAUUSD?**  
The academic evidence base (Zarith Sofia et al. 2024) is specific to this combination. Using a different timeframe will still run, but removes the evidence-based parameter grounding.

**What broker should I use?**  
Any MT5 broker offering raw/ECN spreads on XAUUSD with typical spread ≤ 20 points. IC Markets, Pepperstone, FP Markets are commonly used for gold trading.

**Can I optimize the parameters?**  
Yes — use the MT5 Strategy Tester with Genetic Algorithm and the custom SSR fitness function (built into OnTester). See `docs/ARCHITECTURE.md` for the optimization design.

**What happens during news events?**  
The spread gate (35 points max) blocks trading when spreads spike during high-impact news. The session filter also excludes Asian session liquidity gaps.

---

*Always test on demo before live. Past results do not guarantee future performance.*
