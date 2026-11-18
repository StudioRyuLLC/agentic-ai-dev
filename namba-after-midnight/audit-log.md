# Audit Log — Namba After Midnight

| | |
|---|---|
| **Audited file** | `namba-after-midnight/index.html` (1,010 lines; HTML + inline CSS + inline JS) |
| **Audited against** | `design-standard.md` (primary), `CLAUDE.md`, `site-contract.md`, `content-model.md`, `PRD.md` |
| **Date** | 2026-11-18 |
| **Stance** | Problem-finding review. Clean areas are stated once and skipped. |
| **Status of corrections** | **Recommended only. No change has been made to `index.html`.** |

## How this audit was performed (and its limits)

- **Static review** of the full source against the spec documents.
- **Computed** WCAG contrast ratios from the hex values (script, not estimated).
- **Rendered** the page in headless Chrome at 390, 768, 900, 1024, 1200 px (plus 844×390 landscape) to measure layout and computed styles.
- **Limit — fonts:** the sandbox has no network access, so Google Fonts did not load. Measurements used fallback fonts, so widths can differ slightly from production. Items that depend on this are marked **(verify with real fonts)**.
- **Limit — no screenshot was supplied.** Visual findings come from code plus the headless render, not from a design-review screenshot.
- **Limit — interaction states** (hover, active, focus, timing) are **not verifiable from a static image**. Dimension 5 is flagged for manual verification; findings there are code-derived. A manual checklist is in Section 5.
- **Not tested:** real browsers other than Chromium, screen readers, real touch devices, audio playback.

**Severity:** High = broken or blocks use / spec-critical · Medium = visible defect or spec deviation · Low = polish / hygiene.
**Category:** `structural` (markup, semantics, architecture) · `consistency` (tokens, patterns, spec drift) · `visual` (what the user sees).

## Summary

| Dimension | Verdict | High | Med | Low |
|---|---|:-:|:-:|:-:|
| 1. Color palette | Tokens exact; 6 deviations in use | 0 | 2 | 4 |
| 2. Typography | Fonts correct; 1 spec-breaking gap | 1 | 1 | 3 |
| 3. Spacing & layout | Breakpoints correct; 10 findings | 2 | 4 | 4 |
| 4. Atmosphere & texture | On-brief; 2 deviations | 0 | 2 | 3 |
| 5. Interaction quality | **Manual verification required**; 1 confirmed defect | 1 | 3 | 5 |
| Cross-cutting (CLAUDE.md / PRD / code) | — | 0 | 3 | 4 |

Top priorities: **L-01** (tools tab covers content), **L-02** (mobile header overflow), **T-01** (Japanese text not in Noto Sans JP in English mode), **I-01** (ticker-pause focus ring clipped), **X-01** (jukebox does not meet PRD).

---

## 1. Color palette

**Matches the standard**
- All six spec colors are defined as tokens with the exact hex values (`--dark #100F26`, `--red #BF2A2A`, `--light #F2E4C9`, `--orange #F2A950`, `--pink #F23D91`, `--white #ffffff`, lines 25–30).
- Roles are sensible: `--dark` page background, `--light` body text, `--pink` accent and section headings, `--orange` secondary accent and prices, `--red` masthead band and "home" pill.
- Favicon (line 11) matches `content-model.md`: dark circle, orange crescent, pink outline star.
- Adjacent colors `--dark-2`, `--dark-3`, `--light-muted` are permitted by PRD ("adjacent/complementary colors for accessibility, support, contrast") and are tokenised.
- Computed contrast (normal mode): light on dark 14.95:1; muted on dark 10.37:1; pink on dark 5.23:1; pink on raised 4.66:1; orange on dark 9.45:1; white on red 5.87:1; light on red 4.67:1. All text pairs pass AA.

**Deviations**

