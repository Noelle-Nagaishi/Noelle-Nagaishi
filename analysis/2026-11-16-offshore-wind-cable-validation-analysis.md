---
type: analysis
title: "Validation Analysis: Independent LLM Run vs. Workbook"
engagement: fx-hedging
scenario: offshore-wind-cable
date: 2026-11-16
---

# Validation Analysis: Independent LLM Run vs. Workbook

## 1. Method

- **LLM:** Claude (Anthropic), a new claude.ai chat with no history and no project attached
- **Documents provided (exactly two, as GitHub links):**
  - `capabilities/fx-hedging/spec.md` (v1.3)
  - `data/2026-11-11-offshore-wind-cable-market-data-memo.md`
- **Coaching:** none. No corrections mid-run, no workbook results shared.
- **Prompt (verbatim):**
  > Using only the two documents linked below, compute the USD outcome at maturity for each hedge strategy they describe (forward, money-market hedge, put, call) and the unhedged position, at several settlement spot rates. Show your calculations, then recommend a strategy for the CFO.
- **Raw output:** Appendix A

## 2. Comparison table

| Strategy | S_T | LLM | Workbook (full precision) | Difference | Diagnosis |
|---|---|---:|---:|---:|---|
| Unhedged | 1.057065 (−5%) | 16,913,040 | 16,913,040.00 | 0 | |
| | 1.1127 (0%) | 17,803,200 | 17,803,200.00 | 0 | |
| | 1.168335 (+5%) | 18,693,360 | 18,693,360.00 | 0 | |
| Forward | all rows | 18,891,200 | 18,891,200.00 | 0 | |
| Money market | all rows | 18,037,961 | 18,037,960.63 | rounding | |
| Put | −5% / 0% | 17,393,077 | 17,393,076.53 | rounding | |
| | +5% | 18,283,237 | 18,283,236.53 | rounding | |
| Call | −5% | 16,163,100 | 16,163,099.95 | rounding | |
| | 0% | 17,053,260 | 17,053,259.95 | rounding | |
| | +5% | 18,833,580 | 18,833,579.95 | rounding | |

## 3. Hand-verification table

Calculator arithmetic, named-range notation, no Excel.

```
T_YRS = 365 / 360 = 1.013888889

FORWARD
USD_FWD = FC_AMT × F0_in = 16,000,000 × 1.1807 = $18,891,200.00

MONEY MARKET
FC_FACTOR  = 1 + R_FC × T_YRS  = 1 + 0.03217 × 1.013888889 = 1.032616806
USD_FACTOR = 1 + R_USD × T_YRS = 1 + 0.0456 × 1.013888889 = 1.046233333
Step 1: EUR_BORROW  = FC_AMT / FC_FACTOR       = 16,000,000 / 1.032616806       = €15,494,615.15
Step 2: USD_INITIAL = EUR_BORROW × S0_in       = 15,494,615.15 × 1.1127         = $17,240,858.28
Step 3: USD_MM      = USD_INITIAL × USD_FACTOR = 17,240,858.28 × 1.046233333    = $18,037,960.63

PUT AT THE −5% ROW
S_T          = S0_in × 0.95                   = 1.1127 × 0.95                      = 1.057065
USD_UNHEDGED = FC_AMT × S_T                   = 16,000,000 × 1.057065              = $16,913,040.00
PUT_PAYOFF   = FC_AMT × MAX(K_PUT − S_T, 0)   = 16,000,000 × 0.055635              = $890,160.00
PUT_COST_T   = PREM_PUT × FC_AMT × USD_FACTOR = 0.0245 × 16,000,000 × 1.046233333  = $410,123.47
USD_PUT      = USD_UNHEDGED + PUT_PAYOFF − PUT_COST_T
             = 16,913,040.00 + 890,160.00 − 410,123.47                             = $17,393,076.53
```

**Reconciliation:** 

## 4. Spec retrospective

## Appendix A: Raw LLM output

