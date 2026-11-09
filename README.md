# screen-handoff-kit

**Screen Handoff Kit turns happy-path prototypes into engineering specifications.** It maps screens, interactions and missing states, then produces a gap report, acceptance criteria and an engineering handoff. Every criterion says whether it's confirmed, observed in the prototype, or a recommendation, and every unresolved decision stays with the person who owns it. The prototype is a reference; the spec is the contract.

## Use it

Copy `prototype-handoff/` into your skills folder (Claude Code: `~/.claude/skills/`, or upload it as a skill in Claude). Then drop in or point at what you have:

- the prototype (files, links, live pages)
- requirements or notes
- any brand or token source: CSS variables, theme.json, DESIGN.md, a token export or a repo link

It reads what's there, shows what it will use, asks at most 3 questions, and logs everything else as open questions with owners and a recommendation.

## What it does differently

Evals were written first, run without the skill (baseline), then with it, each in a fresh session.

| Eval | Input | Without skill | With skill |
|---|---|---|---|
| Free Lunch product pages | 4 live WooCommerce pages + theme tokens | 4 of 9 | 8 of 9 (1 partial) |
| Thin PM form | One-screen demo request form | Fails: invented business rules | Passes all criteria |
| A real booking path (private) | 5 entry paths, practitioner-written findings as answer key | Found all findings; invented rules | Found all findings; no invented rules |

Both versions find the big problems. The skill changes three things: **no invented rules, real owners, and untested things said out loud.**

Inputs, criteria and scorecards for the two public evals are in `evals/`. The third eval uses client material and is kept out of this repo.

**How strong is this evidence?** Directional, not controlled. Each result is one run, scored by the person building the skill. The Free Lunch baseline was rescored against criteria added after it ran (one criterion was never exercised). Raw output is published for the v1 run in `evals/results/raw/`; earlier runs have scorecards only.

## Status

**v1.0.0: ready for real-world testing.** Not production-proven.

**Known limitation:** inconsistent coverage of the out-of-stock exit. One Free Lunch run named a next step for the shopper (related plants); the v1 run did not. The cause hasn't been established. The next evidence comes from a real engineering handoff: how many clarification questions come back, and how long the builder's "couldn't do" list is.

## Grounding

- [Anthropic: Skill authoring best practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices) (evals first, concise SKILL.md)
- Intake modeled on Impeccable's `init`: scan before asking, at most 3 questions, record undecided facts instead of inventing them
- Token handling from a real handoff: one brand token file, a "values not yet tokenized" table, and a builder handback that maps brand tokens to engineering's own
- Usability references by name only (WCAG criteria, NN/g heuristics), no severity scores: [NN/g](https://www.nngroup.com/articles/how-to-rate-the-severity-of-usability-problems/) finds one rater's severity too unreliable to trust

## Backlog (not v1 blockers)

| Improvement | Next action |
|---|---|
| Gap grounding | Eval: does the skill separate confirmed missing, unverified and unspecified behavior? |
| Broader review language | Rename questions 2 and 3 to "State continuity" and "Entry conditions"; test on a non-web interface (an AI assistant handoff) |
| Responsive behavior | Build a responsive-specific eval before adding a rule |
| Real engineering validation | One real handoff: count clarification questions, unresolved work and blockers |
| Output length | Measure whether engineering finds it too long before cutting |