| ID | Cat. | Sev. | Location | Problem | Suggested resolution |
|---|---|---|---|---|---|
| C-01 | consistency | Med | L200 (hero gradient), L317 (footer bg) | `#0a091c` and `#241345` are hardcoded, undeclared, and not in the palette. `#241345` is a purple with no relation to any of the six colors, and it is the hero's bottom color. They are not adjacent-for-contrast; they are decoration. | Either tokenise as sanctioned adjacents (`--dark-0`, `--dark-plum`) and record them in `design-standard.md`, or drop the gradient for `--dark` plus a `--dark-2` band. |
| C-02 | visual | Med | L587–588 (`#2fc9a0` nori and scallion in the ramen bowl) | The only green on the page. It is outside the six-color palette and is not serving accessibility, support, or contrast. It breaks the pink/orange/red/cream neon scheme in the hero's focal illustration. | Redraw the garnish with `--light` or `--orange` strokes. If green is wanted, get owner sign-off and tokenise it. |
| C-03 | consistency | Low | SVG art L474–590; L227 `rgba(242,61,145,.25)`; L271 `rgba(242,61,145,.15)`; L129, L358 `rgba(0,0,0,…)` | Palette colors are re-typed as raw hex or rgba instead of using tokens. In high-contrast mode the hero art stays in normal-mode colors, because the art cannot be re-themed. | Use `stroke="currentColor"` or CSS classes bound to tokens inside the SVGs. Use `color-mix(in srgb, var(--pink) 25%, transparent)` for glows. |
| C-04 | consistency | Low | L58, L61, L172, L179, L260, L307 (`#FFEB3B`) | The high-contrast yellow is repeated as a literal six times and never declared as a token. | Add `--hc-accent: #FFEB3B` and reference it. |
| C-05 | consistency | Low | L31 comment | Comment says muted text is "8.9:1 on dark". Computed value is 10.37:1. A wrong number in a contrast comment will mislead the next edit. | Correct the comment. |
| C-06 | visual | Low | L180 (`★` in `--orange` on `--red`) | 2.95:1. Decorative, so exempt from the text-contrast rule, but it is the weakest pair on the page. Pink on `--dark-2` (4.66:1) also has almost no headroom: lightening the raised surface slightly would fail AA for `.menu h3`. | Leave decorative, or switch the star to `--light`. Note the pink/raised constraint beside `--dark-2`. |

---

## 2. Typography

**Matches the standard**
- English: `"Tilt Neon"` on `body` (L79). Japanese: `"Noto Sans JP"` swapped onto `body`, `button`, `input` when `data-lang="ja"` (L85–87), with `line-height: 1.9` for Japanese (L88).
- Both families load via one Google Fonts request with `display=swap` and `preconnect` (L13–15). Noto Sans JP weights 400 and 700 are loaded to support the bold toggle.
- `<html lang>` is updated on toggle (L891), and inactive-language spans are `display:none`, so assistive tech reads one language only.
- Sizes use `rem`, so the text-size control scales everything.

**Deviations**

| ID | Cat. | Sev. | Location | Problem | Suggested resolution |
|---|---|---|---|---|---|
| T-01 | visual | **High** | L604 (`<em>「ほな、行こか!」</em>`), L742 (`日本語` button); font rules L79, L85–87 | **In English mode, Japanese characters are not set in Noto Sans JP.** The font swap is keyed only to `html[data-lang="ja"]`. Measured computed `font-family` for both elements in EN mode: `"Tilt Neon", system-ui, sans-serif`. Tilt Neon has no Japanese glyphs, so they fall to the OS font. The standard says Japanese text uses Noto Sans JP. The `lang="ja"` attribute on the button is present but no CSS uses it. Likely also affected: `★` (L180), `→` (L818), `−` (L729). | Add `:lang(ja) { font-family: "Noto Sans JP", "Hiragino Sans", "Yu Gothic", sans-serif; }`. Add `lang="ja"` to the `<em>` on L604. Or append `"Noto Sans JP"` to the English stack before `system-ui`. |
| T-02 | visual | Med | L68–69 (bold toggle) vs L15 (`family=Tilt+Neon`) | The bold toggle sets `font-weight: 700`, but Tilt Neon is requested and published as a single Regular weight. English "bold" will be browser-synthesized faux bold, which thickens and smears neon-style letterforms. Japanese has a real 700. Confirm on fonts.google.com. **(verify with real fonts)** | Add `font-synthesis: none` and give English a deliberate bold substitute (`-webkit-text-stroke: .03em currentColor` or a size/contrast bump), or document that bold differs by language. |
| T-03 | consistency | Low | `design-standard.md` | The spec names "Noto Sans Japanese". No such Google Fonts family exists. The code correctly uses `Noto Sans JP`. | Update the spec wording to "Noto Sans JP". |
| T-04 | consistency | Low | L215–220 (h1 clamp), L412 (`.hero h1 { font-size: 5.2rem }`) | The h1 is fluid via `clamp(…, 4.6rem)`, then a breakpoint hard-jumps it to 5.2rem at 1400 px. The jump breaks the fluid scale; `body` size also steps at 1200 px (L407). | One `clamp(2.2rem, 7vw, 5.2rem)` and drop the 1400 override. |
| T-05 | consistency | Low | throughout | Ad hoc sizes: `.78 .85 .9 .95 1 1.1 1.15 1.2 1.25 1.35 1.4 1.5 1.6 1.8rem`. No type tokens. | Define 5–6 `--fs-*` steps. |

