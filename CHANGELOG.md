# Changelog

All notable changes to Gold Sniper MT5 are documented here.

---

## [5.50] — 2026-03-27
### Changed
- Complete strategy overhaul with proprietary multi-timeframe architecture
- Proprietary multi-layer signal engine with multiple confirmation gates
- Adaptive trailing profit management with configurable arm level and trail distance
- 5-tier balance-based step-ladder position sizing (fully configurable)
- Equity drawdown circuit breaker
- Holiday filter for low-volume sessions
- Configurable cooldown timer between entries
- Organized input parameters into MT5 input groups
- On-chart information panel with real-time portfolio status
- Rebranded: ScalpGold Sniper EA → Gold Sniper MT5

### Removed
- Previous signal system (replaced by proprietary engine)
- Session time filter (strategy now operates 24/5)
- Previous exit system (replaced by adaptive trailing management)

---

## [5.10] — 2026-03-09
### Changed
- Internal parameter tuning for improved signal quality
- Reduced false signals during ranging conditions
- Expected result: 1–3 trades per session on normal days
- Copyright updated: Hatef Tabbakhian (LEO)

---

## [5.00] — 2026-03-08
### Added
- Initial public release
- Multi-layer signal system
- Trend confirmation filter
- Session + spread filters
- Drawdown halt, consecutive loss halt
- Walk-forward optimization fitness function
