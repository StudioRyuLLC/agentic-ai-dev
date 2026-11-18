# Namba After Midnight

A single-page, bilingual (English / 日本語) landing page for late-night commuters who just missed the last train from Namba, Osaka. Dark, neon, flat vector line-art, with a ramen menu, four places to go, and the first trains home.

Built as a static `index.html` (inline CSS and JS, no dependencies beyond Google Fonts).

## Project documents

| Document | Responsibility |
|---|---|
| [`PRD.md`](PRD.md) | Vision, users, features, success criteria |
| [`content-model.md`](content-model.md) | Page sections and content |
| [`site-contract.md`](site-contract.md) | Behavior and breakpoints |
| [`design-standard.md`](design-standard.md) | Palette, typography, texture, interactions |
| [`CLAUDE.md`](CLAUDE.md) | Development workflow |
| [`audit-log.md`](audit-log.md) | Full Director's Audit (every finding, with line numbers) |

---

## Director's Audit — 2026-11-18

`index.html` was reviewed against `design-standard.md` across five dimensions, plus conformance to `CLAUDE.md`, `PRD.md`, `site-contract.md` and `content-model.md`. The full log is in [`audit-log.md`](audit-log.md).

> **Status: audit complete; corrections are recommended and NOT yet applied.** `index.html` is unchanged by this audit.
>
> **Method and limits:** static code review, computed WCAG contrast, and a headless-Chrome render at 390–1200 px. Web fonts could not load in the audit sandbox, so layout measurements used fallback fonts and are marked "verify with real fonts". No screenshot was supplied. **Interaction states (hover, focus, active, timing) cannot be judged from a static image; the Interaction dimension is flagged for manual verification** (checklist in the log).

### Result by dimension

| Dimension | Verdict | High / Med / Low |
|---|---|:-:|
| Color palette | All six spec tokens exact. Off-palette purple and green in use; raw hex repeated instead of tokens. | 0 / 2 / 4 |
| Typography | Correct families per language. Japanese text inside English mode falls back to a system font. English bold is faux bold. | 1 / 1 / 3 |
| Spacing & layout | All five contract breakpoints correct. Tools tab covers content; mobile header overflows; no spacing scale. (Spec leaves spacing open; audited for consistency.) | 2 / 4 / 4 |
| Atmosphere & texture | On-brief (neon, flat vector line-art, parallax). Dark drop shadows and gradient are not flat; stroke weight not normalised. | 0 / 2 / 3 |
| Interaction quality | **Manual verification required.** 250 ms ease-out applied everywhere by code. One confirmed focus defect. | 1 / 3 / 5 |
| Cross-cutting | Jukebox does not meet the PRD; train data is JS-only. | 0 / 3 / 4 |

### Highest-priority findings and recommended corrections

| ID | Finding | Recommended correction |
|---|---|---|
| L-01 | Fixed tools tab (46 px) overlaps page content: content starts at 20 px (≤900 px) and 32 px (992–~1170 px). | Add a left gutter to `.container` below ~1170 px, or slim the tab to a ~1.25 rem handle. |
| L-02 | Header overflows at 390 px; menu button pushed to x=405 on a 390 px screen (fallback font; verify). | Hide `.brand-text` at ≤575 px; add `min-width:0` and ellipsis. |
| T-01 | Japanese characters in English mode (`「ほな、行こか!」`, the `日本語` button) compute to `Tilt Neon, system-ui`, not Noto Sans JP. | Add `:lang(ja) { font-family: "Noto Sans JP", … }` and `lang="ja"` on the `<em>`. |
| I-01 | Ticker-pause focus ring is clipped by `.masthead{overflow:hidden}` (confirmed in render). | `.ticker-pause:focus-visible { outline-offset: -3px; }` |
| X-01 | Jukebox plays three synthesized original loops; PRD asks for three Tatsuro Yamashita hits. | **Owner decision:** amend the PRD, or supply licensed audio. |
| C-01 / C-02 | Undeclared `#0a091c`, `#241345` (purple) and a teal `#2fc9a0` in the ramen art are outside the palette. | Tokenise with sign-off, or redraw in palette colors. |
| L-03 / L-04 / L-05 | Nav wraps at 768 px; hero CTA overlaps bowl art by ~47 px on short viewports; nav order differs from page order. | Shorten label or raise the nav breakpoint; add hero bottom padding; reorder nav. |
| T-02 | Bold toggle on a single-weight font (Tilt Neon) produces faux bold. | `font-synthesis: none` plus a deliberate English bold substitute. |
| I-02 / I-03 / I-04 | No hover on highlighted train rows; hover on non-interactive cards; clock value hidden from assistive tech. | Row-cell hover rule; drop false-affordance hover; remove `aria-hidden` from `#clock`. |
| X-02 / X-03 | Schedule table and ticker empty without JS; preferences applied after first paint. | Static HTML rows; inline head script for preferences. |

### Spec housekeeping surfaced by the audit

- `design-standard.md` names "Noto Sans Japanese"; the Google Fonts family is **Noto Sans JP** (the code is correct).
- Record whether ambient keyframe loops (linear / ease-in-out) fall under "transitions at 250 ms ease-out" (I-06).
- Decide whether the clock shows viewer time or Asia/Tokyo time (X-05).

### Verified clean

Palette token values; `<html lang>` switching; contract breakpoints; header solid at 80 px and back-to-top at 400 px; reduced-motion CSS; external-link attributes; content coverage against `content-model.md`.

### Correction tracking

| Status | Items |
|---|---|
| Applied | none |
| Awaiting owner decision | X-01, C-01, C-02, I-06, X-05 |
| Ready to implement on approval | all other findings in [`audit-log.md`](audit-log.md) |