---

## 3. Spacing and layout

`design-standard.md` leaves spacing open ("keeping this quite wide open"). There is no spec to match, so this section audits **internal consistency** and the **breakpoint and behavior rules in `site-contract.md`**.

**Matches**
- All five breakpoints from the contract are implemented, mobile-first, with the correct `min-width` values: 576 (2-col grid, jukebox meta), 768 (nav links, eats split), 992 (3-col grid, wider gutters), 1200 (type, gap), 1400 (max-width 1280).
- Header is fixed, transparent over the hero, and goes solid past 80 px (L836), exactly as the contract says. `scroll-padding-top` accounts for the header plus masthead.
- Footer reserves `6rem` bottom padding so fixed controls do not cover it. The table sits in a keyboard-focusable scroll wrapper. Hero uses `100vh` with an `svh` upgrade.

**Deviations**

| ID | Cat. | Sev. | Location | Problem | Suggested resolution |
|---|---|---|---|---|---|
| L-01 | visual | **High** | `.tools` fixed left (L324–325); `.container` padding L115, L404; only hero compensates (L415) | **The tools tab covers page content.** The toggle is 46 px wide at the left edge, vertically centered, always visible. Measured content left edge: 20 px at ≤900 px, 32 px at 992–~1170 px, so the tab overlaps content by ~27 px and ~15 px at mid-viewport height. Only the hero (and only below 576 px) pads for it. Cards, menu, and table text sit under the tab. | Add left gutter to `.container` below ~1170 px (`padding-left: max(1.25rem, 3.25rem)`), or reduce the tab to a slim ~1.25 rem handle. Apply to all sections. |
| L-02 | visual | **High** | `.nav` L131; `.brand` L132 (`white-space: nowrap`); `.clock` L141 | **Header content overflows at 390 px.** Measured (fallback font): nav children extend to x=405 px against a 370 px nav and a 390 px viewport, so the menu button is pushed partly off-screen. `body{overflow-x:hidden}` hides the evidence (scrollWidth reports 390). **(verify with real fonts)** | At ≤575 px hide `.brand-text` (logo mark only) or drop the clock icon. Add `min-width:0` and ellipsis to the brand. Remove `overflow-x:hidden` while debugging. |
| L-03 | visual | Med | nav links L477–481 | **Nav wraps at 768 px.** At 768 px "Where to go / What's open" renders on two lines (79 px tall vs 49 px for the other links) inside a 68 px header. Worsens with text-size 140%. | Shorten the label to "Where to go", or switch the desktop nav on at 992 px and keep the hamburger until then. |
| L-04 | visual | Med | `.hero` L196–200 `padding-bottom: 14rem`; `.layer-bowl` | **Hero CTA overlaps the ramen-bowl art.** Measured: hero content bottom exceeds the bowl SVG top by 47 px at 1280×720, 47 px at 1024×600, 51 px at 844×390. The SVG box includes empty space, so real overlap with the steam and chopsticks needs eyes on it. **(verify visually)** | Increase bottom padding to the bowl height (`calc(min(520px,70vw) * .58 + 2rem)`), or shrink the bowl on short viewports. |
| L-05 | structural | Med | L478–480 vs L612, L646, L682 | **Nav order does not match page order.** Nav: Schedule, Eats, Where to go. Document: Eats, Where to go, Schedule. The first nav click scrolls past two sections. | Reorder the nav to Eats, Where to go, Schedule, or move the schedule section first. |
| L-06 | consistency | Med | L45 `--space: 1rem` (never used) | **No spacing scale.** 54 distinct `rem` values appear in the file (`.05 … 14rem`). The one spacing token is dead. | Define `--sp-1…--sp-8` (.25, .5, .75, 1, 1.5, 2, 3, 4.5rem) and replace literals over time. |
| L-07 | visual | Low | L415 (`padding-inline: 3.5rem 1.25rem`) | Hero content is intentionally pushed right below 576 px to clear the tab, which leaves centered text **18 px off-center** (measured). | Fixed by L-01's global gutter; then make hero padding symmetric. |
| L-08 | consistency | Low | L440, L482, L535, L549, L707, L708 | Six inline `style=` attributes while everything else is class-based. CLAUDE.md asks for presentation kept separate. | Move to classes (`.nav-end`, `.footer-line`, `--delay`). |
| L-09 | structural | Low | L630, L635 | `<h3>` is a direct child of `<ul class="menu">`. Invalid HTML (only `<li>` is allowed). | Wrap each group in its own `<section>`/`<ul>` with the `<h3>` outside the list. |
| L-10 | structural | Low | L682 and L688; L707 | Two regions share `aria-labelledby="trains-h"` (same accessible name twice). `<time datetime="">` is empty until JS runs. | Give the table region its own label (the caption). Render a static `datetime`. |

