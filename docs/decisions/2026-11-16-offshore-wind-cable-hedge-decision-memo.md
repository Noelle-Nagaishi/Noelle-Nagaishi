---
type: decision
title: "CFO Memo: EUR 16 Million Receivable Hedge"
engagement: fx-hedging
capability: fx-hedging
scenario: offshore-wind-cable
date: 2026-11-16
status: committed
llm_used: "Claude (Anthropic): independent run in a separate session; figure checks and draft critique in a separate review session. Codex (OpenAI): revisions."
---

# CFO Memo: EUR 16 Million Receivable Hedge

**To:** CFO  
**From:** Noelle Nagaishi  
**Date:** November 16, 2026  
**Basis:** November 11 market-data memo and retained scenario assumptions

I recommend a forward hedge, subject to a fresh executable quote that meets our minimum proceeds and compares favorably with actual funding alternatives. Under the retained 1.1807 scenario quote, it locks in $18,891,200; even near the parity benchmark, it would secure about $18.04 million. This choice prioritizes budget certainty over paying to retain upside.

## A. Exposure and decision assumptions

The company will receive €16 million in 365 days, so euro depreciation reduces its USD receipts. Every 0.01 decline in EUR/USD reduces unhedged proceeds by $160,000. I assume the contract was priced at $17,803,200, using 1.1127 USD/EUR, and that this receivable supports planned USD spending. I therefore use $17,803,200 as the minimum acceptable maturity proceeds and prioritize securing that amount over upside participation; these are decision assumptions, not stated company policies.

## B. Strategy outcomes and ranking

The outcomes below include option premiums carried to maturity under the workbook convention.

| Strategy | Euro −5%: 1.057065 | Unchanged: 1.1127 | Euro +5%: 1.168335 |
|---|---:|---:|---:|
| Unhedged | $16,913,040 | $17,803,200 | $18,693,360 |
| Forward at 1.1807 | $18,891,200 | $18,891,200 | $18,891,200 |
| Money market | $18,037,961 | $18,037,961 | $18,037,961 |
| Put | $17,393,077 | $17,393,077 | $18,283,237 |
| Call | $16,163,100 | $17,053,260 | $18,833,580 |

At −5%, the ranking is forward, money market, put, unhedged, then call. At unchanged spot, it is forward, money market, unhedged, put, then call. At +5%, it is forward, call, unhedged, put, then money market. The retained forward leads throughout the ±5% grid.

The call payoff covers its maturity-equivalent premium at 1.1596, approximately +4.2%; this is also where the call strategy ties unhedged proceeds. It ties the retained forward at 1.1701, approximately +5.2%, just outside the grid. Above that rate it beats the forward, but neither threshold gives it a protective floor.

The earlier hypothesis that the forward would exceed the put floor by about $1.41 million held directionally: the actual gap is $1,498,123, about $88,123 above that estimate. If the stated falsifier meant a gap within $100,000 of zero, it was not triggered; that was a useful test of near-equivalence, but it did not validate the precision of the $1.41 million estimate.

## C. Sensitivity interpretation

**When the euro depreciates,** the forward and money-market outcomes remain fixed. The put holds net proceeds at $17.39 million below its strike, limiting losses but falling about $410,123 short of my assumed minimum. Unhedged proceeds fall dollar for dollar with the euro; below the strike, the call expires without a payoff and leaves the same currency downside plus its premium cost.

**When the euro appreciates,** the forward and money-market hedge surrender further gains. The put retains euro upside above its strike, less its premium cost. Unhedged proceeds rise with spot, while the call strategy rises at twice the unhedged slope above the strike because both the receivable and the purchased call gain value.

The trade-off is certainty, flexibility, and cost. The fixed-outcome hedges support budgeting; the put buys downside protection with upside participation, but its premium lowers the guaranteed net floor. Unhedged and call positions leave the company exposed to depreciation, so favorable upside scenarios do not make them suitable under my stated floor requirement.

## D. Recommendation and the parity gap

I recommend the forward. The parity gap changes the size of the forward’s advantage, not the choice.

The retained forward of 1.1807 exceeds the CIP-implied benchmark of 1.12737254 by 0.05332746 USD/EUR—about **533 pips**, or **$853,239** on the receivable. Reproducing 1.1807 with the other inputs requires a USD rate of approximately 9.44%, versus the 4.56% Treasury proxy.

