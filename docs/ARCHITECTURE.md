# Architecture — Gold Sniper MT5

> Product overview document. No source code or strategy implementation details are included.

---

## 1. Strategy Overview

Gold Sniper MT5 uses a proprietary multi-layer signal generation system. The EA combines multi-timeframe trend analysis with adaptive entry timing and trailing profit management.

The signal pipeline requires multiple independent confirmation layers to align before any trade is placed. Each layer acts as a gate — if any layer fails, no order is executed. This multi-gate approach significantly reduces false signals.

Specific indicator configurations, signal logic, and entry/exit conditions are proprietary and not disclosed.

---

## 2. Position Sizing

The EA uses a balance-based adaptive sizing system with configurable tiers. Position sizes scale conservatively as the account grows, with all parameters fully adjustable via input settings.

- All sizes are floor-rounded to the broker's `SYMBOL_VOLUME_STEP`
- Clamped between `SYMBOL_VOLUME_MIN` and `SYMBOL_VOLUME_MAX`
- Margin check performed before every order — trade aborted if insufficient free margin

---

## 3. Exit System

The EA uses an adaptive trailing profit management system:

- **Trailing Profit Exit** — Arms at a configurable profit threshold, then trails from peak P/L
- **Equity Drawdown Protection** — Hard circuit breaker at configurable percentage
- **Spread Filter** — Blocks entries when current spread exceeds configurable limit
- **Holiday Filter** — Pauses trading during configurable low-volume periods
- **Cooldown Timer** — Configurable minimum time between entries

---

## 4. Risk Management

| Control | Description |
|---|---|
| Position sizing | Adaptive multi-tier system — scales conservatively |
| Equity DD protection | Hard circuit breaker — configurable threshold |
| Spread gate | Blocks during news/illiquidity |
| Cooldown timer | Prevents rapid-fire entries |
| Holiday filter | Pauses during low-volume sessions |

---

## 5. On-Chart Information Panel

The EA renders a real-time information panel on the chart showing:

- Current spread and position count
- Aggregate P/L with trailing status indicator
- Trend direction and signal status
- Equity drawdown percentage
- Trading status (TRADING / HALTED / HOLIDAY)

---

## 6. Broker Compatibility

The EA handles broker-specific differences at runtime:

| Issue | Detection | Solution |
|---|---|---|
| Order filling mode | `SYMBOL_FILLING_MODE` | Selects IOC → FOK → RETURN |
| Minimum stop distance | `SYMBOL_TRADE_STOPS_LEVEL` | Enforced on every modification |
| Freeze level | `SYMBOL_TRADE_FREEZE_LEVEL` | Checked before every modification |
| Market closed | `SYMBOL_TRADE_MODE` | Aborts if DISABLED or CLOSEONLY |
| Lot normalization | `SYMBOL_VOLUME_STEP` | Floor-rounded to broker step |

---

## 7. Files

| File | Purpose |
|---|---|
| `Gold_Sniper_MT5_v5.50.ex5` | Compiled Expert Advisor (distributed via MQL5 Market) |

Source `.mq5` files are proprietary and not distributed.

---

## 8. Dependencies

- MetaTrader 5 (any build supporting MQL5)
- `Trade\Trade.mqh` — standard MQL5 library
- No external DLLs
- No third-party libraries

---

*Gold Sniper MT5 v5.50 — Hatef Tabbakhian*