---

## 4. Atmosphere and texture

**Matches the standard**
- *Nightlife / neon:* pink text-glow on the h1 (L217) and shop sign (`.shop .sign`), flickering neon-sign groups in the city art (L545–552), twinkling stars, falling stars and ramen, steam, bobbing bowl, a dark navy-to-plum night sky.
- *Flat, vector, color line art:* all imagery is inline SVG stroke art in palette colors (moon and stars, skyline, ramen bowl, shopfront, a 24-symbol icon sprite). No raster images, no photos.
- *PRD texture* ("flat design, parallax scrolling, spacious"): three-layer parallax at speeds 0.15, 0.3, 0.45, disabled under reduced motion.

**Deviations**

| ID | Cat. | Sev. | Location | Problem | Suggested resolution |
|---|---|---|---|---|---|
| A-01 | visual | Med | L129 (header), L358 (jukebox), L200 (hero gradient) | Standard says flat design. These use conventional dark drop shadows (`0 6px 24px rgba(0,0,0,.35)`, `0 8px 30px rgba(0,0,0,.4)`) and a multi-stop gradient. Neon glows are on-brief; neutral elevation shadows are not. | Replace with 1 px `--line` borders or tinted neon glows. Use a flat `--dark` hero with the plum as a solid band. |
| A-02 | visual | Med | SVG `stroke-width` 2–5 in user units, scaled by `width:100%` (L520, L535, L566, L623) | **Line weight is not normalised.** The moon SVG (1100 units wide) renders at ~0.35× on a 390 px phone, so its 3-unit stroke becomes ~1 px, while the icon sprite and bowl stay heavy. The line-art "system" reads at different weights by device. **(verify visually)** | Add `vector-effect: non-scaling-stroke` to art paths and set one stroke token per tier (art 2.5 px, icons 1.75 px). |
| A-03 | visual | Low | L866–870 | Falling items spawn at `left: 10–110%` with a −18 vw drift: the left 10% of the hero is sparse and up to 10% of spawns start off-screen. | `left: rand*118%`. |
| A-04 | visual | Low | L217, `.shop .sign` only | Neon glow exists on two elements. Section h2s (pink) and the clock pill (pink outline) have none, so "neon" is applied unevenly. | Introduce a `--glow` token and apply it deliberately, or leave it as is. |
| A-05 | visual | Low | `.hero-content` over `.layer-*` | Hero text sits on line art with no scrim. Contrast against stroke crossings was not measurable statically. **(verify visually)** | Check the lede at 1.4× text size over the skyline strokes. |

