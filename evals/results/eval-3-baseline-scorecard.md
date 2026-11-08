# Eval 3 baseline: thin PM form, no skill

Run: Nov 8, 2026. Fresh agent, given only the query and the form file. No skill, no brief.

## Expected behavior

| Criterion | Result | Evidence |
|---|---|---|
| Error state | Pass | Field errors, submit failure, copy for each |
| Loading state | Pass | "Sending..." with aria-disabled |
| Success state | Pass | Focus moves to heading |
| Back and edit | Partial | Values kept on failure. No edit or "submit another" after success |
| Duplicate submit protection | Pass | Blocked while in flight |
| Dependency on the form tool, with owner | Partial | Endpoint flagged as placeholder. No owner named |
| No invented business rules | **Fail** | See below |

## 8 per-screen questions

| # | Question | Result |
|---|---|---|
| 1 | Branches | Not addressed |
| 2 | Back, edit, re-entry | Partial |
| 3 | Who arrives, what they have | Not addressed |
| 4 | Loading, error, empty, timeout, duplicate | Pass |
| 5 | What's faked | Pass (strong: led with "every lead is thrown away") |
| 6 | Left out on purpose | Not addressed |
| 7 | Open, and who decides | Partial: 5 questions, all addressed "for the PM" (sales and legal mentioned in text, not as owners) |
| 8 | Dependencies with owner | Partial: checklist, no owners |

**Covered: 2 full, 4 partial, 2 missing. Pass bar is 6 of 8.**

## Invented business rules (decided, then asked)
- Added a "within 1 business day" response promise to the copy
- Changed team-size buckets (200+ to 201+) and made the field required
- Renamed every field (`name` to `full_name`, etc.), which breaks any existing form-tool mapping
- Added a honeypot, privacy line and analytics event names
- Invented a token palette with raw hex values (`--error #b3261e`, etc.)

It flagged most of these as defaults "so engineering isn't blocked." The skill's rule is the opposite: these become open questions with owners, not shipped defaults.

## Result: fails (invented rules; 2 of 8 fully covered)

## Good catches to keep
- Fake success with a placeholder endpoint means leads would be silently dropped. Rightly P0.
- Overlapping ranges (51-200 / 200+).
- #ccc input borders fail non-text contrast.
