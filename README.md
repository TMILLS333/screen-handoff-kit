# screen-handoff-kit

A convincing prototype can still contain incomplete interactions, accidental behavior, and unresolved decisions.

**Screen Handoff Kit maps screens, states and branches, then prepares the engineering handoff:** a gap report, acceptance criteria, `HANDOFF.md` and a builder brief for engineering's AI session. Every criterion is tagged **Source**, **Observed**, or **Recommended**, so behavior in the prototype doesn't silently become an approved requirement. Unresolved decisions name an owner, or say when no owner has been assigned.

## Who it's for

- **PMs and founders** who built a clickable prototype with AI and need to hand it to engineering
- **Designers** who want the first pass of the screen-and-state map before the handoff
- **Engineering leads** who received a prototype and need to know what's decided versus what's just in the mockup

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

You prototyped a plant shop's product pages and want them ready for engineering:

> I built product pages for our plant shop. Shoppers pick a plant and a pot size, then add it to cart. Can you get this ready for engineering? Here are the four pages, and our brand tokens are in theme.css.

The skill starts by saying what it found, then asks only what the pages can't tell it (illustrative, condensed):

> I found four product pages: one in stock, one with a single size, one on backorder, one out of stock. I'll use theme.css for brand values. Three questions first:
> 1. Is theme.css your full token source?
> 2. Who approves a new token if one is missing?
> 3. Can I see engineering's coded design system, or only these brand values?

Then it writes the handoff. Two lines from the backorder page, in plain words:

> When the page loads, the stock line reads "Available on backorder" and Add to cart has no quantity limit.
> **[Observed]** The prototype does this; nobody has confirmed it's intended.
>
> When a shopper picks a backorder size, a ship-timing note sits next to the button.
> **[Recommended]** No ship date is shown until the owner confirms one.

Nothing gets invented: the ship date stays an open question with an owner. Full output, with the criteria as written: [`evals/results/raw/`](evals/results/raw/).

## Evidence

The evals were written before the skill. Each case ran in a fresh session, without the skill and then with it.

| Eval | Input | Without skill | With skill |
|---|---|---|---|
| Free Lunch product pages | 4 live WooCommerce pages + theme tokens | 4 of 9 | 8 of 9 (1 partial) |
| Thin PM form | One-screen demo request form | Fails: invented business rules | Passes all criteria |

The baseline runs found major gaps, too. With the skill, unapproved behavior stayed separate from requirements, no business rules were invented, and the output named who needed to decide.

These results are directional. Each case has one baseline run. The skill was rerun as it changed, three runs per case, and the table shows each case's most recent run. All runs were scored by the skill's author. The Free Lunch baseline was rescored against criteria added after it ran, and one criterion was never exercised. Inputs, scorecards and the raw v1 output are in [`evals/`](evals/).

v1.0.0 is ready for testing. It hasn't been validated in a real engineering handoff yet. The one partial result, the out-of-stock exit, varied between runs; details are in the scorecards.
