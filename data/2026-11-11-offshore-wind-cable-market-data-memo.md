---
type: data
title: "Market-Data Memo: Live Inputs for the EUR 16M Receivable Hedge"
engagement: fx-hedging
capability: fx-hedging
scenario: offshore-wind-cable
date: 2026-11-11
status: committed
workbook: "capabilities/fx-hedging/model.xlsx (Stage 4 commit, 2026-11-11)"
llm_used: "Claude (Anthropic): retrieved and checked sources, CIP calculation, workbook check review, FX Lab cross-check. Codex (OpenAI): memo revision and source publication checks."
---

# Market-Data Memo: Live Inputs for the EUR 16M Receivable Hedge

**Created by:** Noelle Nagaishi, with AI assistance disclosed above

## Summary

This memo refreshes the EUR 16 million receivable hedge using the November 11, 2026 ECB reference spot and November 10 interest rates. Under the model’s simple-interest ACT/360 convention, the implied forward is 1.12737254 USD/EUR, making the retained scenario bank quote of 1.1807 higher by approximately 533 pips, or $853,239 on the receivable. The workbook was populated through the named input cells; checks went from 9 FAIL to ALL PASS after three documented fixes.

## 1. Provenance table

Market values were checked against the source pages and screenshots. Retrieval times are approximate (≈21:15 HST, 11 Nov 2026). FX Lab values are from a run by Claude using these inputs; workbook values are from the committed model.xlsx.

