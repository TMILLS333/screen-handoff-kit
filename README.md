# screen-handoff-kit

A convincing prototype can still contain incomplete interactions, accidental behavior, and unresolved decisions.

**Screen Handoff Kit maps screens, states and branches, then prepares the engineering handoff:** a gap report, acceptance criteria, `HANDOFF.md` and a builder brief for engineering's AI session. Every criterion is tagged **Source**, **Observed**, or **Recommended**, so behavior in the prototype doesn't silently become an approved requirement. Unresolved decisions name an owner, or say when no owner has been assigned.

## Use it

**Install**
- Claude Code: clone this repo and link the skill, so `git pull` updates it:
  ```bash
  git clone https://github.com/TMILLS333/screen-handoff-kit
  ln -s "$PWD/screen-handoff-kit/prototype-handoff" ~/.claude/skills/prototype-handoff
  ```
- Codex: link it into `~/.codex/skills/` the same way, then call `$prototype-handoff`
- Claude: upload `prototype-handoff/` as a skill. Re-upload to update.

**Give it what you have**
- Prototype files, links or live pages
- Requirements or notes
- A brand/token source, if available: CSS variables, `theme.json`, `DESIGN.md`, a token export or a repo link

The skill checks the sources first, shows what it will use, and asks up to 3 questions at a time. You get a gap report, screen specs, `HANDOFF.md` and a builder brief.

**Pair it with Impeccable**
Screen Handoff Kit maps behavior and open decisions. It doesn't critique visual design. For a UI review, run [Impeccable](https://github.com/pbakaus/impeccable)'s `critique` alongside it: Impeccable reviews the design; Screen Handoff Kit specs what engineering builds and lists what the team still has to decide.

## Example

> Requirement: let shoppers pick a plant and pot size and add it to cart. Here are the four product pages and our tokens (theme.css). Map the gaps and prepare the handoff.

One page sells a plant on backorder. Part of what came back:

> **AC-C1** Given Yellow trumpet pitcher, When the page loads, Then the stock line reads "Available on backorder", both sizes (4" $18, 6" $32) are backorderable, and Add to cart is offered with no quantity maximum.
> [Observed: stock indicator, variation data]
>
> **AC-C3** Given a backorderable size is chosen, When the shopper sees the button, Then a ship-timing note sits next to it using one term throughout, with wording and timing from Q1.
> [Recommended] No ship window is shown until the owner confirms it.

The page does it, so it's Observed. The skill proposes a fix, so it's Recommended and waits for a yes. No ship date was invented. Full output: [`evals/results/raw/`](evals/results/raw/).

## Evidence

The evals were written before the skill. Each case ran in a fresh session, without the skill and then with it.

| Eval | Input | Without skill | With skill |
|---|---|---|---|
| Free Lunch product pages | 4 live WooCommerce pages + theme tokens | 4 of 9 | 8 of 9 (1 partial) |
| Thin PM form | One-screen demo request form | Fails: invented business rules | Passes all criteria |

The baseline runs found major gaps, too. With the skill, unapproved behavior stayed separate from requirements, no business rules were invented, and the output named who needed to decide.

These results are directional. Each case has one baseline run. The skill was rerun as it changed, three runs per case, and the table shows each case's most recent run. All runs were scored by the skill's author. The Free Lunch baseline was rescored against criteria added after it ran, and one criterion was never exercised. Inputs, scorecards and the raw v1 output are in [`evals/`](evals/).

v1.0.0 is ready for testing. It hasn't been validated in a real engineering handoff yet. The one partial result, the out-of-stock exit, varied between runs; details are in the scorecards.
