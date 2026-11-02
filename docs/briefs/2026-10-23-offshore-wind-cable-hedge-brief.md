---
type: brief
title: "Executive Brief: Hedging the EUR 16M Danish Receivable"
engagement: fx-hedging
capability: fx-hedging
scenario: offshore-wind-cable
date: 2026-10-30
status: committed
hypothesis: "I expect the forward to lock in about $18.89M, roughly $1.4M above the put's net floor, because the forward passes the USD-EUR interest gap through to us while the put costs $392K and strikes at the lower spot."
llm_used: "Claude (Anthropic)"
---

# Executive Brief: Hedging the EUR 16M Danish Receivable

**Created by:** Noelle Nagaishi  
**Updated by:** Noelle Nagaishi  
**Date Created:** 2026-10-30  
**Date Updated:** 2026-11-02  
**Version:** 1.1  
**LLM Used:** Claude (Anthropic)

---

## Executive Summary

We will receive EUR 16,000,000 from a Danish wind developer in 365 days, but the dollars we bank depend on the exchange rate that day. Every one-cent drop in EURUSD costs us $160,000. Three tools can protect the receivable: a forward contract, a money-market hedge, and a put option. I expect the forward to give the best guaranteed result, about $18.89 million, and I will build a model to test that before recommending anything.

---

## Background & Objectives

We have invoiced our customer EUR 16,000,000 for offshore-wind cable, payable in 12 months. The euro amount is fixed by contract; the dollar amount is not. At the ECB reference rate of 1.1172 on 30 October 2026, the receivable is worth about $17.9 million. A 10% euro decline would cut that to about $16.1 million, a $1.8 million hole in the contract's margin.

**Objectives:** protect the dollar value of the receivable at an acceptable cost, and choose a hedge based on numbers rather than instinct.

---

## Methods

I will compare three hedge families in a spreadsheet model:

- **Forward contract.** Sell EUR 16M today for delivery in one year at 1.1807, locking in $18,891,200. *Pro:* complete certainty, above today's spot. *Con:* no benefit if the euro strengthens.
- **Money-market hedge.** Borrow euros now, convert at spot, and invest the dollars; the customer's payment repays the loan. *Pro:* the same certainty, and a check on the bank's forward price. *Con:* uses about EUR 15.6M of borrowing capacity.
- **Put option (insurance).** Pay $392,000 (USD 0.0245 per euro) for the right to sell euros at today's spot of 1.1172. *Pro:* a floor near $17.5M after the premium, with the upside kept. *Con:* the premium is spent either way.

**Hypothesis:** I expect the forward to beat the put's net floor by about $1.41M, because the forward passes the higher U.S. interest rate through to us while the put strikes at the lower spot and costs $392K. I expect the money-market hedge to land slightly below the forward, because our borrowing and deposit spreads are wider than the bank's. The put wins only if EURUSD ends above about 1.205. The sourced spot doubles my placeholder estimate of $720K, because the put's strike falls with spot while the forward is fixed; the 1.205 breakeven holds, since it depends only on the forward and the premium.

**How I would know I was wrong:** the money-market hedge beats the forward by more than transaction costs, or the put's net floor comes within $100K of the forward.

---

## Limitations & Next Steps

**Assumptions:**

- Spot is the ECB reference rate on 30 October 2026 (1.1172); Stage 4 refreshes it with a timestamped quote.
- The 10% adverse move is illustrative; Stage 4 checks it against EURUSD's historical volatility.
- The customer pays in full and on time.
- We borrow EUR and deposit USD at market rates plus a modest corporate spread.
- The EUR call quote (USD 0.0448) matters only as the sold leg of a collar; buying it would add risk to a receivable.

**Next steps (owner: Noelle Nagaishi):**

1. **Stage 2: Model specification.** Inputs, tabs, formulas, and validation checks, designed on paper.
2. **Stage 3: AI-assisted build and audit.** An AI builds the workbook from the spec; I audit and correct it.
3. **Stage 4: Live market data.** Sourced, timestamped spot, forward, and interest rates replace the placeholders.
4. **Stage 5: Validation and decision memo.** Independent check, hand verification, and my recommendation.

---

## References

European Central Bank. (2026, October 30). *Euro foreign exchange reference rates: US dollar (USD)*. https://www.ecb.europa.eu/stats/policy_and_exchange_rates/euro_reference_exchange_rates/html/eurofxref-graph-usd.en.html

Anthropic. (2026). *Claude* [Large language model]. https://claude.ai

Stauffer, A. (2026). *FX hedging project scenarios: Scenario 23, offshore-wind-cable* [Course material]. https://github.com/adamwstauffer/shidler/blob/main/courses/International-Finance-And-Securities/projects/fx-hedging/scenarios.md

Stauffer, A. (2026). *Stage 1: Executive brief* [Course material]. Kumu. https://adamwstauffer.github.io/ai-lms/fx-hedging-stage1.html