---

## 5. Interaction quality — ⚠ MANUAL VERIFICATION REQUIRED

Interaction states are not visible in a static image. The items below are code-derived. The manual checklist at the end of this section must be run by a person.

**Matches the standard (by code)**
- `--t: 250ms ease-out` (L40) is used by every `transition` in the stylesheet. The only other `transition` declaration is the reduced-motion override (L427). Verified by grep.
- Hover is defined for each interactive type: links, nav pills, brand, menu button, buttons, chips, jukebox buttons, ticker pause, back-to-top, tools tab, cards, menu rows, table rows.
- Global `:focus-visible` is a 3 px white outline with 3 px offset (L100). A skip link is present. Esc closes the tools panel and returns focus.
- `prefers-reduced-motion` disables animation, parallax, and the falling layer. Smooth scroll is native with no libraries, per contract.

**Deviations**

| ID | Cat. | Sev. | Location | Problem | Suggested resolution |
|---|---|---|---|---|---|
| I-01 | visual | **High** | `.masthead { overflow:hidden }` L163; `.ticker-pause` L182; global focus L100 | **Focus ring on the ticker pause button is clipped.** The button is full masthead height; its 3 px outline with 3 px offset is cut by `overflow:hidden`. Confirmed in render: only a sliver on the left edge is visible. This is the control that stops moving content (WCAG 2.2.2). Fails visible-focus intent (2.4.7 / 2.4.11). | `.ticker-pause:focus-visible { outline-offset: -3px; }` |
| I-02 | visual | Med | L309–310 | **No hover feedback on highlighted rows.** `tr.is-home td` sets a cell background, and cell backgrounds paint above `tbody tr:hover`'s row background, so 3 of 6 rows ("home" trains) cannot show hover. The hover color is also `--bg-raised`, the same as the surrounding `.alt` section, so it reads as a hole. By CSS painting order; confirm manually. | `tbody tr:hover td { background: color-mix(in srgb, var(--pink) 20%, var(--bg-raised)); }` |
| I-03 | consistency | Med | L271 (`.card:hover`), L292 (`.menu li:hover`) | **False affordances.** Cards lift, glow pink, and re-border on hover, and menu rows indent, but neither is a link or control. Hover suggests clickability that does not exist. | Drop hover on static content, or make the cards real links. |
| I-04 | structural | Med | L483–485 | **Clock hides its own value.** `role="timer"` with `aria-label="Current time"` wraps a `<span id="clock" aria-hidden="true">`. Assistive tech gets "Current time" and never the time. Content model requires the time in the nav. | Remove `aria-hidden` from `#clock`. |
| I-05 | consistency | Low | L292, L333 | `padding-left` and `width` are transitioned (layout-triggering). | Use `transform` for the row nudge. Fine to keep `width` if the panel stays simple. |
| I-06 | consistency | Low | keyframes L191, L234–252; ticker 55 s linear | Ambient loops use `linear`, `ease-in-out`, `steps(1)`, not `ease-out`. The standard says "Transitions"; whether ambient animation is in scope is an owner call. | Confirm scope and record it in the spec. |
| I-07 | consistency | Low | UI icons | Micro-animation of iconography exists only in the hero art. UI icons (speaker, play/pause, tools, card icons) are static. The spec says "could", so it is optional: partial, not a defect. | Optional: a small `transform` on icon hover, 250 ms ease-out. |
| I-08 | visual | Low | L729–731, L907–911 | Text-size buttons show no current or limit state. Users cannot tell which size is active. | `aria-pressed` on the active size, `disabled` at limits. |
| I-09 | structural | Low | L154, L856, L884 | Mobile menu has no Esc or outside-click close and no focus management. | Mirror the tools-panel Esc handler. |

