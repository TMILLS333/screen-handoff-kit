# Eval 2 baseline: Free Lunch, no skill

Run: Nov 7, 2026. Fresh subagent, given only the query and 4 product URLs. No brief, no answer key, no memory.
Read method: WebFetch plus raw HTML (states taken from the `data-product_variations` JSON). No browser, no add to cart.

## Score against expected_behavior

| # | Criterion | Result | Evidence from the run |
|---|---|---|---|
| 1 | In-stock state (Venus flytrap) | Pass | Ticket C: loading, success, failure, no-size-picked |
| 2 | Backorder state + asks about timing (Yellow trumpet) | Pass | Ticket D: ship window next to button; flags "backorder" vs "Dormant now. Preorder ships in spring." conflict |
| 3 | Out of stock + exit (Cobra lily) | Pass | Ticket E: Notify me, in-stock alternatives row; flags button still looks active |
| 4 | Pot size selection, price changes with it | Pass | Ticket A: range resolves to size price on select |
| 5 | Existing tokens only, missing values become proposed tokens | Not exercised | No design system was given and none was asked for. Zero hex or px, but also zero tokens |

**Result: passes as written (all states present, zero off-system values).** That means eval 2 does not separate skill from no-skill yet.

## Rescored against the tightened eval 2 (Nov 8, 9 criteria)

| # | New criterion | Result |
|---|---|---|
| 6 | Open questions have owners across roles | Fail: all 5 owned by "PM" |
| 7 | Dependencies listed with owners | Fail: none listed |
| 8 | Back, edit, re-entry specced | Fail: not covered |
| 9 | Token file applied | Not run: no token file attached yet |

**Rescored result: 4 of 9, fails.** Eval 2 now separates skill from no-skill.

## Found beyond the answer key
- Spoon-leaf sundew: single size but still forces "Choose an option" (the no-variant case flagged Nov 6)
- `is_purchasable: true` on out-of-stock items, so engineering can't branch on it
- Out-of-stock Cobra lily promoted in "Also hungry" on other pages
- "Pick a plant" has no picker: asked whether the requirement means the product page itself

## What it missed (the skill's 8 patterns + builder brief)
| Pattern | Baseline |
|---|---|
| 2. Back, Edit, re-entry | Missing (no "return to page with size kept", no cart edit) |
| 3. New vs returning | Missing |
| 5. What's faked | Partial: noted "test store" once, no faked-vs-real list |
| 7. Open questions with owner | Partial: 5 questions, every owner is "PM". No ops, merchandising or content owners |
| 8. Dependencies with owner | Missing: ship-window meta field, stock counts, notify-me email tool, nobody owns them |
| Builder brief: tokens only | Missing |
| Builder brief: return format with "anything you couldn't do" | Partial: gave a "couldn't see" list, unprompted |

## Invented or unconfirmed content
Mostly flagged correctly ("needs PM to confirm"). Three example strings were not:
- "Larger pots ship more mature plants" (flagged)
- "Ships March to April" (example only, page says "spring")
- "Expert plant: needs cool roots in summer" (not flagged, not confirmed on page)
