# Gold Sniper MT5
### Intelligent Trend-Following XAUUSD Expert Advisor for MetaTrader 5

![Version](https://img.shields.io/badge/version-5.50-gold)
![Platform](https://img.shields.io/badge/platform-MetaTrader%205-blue)
![Symbol](https://img.shields.io/badge/symbol-XAUUSD-yellow)
![Timeframe](https://img.shields.io/badge/timeframe-Multi--TF-orange)
![License](https://img.shields.io/badge/license-Commercial-red)

> **This repository documents the product overview of Gold Sniper MT5.**
> Source code and strategy details are proprietary and not distributed here.
> The compiled EA is available on [MQL5 Market →](https://www.mql5.com/en/market/product/168064)

---

## Overview

Gold Sniper MT5 is a fully automated Expert Advisor for MetaTrader 5, engineered specifically for XAUUSD. Developed after extensive quantitative research, it combines multi-timeframe analysis with adaptive position management to capture gold's momentum while protecting capital during corrections.

The strategy design was informed by analysis of academic research in quantitative trading and volatility modelling. The developer holds an MSc in Economics & Finance from a globally ranked university.

---

## Key Features

- Fully automated — attach and run, no manual intervention required
- Multi-timeframe trend identification system
- Proprietary entry signal algorithm with multiple confirmation layers
- Adaptive trailing profit management
- Balance-based position sizing that scales conservatively with account growth
- Built-in equity drawdown circuit breaker
- Spread filter to prevent execution during illiquid conditions
- Holiday filter to pause trading during low-volume sessions
- Configurable cooldown between entries
- Clean on-chart information panel with real-time portfolio status
- Works on any chart timeframe (self-managed internal timeframes)
- Transparent journal logging for full trade audit trail

---

## Backtest Results

Tested on XAUUSD, January 2025 — March 2026, $1,500 deposit, Every Tick, 100% history quality.

| Metric | Value |
|---|---|
| Total Net Profit | $2,688.73 (+179%) |
| Profit Factor | 4.24 |
| Total Trades | 361 |
| Win Rate | 81.72% |
| Balance Drawdown | 4.11% |
| Equity Drawdown | 33.51% |
| Sharpe Ratio | 5.37 |
| Recovery Factor | 3.89 |
| LR Correlation | 0.99 |

---

## Recommended Setup

| Requirement | Specification |
|---|---|
| Symbol | XAUUSD (Gold) |
| Broker | ECN/STP with raw spread (IC Markets, Pepperstone, FPMarkets) |
| Leverage | 1:500 |
| Minimum Deposit | $1,500 |
| Timeframe | Attach to any chart (internal logic manages its own timeframes) |
| VPS | Recommended for uninterrupted 24/5 operation |

---

## Risk Disclosure

This Expert Advisor uses position management techniques that may result in temporary floating drawdowns. While balance drawdown has been historically low (4.11%), equity drawdown can be higher during active management periods. This is a normal part of the strategy's design. Past performance does not guarantee future results. Never risk money you cannot afford to lose.

---

## Get the EA

**Available on MQL5 Market**
Search: `Gold Sniper MT5` by *Hatef Tabbakhian*

[View on MQL5 Market](https://www.mql5.com/en/market/product/168064)

Introductory price: **$49** for the first 10 buyers. Early adopters receive lifetime access to all future improvements.

---

## Author

**Hatef Tabbakhian**
MSc Economics & Finance — University of Naples Federico II
GitHub: [github.com/Leotaby](https://github.com/Leotaby)

---

> *Past backtest performance does not guarantee future results. Always test on a demo account before deploying live capital.*

---

*Built with evidence. Not with hope.*
