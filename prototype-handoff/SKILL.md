---
name: prototype-handoff
description: Maps screens, interactions, states, and unresolved decisions in a prototype. Produces a gap report, source-labeled acceptance criteria, HANDOFF.md, and a builder brief for engineering's AI session. Use when someone needs to identify missing interaction behavior, clarify open decisions, or prepare an engineering handoff without inventing requirements.
---

# Prototype handoff

The prototype is a reference. The spec is the contract. The job is to map missing interaction behavior and unresolved decisions without inventing requirements.

## 1. Start: gather, confirm, ask

1. **Gather.** Read everything the user dropped in or pointed to: prototype (files, links, live pages), requirements, notes, any brand or token source (CSS variables, theme.json, DESIGN.md, a token export, a repo link), PRODUCT.md. If a link won't open, say "I couldn't open X. Attach it or skip it." Never guess its contents.
2. **Confirm.** Show a short list: what each source gives you and what you'll use. Treat it as a hypothesis until the user confirms.
3. **Ask.** Only about real gaps, at most 3 questions at a time. Always settle these three if the sources don't:
   - Where do brand values live (token file, theme config, nothing)?
   - Does anyone own the design system? Who approves a new token?
   - Can you see engineering's coded system, or only the brand values?

If the user is away, take the most reasonable reading, state it at the top, and log the rest as open questions.

## Scale depth to the prototype

Match the output to the size of the thing. A single screen or form gets a short pass: one spec block, the 8 answers as a list, open questions only where they block the build. A multi-path flow gets per-path specs. Merge states that share the same answers instead of repeating them. Prefer fewer, sharper questions over a long list.

## 2. Map behavior: 8 questions per screen and state

Ask all 8 for every screen, and for every state of it (for example in stock, backorder, out of stock). Answer each, or write "Doesn't apply, because ..." Never skip silently.

1. **Branches.** Different answers or data, different paths?
2. **Back, edit, re-entry.** Going back, editing a past step, leaving and returning: what is kept?
3. **Who arrives, and what they already have.** New vs returning, signed in or not, an existing cart or saved data.
4. **Loading, error, empty, timeout, duplicate submit.** Designed, or named as not designed.
5. **What's faked.** What the prototype simulates (instant success, hardcoded data, a placeholder endpoint) versus what's real.
6. **Left out on purpose.** Scope that's parked, said out loud.
7. **Open, and who decides.** Each question gets whoever actually owns the answer, not "PM" by default. Use the team's own names or roles when the sources give them. If one person wears several hats, name the person once. If you're guessing a role, mark it "(assumed)". If nobody owns it, write "No owner yet: [who decides for now]". Pair each question with your recommendation and why, labeled as a recommendation, so the owner can say yes or no instead of starting from scratch.
8. **Dependencies.** Data, config, tools or vendors the screen assumes, each with an owner.

**Never invent business rules.** Ship windows, limits, thresholds, policies, copy claims and dates are open questions with owners unless a source states them. Example copy is labeled "Example, needs [owner] to confirm."

## 3. Tokens

- Specs reference token names, never raw hex or px.
- Any value with no token goes in a **Values not yet tokenized** table: value, used for, proposed token name. Expect gaps in states (success, error, warning, disabled, focus, stock status), radius and control borders.
- Scale the table too. For a single screen, list only colors and states the specs use; leave spacing and type sizes as one line ("spacing and type: map to your scale"). A full table is for a flow or a team building a system.
- If a platform can't read tokens (a vendor form styled from outside), give exact values there and say why.
- Name who approves new tokens. If nobody owns the system, say so and name who decides for now.
- If you can't see engineering's coded system: hand over brand values and ask for a mapping back (see the brief).

## 4. Outputs, in this order

0. **One-screen summary first.** Five lines max: what the prototype covers, the P0s, how many open questions and who owns most of them, what's faked, and the next decision.
1. **Gap report.** Grouped by area, each item P0 (blocks the requirement or misleads the user), P1 (friction), P2 (polish). When a gap clearly breaks a named rule, name it (for example "WCAG 1.4.11" or "NN/g #1 Visibility of system status"). No usability severity scores: one rater's severity isn't reliable. Lead with what the prototype does and doesn't cover.
2. **Specs.** One block per screen and state, written as acceptance criteria a PM can paste into a ticket (Given / When / Then). Include the 8 answers. Tag each criterion on its own line (one interaction can mix tags), and point to its evidence:
   - **[Source: where]** stated in requirements, notes or a confirmed answer. Engineering implements it.
   - **[Observed: where]** the prototype does it; nobody has confirmed it's intended. Engineering verifies before treating it as a requirement. Keep this tag even when the behavior looks reasonable: deciding intent isn't your job.
   - **[Recommended]** your suggestion. It stays provisional until the owner says yes, even after it lands in HANDOFF.md.
   - Carry the tags everywhere a criterion is restated (HANDOFF.md logic, builder brief tasks). A rule with no tag reads as approved, so tag it or point to the tagged criterion.
3. **HANDOFF.md.** Flow and navigation rules, screen specs (link to 2), logic and state, token usage plus the values-not-yet-tokenized table, accessibility, what's faked, dependencies with owners, open questions with owners.
4. **Builder brief** for engineering's own AI session:
   - What to attach (or one link that holds it all), and an access check: can they open every link?
   - Tasks in order.
   - Fixed rules and platform limits (licensing, data that must stay in a vendor's form, data to re-check before launch).
   - Tokens only; a missing value becomes a proposed token, never a new hex.
   - Flag, don't guess: the named open questions.
   - Handback: a list of anything they couldn't do, plus a token mapping (brand token, their token, match / close match / no match).
   - Untested things said out loud.
