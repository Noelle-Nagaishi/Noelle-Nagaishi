<div style="border-top: 6px solid #024731; border-bottom: 1px solid #B2B2B2; padding: 12px 0; margin-bottom: 24px; font-family: 'Open Sans', Helvetica, Arial, sans-serif;">
  <div style="color: #024731; font-weight: 700; letter-spacing: 0.06em; text-transform: uppercase; font-size: 0.85rem;">University of Hawaiʻi at Mānoa · Shidler College of Business</div>
  <div style="color: #000000; font-weight: 700; font-size: 1.25rem; margin-top: 4px;">FIN 321: International Finance and Securities</div>
  <div style="color: #525252; font-weight: 400; font-size: 0.95rem;">FX Transaction Hedging Project — Technical Specification</div>
</div>

<!--
BRAND FORMATTING — applied per docs/_branding/design.json (v1.0.0)
  ┌─ Colors ─────────────────────────────────────────────────────────────┐
  │ Primary green ........... #024731  RGB 2,71,49    Pantone 3435 C     │
  │ Primary black ........... #000000  RGB 0,0,0      Process Black      │
  │ Silver (secondary) ...... #B2B2B2  RGB 178,178,178 Cool Gray 5 C     │
  │ White (secondary) ....... #FFFFFF                                    │
  │ Neutral-600 (muted) ..... #525252  (secondary text, captions)        │
  │ UH-green 700 (hover) .... #013D26  (link hover, pressed state)       │
  │ UH-green 50 (tint) ...... #E6F2EF  (callout backgrounds)             │
  │ Yellow (Excel only) ..... #FFFF00  (input highlight — not brand)     │
  ├─ Typography ─────────────────────────────────────────────────────────┤
  │ Headings (web)  .......... Open Sans Bold (H1/H2) / Semibold (H3/H4) │
  │ Headings (print)  ........ Avenir Bold                               │
  │ Body (web)  .............. Open Sans Regular                         │
  │ Body (print)  ............ Avenir Book                               │
  │ Fallback stack  .......... Helvetica, Arial, sans-serif              │
  │ Monospace  ............... Consolas / ui-monospace                   │
  │ Body minimum size  ....... 10 pt (11–12 pt preferred for print)      │
  │ Leading  ................. 3–5 pt greater than type size (print)     │
  │ Alignment  ............... Flush left, ragged right                  │
  ├─ Accessibility ──────────────────────────────────────────────────────┤
  │ • ADA-compliant contrast ratios for ALL text and UI elements         │
  │ • No red body type                                                   │
  │ • No layouts that are too dark for readability                       │
  │ • No custom palettes or gradients outside the official brand         │
  └──────────────────────────────────────────────────────────────────────┘
  Full brand standard: docs/_branding/design.json · Source: https://manoa.hawaii.edu/brand/
-->

# U.S. Offshore-Wind Cable Manufacturer — FX Transaction Hedge Model · Technical Specification

> <span style="color:#024731; font-weight:700;">Technical specification</span> for the FX transaction hedge model — the named-range contract, calculation flow, and validation checks, precise enough that an AI or a colleague could build (or rebuild) the workbook from this document alone. This spec is the input the AI-assisted build works from.

| Field | Value |
|------|------|
| **Created by** | Noelle Nagaishi |
| **Updated by** | Noelle Nagaishi |
| **Date Created** | 2026-11-04 |
| **Date Updated** | 2026-11-11 |
| **Version** | 1.3 |
| **LLM Used** (optional) | Claude (Anthropic): reviewed my draft for gaps; Stage 4 data retrieval and check review |
| **Role** | Treasury Analyst / FP&A Analyst |
| **Audience** | CFO / Director of Treasury |
| **Companion Workbook** | student build |

---
## 1. Problem Statement
The company is owed EUR 16,000,000 by a Danish wind developer, payable in 365 days. At the spot rate at inception of 1.1127 USD per EUR (ECB, 11 Nov 2026), the receivable is worth approximately $17.8 million, but each $0.01 decline in the EUR-USD exchange rate reduces its USD value by $160,000. The goal is to protect the receivable's USD value at an acceptable cost. The model compares a forward hedge, a money market hedge, and a purchased put against leaving the receivable unhedged, with settlement spot rates within ±5% of the initial spot rate, and includes a purchased call for comparison. These results support the Stage 5 decision memo. 