```text
I worked the numbers from both documents. All five cases below match the spec's own check figures to the dollar.

What I used (the spec's Stage 4 inputs, plus the market-data memo dated 11 Nov 2026)

The receivable is €16,000,000, due in 365 days. Interest is simple, ACT/360, so T = 365/360.
Spot is 1.1127 USD/EUR, the USD rate is 4.56% and the EUR rate is 3.217%.
The bank's forward quote is 1.1807. The put and call premiums are 0.0245 and 0.0448 USD/EUR, and both strikes are 1.1127.
Interest factors: USD factor = 1 + 0.0456 × 365/360 = 1.046233. EUR factor = 1 + 0.03217 × 365/360 = 1.032617.

Fixed-outcome hedges

Forward: 16,000,000 × 1.1807 = $18,891,200
Money-market hedge:
Borrow €16,000,000 ÷ 1.032617 = €15,494,615.
Convert at 1.1127 to get $17,240,858.
Invest at the USD rate: × 1.046233 = $18,037,961. That works out to an implied forward of 1.1274.
The forward beats the money-market hedge by $853,239.
Option costs at maturity: each premium is grown at the USD rate, as the spec requires.
Put: 0.0245 × 16M × 1.046233 = $410,123
Call: 0.0448 × 16M × 1.046233 = $749,940
Put floor: 16M × 1.1127 − $410,123 = $17,393,077

Results at settlement

The spot rate at settlement (S_T) is 1.1127 × (1 + step).

Step	S_T	Unhedged	Forward	Money market	Put	Call
−5%	1.0571	16,913,040	18,891,200	18,037,961	17,393,077	16,163,100
−4%	1.0682	17,091,072	18,891,200	18,037,961	17,393,077	16,341,132
−3%	1.0793	17,269,104	18,891,200	18,037,961	17,393,077	16,519,164
−2%	1.0904	17,447,136	18,891,200	18,037,961	17,393,077	16,697,196
−1%	1.1016	17,625,168	18,891,200	18,037,961	17,393,077	16,875,228
0%	1.1127	17,803,200	18,891,200	18,037,961	17,393,077	17,053,260
+1%	1.1238	17,981,232	18,891,200	18,037,961	17,571,109	17,409,324
+2%	1.1350	18,159,264	18,891,200	18,037,961	17,749,141	17,765,388
+3%	1.1461	18,337,296	18,891,200	18,037,961	17,927,173	18,121,452
+4%	1.1572	18,515,328	18,891,200	18,037,961	18,105,205	18,477,516
+5%	1.1683	18,693,360	18,891,200	18,037,961	18,283,237	18,833,580

These formulas produce the table:

Unhedged = 16M × S_T
Put = 16M × S_T + 16M × max(1.1127 − S_T, 0) − $410,123
Call = 16M × S_T + 16M × max(S_T − 1.1127, 0) − $749,940

Break-even points

The put only beats the forward if spot at settlement is above 1.2063, the same figure the memo gives. Against the money-market level, the threshold is 1.1530.
The put beats being unhedged only below 1.0871.
The call never beats the forward anywhere in the table. At +5% it is still $57,620 short, which matches the memo. Buying a call on euros you are owed doubles your upside exposure and loses the most when the euro falls, so it is not a hedge for this position. The spec says it is modeled for comparison only.

Recommendation for the CFO

Hedge with the forward contract, but only after getting a fresh, executable quote from the bank.

At 1.1807, the forward gives the best result at every spot rate in the table, ±5%. It locks in $18.89M, which is $1.09M more than leaving the receivable unhedged at today's spot. It also costs nothing upfront and needs no borrowing.

The 1.1807 quote is the weak point. The memo calls it a retained scenario assumption, not a confirmed current price. It sits about 533 pips above the forward implied by today's interest rates, and only makes sense if the USD rate were about 9.44% instead of the 4.56% observed. A real quote is likely to land much closer to 1.1274, or about $18.04M.

If the new quote comes in near that level, the forward and the money-market hedge give about the same result. The forward is then still the simpler choice, and the money-market hedge is a usable backup if the bank won't quote.

The put is worth considering only if the CFO wants to keep upside from a stronger euro. It costs about $410K and guarantees a minimum of $17.39M.

Two smaller caveats from the memo:

The option premiums were not repriced when spot and strikes were reset.
The spot rate and the interest rates come from different days (11 Nov and 10 Nov).

Want me to turn this into a decision memo you can share with the CFO?

Sources:

FX hedging spec (v1.3)
2026-11-11 offshore-wind cable market data memo
```
