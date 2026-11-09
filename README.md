# screen-handoff-kit

A convincing prototype can still contain incomplete interactions, accidental behavior, and unresolved decisions.

**Screen Handoff Kit maps screens, states and branches, then prepares the engineering handoff:** a gap report, acceptance criteria, `HANDOFF.md` and a builder brief for engineering's AI session. Every criterion is tagged **Source**, **Observed**, or **Recommended**, so behavior in the prototype doesn't silently become an approved requirement. Unresolved decisions name an owner, or say when no owner has been assigned.

## Use it

Copy `prototype-handoff/` into your skills folder (Claude Code: `~/.claude/skills/`), or upload it as a skill in Claude. Provide whatever you have:

- Prototype files, links or live pages
- Requirements or notes
- A brand/token source, if available: CSS variables, `theme.json`, `DESIGN.md`, a token export or a repo link

The skill checks the sources first, shows what it will use, and asks up to 3 questions at a time. Unresolved decisions are recorded with owners and recommendations.

## Evidence

The evals were written before the skill. Each case ran in a fresh session, without the skill and then with it.

| Eval | Input | Without skill | With skill |
|---|---|---|---|
| Free Lunch product pages | 4 live WooCommerce pages + theme tokens | 4 of 9 | 8 of 9 (1 partial) |
| Thin PM form | One-screen demo request form | Fails: invented business rules | Passes all criteria |

The baseline runs found major gaps, too. With the skill, unapproved behavior stayed separate from requirements, no business rules were invented, and the output named who needed to decide.

These results are directional. Each case has one baseline run. The skill was rerun as it changed, three runs per case, and the table shows each case's most recent run. All runs were scored by the skill's author. The Free Lunch baseline was rescored against criteria added after it ran, and one criterion was never exercised. Inputs, scorecards and the raw v1 output are in [`evals/`](evals/).

v1.0.0 is ready for testing. It hasn't been validated in a real engineering handoff yet. The one partial result, the out-of-stock exit, varied between runs; details are in the scorecards.