## 2. Inputs: Named-Range Contract
Market inputs were refreshed with live data at Stage 4 (2026-11-11); provenance is in data/2026-11-11-offshore-wind-cable-market-data-memo.md. Contract terms are fixed.

| Named range | Description | Value | Unit | Source today | Stage-4 source |
|---|---|---|---|---|---|
| `FC_AMT` | Foreign-currency receivable owed by the Danish wind developer | 16,000,000 | EUR | Scenario 23 (contract amount) | No change (fixed by contract) |
| `S0_in` | Spot rate at inception (11 Nov 2026) | 1.1127 | USD per EUR | ECB reference rate, 11 Nov 2026 (accessed 2026-11-11) | Refreshed at Stage 4 |
| `F0_in` | Forward exchange rate for exchanging EUR to USD on the maturity date; quoted in USD per EUR | 1.1807 | USD per EUR | Scenario 23, indicative 365-day bank forward | Kept: Scenario 23 value (stated assumption); CIP check in market-data memo |
| `R_USD` | Annual USD interest rate for the money market hedge | 4.56% | Annual % | U.S. Treasury 1-year par yield, 10 Nov 2026 (accessed 2026-11-11; no 11 Nov release, Veterans Day) | Refreshed at Stage 4 |
| `R_FC` | Annual foreign currency (EUR) interest rate for the money market hedge | 3.217% | Annual % | 12-month Euribor, Bank of Finland (EMMI), 10 Nov 2026 (accessed 2026-11-11; 24-hour publication delay) | Refreshed at Stage 4 |
| `K_PUT` | Strike exchange rate of the EUR put option (rate at which EUR can be sold for USD) | 1.1127 | USD per EUR | Reset to refreshed spot (S0_in) per Scenario 23 | Refreshed at Stage 4 |
| `K_CALL` | Strike of the EUR call option; modeled for comparison only, not part of the recommended hedge | 1.1127 | USD per EUR | Reset to refreshed spot (S0_in) per Scenario 23 | Refreshed at Stage 4 |
| `PREM_PUT` | Premium paid upfront at inception to purchase the EUR put option | 0.0245 | USD per EUR | Scenario 23 | Kept: Scenario 23 value (stated assumption) |
| `PREM_CALL` | Premium for the at-spot EUR call option, paid upfront at inception; comparison only | 0.0448 | USD per EUR | Scenario 23 | Kept: Scenario 23 value (stated assumption) |
| `T_DAYS` | Number of days until the receivable is due and the hedge matures | 365 | Days | Scenario 23 (contract terms) | No change (fixed by contract) |
| `BASIS` | Day-count denominator for both interest rates (ACT/360) | 360 | Days | Money-market convention | No change |

## 3. Tab Architecture
The workbook has 9 tabs, in the order below. Cells follow the Legend/Key color code (yellow inputs, blue assumptions, green formulas, gray outputs), and every calculation tab reads only from named ranges.

| Tab | Purpose | Contents |
|---|---|---|
| Cover | Identify the model, its owner, and the scenario being analyzed | Title, author, date, version, scenario (Scenario 23); data-provenance block listing each §2 market input with its source and date, marked live (refreshed at Stage 4, 2026-11-11) |
| Legend/Key | Explain how to recognize inputs, assumptions, formulas, and outputs. | Color code: yellow = market inputs refreshed at Stage 4, blue = assumptions (FC_AMT, T_DAYS, BASIS, sensitivity STEP values), green = intermediate formulas, gray = §8 outputs|
| Inputs | Provide the single place where model input values are changed, and all other tabs reference them through named ranges | The 11 named ranges from §2, with units and sources |
| Summary | Give the CFO a concise view of the main results and provide calculation factors | §5.1 derived values and the §8.1 summary results |
| Forward | Show how selling the receivable forward locks its USD value | §5.2: USD_FWD |
| Money Market | Show how borrowing EUR, converting to USD, and investing replicates a locked USD receipt | §5.3: the three steps as separate cells, F_IMPLIED, F_GAP, USD_GAP |
| Options | Show the put's protection and cost, with the purchased call's additional EUR exposure | §5.4 put and §5.5 call cells, including PUT_BREAKEVEN |
| Sensitivity | Compare strategy outcomes as the settlement spot rate changes | §6 grid, sensitivity table, and chart |
| Notes & Assumptions | Document the model's conventions, limitations, and supporting evidence | §4 assumptions; every source with its access date |

