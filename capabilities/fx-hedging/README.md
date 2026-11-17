# FX Hedging

Comparing ways to hedge a foreign-currency receivable: forward contracts, money-market hedges, and currency options.

## Files
- `spec.md`: model specification (v1.3): named-range inputs, tab architecture, calculation flow, sensitivity plan, and validation rules.
- `model.xlsx`: hedge comparison workbook built from `spec.md`, populated with live market data at Stage 4 (2026-11-11) and validated at Stage 5 against an independent LLM run and hand calculations; all graded checks pass, with the call-vs-forward comparison reported as INFO.

## Engagements
- offshore-wind-cable: hedging a EUR 16,000,000 receivable due in 12 months (complete). Brief: `docs/briefs/2026-10-23-offshore-wind-cable-hedge-brief.md`. Build audit: `analysis/2026-11-11-offshore-wind-cable-build-audit-analysis.md`. Market data: `data/2026-11-11-offshore-wind-cable-market-data-memo.md`. Validation: `analysis/2026-11-16-offshore-wind-cable-validation-analysis.md`. Decision memo (forward recommended): `docs/decisions/2026-11-16-offshore-wind-cable-hedge-decision-memo.md`
