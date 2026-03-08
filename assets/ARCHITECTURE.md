# Architecture — ScalpGold Sniper EA

> Technical design document. No source code is included.

---

## 1. Signal Generation Pipeline

The EA generates signals through three sequential layers. Each layer acts as a gate — a signal must pass all active layers to trigger an order.

### Layer 1 — Macro Trend (M15 LWMA)

**Purpose:** Prevent counter-trend entries by filtering on a higher timeframe.

**Implementation:**
- Linear Weighted Moving Average (LWMA) with period 20 applied to M15 close prices
- LWMA slope direction (current vs previous bar) determines allowed signal direction
- `trendBuy = true` only when: M5 close > M5 LWMA AND M15 LWMA is rising
- `trendSell = true` only when: M5 close < M5 LWMA AND M15 LWMA is falling

**Rationale:** LWMA weights recent prices more heavily than EMA, making it more responsive to current market structure. The dual-timeframe confirmation was a key element of the Zhang & Khushi (2020) multi-layer design.

---

### Layer 2 — Entry Trigger (Stochastic Crossover)

**Purpose:** Time the entry within the trend using momentum confirmation from an extreme zone.

**Implementation:**
- Stochastic Oscillator: K=14, D=3, Slowing=3, STO_LOWHIGH
- BUY trigger: `%K[prev] < %D[prev]` AND `%K[cur] >= %D[cur]` AND `%K[cur] < 30`
- SELL trigger: `%K[prev] > %D[prev]` AND `%K[cur] <= %D[cur]` AND `%K[cur] > 70`

**Rationale:** Crossover from the oversold/overbought zone (not mid-range) was the specific finding of Zarith Sofia et al. (2024) on XAUUSD M5. Mid-range crossovers are excluded to avoid noise.

---

### Layer 3 — Confidence Upgrade (BB + RSI)

**Purpose:** Distinguish standard entries from high-confidence entries. Does **not** block trades — only upgrades signal strength.

**Implementation:**
- Bollinger Bands: period 20, deviation 2.0
- RSI: period 14
- `cfmBuy = true` when: close ≤ BB_Lower × 1.002 AND RSI < 35
- `cfmSell = true` when: close ≥ BB_Upper × 0.998 AND RSI > 65

**Signal strength levels:**
| Strength | Conditions met | Position sizing |
|---|---|---|
| 2 — Standard | Layers 1 + 2 only | 1.0× base lot |
| 3 — High Confidence | Layers 1 + 2 + 3 | 1.25× base lot, tighter SL/TP |

**Rationale:** Based on Hutabarat et al. (2025) BB+RSI framework. Implemented as an amplifier rather than a hard gate to avoid Hutabarat's sparse-trade problem (only 3 trades in their study period).

---

## 2. Position Sizing

Risk-based fractional sizing per trade:

```
lot_size = (equity × risk_pct) / (SL_distance / tick_size × tick_value)
```

- `risk_pct` defaults to 0.75% of account equity
- Strength-3 signals use `risk_pct × 1.25`
- Result is floor-rounded to broker's `SYMBOL_VOLUME_STEP`
- Clamped between `SYMBOL_VOLUME_MIN` and `SYMBOL_VOLUME_MAX`
- `OrderCalcMargin()` called before every order — trade aborted if insufficient free margin

---

## 3. Exit System

All exit distances are expressed as multiples of ATR(14), making them fully adaptive to XAUUSD's current volatility regime.

### Stop Loss
```
SL = 1.5 × ATR(14)
```
Enforced to be above broker's minimum stop level + freeze level + 2×point.

### Take Profit
```
TP = 2.5 × ATR(14)
```
Risk-reward ratio: 1:1.67. Breakeven win rate: 37.5%.

### Trailing Stop
- Activates when trade profit ≥ 1.0 × ATR
- Trail distance: 0.75 × ATR from current price
- Each modification validated against broker freeze level before sending

### Partial Exit
- 50% of position closed when price reaches BB midline
- Condition: profit ≥ 0.3 × ATR (prevents haircut exit)
- Only active on live hedging accounts (disabled on netting/tester)

### Time-Based Exit
- Position forcibly closed after 4 hours if TP/SL not hit
- Only active in live mode — not in Strategy Tester

---

## 4. Risk Management

| Control | Value | Behaviour |
|---|---|---|
| Risk per trade | 0.75% equity | Position sized on every entry |
| Daily DD halt | 3.0% | Blocks new entries until next day |
| Max positions | 2 | Simultaneous open trades cap |
| Consecutive loss halt | 5 | Manual review required |
| Min bars between trades | 5 (25 min) | Cooldown after any trade |
| Session filter | 08:00–17:00 GMT | London + NY overlap only |
| Spread gate | 35 points | Blocks during news/illiquidity |
| EOD close | 16:55 GMT | All positions closed before session end |

---

## 5. Optimization Design

### Custom OnTester() Fitness Function

Adapted from the Sharpe-Sterling Ratio criterion in Zhang & Khushi (2020):

```
SSR = sharpe_ratio × recovery_factor × log(n_trades) × (1 − maxDD / 100)
```

Rejection conditions (returns -999):
- Fewer than 80 trades
- Maximum drawdown > 25%
- Profit factor < 1.1
- Total profit ≤ 0

**Rationale:** Pure return maximization leads to high-drawdown overfitted results. The SSR penalizes drawdown, rewards consistency, and requires statistical relevance (log term).

### Walk-Forward Methodology

Based on Vezeris et al. (2020) d-Backtest PS recommendation:
- In-sample window: 6 months
- Out-of-sample window: 1 month
- Rolling forward monthly
- Minimum dataset: 2020–2024 (pre-COVID through post-rate-hike regimes)

### Genetic Algorithm Parameters (MT5 Strategy Tester)
- Algorithm: Genetic
- Population: default MT5 (scales with parameter space)
- Fitness: Custom SSR (OnTester)
- Optimization order: LWMA/Stoch → BB/RSI → ATR multipliers

---

## 6. Broker Compatibility

The EA handles broker-specific differences at runtime:

| Issue | Detection | Solution |
|---|---|---|
| Order filling mode | `SYMBOL_FILLING_MODE` | Selects IOC → FOK → RETURN |
| Minimum stop distance | `SYMBOL_TRADE_STOPS_LEVEL` | Enforced on every SL/TP |
| Freeze level | `SYMBOL_TRADE_FREEZE_LEVEL` | Checked before every `PositionModify` |
| Market closed | `SYMBOL_TRADE_MODE` | Aborts if DISABLED or CLOSEONLY |
| Netting accounts | `ACCOUNT_MARGIN_MODE` | Partial exits disabled |

---

## 7. Files

| File | Purpose |
|---|---|
| `ScalpGold_EA.ex5` | Compiled Expert Advisor (distributed via MQL5 Market) |
| `ScalpGold_Visual.ex5` | Companion indicator — plots signals on chart |

Source `.mq5` files are proprietary and not distributed.

---

## 8. Dependencies

- MetaTrader 5 (any build supporting MQL5)
- `Trade\Trade.mqh` — standard MQL5 library
- `Trade\PositionInfo.mqh` — standard MQL5 library
- No external DLLs
- No third-party libraries

---

*ScalpGold Sniper EA v5.0 — Hatef Tabbakhian*