## 4. Assumptions & Constraints

- **Quote convention:** All exchange rates are quoted in USD per EUR; a higher number means a stronger euro.
- **Day count and interest:** Both R_USD and R_FC use simple interest (r x T_DAYS / BASIS, where BASIS = 360 under ACT/360). The 1-year Treasury yield is treated as an ACT/360 rate as a simplifying assumption.
- **Forward parity:** The model keeps the market forward (`F0_in`) and reports its difference from the parity-implied forward (`F_IMPLIED`, §5.3) as a market finding rather than a build error.
- **Option Premiums:** Premiums are quoted in USD per EUR, paid upfront at inception, and multiplied by the EUR notional with no contract multiplier. As a result, the $392,000 put premium becomes approximately $410,123 USD at maturity at Stage 4 inputs, yielding a net put floor of approximately $17,393,077 USD. The financing cost of the upfront premium ($18,123) is why the FX Hedging Lab, which deducts the premium as paid, reports put proceeds $18,123 higher. 
- **Call Option:** The call option is included for comparison purposes. A collar is outside the model's scope. This deviates from the brief by treating the call as a separate comparison rather than combining it with the purchased put.
- **Strikes:** K_PUT and K_CALL are independent contract inputs, set equal to spot at inception per Scenario 23. They do not move automatically with S0_in; at Stage 4 they are reset to the refreshed spot.
- **Exclusions and payment assumption:** The model excludes transaction costs, bid-ask spread, counterparty and credit risk, taxes, and hedge accounting. It assumes that the customer pays the receivable in full and on time, which is consistent with the brief.

## 5. Calculation Flow
S_T is the settlement spot rate for each row of the sensitivity grid (§6); USD_PUT, USD_CALL, and USD_UNHEDGED are calculated once per grid row. All other values are single cells.

### 5.1 Derived values
Calculate the time fraction and simple-interest accumulation factors used throughout the model.

```
T_YRS = T_DAYS / BASIS
USD_FACTOR = 1 + R_USD × T_YRS
FC_FACTOR = 1 + R_FC × T_YRS
```

### 5.2 Forward hedge
Sell `FC_AMT` forward at `F0_in`; proceeds are locked and do not depend on `S_T`.

```
USD_FWD = FC_AMT × F0_in
```

### 5.3 Money market hedge
Borrow the present value of the EUR receivable, convert it to USD, and invest the USD until maturity. Use the receivable to repay the EUR loan.

```
Step 1 — Borrow EUR:
EUR_BORROW = FC_AMT / FC_FACTOR

Step 2 — Convert EUR to USD at inception:
USD_INITIAL = EUR_BORROW × S0_in

Step 3 — Invest USD until maturity:
USD_MM = USD_INITIAL × USD_FACTOR

Parity-implied forward:
F_IMPLIED = S0_in × USD_FACTOR / FC_FACTOR

Forward-versus-parity differences:
F_GAP = F0_in − F_IMPLIED
USD_GAP = USD_FWD − USD_MM
```
USD_GAP ≈ +$853,239 (positive means the forward beats the money-market hedge); F_GAP ≈ 0.0533 at Stage 4 inputs.
Each step is a separate cell; do not combine into one nested formula.

### 5.4 Put option hedge
Purchase a EUR put to establish a minimum USD value for the receivable while retaining upside; the premium is carried forward to maturity.

```
PUT_PAYOFF = FC_AMT × MAX(K_PUT − S_T, 0)
PUT_COST_T = PREM_PUT × FC_AMT × USD_FACTOR
PUT_FLOOR = FC_AMT × K_PUT − PUT_COST_T
USD_PUT = USD_UNHEDGED + PUT_PAYOFF - PUT_COST_T
PUT_BREAKEVEN = F0_in + PUT_COST_T / FC_AMT
```

### 5.5 Call option comparison
Purchase a EUR call and calculate its standalone payoff and the combined proceeds from the receivable for comparison. This position does not establish a downside floor, and a collar is out of scope.

