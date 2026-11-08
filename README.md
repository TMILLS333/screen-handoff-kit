# screen-handoff-kit

A skill that runs a fix pass on a happy-path prototype built by a PM or non-designer, then hands it to engineering: gap report, specs as acceptance criteria, HANDOFF.md and a builder brief for engineering's own AI session.

**The prototype is a reference. The spec is the contract.** Your MVP prototype doesn't need to be complete. It needs to show what's missing. The skill makes that visible, with owners, so a minimum viable prototype is enough to hand off.

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

## Grounding

- [Anthropic: Skill authoring best practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices) (evals first, concise SKILL.md)
- Intake modeled on Impeccable's `init`: scan before asking, at most 3 questions, record undecided facts instead of inventing them
- Token handling from a shipped handoff: one brand token file, a "values not yet tokenized" table, and a builder handback that maps brand tokens to engineering's own

## Open

- Responsive behavior ("what changes at phone width?") isn't a check yet. Waiting for an eval to show it's needed.
- Single-screen runs still propose tokens for every spacing value.
