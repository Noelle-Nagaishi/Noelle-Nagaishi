# FX Hedging

Comparing ways to hedge a foreign-currency receivable: forward contracts, money-market hedges, and currency options.

## Files
- `spec.md`: model specification (v1.3): named-range inputs, tab architecture, calculation flow, sensitivity plan, and validation rules.
- `model.xlsx`: hedge comparison workbook built from `spec.md`, populated with live market data at Stage 4 (2026-11-11); all graded checks pass, with the call-vs-forward comparison reported as INFO.

## Engagements
- offshore-wind-cable: hedging a EUR 16,000,000 receivable due in 12 months (in progress; Stage 4 complete). Build audit: `analysis/2026-11-11-offshore-wind-cable-build-audit-analysis.md`. Market data: `data/2026-11-11-offshore-wind-cable-market-data-memo.md`