The most plausible explanation is that 1.1807 is a retained scenario quote rather than a contemporaneous executable quote. Spot and rates also come from different dates, and Treasury and Euribor have different credit bases. Those limitations prevent treating the comparison as executable arbitrage; the implied forward is a benchmark, not a forecast of a bank quote.

A requote near that benchmark would pay about $18.04 million, clearing my assumed minimum by approximately **$235,000**. The put’s breakeven against the forward would fall from 1.2063 to 1.1530, only a 3.6% euro rise and inside the grid. That makes the put more competitive in stronger-euro outcomes, but does not change my choice: its $17.39 million floor still misses the required proceeds, and it requires $392,000 upfront. The call also provides no protective floor.

### Executable funding alternatives

The company needs quotes for its own EUR borrowing facility and a matched-maturity USD deposit or investment. Euribor and Treasury are proxies, not promised company rates. Borrowing EUR above Euribor or investing USD below the Treasury proxy reduces money-market proceeds.

Combined adverse spreads of **134.3 basis points** erase the money-market hedge’s $234,761 cushion before fees.[^1] This could be a EUR borrowing spread alone, a USD investment haircut alone, or 67.15 basis points on each side.

| Illustrative adjustments | Money-market proceeds | Meets $17,803,200 floor? |
|---|---:|---|
| No spreads | $18,037,961 | Yes |
| EUR borrowing +50 bp; USD investment −50 bp | $17,862,865 | Yes |
| EUR borrowing +100 bp; USD investment −100 bp | $17,689,471 | No |

These sensitivities show why the money-market alternative must be priced before relying on its cushion. Actual spreads could make it fail the floor, strengthening the preference for a forward that clears it.

Execute a forward only if its **net effective rate is at least 1.1127** and its proceeds compare favorably with a feasible money-market hedge after fees. Otherwise, test the executable money-market alternative. If neither meets the floor, return to the CFO to revise the minimum or approve another funding or hedging arrangement.

## E. Executive justification

**Cash-flow stability.** At the retained scenario quote, the forward fixes receipts at $18,891,200 regardless of settlement spot. This protects the assumed USD spending plan against euro depreciation.

**Budget certainty.** Both the retained forward and the proxy-rate money-market hedge exceed the $17,803,200 minimum. A forward near parity still clears it by about $235,000, whereas the put’s guaranteed net proceeds do not.

**Liquidity.** The modeled forward requires no upfront premium, although the bank may require a credit line or collateral. The money-market hedge uses approximately €15.5 million of borrowing capacity; the put spends $392,000 of cash immediately.

**Optionality.** Among the protective strategies compared, the put alone combines a downside floor with participation in euro appreciation. I would reconsider it if the CFO accepted the lower floor and valued that flexibility enough to pay the premium.

**Premium cost.** The put costs $392,000 upfront, equivalent to $410,123 at maturity, including $18,123 of financing or opportunity cost. Under my budget-certainty posture, preserving upside does not justify that expense while leaving guaranteed proceeds below the assumed minimum.

Before execution, confirm the CFO’s actual floor, obtain executable forward and funding quotes, and reprice the options. The retained option premiums were not updated when spot and strikes changed.

[^1]: Under the model’s simple-interest convention, let b be the EUR borrowing spread above 3.217%, d the USD investment haircut below 4.56%, and T = 365/360. USD_MM = 16,000,000 × 1.1127 × [1 + (0.0456 − d)T] / [1 + (0.03217 + b)T]. Meeting the assumed floor requires b + d ≤ 0.01343 before fees. These are sensitivities, not observed company funding quotes.

**Sources:** [Market-data memo](../../data/2026-11-11-offshore-wind-cable-market-data-memo.md), including input provenance and parity comparison; [Validation analysis](../../analysis/2026-11-16-offshore-wind-cable-validation-analysis.md); [Workbook](../../capabilities/fx-hedging/model.xlsx).

**AI assistance disclosure:** Claude (Anthropic) produced the independent run in a separate session and, in a separate review session, checked the figures and critiqued drafts. Codex assisted with revisions. Company risk posture and spread examples are assumptions and sensitivities, not verified company policies or funding terms.
