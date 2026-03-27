# Setup Guide — Gold Sniper MT5

## Requirements

| Requirement | Specification |
|---|---|
| Platform | MetaTrader 5 (MT5) |
| Symbol | XAUUSD (Gold vs US Dollar) |
| Timeframe | Attach to any chart (EA manages its own internal timeframes) |
| Account type | ECN or STP, hedging mode |
| Typical spread | Raw spread recommended |
| Minimum deposit | $1,500 recommended |
| Leverage | 1:500 |
| VPS | Strongly recommended for 24/5 uptime |

---

## Installation

1. Purchase the EA from [MQL5 Market](https://www.mql5.com/en/market/product/168064) (search: *Gold Sniper MT5*)
2. Open MetaTrader 5
3. Navigate to **File → Open Data Folder → MQL5 → Experts**
4. The `.ex5` file will appear automatically after purchase and platform restart
5. Open any XAUUSD chart (any timeframe)
6. Drag the EA from the Navigator panel onto the chart
7. Enable **Allow Automated Trading** in the EA settings dialog
8. Press **OK**

The information panel will appear on the chart immediately showing real-time portfolio status.

---

## Configuration

All parameters are organized into clearly labeled input groups in the MT5 settings dialog. Default values are pre-configured for optimal performance on XAUUSD with a $1,500 starting balance.

Key configurable areas include:

- **Position sizing** — Adaptive multi-tier system with configurable thresholds
- **Exit management** — Trailing profit parameters, fully adjustable
- **Risk protection** — Equity drawdown limit, spread gate, cooldown timer
- **Filters** — Holiday filter, spread threshold

Specific parameter values and recommended configurations are included in the EA's built-in documentation and journal logging.

---

## Testing Protocol (Before Live Trading)

### Phase 1 — Strategy Tester Backtest
1. Open MT5 Strategy Tester (Ctrl+R)
2. Select: Expert = Gold_Sniper_MT5, Symbol = XAUUSD, Timeframe = Any
3. Date range: 2025-01-01 to present
4. Modelling: Every tick based on real ticks (if available)
5. Deposit: $1,500 with 1:500 leverage
6. Run and review: equity curve, profit factor, balance DD

### Phase 2 — Demo Account (Minimum 4 Weeks)
- Use a demo account with raw spreads (IC Markets, Pepperstone, FPMarkets)
- Monitor: trade frequency, signal quality, equity curve smoothness
- Do not modify parameters during this phase

### Phase 3 — Live with Minimum Risk
- Start with conservative sizing settings
- Scale up after consistent demo results

---

## Recommended Brokers

| Broker | Account Type | Typical XAUUSD Spread |
|---|---|---|
| IC Markets | Raw Spread | 5–15 points |
| Pepperstone | Razor | 5–12 points |
| FP Markets | Raw | 6–14 points |

Any ECN/STP broker with raw spread on XAUUSD and 1:500 leverage should work.

---

## Common Questions

**Why XAUUSD only?**
The strategy was developed specifically for gold's market microstructure and volatility profile.

**What timeframe should I attach it to?**
Any timeframe works. The EA manages its own internal timeframes. The chart timeframe doesn't affect performance.

**What about the equity drawdown?**
Balance drawdown is historically very low (4.11%), but equity drawdown can be higher during active management periods (~33%). This is by design. If temporary floating drawdown is uncomfortable, this strategy may not be the right fit.

**What broker should I use?**
Any MT5 broker offering raw/ECN spreads on XAUUSD with 1:500 leverage.

---

*Always test on demo before live. Past results do not guarantee future performance.*
