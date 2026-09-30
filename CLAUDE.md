# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

Cindy Lewis's personal UX/UI portfolio site: a set of static, self-contained HTML files with no build step, no package manager, and no test suite. There is no `package.json`, bundler, or linter configured — each page is a single `.html` file with its CSS and JS inline.

## Commands

- **Preview locally:** serve this folder with any static file server and open the page you're working on, e.g. `python3 -m http.server 8765` from the repo root, then visit `http://localhost:8765/work.html`. There is no dev server config baked into the repo.
- **Build/lint/test:** none exist. Verify changes by opening the page in a browser and checking it renders and scrolls correctly — there is no automated way to check this.

## Architecture

Two different page systems live side by side.

### Plain static pages (most of the site)

`index.html` (home), `work.html`, `about.html`, `tangerine.html`, `habitanto.html` — each is one self-contained file: inline `<style>`, inline `<script>` at the bottom, no shared stylesheet.

- **`work.html` is the project hub.** A tab UI reads a JS object `DATA[lang]` (`p0` Habitanto, `p1` Tangering — default; `work.html#habitanto` opens `p0`) and renders the active one into `#stage` via `render(d)`. Each entry has `tag`, `title`, `statement`, `italicLine` (rendered as a plain lead line), `details[]` (shown as numbered hairline-top points), optional `heroDevice` (laptop PNG) or `heroPhones[]` (phone PNGs) for the hero, optional `shot` (one wide real screenshot) or `shots[]` (a row of phone screenshots) + `shotCaption`, `badge` (rendered as a mono eyebrow above the closing), `closing`, `href`, `hrefLabel`. The body is one left-aligned column (`.work-body`); there are no placeholder boxes or pending-video blocks any more — if a real capture doesn't exist, leave `shot` out.
- **`soluciones.html` ("Soluciones a medida" / "Custom solutions")** is the freelance page: static sections for ÚNA (una.eco — captures in `assets/una/`), Stone Art Precision (internal client app — phone screenshots in `assets/stone/` with every client name and the owner's name covered by gray bars; never publish unredacted ones) and ProfitPeek (Cindy's own product, profitpeek.site — captures in `assets/profitpeek/`). Built from `work.html`'s shell (same navbar/footer/lang logic); copy lives in its `I18N` dict. Linked from every page's Trabajo dropdown and footer (`nav.custom`).
- **`tangerine.html` and `habitanto.html` are full case studies** and share one template — see below.
- **`about.html`** is the bio/timeline page. Treat it as the source of truth for job dates and role scope when writing case-study copy.

### `x-dc` interactive pages, powered by `support.js`

Only `Portfolio Concepts.dc.html` still loads `support.js` (`index.html` used to be the animated-eye `x-dc` page but is now a plain static page: name + photo-strip hero, roles row, road). `support.js` is a generated runtime (its own header says: *"GENERATED from dc-runtime/src/*.ts — do not edit. Rebuild with `cd dc-runtime && bun run build`"*). It implements a custom `<x-dc>` element plus a `class Component extends DCLogic` pattern for embedding React-driven interactive components directly in the HTML.

The `dc-runtime` TypeScript source referenced in that comment is not part of this repo — `support.js` is a vendored build artifact. Don't hand-edit it; only touch the `<x-dc>...</x-dc>` markup and the `<script type="text/x-dc" data-dc-script">` block inside that file.

## Case-study page template (`tangerine.html` / `habitanto.html`)

When starting a new full case study (Stone Art Precision and ÚNA only have summaries so far — see Pending work), copy the structure of one of these two files rather than starting from scratch:

- navbar linking back to `about.html` / `work.html` plus a LinkedIn pill
- hero: eyebrow, `<h1>`, lede paragraph
- hero image + a numbered "strip" nav (grid of jump links to section ids), with an `IntersectionObserver` driving an `.active` state as the reader scrolls
- alternating two-column `.attempt` / `.attempt.reverse` sections pairing narrative text with real screenshots
- `.verdict.fail` (❌) / `.verdict.win` (✅) badges inside a `.subgrid` to contrast a failed iteration against the one that worked
- `.detail-toggle` — collapsed "+ Ver por qué esto importaba" asides for reasoning that shouldn't be visible by default
- closing `.pull` pull-quote block, then a `.close` section with a single `mailto:` CTA
- a reading-progress bar (`#progress`, updated on scroll) and `.reveal` fade-ins on scroll, same JS pattern in both files

All pages share one brand (set 2026-09-30): white background (`#fff`, surfaces `#f5f5f3`), black ink (`#0b0b0c`), display headings (h1/h2/pull quotes) in `'Didact Gothic'`, body/UI in `'Inter'`, eyebrows/meta in `'IBM Plex Mono'`. The only accent is the neon green of Cindy's glasses, `#35e80c` (`--accent`), used for **details only** — progress bar, current-item markers, active tab ring, link underlines, selection, marker-style highlights. Never use it as text color on white (unreadable); text stays ink. Case studies no longer get their own accent color or heading font. Each non-home page carries the shared rules in the "shared light theme" / "shared brand" blocks at the end of its `<style>`; new pages should copy those blocks.

## Assets

- Real screenshots live under `assets/<project-slug>/`, e.g. `assets/habitanto/residente-home.png`, with descriptive kebab-case names (`residente-…` / `admin-…` prefixes distinguish the two Habitanto apps; `proceso-…` marks process/working artifacts rather than final UI).
- Tab-card hero images for `work.html` live directly under `assets/` as `<slug>-hero.webp`.
- Never invent a screenshot reference. If a feature is described but no real capture exists for it, describe it in prose instead (see the "Fuera de esta página" list at the end of `habitanto.html` for the pattern) rather than pointing an `<img>` at a file that doesn't exist.

## Content accuracy

- Don't add specific metrics or numbers to case-study copy unless Cindy has explicitly confirmed them — describe impact qualitatively when a real figure isn't available.
- Known content debt: `tangerine.html`'s closing CTA still uses a placeholder `mailto:hola@ejemplo.com` instead of Cindy's real address (`uxclewis@gmail.com`, already correct in `habitanto.html`).
- Language is currently inconsistent by design: `work.html` tab teasers and `about.html` are in English; the full case studies (`tangerine.html`, `habitanto.html`) are in Spanish. Match whichever file you're editing rather than "fixing" this unless asked.

## Pending work

Stone Art Precision, ÚNA and ProfitPeek are covered as summaries on `soluciones.html`; none has a full case study yet.
