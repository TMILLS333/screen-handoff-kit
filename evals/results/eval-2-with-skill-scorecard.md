# Eval 2 with skill: Free Lunch

Run: Nov 8, 2026. Fresh agent, given SKILL.md, the query, 4 URLs and the token file's :root block. No brief, no answer key.

| # | Criterion | Baseline | With skill | Evidence |
|---|---|---|---|---|
| 1 | In-stock state | Pass | Pass | PDP-1, Venus $12 / $26 |
| 2 | Backorder + asks about timing | Pass | Pass | PDP-3; Q2 backorder vs preorder (ops + finance), Q3 ship window (ops) |
| 3 | Out of stock + exit | Pass | Pass | PDP-4; button live without JS; next step is Q9 |
| 4 | Pot size, price changes | Pass | Pass | G1: per-size prices hidden until selection |
| 5 | Tokens only, gaps proposed | Not exercised | Pass | Token usage table, all brand names |
| 6 | Owners across roles | Fail (all PM) | Pass | 13 questions: ops, merchandising, content, legal, finance, engineering |
| 7 | Dependencies with owners | Fail | Pass | 9-row table |
| 8 | Back, edit, re-entry specced | Fail | Partial | Logged as "not designed", owner engineering, Q10. Cart edit out of scope |
| 9 | Gaps table + brief attaches tokens + mapping handback | Not run | Pass | 12-row values-not-yet-tokenized table; handback asks for brand token / your token / match |

**Baseline 4 of 9. With skill 8 of 9, 1 partial.**

## No invented rules
Example copy labeled "needs content to confirm." Ship timing, caps and thresholds all became questions with owners. Compare eval 3 baseline, which shipped a "1 business day" promise.

## Eval problem found
Criterion 8 asks the output to spec "returning keeps the chosen size." That is itself an invented rule. The skill correctly logged it as open instead. Proposed rewrite: "Back, edit and re-entry are addressed: specced from a source, or logged as open with an owner."

## Watch list
- **Length.** Output is long for a PM. Consider a one-screen summary at the top.
- **Responsive.** Neither run mentioned phone width. The eval doesn't test it, so this doesn't prove anything yet.
- **Contrast ratios** (13.16:1, 5.19:1, etc.) are the agent's own math. Not re-verified.
- **New catches** not in the baseline: the trumpet's "Dormant now" copy will show out of season; out-of-stock still shown in "Also hungry"; `role="alert"` too assertive; nectar and bloom-light fail contrast on dew.

## Rerun, Nov 8: source tags on every criterion

29 criteria: 2 [Source], 11 [Observed], 16 [Recommended]. Still passes all 9 checks.

| Check | Result |
|---|---|
| Source only where the requirement says it | Pass: only "pick a size" and "size and quantity land in the cart" |
| Observed kept even when reasonable | Pass: min quantity 1, focus ring, max quantity = stock all stay Observed, with evidence |
| Evidence on every Source and Observed | Pass: points to markup, variation data or the requirement |
| Recommendations stay provisional | Pass in specs and brief ("build [Recommended] only after the owner approves") |
| No upgrades in HANDOFF.md | **Slip**: the logic section restated two Recommended rules (A6, B4) as plain logic, untagged |

Fix: SKILL.md now says to carry tags wherever a criterion is restated. Not yet rerun.

Length: about 3,350 words (agent's own count), up from the first run because of per-criterion evidence.
