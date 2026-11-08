# Eval 3 with skill: thin PM form

Run: Nov 8, 2026. Fresh agent, given SKILL.md, the query and the form file. No brief, no answer key.

| Criterion | Baseline | With skill | Evidence |
|---|---|---|---|
| Error state | Pass | Pass | S2 validation, S4 submit failed |
| Loading state | Pass | Pass | S3, busy + aria-busy |
| Success state | Pass | Pass | S5, only on confirmed vendor response; focus moves |
| Back and edit | Partial | Pass | Answered per state; values kept on failure; refresh-after-success duplicate flagged |
| Duplicate submit protection | Pass | Pass | S3 single in-flight; dedupe is OQ-16 (sales ops) |
| Form tool dependency, with owner | Partial | Pass | OQ-1, marketing ops |
| No invented business rules | **Fail** | **Pass** | "Soon" flagged as a promise for sales to confirm; free-mail list, bands, limits, consent all left as questions |

**8 per-screen questions:** baseline 2 full; with skill 8 of 8, answered for all 5 states.

**Result: baseline fails, with skill passes.**

## What the skill added
- Lead with the right P0: fake submit means every lead is lost while users see success (baseline caught this too).
- 18 open questions across 9 owners: marketing ops, sales ops, sales, legal, content, marketing analytics, engineering, brand, web owner.
- Builder brief says "don't make up" the exact things the baseline invented.

## Rerun, Nov 8: with scale-depth and recommendation rules

| | First run | Rerun |
|---|---|---|
| Open questions | 18 | 12, each with a labeled recommendation |
| 8 answers | Repeated for all 5 states | Once, shared across states |
| Token table rows | 25 | 20 (spacing still included) |
| Length | not measured | about 2,250 words (agent's own count) |
| Invented rules | None | None. Recommendations are labeled, not decided |

Still passes every criterion.

## Rerun 2, Nov 8: token table scales to the prototype

| | Rerun 1 | Rerun 2 |
|---|---|---|
| Token table rows | 20 | 7 (colors and radius); spacing and type as one line |
| Open questions | 12 | 8, each with a recommendation |
| Length | about 2,250 words | about 1,750 words (agent's own count) |
| Named rules | none | NN/g #1, #9; WCAG 1.3.5, 1.4.11, 2.4.3, 3.3.1, 3.3.2, 4.1.3 |
| Invented rules | none | none |

Still passes every criterion.

## Watch list
- **Length.** 18 questions and a 25-row token table for a one-screen form. The 5-line summary helps; consider a rule to scale depth to the size of the prototype.
- **Over-tokenizing.** It proposed tokens for every padding value. Fine for a team with no system, heavy for one that has one.
- **Role names** (marketing ops, sales ops) are assumed. Reasonable, but a real team might use different ones.
