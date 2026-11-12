# Prompt Log

## 2026-09-28

**What I asked:**  
I asked ChatGPT to explain the overall portfolio repository setup process and guide me through the required files. I also asked for help reviewing and personalizing the AI conventions in AGENTS.md based on the course guidance.

**What AI produced:**  
ChatGPT explained the purpose of the repository files and guided me through the setup process. It also explained the purpose of AGENTS.md, reviewed my conventions against the course guidance, and suggested revisions to make the instructions more specific to how I work.

**What was wrong, if anything:**  
The initial version of my personalized conventions was longer than necessary and included some overlapping instructions.

**How the error was caught:**  
I compared the draft with the course guidance, which emphasizes concise, specific instructions, and revised the file before committing it.
## 2026-10-20

**What I asked:**
I asked Claude to explain my FX hedging scenario (Scenario 23, offshore-wind-cable), go over the Stage 1 brief requirements, coach me through the exposure section, and check what GitHub setup I was missing.

**What AI produced:**
Claude explained the hedge options, checked my 9% adverse-move calculation (about $1.7M shortfall from the 1.1807 forward), found that capabilities/fx-hedging/README.md was missing, and drafted that README for me to review.

**What was wrong, if anything:**
My exposure calculation used a 9% adverse move in EURUSD, which I took from the instructor's example without a source or justification.

**How the error was caught:**
Claude pointed out during review that a CFO would ask "why 9%?" I will either list it as an assumption to test in Stage 4 or cite EURUSD's historical volatility.

## 2026-10-30

**What I asked:**
I asked Claude to read the Stage 1 page, walk me through what I needed to do in my GitHub repo, and draft the executive brief for Scenario 23 (offshore-wind-cable).

**What AI produced:**
Claude checked my repo setup, drafted the executive brief, restructured it to match the course memo template, and walked me through committing it to docs/briefs/. I reviewed and edited it before committing.

**What was wrong, if anything:**
The brief uses a spot rate of about 1.16 as a placeholder, not a sourced quote, and the 10% adverse move is also unsourced. My first paste into GitHub lost the front matter and Markdown formatting, and a later paste was cut off partway through, so the committed brief was missing its assumptions, hypothesis, and next steps sections. The first version also didn't follow the course memo template.

**How the error was caught:**
Claude flagged the unsourced spot rate and the 10% move; I'll replace the spot with a timestamped quote in Stage 4 and either cite EURUSD's historical volatility or list the 10% as an assumption. I caught the formatting problem by checking the Preview tab and noticed a cut-off brief when I went to add an assumption; the section wasn't there. When I asked Claude to check the brief against the assignment, we compared it to template-decision-memo.md, and I restructured it into the template's sections (Executive Summary, Background & Objectives, Methods, Limitations & Next Steps, References) before final submission.

## 2026-11-02

**What I asked:**
I gave Claude my professor's Stage 1 feedback and asked it to help replace the placeholder spot rate with a dated, sourced one and update every figure that depended on it.

**What AI produced:**
Claude found the ECB reference rate for 30 October 2026 (1.1172), listed each figure that used the 1.16 placeholder with its formula, and recomputed them: receivable value $17.9M, 10% downside $16.1M, the put's net floor $17.5M, and the forward-over-put gap $1.41M. It drafted the updated brief and explained that the 1.205 breakeven doesn't depend on spot. I checked the rate against the ECB and a Wise quote (1.1167), checked the math, and changed the brief's date to 2026-10-30.

**What was wrong, if anything:**
The original brief used 1.16 as a placeholder spot without a source or date. With the real spot, the gap between the forward and the put's worst case was $1.41M, about double my original $720K. The brief's frontmatter date also didn't match the day I actually wrote it.

**How the error was caught:**
My professor flagged the placeholder spot in the Stage 1 feedback. I noticed the date mismatch myself while making the fix.

## 2026-11-05

**What I asked:**
I asked Claude to explain the Stage 2 model specification requirements, check my GitHub repo, help me find current USD and EUR interest rates, and review each section of my spec.md draft for gaps as I wrote it (§1–§8).

**What AI produced:**
Claude explained the eight required sections, gave me section templates and outlines, and calculated what my chosen rates (1-year Treasury 4.36%, 12-month Euribor 3.306%, 30 Oct 2026) imply for parity: an implied forward of about 1.1288 and a money-market hedge about $831,000 below the forward. It reviewed each section after I drafted it and supplied check figures for §7. I wrote the content of each section and decided how to handle parity, the day-count basis, the premium, and the call.

**What was wrong, if anything:**
- My §6 draft said, "no strategy beats the forward within this grid." The review showed that the purchased call beats the forward by about $23,000 at the +5% row, and that the put breakeven had moved from the brief's 1.205 to 1.2063 because the model now carries the premium forward at R_USD. I rewrote the §6.3 chart description and added both figures to §7.
- My §2 inputs table had rows with missing or shifted cells, and the K_CALL and PREM_CALL rows contradicted each other ("comparison only" vs. "sold as part of a collar"). I fixed the rows and made the call comparison-only throughout.
- Claude made an error: it gave a call check figure computed at a rounded spot of 1.1731, which is not on the grid, and rounded CALL_COST_T down to $748,486 instead of $748,487.

**How the error was caught:**
Claude's review caught the §6 and §2 problems. I caught Claude's check-figure error myself by recomputing the +5% row at full precision (1.1172 × 1.05 = 1.17306), which gives USD_CALL = $18,914,233. I replaced the off-grid check in §7.2 and added a rule to prevent rounding of grid values.

## 2026-11-11

**What I asked:**
I asked Claude to walk me through Stage 3, check my spec against the build contract, build model.xlsx from my committed spec, and review my audit. I also used Codex (OpenAI) to draft and revise the audit note.

**What AI produced:**
Claude found that spec v1.0 didn't meet the Stage 3 contract (3-color legend, no cover provenance block); I fixed both in v1.1 (82b99c5). Claude built the 9-tab workbook from v1.1 with 37 named ranges, formulas only, the sensitivity table and chart, and 20 live §7 checks. After my audit, I revised the spec to v1.2 (e414f10), Claude regenerated the workbook, and I committed it (6da460d). Codex revised the audit note from my audit findings, and Claude reviewed it before I committed it (ccb0b90).

**What was wrong, if anything:**
- When I changed S0_in to 1.10, the put-kink check failed because K_PUT is a typed input that doesn't move with spot, so no grid row landed on the strike. This was a spec defect: §7.3 assumed K_PUT is always on the grid.
- The spec had no source access dates, so the workbook's sources table was incomplete.
- My first stress test changed F0_in instead of S0_in, so it didn't test whether the grid moved.
- An early audit-note draft said the access dates still needed confirmation even though the spec stated them as fact.

**How the error was caught:**
I found the kink failure and the missing access dates while auditing the workbook in Excel and fixed both in spec v1.2 before regenerating. Claude pointed out that my first stress test had changed F0_in, and I reran it with S0_in. Claude's review of the audit note caught the inconsistency about the access dates, and I removed it.