| Input | Value | Source (URL) | Data date | Retrieved (HST) | Proxy / computation / rationale |
|---|---|---|---|---|---|
| S0_in | 1.1127 USD/EUR | [ECB euro reference rate, USD](https://www.ecb.europa.eu/stats/policy_and_exchange_rates/euro_reference_exchange_rates/html/eurofxref-graph-usd.en.html) | 11 Nov 2026; usual concertation ≈14:10 CET, publication ≈16:00 CET | 2026-11-11 ≈21:15 | ECB reference rate used as the model’s spot proxy. Timing follows the ECB’s usual published schedule, not a verified release timestamp for this observation. |
| R_USD | 4.56% | [U.S. Treasury Daily Par Yield Curve, 1 Yr](https://home.treasury.gov/resource-center/data-chart-center/interest-rates/TextView?type=daily_treasury_yield_curve&field_tdr_date_value_month=202611) | 10 Nov 2026 | 2026-11-11 ≈21:15 | The one-year Treasury maintains the Stage 2 proxy and approximately matches the hedge tenor using a freely accessible source; it is a government yield rather than an executable funding rate. |
| R_FC | 3.217% | [Bank of Finland: 12-month Euribor](https://www.suomenpankki.fi/en/statistics/interest-rates-and-exchange-rates/euribor-rates/) | 10 Nov 2026 | 2026-11-11 ≈21:15 | Twelve-month Euribor maintains the Stage 2 proxy and approximately matches the hedge tenor using a freely accessible source; its interbank credit basis differs from Treasury’s. |
| F0_in | 1.1807 USD/EUR | Scenario 23 bank quote | Scenario assumption | Not applicable | Retained to evaluate the quoted hedge against the refreshed parity benchmark; see §2. |
| K_PUT, K_CALL | 1.1127 USD/EUR | Reset to S0_in per Scenario 23 | 11 Nov 2026 spot basis | Not applicable | Both strikes were reset to the refreshed spot input. |
| PREM_PUT, PREM_CALL | 0.0245 / 0.0448 USD/EUR | Scenario 23 premiums | Scenario assumptions | Not applicable | Retained scenario premiums, not new market quotes; resetting spot and strikes while retaining premiums does not reprice the options. |
| FC_AMT, T_DAYS, BASIS | EUR 16,000,000 / 365 / 360 | Scenario 23; ACT/360 convention | Scenario assumptions | Not applicable | The specified model convention was applied consistently; the rate proxies are not a matched pair of executable funding quotes. |

**As-of date choice:** I use the November 11 ECB reference spot and the latest available November 10 rates to follow the assignment’s “day you begin” requirement. This combines observations from different dates, and the ECB reference rate is a spot proxy rather than a closing quote. The Treasury November table ends at November 10 with no November 11 row; the Bank of Finland states that its Euribor information is published with a 24-hour delay. These observations explain why November 10 rates were used. Treasury and Euribor also differ in credit basis, limiting interpretation of the parity comparison as an executable arbitrage.

## 2. CIP-implied forward vs. bank quote

Using USD per EUR and the model’s simple-interest ACT/360 convention:

```text
t = 365 / 360

F_IMPLIED = S0_in × (1 + R_USD × t) / (1 + R_FC × t)

          = 1.1127 × (1 + 0.0456 × 365/360)
                   / (1 + 0.03217 × 365/360)

          = 1.1273725391 USD/EUR
```

| | Rate (USD/EUR) | USD on EUR 16 million receivable |
|---|---:|---:|
| Bank quote F0_in | 1.1807 | $18,891,200 |
| CIP-implied benchmark | 1.1273725391 | $18,037,961 |
| Bank quote minus benchmark | +0.0533274609 (+533.27 pips) | +$853,239 |

Dollar amounts use the unrounded benchmark and are rounded only for display.

The bank quote produces higher USD proceeds for this EUR receivable than the refreshed parity benchmark. Holding the spot, EUR rate, and time convention fixed, the USD rate needed to reproduce 1.1807 is:

```text
R_USD_implied = [(F0_in / S0_in) × (1 + R_FC × t) − 1] / t

              = 9.4411443%
```

This discrepancy is relative to the model benchmark and does not establish an executable arbitrage: the observations have different dates, the rate proxies have different credit bases, and the bank quote is a retained scenario assumption.

I retain 1.1807 as the supplied scenario assumption for the assigned comparison rather than treating it as a validated current executable quote. Its implied USD rate of 9.44%, versus the observed 4.56% proxy, makes it difficult to defend as a contemporaneous parity-consistent quote. No requote has been requested because this analysis deliberately preserves the scenario benchmark and reports the discrepancy; an actual hedge decision would require a fresh executable quote.

## 3. Structural fixes

| # | What broke | Why | Fix |
|---|---|---|---|
| 1 | S0_in (C6) retained 1.1172 while the other five inputs were updated. | The grid was built from 1.1172 while both strikes were 1.1127. Checks showed 8 FAIL instead of the expected 9: row 38 passed, and row 33 showed “N/A — K_PUT not on grid.” | C6 was corrected through S0_in to 1.1127, producing the expected 9 FAIL. The input mismatch and its check results were retained in this audit account. |
| 2 | §7.2 expected values were typed constants for the old placeholder inputs. | The benchmarks could not follow the refreshed market inputs; rows 24–31 failed. | The eight stale values were replaced with independently hand-checked figures for the new inputs rather than formulas linked to the outputs. The check count fell from 9 FAIL to 1 FAIL. |
| 3 | Row 38 graded “call beats forward at +5%” as a correctness requirement. | Relative hedge performance depends on market inputs; a negative result does not indicate incorrect call logic. | E38 was converted to the ungraded message “INFO — call below forward,” reporting −$57,620. Row 36 already tested call logic through the call-slope check. The graded checks then showed ALL PASS. |

## 4. Re-run checks

| Stage | Result |
|---|---|
| Final market inputs entered through named cells only, including corrected S0_in | 9 FAIL: rows 24–31 and 38 |
| After fix #2: refreshed §7.2 expected values | 1 FAIL: row 38 |
| After fix #3: row 38 converted to ungraded INFO | ALL PASS |

Row 33’s kink check passed with the strike on the 0% row. The breakeven of 1.206333 passed against the displayed benchmark 1.2063 at the check’s rounding precision. The sensitivity grid was centered on 1.1127, and the forward produced the highest proceeds in all 11 rows. The call’s −$57,620 difference remained visible as information rather than being changed to force a favorable outcome.

## 5. FX Lab cross-check

| Output | FX Lab | Workbook | Resolution |
|---|---:|---:|---|
| Forward | $18,891,200 | $18,891,200 | Exact match |
| MM step 1: EUR borrowed today | €15,494,615 | €15,494,615 | Exact match |
| MM step 2: USD converted today | $17,240,858 | $17,240,858 | Exact match |
| MM step 3: USD proceeds at maturity | $18,037,961 | $18,037,961 | Exact match |
| Implied forward | 1.1274 | 1.127373 | Match at displayed precision |
| Parity gap | −$853,239 | USD_GAP +$853,239 | Same magnitude; Lab = MM − forward, workbook = forward − MM |
| Put, floor rows (−5% to 0%) | $17,411,200 | $17,393,077 | $18,123 difference from premium financing, per specification §4 (same $18,123 in every grid row) |

The put difference comes from financing a premium of 0.0245 USD/EUR under the stated convention:

```text
Initial put premium = 16,000,000 × 0.0245
                    = $392,000

Premium financing cost = 392,000 × 0.0456 × 365/360
                       = $18,123.47
```

The Lab subtracts the initial premium, while the workbook carries that premium forward at the USD rate under specification §4. This makes Lab put proceeds higher by $18,123 after rounding. The parity-gap signs differ solely because the tools subtract in opposite orders, as labeled in the table.

## References

- [ECB reference-rate schedule](https://www.ecb.europa.eu/stats/policy_and_exchange_rates/euro_reference_exchange_rates/html/index.en.html): concertation normally around 14:10 CET; publication usually around 16:00 CET.
- [ECB euro reference exchange rate: US dollar](https://www.ecb.europa.eu/stats/policy_and_exchange_rates/euro_reference_exchange_rates/html/eurofxref-graph-usd.en.html).
- [U.S. Treasury Daily Treasury Par Yield Curve Rates, November 2026](https://home.treasury.gov/resource-center/data-chart-center/interest-rates/TextView?type=daily_treasury_yield_curve&field_tdr_date_value_month=202611).
- [Bank of Finland: Euribor rates](https://www.suomenpankki.fi/en/statistics/interest-rates-and-exchange-rates/euribor-rates/).
- [FX Hedging Lab](https://adamwstauffer.github.io/ai-lms/fxlab.html).
- [Instructor scenario list: Scenario 23, offshore-wind-cable](https://github.com/adamwstauffer/shidler/blob/main/courses/International-Finance-And-Securities/projects/fx-hedging/scenarios.md).
- `capabilities/fx-hedging/spec.md` v1.2, especially §4 (premium financing).