```
CALL_PAYOFF = FC_AMT × MAX(S_T − K_CALL, 0)
CALL_COST_T = PREM_CALL × FC_AMT × USD_FACTOR
CALL_NET_PAYOFF = CALL_PAYOFF − CALL_COST_T
USD_CALL = FC_AMT × S_T + CALL_NET_PAYOFF
```
### 5.6 Unhedged receivable
Convert the full EUR receivable at the future spot rate, leaving USD proceeds fully exposed to S_T.

```
USD_UNHEDGED = FC_AMT × S_T
```

## 6. Sensitivity Plan
### 6.1 Grid
Test future spot rates (S_T) from 95% to 105% of S0_in in 1% increments, yielding 11 scenarios ranging from approximately 1.0571 to 1.1683 USD per EUR under the Stage 4 inputs.

```
STEP = −5%, −4%, … , +4%, +5%   (11 rows, 1% increments)
S_T = S0_in × (1 + STEP)
```
Each row's S_T is calculated from S0_in rather than being entered as a fixed value, so the grid updates when the spot changes at Stage 4. 

### 6.2 Sensitivity table
One row per S_T, with these columns:

| Column | Formula source |
|---|---|
| `S_T` | §6.1 |
| `USD_UNHEDGED` | §5.6 |
| `USD_FWD` | §5.2 |
| `USD_MM` | §5.3 |
| `USD_PUT` | §5.4 |
| `USD_CALL` | §5.5 |

### 6.3 Chart
- **Type:** One line chart
- **X-axis:** S_T (USD per EUR) plotted at its numerical values
- **Y-axis:** USD proceeds at maturity
- **Series:** USD_UNHEDGED, USD_FWD, USD_MM, USD_PUT, and USD_CALL
- **What the CFO should see:** USD_FWD is flat and highest in all 11 rows, while USD_MM is also flat but about $853,000 lower. At +5% the call comparison trails the forward by about $57,620; the call adds euro exposure rather than protection, which supports keeping it out of the recommendation. USD_PUT has a net floor of approximately $17,393,077 USD below K_PUT and a kink at S_T = K_PUT (1.1127, the 0% row), above which proceeds rise with spot. The put's breakeven against the forward is approximately 1.2063 USD per EUR (including the premium's financing cost), outside the upper endpoint of 1.1683 (1.168335 at full precision).

