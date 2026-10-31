---
type: brief
engagement: fx-hedging
capability: fx-hedging
scenario: offshore-wind-cable
date: 2026-10-23
status: committed
hypothesis: "I expect the forward to lock in about $18.89M, roughly $0.7M above the put's net floor, because the forward passes the USD-EUR interest gap through to us while the put costs $392K and strikes at the lower spot."
---

# Executive Brief: Hedging the EUR 16M Danish Receivable

**To:** CFO  
**Re:** Offshore-wind cable contract: currency exposure and hedge plan

## The exposure

We have invoiced our Danish wind-developer customer EUR 16,000,000, payable in 365 days. The euro amount is fixed by contract. The dollar amount we bank is not: it depends on the EURUSD rate on the day we convert.

## Why it can hurt us

Every one-cent move in EURUSD changes our proceeds by $160,000. At today's spot of roughly 1.16, the receivable is worth about $18.6 million. A 10% euro decline, well within EURUSD's normal range over a year, would cut that to about $16.7 million. That $1.9 million hole would come straight out of the contract's margin, and we carry the full risk until the cash arrives.

## Three ways to protect it

**Forward contract.** We agree today to sell EUR 16M in one year at 1.1807, locking in $18,891,200. *Pro:* complete certainty, at a rate above today's spot. *Con:* if the euro strengthens, we don't benefit.

**Money-market hedge.** We borrow euros today, convert them to dollars at spot, and invest the dollars; the customer's payment repays the euro loan. *Pro:* the same certainty as a forward, and a check on the bank's forward price. *Con:* it uses about EUR 15.6M of borrowing capacity and depends on our actual borrowing and deposit rates.

**Put option (insurance).** We pay $392,000 today (USD 0.0245 per euro) for the right to sell euros at today's spot. *Pro:* a floor of roughly $18.2M after the premium, and we keep the upside if the euro rises. *Con:* the premium is spent whether or not we need the protection.

## What I am assuming

- Spot near 1.16 for framing; Stage 4 replaces it with a sourced, timestamped quote.
- A 10% adverse move is illustrative; Stage 4 checks it against EURUSD's historical volatility.
- The customer pays in full and on time; customer credit risk is out of scope.
- We borrow EUR and deposit USD at market rates plus a modest corporate spread.
- The EUR call quote (USD 0.0448) matters only as the sold leg of a collar or as a pricing check. Buying it would add risk to a receivable, not reduce it.

## Hypothesis

I expect the forward to deliver the best certain outcome, about $18.89M, roughly $720K above the put's net floor, because the forward passes the higher U.S. interest rate through to us while the put strikes at the lower spot and costs $392K up front. I expect the money-market hedge to land slightly below the forward, because our corporate borrowing and deposit spreads are wider than the bank's. The put beats the forward only if EURUSD ends above about 1.205.

## How I would know I was wrong

If, with real interest rates, the money-market hedge beats the forward by more than transaction costs, or the put's net floor comes within $100K of the forward, the hypothesis fails.

## Next steps

1. **Stage 2: Model specification.** Design the workbook on paper: inputs, tabs, formulas, validation checks.
2. **Stage 3: AI-assisted build and audit.** Generate the workbook from the spec, then audit and correct it.
3. **Stage 4: Live market data.** Replace placeholders with sourced, timestamped spot, forward, and interest rates.
4. **Stage 5: Validation and decision memo.** Independent check, hand verification, and my recommendation.

You'll see the numbers before I recommend a hedge.

---

*Drafted with help from Claude (Anthropic, 2026); reviewed and edited by me.*
