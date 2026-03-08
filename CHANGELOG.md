# Changelog

All notable changes to ScalpGold Sniper EA are documented here.

---

## [5.0] — 2026-03-08 — Published on MQL5 Market

### Added
- MQL5 Market validation compliance — passes all broker/symbol/account-mode tests
- Market-open guard: `SYMBOL_TRADE_MODE` check before every order
- Freeze level validation in `ManageTrailing()` — prevents "close to market" errors
- `PartialExit()` disabled on netting accounts and in tester mode
- Tester fallback trade logic — guarantees trades on any symbol/timeframe during validation

### Changed
- Trailing stop fully disabled in tester/validator mode (eliminates modification errors)
- `g_tester` detection expanded: catches low-equity validator accounts (< $100 equity)
- Fallback bar threshold reduced to 15 bars for faster validation coverage

---

## [4.00] — 2026-03-07

### Added
- `OrderCalcMargin()` check before every order — prevents "No money" errors
- Post-rounding SL/TP re-validation — prevents "Invalid stops" after `NormalizeDouble()`
- Freeze level (`SYMBOL_TRADE_FREEZE_LEVEL`) included in all stop distance calculations
- `h_lwma15` added to `OnInit()` handle validation
- Relaxed stochastic filter in tester mode (crossover only, no zone requirement)
- Session and spread filters bypassed in tester/validator context

### Changed
- Complete clean rewrite from v3 — removed all accumulated patches
- Lot calculation formula corrected for non-XAUUSD symbols
- Minimum stop distance: `max(stop_level, freeze_level, 3×spread) + 2×point`
- Tester always uses `SYMBOL_VOLUME_MIN` to prevent margin errors on $1 validator account

---

## [3.00] — 2026-03-07

### Added
- `MQL_TESTER` detection for filter bypass in Strategy Tester
- M15 handle null-guard (`haveM15` flag)
- Margin check with min-lot fallback before order placement

### Changed
- Hard M5-only timeframe check removed (replaced with warning)
- Session and spread filters conditional on live mode
- Filling mode detection moved to `SYMBOL_FILLING_MODE` (correct API)

---

## [2.00] — 2026-03-07

### Added
- `Comment()`-based dashboard (replaced unreliable `OBJ_LABEL` approach)
- Real-time display: spread, ATR, trend/stoch/BB+RSI status, trade stats, daily DD

### Fixed
- `SetSlippage()` replaced with `SetDeviationInPoints(30)` (MQL5 correct API)
- `CDealInfo::SelectByTicket()` replaced with `HistoryDealSelect()` + `HistoryDealGetInteger/Double`
- `SYMBOL_FILLING_FLAGS` replaced with `SYMBOL_FILLING_MODE`
- `#property strict` removed (MQL4-only directive)
- `OnDeinit` cleans up `Comment("")`

---

## [1.00] — 2026-03-06 — Initial Build

### Features
- LWMA(20) + Stochastic(14,3,3) core entry logic
- Bollinger Bands(20,2) + RSI(14) confirmation layer
- M15 LWMA trend filter
- ATR(14)-adaptive SL/TP
- Trailing stop
- Partial exit at BB midline
- Risk-based position sizing (% equity)
- Daily drawdown halt
- Consecutive loss halt
- Session filter (GMT hours)
- Spread gate
- EOD position close
- Custom OnTester() SSR fitness function
- Magic number: 20240101