## 7. Validation Rules
### 7.1 Structural checks
- No error cells (#DIV/0!, #NAME?, #REF!, #VALUE!) anywhere in the workbook
- Every output cell is a formula, never a typed number
- Every formula uses named ranges from §2 and §5, not cell addresses
- The S_T grid must contain exactly 11 rows, calculated as S0_in x (1 + STEP) for steps from -5% to +5% in 1% increments
- Retain full precision calculations; use formatting to round displayed values

### 7.2 Check figures (base case)
Compare monetary check figures to the nearest dollar and F_IMPLIED to four decimal places. These figures apply to the Stage 4 live inputs (2026-11-11). They are typed benchmarks, not formulas: update them by hand whenever inputs change.

| Output | Expected value | What a mismatch means |
|---|---|---|
| `USD_FWD` | $18,891,200 | Check FC_AMT, F0_in, and multiplication under the USD-per-EUR quote conversion |
| `EUR_BORROW` | €15,494,615 | Check R_FC, T_DAYS, BASIS, and division by FC_FACTOR |
| `USD_MM` | $18,037,961 | Check the EUR borrowing amount, spot conversion, and the USD investment accumulation |
| `F_IMPLIED` | 1.1274 | Check the USD_FACTOR is in the numerator and FC_FACTOR is in the denominator |
| `USD_GAP` | +$853,239 | Check that the formula is USD_FWD - USD_MM and that both underlying proceeds match |
| `PUT_COST_T` | $410,123 | Check PREM_PUT x FC_AMT x USD_FACTOR, including the financing cost |
| `PUT_FLOOR` | $17,393,077 | Check FC_AMT x K_PUT - PUT_COST_T |
| `CALL_COST_T` | $749,940 | Check PREM_CALL x FC_AMT x USD_FACTOR, using the same premium treatment as the put |
| `USD_CALL` at the +5% row (S_T = 1.168335) | $18,833,580 | Check the receivable proceeds plus the purchased call payoff, less CALL_COST_T |

Grid precision: Do not round S_T grid values; the +5% row is 1.168335 at full precision (displayed as 1.1683).

### 7.3 Logic checks
- **Parity (build check):** `USD_MM` = `FC_AMT × F_IMPLIED` within $1. A failure would mean inconsistent formulas, inputs, or premature rounding between the money market and implied-forward calculations.
- **Put kink:** applies only when K_PUT lies on the S_T grid (some row has S_T = K_PUT). In that row, `USD_PUT` = `PUT_FLOOR` within $1, which ensures that the option payoff reaches zero at the strike and continuously joins the rising-proceeds segment. If K_PUT is not on the grid (for example, after S0_in is refreshed but the strike is not reset), the check reports **N/A — K_PUT not on grid** rather than FAIL; the "put floor holds" check still applies to every row with S_T ≤ K_PUT.
- **Put floor holds:** In every row where S_T ≤ K_PUT, USD_PUT must equal PUT_FLOOR within $1. No scenario may produce USD_PUT below that floor.
- **Put breakeven:** `PUT_BREAKEVEN` ≈ 1.2063; above it, the put beats the forward.
- **Call comparison:** Above K_CALL, USD_CALL must increase by 2 x FC_AMT x ΔS_T, since both the receivable and the call gain value. The forward must also be highest in the first 10 grid rows. Call minus forward at the +5% row is reported as INFO, not graded: it is a market outcome that depends on inputs, not a test of model logic (changed at Stage 4, when it turned negative).
- **Flat lines:** USD_FWD and USD_MM must each remain identical across all 11 rows since neither depends on S_T.

## 8. Outputs
All output cells are formulas, shaded gray, and use the names below exactly.

### 8.1 Summary results (single cells)

| Output name | Description | Section |
|---|---|---|
| `USD_FWD` | Locked USD proceeds from the forward hedge | §5.2 |
| `USD_MM` | Locked USD proceeds from the money market hedge | §5.3 |
| `F_IMPLIED` | Parity-implied forward exchange rate, in USD per EUR | §5.3 |
| `USD_GAP` | Forward proceeds minus money market proceeds; a positive value favors the forward | §5.3 |
| `PUT_FLOOR` | Minimum USD proceeds under the put hedge, net of the premium carried forward to maturity | §5.4 |
| `PUT_BREAKEVEN` | Future spot rate at which put-hedged proceeds equal forward proceeds, in USD per EUR | §5.4; §7.3 |
| `PUT_COST_T` | Put premium cost carried forward to maturity at R_USD, in USD | §5.4|
| `CALL_COST_T`| Call premium cost carried forward to maturity at R_USD, in USD | §5.5 |

### 8.2 Sensitivity table
The 11-row sensitivity table defined in §6.2 contains S_T, USD_UNHEDGED, USD_FWD, USD_MM, USD_PUT, and USD_CALL, and appears on the Sensitivity tab.

### 8.3 Chart
The line chart defined in §6.3 plots the unhedged case and the USD proceeds of four strategies against S_T and appears on the Sensitivity tab.

---

## Change Log

| Version | Date | Author | Change |
|---------|------|--------|--------|
| 0.1 | 2026-11-04 | Noelle Nagaishi | Initial skeleton |
| 0.2 | 2026-11-05 | Noelle Nagaishi | Drafted §1–§8 |
| 1.0 | 2026-11-05 | Noelle Nagaishi | Revised after Claude review: call beats forward at +5%, put breakeven 1.205 → 1.2063, grid-precision check figure |
| 1.1 | 2026-11-11 | Noelle Nagaishi | Aligned legend colors (4-color convention) and cover provenance block with the Stage 3 build contract |
| 1.2 | 2026-11-11 | Noelle Nagaishi | Put kink check applies only when K_PUT is on the grid (N/A otherwise); strikes documented as independent inputs; added source access dates. Found in Stage 3 audit: kink check failed when S0_in changed. |
| 1.3 | 2026-11-11 | Noelle Nagaishi | Stage 4: §2 refreshed with live inputs (S0 1.1127, R_USD 4.56%, R_FC 3.217%; strikes reset); §7.2 benchmarks updated to live inputs; §7.3 call-vs-forward at +5% changed from graded check to INFO; §1, §4, §5.3, §6.1, §6.3 figures updated. |
