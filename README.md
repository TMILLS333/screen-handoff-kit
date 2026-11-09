# screen-handoff-kit

A convincing prototype can still contain incomplete interactions, accidental behavior, and unresolved decisions.

**Screen Handoff Kit maps screens, states and branches, then prepares the engineering handoff:** a gap report, acceptance criteria, `HANDOFF.md` and a builder brief for engineering's AI session. Every criterion is tagged **Source**, **Observed**, or **Recommended**, so behavior in the prototype doesn't silently become an approved requirement. Unresolved decisions name an owner, or say when no owner has been assigned.

The prototype is a reference. The spec is the contract.

## Use it

Copy `prototype-handoff/` into your skills folder (Claude Code: `~/.claude/skills/`), or upload it as a skill in Claude. Provide whatever you have:

- Prototype files, links or live pages
- Requirements or notes
- A brand/token source, if available: CSS variables, `theme.json`, `DESIGN.md`, a token export or a repo link

The skill checks the sources first, shows what it will use, and asks up to 3 questions at a time. Unresolved decisions are recorded with owners and recommendations.

## What it does differently

The evals were written before the skill. Each case was run in a fresh session, first without the skill and then with it.

| Eval | Input | Without skill | With skill |
|---|---|---|---|
| Free Lunch product pages | 4 live WooCommerce pages + theme tokens | 4 of 9 | 8 of 9 (1 partial) |
| Thin PM form | One-screen demo request form | Fails: invented business rules | Passes all criteria |

The baseline runs found major gaps, too. In these cases, the skill kept unapproved behavior separate from requirements, avoided invented business rules, and named who needed to decide.

Inputs, criteria and scorecards are in `evals/`.

### Evidence limits

These results are directional, not controlled: a small set of runs and reruns scored by the skill's author. The Free Lunch baseline was rescored against criteria added after it ran; one criterion was never exercised. The raw v1 output is in `evals/results/raw/`. Earlier runs have scorecards only.

## Status

**v1.0.0 is ready for real-world testing, not production-proven.**

**Known limitation:** coverage of the out-of-stock exit varied between runs. One Free Lunch run suggested related plants as a next step; the v1 run didn't. The cause is unknown.

Next test: one real engineering handoff. Count the clarification questions engineering sends back and the tasks its builder can't complete.

## Grounding

- [Anthropic: Skill authoring best practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices): evals first, concise `SKILL.md`
- [Impeccable](https://github.com/pbakaus/impeccable) `init`: scan before asking, ask up to 3 questions, record undecided facts instead of inventing them
- Token handling from a real handoff: one brand token file, a "values not yet tokenized" table, and a builder handback mapping brand tokens to engineering's
- Usability references by name only (WCAG criteria and NN/g heuristics), without severity scores. [NN/g](https://www.nngroup.com/articles/how-to-rate-the-severity-of-usability-problems/) cautions against relying on severity judgments from a single evaluator.

## Backlog (not v1 blockers)

| Improvement | Next action |
|---|---|
| Gap grounding | Eval: does the skill separate confirmed missing, unverified and unspecified behavior? |
| Broader review language | Rename questions 2 and 3 to "State continuity" and "Entry conditions"; test on a non-web interface (an AI assistant handoff) |
| Responsive behavior | Build a responsive-specific eval before adding a rule |
| Real engineering validation | One real handoff: count clarification questions, unresolved work and blockers |
| Output length | Measure whether engineering finds it too long before cutting |
