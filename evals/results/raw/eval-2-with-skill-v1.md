# Free Lunch: pick a plant and pot size, add to cart

Prototype fix pass, gap report, specs and handoff. Prepared Nov 8, 2026.

## Reading I took (user unavailable)

- **The prototype is one screen: the WooCommerce product page**, shown in four stock states. "Pick a plant" means arriving on that plant's page (from the catalog or the "Also hungry" row). Picking a plant from a list is outside these inputs and is logged as an open question, not specced.
- **The requirement ends at "in the cart."** Cart page, checkout and payment are out of scope. The footer says the store is a test store and no order ships.
- **Brand values live in the token file** (`free-lunch-tokens.css`, `:root` block only), which matches the theme's `theme.json` presets (`--wp--preset--*`). I can see the theme's CSS but not engineering's component code, so the brief asks for a token mapping back.
- **Owners.** The only person named in the sources is Tania (a design decision in the theme's `sections.css` is credited to her). I assume she owns store policy, copy and tokens, and mark each of those "(assumed)". Engineering is "Engineering lead (assumed)".
- **How I looked.** I fetched each page and the theme CSS with plain GET requests and read the server HTML, the variation data and the styles. I did not run the page scripts, select sizes, add to cart or submit anything. Every behavior after page load (button states, notices, mini-cart) is **unverified** and labeled that way.

## Sources and what I used

| Source | What it gives | Used for |
|---|---|---|
| Requirement (1 sentence) | Shoppers pick a plant and pot size and add it to cart | The only **[Source]** criterion |
| `/product/venus-flytrap/` | 2 sizes (2.5" $12, 4" $26), both "12 in stock" | State A: in stock, several sizes |
| `/product/spoon-leaf-sundew/` | 1 size (2.5" $16), "12 in stock" | State B: in stock, one size |
| `/product/yellow-trumpet-pitcher/` | 2 sizes (4" $18, 6" $32), "Available on backorder"; copy "Dormant now. Preorder ships in spring." | State C: backorder / preorder |
| `/product/cobra-lily/` | 2 sizes (4" $52, 6" $90), both out of stock, no backorders | State D: out of stock |
| Theme CSS (`sections.css`, global styles) | Button, select, quantity and focus styling, all on tokens except radius, borders, focus sizes | Token map, values not yet tokenized |
| `free-lunch-tokens.css` `:root` | Colors, font families, type sizes, spacing scale, layout widths | Token names in specs |

---

## 0. One-screen summary

1. **Covers:** one product page template in four stock states (several sizes, one size, backorder, out of stock). No catalog picker, no designed post-add, error or loading states.
2. **P0s (2):** the out-of-stock plant still offers a size select and an Add to cart button; the backorder plant hides its ship timing in the description and calls it "preorder" in one place and "backorder" in another.
3. **Open questions: 9.** Tania (assumed) owns 7; Engineering lead (assumed) owns 2.
4. **Faked:** the whole store (nothing ships), stock counts and prices ("reference prices from real US nurseries, Oct 31, 2026"); post-add feedback is WooCommerce default, not designed.
5. **Next decision:** Tania (assumed) decides what an out-of-stock and a backorder plant may do at the Add to cart button, and the ship-timing wording.

---

## 1. Gap report

**What the prototype covers:** product page layout, pot-size select, quantity, Add to cart, price range, stock line, care facts, related plants, header mini-cart badge, focus rings on the form controls.
**What it doesn't cover:** choosing a plant from a list, a plant with one size out of stock and one in stock, add-to-cart success, error, loading and duplicate submit, max-quantity handling, what the cart shows for a backorder line.

### Stock and availability

| ID | Pri | Gap | Evidence | Rule |
|---|---|---|---|---|
| G1 | **P0** | Cobra lily is out of stock in every size, yet the page renders the size select, quantity and an active-looking Add to cart. The shopper finds out only after choosing a size (WooCommerce default, unverified). | `cobra-lily` server HTML: stock indicator "Out of stock", both variations `is_in_stock: false`, form still rendered | NN/g #5 Error prevention |
| G2 | **P0** | Yellow trumpet pitcher ship timing ("Dormant now. Preorder ships in spring.") sits in the long description, far from the button. The stock line says "Available on backorder". Two terms, and a ship claim nobody has confirmed. A shopper can add it without seeing when it ships. | `yellow-trumpet-pitcher` About section and stock indicator | NN/g #1 Visibility of system status; NN/g #4 Consistency |
| G3 | P1 | In-stock plants show an empty stock line on load (filled by script, if at all). | `venus-flytrap`, `spoon-leaf-sundew`: stock indicator `--in-stock` with no text in server HTML | NN/g #1 |
| G4 | P1 | Backorder sizes have no quantity limit (`max_qty` empty). | `yellow-trumpet-pitcher` variation data | none |
| G5 | P1 | No design for a plant where only some sizes are out of stock. None of the four pages shows it, but the data model allows it. | variation data per size | none |

### Choosing a size

| ID | Pri | Gap | Evidence | Rule |
|---|---|---|---|---|
| G6 | P1 | Spoon-leaf sundew has one size but still makes the shopper "Choose an option" before adding. | `spoon-leaf-sundew` select | NN/g #7 Flexibility and efficiency |
| G7 | P1 | What happens on Add to cart with no size chosen isn't designed. WooCommerce's default is an alert; not verified here. | all four pages | NN/g #9 Help users recover from errors |
| G8 | P2 | "Pot size" values (2.5", 4", 6") don't say what's measured or whether the plant differs by size. | select options, care table | NN/g #2 Match with the real world |

### Adding to cart

| ID | Pri | Gap | Evidence | Rule |
|---|---|---|---|---|
| G9 | P1 | No designed confirmation after adding: what text, where, does the mini-cart open, is it announced. Only the WooCommerce store-notices area and the mini-cart badge exist. | `woocommerce/store-notices` block, mini-cart markup | NN/g #1; WCAG 4.1.3 Status messages |
| G10 | P1 | No loading, error, timeout or duplicate-submit state for the button. | button markup only | NN/g #1 |
| G11 | P1 | Quantity has `min="1"` but no `max` until a size is chosen; over-max handling isn't designed. | quantity input; `max_qty: 12` in variation data | NN/g #5 |

### Accessibility and polish

| ID | Pri | Gap | Evidence | Rule |
|---|---|---|---|---|
| G12 | P2 | Quantity input's `aria-label="Product quantity"` overrides its label "Venus flytrap quantity", so the plant name is lost for screen readers. | quantity markup | WCAG 2.5.3 Label in Name (best practice) |
| G13 | P2 | "Clear" is a link with `href="#"` that acts as a button. | `a.reset_variations` | WCAG 4.1.2 Name, Role, Value |
| G14 | P2 | Notices use WooCommerce's default colors (off-brand, not tokens). | `woocommerce.css` notice rules | none |

Checked and fine: button text (`dew` on `ink`) 13.2:1, hover (`dew` on `pitcher`) 8.9:1, focus ring `bloom` on white 6.6:1, control border `ink` on white 16.8:1 (WCAG 1.4.3, 1.4.11). Stock status is text, not color alone (WCAG 1.4.1).

---

## 2. Specs

One screen: **Product page, add-to-cart area.** Shared criteria apply to every state; each state then lists only what differs. Tags: **[Source]** is stated in the requirement; **[Observed]** is what the prototype does and engineering verifies before treating it as required; **[Recommended]** is my suggestion and stays provisional until the named owner says yes.

### 2.1 Shared: all states

**AC-01** Given a shopper on a plant's product page, When they want to buy it, Then they can choose one pot size for that plant and add that plant in that size to the cart.
[Source: requirement]

**AC-02** Given the page has loaded, When the shopper looks at the add-to-cart area, Then they see a select labeled "Pot size" with first option "Choose an option" and no size preselected, a quantity field set to 1, a "Clear" control and an "Add to cart" button.
[Observed: all four pages, `select#pa_pot-size`, `.single_add_to_cart_button`]

**AC-03** Given a plant whose sizes differ in price, When no size is chosen, Then the price shows as a range ("$12.00 – $26.00") with screen-reader text "Price range: $12.00 through $26.00".
[Observed: venus-flytrap, yellow-trumpet-pitcher, cobra-lily price block]

**AC-04** Given a size is chosen, When the selection changes, Then the price, stock text and maximum quantity for that size appear next to the button, and a screen reader announces them.
[Recommended] Shoppers confirm what they're buying where they act (NN/g #1).

**AC-05** Given no size is chosen, When the shopper activates Add to cart, Then the cart doesn't change, an inline message beside the select says "Choose a pot size" (example, needs Tania (assumed) to confirm), and focus moves to the select.
[Recommended] Replaces the unverified WooCommerce alert (G7).

**AC-06** Given an in-stock size with tracked stock, When the shopper sets quantity, Then the minimum is 1 and the maximum equals that size's stock (12 at time of review).
[Observed: variation data `min_qty: 1`, `max_qty: 12`]

**AC-07** Given a quantity below 1 or above the maximum, When the shopper activates Add to cart, Then nothing is added and an inline message states the allowed range; the value is never silently changed.
[Recommended] (G11)

**AC-08** Given a valid size and quantity, When Add to cart is in progress, Then the button shows a busy state, is announced as busy, and ignores further presses until the request returns.
[Recommended] Prevents duplicate lines (G10).

**AC-09** Given the add succeeds, When the response returns, Then a status message names the plant, size and quantity added, offers "View cart", the header cart badge updates, and the message is announced without moving focus.
[Recommended] (G9, WCAG 4.1.3). Whether the mini-cart drawer also opens is open question Q5.

**AC-10** Given the add fails (network, stock changed, session expired, timeout), When the response returns or times out, Then an error appears in the store-notices area and beside the button, the chosen size and quantity stay, and the cart is unchanged.
[Recommended] (G10)

**AC-11** Given the shopper changes the size after choosing one, When the new size is selected, Then price, stock text and maximum update and the quantity is re-checked against the new maximum.
[Recommended]

**AC-12** Given a size is chosen, When the shopper activates "Clear", Then the size returns to "Choose an option" and the size-specific price and stock text are removed.
[Observed: `a.reset_variations` "Clear" present] Behavior after click unverified.

**AC-13** Given any page, When the shopper looks at the header, Then a cart icon with an item-count badge is shown.
[Observed: header mini-cart, badge "0"]

**AC-14** Given keyboard use, When focus lands on the select, quantity or Add to cart, Then a 2px `bloom` focus ring with 3px offset is visible.
[Observed: `sections.css` `.fl-product form.cart :is(select,.qty):focus-visible`; global styles `.wp-element-button:focus-visible`]

**AC-15** Given a screen reader, When focus reaches the quantity field, Then its accessible name includes the plant name ("Venus flytrap quantity"), and "Clear" is exposed as a button named "Clear pot size".
[Recommended] (G12, G13)

#### 8 answers: shared (apply to every state unless the state says otherwise)

1. **Branches.** Sizes differ in price and stock, so the area changes per size chosen (AC-04, AC-11). Stock state of the whole product branches into A to D below.
2. **Back, edit, re-entry.** Changing size updates the area (AC-11). Leaving and returning: whether the chosen size is kept isn't designed. Recommendation: don't keep it; the page loads fresh with no size chosen (AC-02), which is the WooCommerce default (unverified). Cart contents persist through the WooCommerce session (unverified).
3. **Who arrives.** Anyone; no sign-in on these pages. A shopper may already have this plant and size in the cart: what happens is open question Q6. Arrival from the "Also hungry" row or catalog is the same page.
4. **Loading, error, empty, timeout, duplicate submit.** Not designed. Specced as recommendations in AC-05, AC-07, AC-08, AC-10.
5. **What's faked.** The store ("Free Lunch is a test store: nothing here is for sale"); prices ("reference prices from real US nurseries, Oct 31, 2026"); stock counts (every in-stock size shows exactly 12). Post-add feedback is WooCommerce default, not designed.
6. **Left out on purpose.** Cart page, checkout, payment, shipping. Picking a plant from a list (Q9). Notify-me for out-of-stock (Q2).
7. **Open, and who decides.** Q1 to Q9 below.
8. **Dependencies.** WooCommerce variable products with a `pa_pot-size` attribute and per-size stock (Engineering lead, assumed); prices and stock data (Tania, assumed); theme tokens (Tania, assumed); product photos under CC BY / CC BY-SA from Wikimedia with credit lines on the page (Tania, assumed).

### 2.2 State A: in stock, several sizes (Venus flytrap)

**AC-A1** Given Venus flytrap, When the shopper opens the size select, Then the options are 2.5" ($12) and 4" ($26), each "12 in stock".
[Observed: venus-flytrap variation data]

**AC-A2** Given an in-stock plant, When the page loads and no size is chosen, Then the stock line reads "In stock".
[Recommended] The line is empty in the server HTML today (G3).

8 answers: same as shared. Branch 1 is the two sizes. Nothing else differs.

### 2.3 State B: in stock, one size (Spoon-leaf sundew)

**AC-B1** Given Spoon-leaf sundew, When the page loads, Then the select offers one size, 2.5" ($16, 12 in stock), and still requires "Choose an option".
[Observed: spoon-leaf-sundew select and variation data]

**AC-B2** Given a plant with exactly one pot size, When the page loads, Then that size is preselected, its price and stock show, and Add to cart works in one step; the size still appears as text.
[Recommended] (G6, Q4)

8 answers: same as shared, except 1. **Branches:** doesn't apply, because there's one size; AC-11 and AC-12 don't apply for the same reason.

### 2.4 State C: backorder / preorder (Yellow trumpet pitcher)

**AC-C1** Given Yellow trumpet pitcher, When the page loads, Then the stock line reads "Available on backorder", both sizes (4" $18, 6" $32) are backorderable, and Add to cart is offered with no quantity maximum.
[Observed: yellow-trumpet-pitcher stock indicator, variation data `backorders_allowed: true`, `max_qty` empty]

**AC-C2** Given Yellow trumpet pitcher, When the shopper reads the description, Then it says "Dormant now. Preorder ships in spring." and notes this is store policy.
[Observed: yellow-trumpet-pitcher "About this plant"]

**AC-C3** Given a backorderable size is chosen, When the shopper sees the button, Then a ship-timing note sits next to it using one term throughout (page, button area, cart line), with wording and timing from Q1.
[Recommended] (G2) No ship window is shown until Tania (assumed) confirms it.

**AC-C4** Given a backorderable size, When the shopper sets quantity, Then the maximum is the cap decided in Q3.
[Recommended] (G4)

8 answers, where they differ from shared:
1. **Branches.** Backorder vs in stock changes what the button area must say (AC-C3).
3. **Who arrives.** A shopper may not know Sarracenia go dormant; the timing note serves them.
5. **What's faked.** "Ships in spring" is a claim, not a confirmed policy.
6. **Left out on purpose.** How a backorder line looks in the cart: not in these inputs.
7. **Open.** Q1, Q3.
8. **Dependencies.** Backorder setting per size in WooCommerce (Engineering lead, assumed); ship timing (Tania, assumed).

### 2.5 State D: out of stock (Cobra lily)

**AC-D1** Given Cobra lily, When the page loads, Then the stock line reads "Out of stock", both sizes (4" $52, 6" $90) are out of stock with no backorders, and the select, quantity and Add to cart still render.
[Observed: cobra-lily stock indicator and variation data]

**AC-D2** Given every size is out of stock, When the page loads, Then Add to cart and quantity are not offered, the sizes and prices stay visible as information, and the area reads "Out of stock".
[Recommended] (G1, Q2)

**AC-D3** Given a plant where some sizes are out of stock and some aren't, When the shopper opens the select, Then out-of-stock sizes are labeled "(out of stock)" and can't be added.
[Recommended] (G5) Not shown in the prototype.

8 answers, where they differ from shared:
1. **Branches.** All sizes unavailable vs some (AC-D2, AC-D3).
4. **Loading, error...** The add-to-cart states don't apply when nothing can be added.
6. **Left out on purpose.** Notify-me / restock alerts (Q2).
7. **Open.** Q2.

---

## 3. HANDOFF.md

### Flow and navigation

Catalog or "Also hungry" row → product page → choose pot size → set quantity → Add to cart → stay on page with confirmation (AC-09, Q5) → View cart (out of scope beyond the link). Breadcrumb: Home / Plants / category / plant [Observed: breadcrumb block].

### Screen specs

See section 2: shared AC-01 to AC-15, State A (AC-A1, AC-A2), B (AC-B1, AC-B2), C (AC-C1 to AC-C4), D (AC-D1 to AC-D3).

### Logic and state

| Rule | Tag |
|---|---|
| One pot size per cart line; size required before adding | [Source: requirement] AC-01 |
| Size select starts empty | [Observed] AC-02; one-size exception [Recommended] AC-B2 |
| Price, stock, max follow the chosen size | [Recommended] AC-04, AC-11 |
| Quantity 1 to stock for tracked in-stock sizes | [Observed] AC-06 |
| Backorder quantity cap | Open, Q3; [Recommended] AC-C4 |
| Out of stock: no add | [Recommended] AC-D2, AC-D3 |
| Same plant and size added twice | Open, Q6 |
| Busy, success, error states | [Recommended] AC-08 to AC-10 |

### Token usage

| Element | Token | Tag |
|---|---|---|
| Body text, control text, control border | `--wp--preset--color--ink` | [Observed: sections.css] |
| Control fill | `--wp--preset--color--white` | [Observed: sections.css] |
| Button fill / border | `--wp--preset--color--ink` | [Observed: global styles] |
| Button text | `--wp--preset--color--dew` | [Observed: global styles] |
| Button hover fill / border | `--wp--preset--color--pitcher` | [Observed: global styles] |
| Focus ring | `--wp--preset--color--bloom` | [Observed: sections.css, global styles] |
| Button font | `--wp--preset--font-family--body`, `--wp--preset--font-size--small` | [Observed: global styles] |
| Title | `--wp--preset--font-size--xx-large` | [Observed: h1 class] |
| Control padding | `--wp--preset--spacing--24` (select), `--wp--preset--spacing--8` (quantity) | [Observed: sections.css] |

Spacing and type: map to the existing scale (`--wp--preset--spacing--*`, `--wp--preset--font-size--*`).

### Values not yet tokenized

Only values the specs use. New tokens are proposals; Tania (assumed) approves, since no design-system owner is named.

| Value | Used for | Proposed token |
|---|---|---|
| `999px` | Pill radius: button, select, quantity | `--wp--custom--radius--pill` |
| `24px` | Media card radius | `--wp--custom--radius--card` |
| `2px` | Control and button border width | `--wp--custom--border--control` |
| `2px` / `3px` | Focus ring width / offset | `--wp--custom--focus--width`, `--wp--custom--focus--offset` |
| `3.25rem` | Control min-height | `--wp--custom--control--height` |
| none yet | Inline error text and border (AC-05, AC-07, AC-10) | `--wp--preset--color--error` |
| none yet | Success status (AC-09) | `--wp--preset--color--success` |
| none yet | Backorder / preorder note (AC-C3) | `--wp--preset--color--status-preorder` |
| none yet | Unavailable size / disabled control (AC-D2, AC-D3) | `--wp--preset--color--disabled` |
| `#b81c23`, `#8fae1b`, `#720eec`, `#f6f5f8`, `#515151` | WooCommerce default notice colors (error, success, info border, surface, text) | Replace with `error`, `success`, and existing `dew` / `ink` once approved |

Values for the "none yet" rows are Tania's (assumed) call; any new color needs 4.5:1 for text and 3:1 for borders against its background.

### Accessibility

- Visible focus on all controls [Observed] AC-14.
- Status messages announced without moving focus [Recommended] AC-04, AC-09.
- Errors inline, next to the control, with focus moved on blocked submit [Recommended] AC-05, AC-07.
- Quantity name includes plant; "Clear" is a button [Recommended] AC-15.
- Contrast passes for current pairs (see gap report). Stock status is text, not color alone.

### What's faked

Whole store (no orders ship); prices from a dated reference snapshot; stock counts (all 12); "Preorder ships in spring"; post-add feedback is platform default, not designed.

### Dependencies

| Dependency | Owner |
|---|---|
| WooCommerce variable products, `pa_pot-size` attribute, per-size stock and backorder settings | Engineering lead (assumed) |
| Price and stock data, re-checked before launch | Tania (assumed) |
| Ship-timing policy for dormant plants | Tania (assumed) |
| Theme tokens (`theme.json`, `free-lunch-tokens.css`) | Tania (assumed) |
| Photo licenses and credit lines (CC BY, CC BY-SA via Wikimedia) | Tania (assumed) |

### Open questions

| # | Question | Owner | Recommendation and why |
|---|---|---|---|
| Q1 | What does a dormant plant's button area say, and when does it ship? "Preorder" or "backorder"? | Tania (assumed) | **Recommendation:** use "Preorder" everywhere and show the window next to the button. One term stops shoppers wondering if these are two different things. |
| Q2 | Out of stock: hide the form, or show sizes with no add? Is notify-me in scope? | Tania (assumed) | **Recommendation:** show sizes and prices, no add, no notify-me in v1. Keeps the info, removes the dead end, adds no new system. |
| Q3 | Quantity cap for backorder sizes? | Tania (assumed) | **Recommendation:** set a cap. The number is her call; unlimited preorders on a live plant is a fulfillment risk. |
| Q4 | Preselect the only size when there's one? | Tania (assumed) | **Recommendation:** yes. Saves a step that carries no decision. |
| Q5 | After adding: stay with a message, open the mini-cart drawer, or go to the cart? | Tania (assumed) | **Recommendation:** stay with a message and badge update. Shoppers often add more than one plant. |
| Q6 | Same plant and size added twice: one merged line or two? | Engineering lead (assumed) | **Recommendation:** merge, capped at stock. Matches WooCommerce default and avoids split lines. |
| Q7 | Who approves new tokens? | No owner yet: Tania decides for now (assumed) | **Recommendation:** Tania approves the table above before build. |
| Q8 | Can design see engineering's coded system, or only brand values? | Engineering lead (assumed) | **Recommendation:** return the token mapping in the handback. |
| Q9 | Is "pick a plant" only the product page, or also a catalog / chooser? | Tania (assumed) | **Recommendation:** product page only for this ticket; a chooser is a separate requirement. |

---

## 4. Builder brief

### What to attach, and access check

- This document.
- The four product URLs: `https://lab.assistantsinthewild.fyi/product/{venus-flytrap, yellow-trumpet-pitcher, cobra-lily, spoon-leaf-sundew}/`.
- `free-lunch-tokens.css` (`:root` block) and the theme's `theme.json` and `assets/css/sections.css`.
- **Access check:** confirm you can open every URL and both files before starting. If one won't open, stop and say which.

### Tasks, in order

1. Read section 2 and list any criterion you can't build as written.
2. Out-of-stock handling: AC-D2, AC-D3 [Recommended, pending Q2].
3. Backorder note and cap: AC-C3, AC-C4 [Recommended, pending Q1, Q3].
4. Size required and inline error: AC-05 [Recommended].
5. Size-driven price, stock, max: AC-04, AC-06 [Observed], AC-11 [Recommended].
6. Busy, success, error: AC-08, AC-09, AC-10 [Recommended, AC-09 pending Q5].
7. One-size preselect: AC-B2 [Recommended, pending Q4].
8. In-stock line text: AC-A2 [Recommended].
9. Accessibility fixes: AC-15 [Recommended]; keep AC-14 [Observed].
10. Verify every [Observed] criterion still holds after your changes.

### Fixed rules and platform limits

- Stay on WooCommerce variable products; price and stock come from WooCommerce data, never hardcoded.
- Requirement scope ends at the cart [Source: requirement]; checkout is out of scope.
- Keep the photo credit lines; images are CC BY / CC BY-SA.
- Re-check before launch: prices (dated Oct 31, 2026), stock counts, care, pet-safety and legal claims on each page.
- Don't add to cart or change data on the live lab site while building; use a local copy.

### Tokens only

Use the token names in HANDOFF.md. A value with no token becomes a proposed token in the "Values not yet tokenized" table, never a new hex or px in code.

### Flag, don't guess

Q1 (ship wording and timing), Q2 (out-of-stock behavior), Q3 (backorder cap), Q4 (preselect), Q5 (post-add behavior), Q6 (merge lines), Q9 (plant chooser). Build the [Recommended] version behind a clear flag or leave a TODO naming the question; don't invent ship dates, limits or copy.

### Handback

1. A list of anything you couldn't build or verify, with why.
2. Token mapping:

| Brand token | Your token | Match / close match / no match |
|---|---|---|
| `--wp--preset--color--ink` | | |
| `--wp--preset--color--dew` | | |
| `--wp--preset--color--pitcher` | | |
| `--wp--preset--color--bloom` | | |
| `--wp--preset--color--white` | | |
| `--wp--preset--font-family--body` | | |
| `--wp--preset--font-size--small` | | |
| `--wp--preset--spacing--8`, `--24` | | |
| proposed radius, border, focus, status tokens | | |

### Untested, said out loud

- I read server HTML and CSS only. I did not run scripts, choose sizes, press Add to cart, open the mini-cart or view the cart.
- WooCommerce's default behavior for no size chosen, unavailable sizes and add-to-cart notices is assumed, not seen.
- Whether the in-stock stock line fills in after scripts run is unknown (G3).
- No screen reader, keyboard-only or mobile pass was run; contrast was computed from token values.

---

Approximate word count: 4,200
Acceptance criteria: 26
Tags: Source 1, Observed 11, Recommended 14