**Manual verification checklist**
- [ ] Hover, focus-visible, and active for each element type; transition timing 250 ms ease-out in DevTools.
- [ ] Ticker pause: button, hover-pause, focus ring visible (I-01).
- [ ] Table row hover on "home" rows (I-02).
- [ ] Tools panel open/close, Esc returns focus, no focus reachable while closed.
- [ ] Mobile menu open/close at 390 px (also L-02).
- [ ] `prefers-reduced-motion: reduce` emulation: parallax, falling, ticker, transitions.
- [ ] High-contrast mode: focus ring color, hero art, masthead.
- [ ] Text size 140%: header, nav wrap (L-03), hero.
- [ ] Jukebox: start needs a click (autoplay policy), track skip, volume, language change mid-play.

---

## Cross-cutting findings (CLAUDE.md / PRD / site-contract / code quality)

| ID | Cat. | Sev. | Location | Problem | Suggested resolution |
|---|---|---|---|---|---|
| X-01 | consistency | Med | L915–923 vs `PRD.md` "Nice to have(s)" | **Jukebox does not meet the PRD.** PRD: "play at least 3 Tatsuro Yamashita's hits from the 90s." Implementation: three Web Audio–synthesized original loops ("Midnight Neon", "Last Train Groove", "Dawn Kissaten"). A code comment records the licensing rationale, but the PRD was not amended. It also adds prev/next buttons, title, and status beyond `content-model.md` ("on/off, volume"). CLAUDE.md: "Do not invent features"; identify spec conflicts and ask. | **Owner decision needed:** (a) amend the PRD to original city-pop-style tracks, or (b) supply licensed audio. Record the added prev/next as accepted or remove it. |
| X-02 | structural | Med | L697 `<tbody id="train-rows">`, ticker L503 | **Train schedule exists only via JS.** With JS off or failed, the table is headers only and the ticker is empty. The schedule is a PRD should-have. | Ship the rows as static HTML and let JS enhance. |
| X-03 | structural | Med | L1001–1006 (prefs restored at end of `<body>`) | **Flash of default state.** Returning users with Japanese, high contrast, or larger text see English/normal first, then a reflow. | Inline `<script>` in `<head>` that sets `data-lang`/`data-contrast`/`data-size` from storage before first paint. |
| X-04 | structural | Low | L896 | `datetime` uses `toISOString()` (UTC) while the visible date is local. They disagree for part of the day in UTC-offset timezones. | Build the `YYYY-MM-DD` from local date parts. |
| X-05 | structural | Low | L860 | Clock uses the viewer's locale and timezone, not Asia/Tokyo. Spec only says "current time". Open question for the owner: viewer's time or Namba time? | Decide; if Namba, add `timeZone: 'Asia/Tokyo'`. |
| X-06 | structural | Low | L779 | `reduceMotion` is read once at load; mid-session OS changes are not honored by JS (parallax, falling spawn). CSS responds live. | Listen to `matchMedia(...).addEventListener('change', …)`. |
| X-07 | consistency | Low | L45; L350 `.chip[aria-checked]`; sprite symbols `i-moon`, `i-gear`, `i-globe`, `i-close`; L152; L988; L918–1000 | Dead and sloppy code: unused `--space`; unused `[aria-checked]` selector; four unused icon symbols; `.menu-btn` border rule equal to `--line`; `wantOn = on && true`; the jukebox block is un-indented inside the IIFE. | Remove or fix. |

---

## Clean / no-finding areas

- Palette token values (exact match to spec).
- Language switch of `<html lang>`, title, and hidden-language handling.
- Breakpoint set and mobile-first structure per `site-contract.md`.
- Sticky header solid threshold (80 px) and back-to-top threshold (400 px).
- Reduced-motion handling in CSS.
- External link: `target="_blank"` with `rel="noopener noreferrer"` and an sr-only "(opens in a new tab)" (L710).
- Content coverage per `content-model.md`: favicon, nav with time, masthead ticker, hero line, ramen menu, four places, schedule, footer date and StudioRyu link, tools (accessibility and language), bottom-left sound and volume, bottom-right back-to-top.
