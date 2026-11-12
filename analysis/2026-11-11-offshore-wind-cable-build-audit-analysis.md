---
type: analysis
title: "Stage 3 Build Audit: FX Hedge Workbook"
engagement: fx-hedging
capability: fx-hedging
scenario: offshore-wind-cable
date: 2026-11-11
status: committed
spec: "capabilities/fx-hedging/spec.md v1.2 (e414f10)"
workbook: "capabilities/fx-hedging/model.xlsx (6da460d)"
llm_used: "Claude (Anthropic); Codex (OpenAI): revised the audit note"
---

# Stage 3 Build Audit: FX Hedge Workbook

**Created by:** Noelle Nagaishi

## Summary
The committed workbook (`6da460d`) passes all 20 checks at the base case. Four findings cover a corrected sensitivity test, a specification defect in the kink check, verification of the put-payoff sign, and missing source access dates. Spec v1.2 (`e414f10`) addresses the kink-check defect and access-date gap, and the regenerated workbook implements those changes.

## Finding 1 — Corrected the sensitivity test
**Checked:** Changed an input to test whether the sensitivity grid and dependent outputs recalculated.

**Found:** My first stress test changed F0_in instead of S0_in, so it moved the forward line without testing the grid. With S0_in = 1.1000 and F0_in = 1.1807, the corrected test moved S_T to 1.0450–1.1550 and shifted the chart's x-axis. Forward proceeds stayed at $18,891,200, while money market proceeds fell to $17,781,981.

The revised workbook reported five failures: four §7.2 benchmark mismatches—USD_MM, F_IMPLIED, USD_GAP, and USD_CALL at +5%—and one changed economic outcome. At the lower spot, the call's +5% proceeds were $554,887 below the forward, so "call exceeds forward" correctly failed.

**Did:** Caught the wrong-input testing error and reran the test with S0_in = 1.1000. Kept the stress-test results separate from the committed base case, which retains S0_in = 1.1172.

## Finding 2 — Fixed a specification defect in the kink check
**Checked:** Compared the fixed put strike with the shifted sensitivity grid and reviewed the §7.3 kink rule.

**Found:** K_PUT remains 1.1172 when S0_in changes. With S0_in = 1.1000, the strike lies between the +1% row, 1.1110, and the +2% row, 1.1220. The original check incorrectly treated an unavailable test as a failure. This was a spec defect: §7.3 assumed the strike would always appear on the grid.

**Did:** Revised the spec to v1.2 (`e414f10`) and regenerated the workbook (`6da460d`). Retained independent typed strikes and made the kink check conditional on K_PUT appearing on the grid. The v1.1 workbook reported **FAIL** when no grid row matched the strike; the revised workbook reports **N/A — K_PUT not on grid** for the same test. The put-floor check remains active and passes.

## Finding 3 — Verified the put-payoff sign
**Checked:** Hand-calculated the −5% and +2% rows with S0_in = 1.1000.

**Found:** At −5%, S_T = 1.0450:

16,000,000 × MAX(1.1172 − 1.0450, 0) = **$1,155,200**.

Net proceeds are $16,720,000 + $1,155,200 − $409,329 = **$17,465,871**.

At +2%, S_T = 1.1220 exceeds the strike, so the put payoff is zero:

$17,952,000 + $0 − $409,329 = **$17,542,671**.

Both results match the workbook to the nearest dollar. These hand calculations use displayed whole-dollar amounts; workbook formulas retain full precision.

**Did:** Confirmed that the purchased put pays positively below the strike and zero above it. No payoff-sign correction was needed.

## Finding 4 — Added missing source access dates
**Checked:** Reviewed the source documentation against the requirement to record access dates.

**Found:** The original specification identified the market-data dates but omitted retrieval dates. These describe different events and are both needed for source documentation.

**Did:** Added access dates to §2 of spec v1.2 (`e414f10`): November 2, 2026 for ECB, and November 5, 2026 for Treasury and Euribor. Regenerated the workbook (`6da460d`) to carry the source records into the model.

## Open items for Stage 4

- Replace indicative spot, forward, interest-rate, and option-premium inputs with live quotes, recording sources, data dates, access dates, and quote timestamps where applicable.
- Explicitly reset the independent strike inputs when selecting refreshed at-spot options.
- Update applicable benchmark figures and reassess strategy rankings after refreshing inputs.
- Repeat the sensitivity and payoff checks, distinguishing formula defects from changed economic outcomes and checks that are not applicable.
