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
| **Date Updated** | 2026-11-04 |
| **Version** | 0.1 |
| **LLM Used** (optional) | Claude (Anthropic): reviewed my draft for gaps |
| **Role** | Treasury Analyst / FP&A Analyst |
| **Audience** | CFO / Director of Treasury |
| **Companion Workbook** | student build |

---
## 1. Problem Statement

## 2. Inputs: Named-Range Contract
Market inputs are indicative placeholders, replaced with live market data at Stage 4; contract terms are fixed.

| Named range | Description | Value | Unit | Source today | Stage-4 source |
|---|---|---|---|---|---|
| `FC_AMT` | Foreign-currency receivable owed by the Danish wind developer | 16,000,000 | EUR | Scenario 23 (contract amount) | No change (fixed by contract) |
| `S0_in` | Spot rate at inception (30 Oct 2026)  | 1.1172 | USD per EUR | ECB reference rate, 30 Oct 2026 | Live timestamped EURUSD spot quote |
| `F0_in` | Forward exchange rate for exchanging EUR to USD on the maturity date; quoted in USD per EUR| 1.1807  |  USD per EUR | Scenario 23, indicative 365-day forward | Live 1-year forward quote from a bank or dealer |
| `R_USD` | Annual USD interest rate for the money market hedge | 4.36% | Annual % | U.S. Treasury 1-year par yield, 30 Oct 2026 | Fed H.15 or 12-month term SOFR |
| `R_FC` | Annual foreign currency (EUR) interest rate for the money market hedge| 3.306% | Annual % | 12-month Euribor, euribor-rates.eu, 30 Oct 2026 | 12-month Euribor from EMMI or the ECB on the refresh date |
| `K_PUT` | Strike exchange rate of the EUR put option (rate at which EUR can be sold for USD) | 1.1172 | USD per EUR | Set at spot (S0_in) per Scenario 23 | Reset to the refreshed spot |
| `K_CALL` | Strike of the EUR call option; modeled for comparison only, not part of the recommended hedge | 1.1172 | USD per EUR | Set at spot (S0_in) per Scenario 23 | Reset to the refreshed spot |
| `PREM_PUT` | Premium paid upfront at inception to purchase the EUR put option | 0.0245 | USD per EUR | Scenario 23 | A live dealer quote for the at-spot 1-year put |
| `PREM_CALL` | Premium for the at-spot EUR call option, paid upfront at inception; comparison only | 0.0448 | USD per EUR | Scenario 23 | Live dealer quote for the at-spot 1-year call |
| `T_DAYS` | Number of days until the receivable is due and the hedge matures | 365 | Days | Scenario 23 (contract terms) | No change (fixed by contract) |
| `BASIS` | Day-count denominator for both interest rates (ACT/360) | 360 | Days | Money-market convention | No change |

## 3. Tab Architecture

## 4. Assumptions & Constraints

## 5. Calculation Flow

## 6. Sensitivity Plan

## 7. Validation Rules

## 8. Outputs

---

## Change Log

| Version | Date | Author | Change |
|---------|------|--------|--------|
| 0.1 | 2026-11-04 | Noelle Nagaishi | Initial skeleton |
