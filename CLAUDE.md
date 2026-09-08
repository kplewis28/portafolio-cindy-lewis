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

`work.html`, `about.html`, `tangerine.html`, `habitanto.html` — each is one self-contained file: inline `<style>`, inline `<script>` at the bottom, no shared stylesheet.

- **`work.html` is the project hub.** A tab UI reads a single JS object `data` (keys `p0`–`p3`, one per project) and renders the active one into `#stage` via `render(d)`. Each entry has `tag`, `title`, `heroImage`, `statement`, `italicLine`, `details[]`, `badge`, `closing`, `href`, `hrefLabel`. Projects with a finished case study point `href` at their own file (e.g. `./habitanto.html`); projects without one yet point at an anchor inside `El Ojo.dc.html` (e.g. `./El%20Ojo.dc.html#stone-art-precision`) and carry `[Placeholder — …]` copy in `details[]`.
- **`tangerine.html` and `habitanto.html` are full case studies** and share one template — see below.
- **`about.html`** is the bio/timeline page. Treat it as the source of truth for job dates and role scope when writing case-study copy.

### `x-dc` interactive pages, powered by `support.js`

`index.html` (the animated eye home/landing page — this is the deployed site root; was `El Ojo.dc.html` before the Vercel deploy, renamed so `/` resolves) and `Portfolio Concepts.dc.html` load `support.js`, a generated runtime (its own header says: *"GENERATED from dc-runtime/src/*.ts — do not edit. Rebuild with `cd dc-runtime && bun run build`"*). It implements a custom `<x-dc>` element plus a `class Component extends DCLogic` pattern for embedding React-driven interactive components directly in the HTML.

The `dc-runtime` TypeScript source referenced in that comment is not part of this repo — `support.js` is a vendored build artifact. Don't hand-edit it; only touch the `<x-dc>...</x-dc>` markup and the `<script type="text/x-dc" data-dc-script">` block inside these two files.

## Case-study page template (`tangerine.html` / `habitanto.html`)

When starting a new full case study (Stone Art Precision and ProfitPeek still need one — see Pending work), copy the structure of one of these two files rather than starting from scratch:

- navbar linking back to `about.html` / `work.html` plus a LinkedIn pill
- hero: eyebrow, `<h1>`, lede paragraph
- hero image + a numbered "strip" nav (grid of jump links to section ids), with an `IntersectionObserver` driving an `.active` state as the reader scrolls
- alternating two-column `.attempt` / `.attempt.reverse` sections pairing narrative text with real screenshots
- `.verdict.fail` (❌) / `.verdict.win` (✅) badges inside a `.subgrid` to contrast a failed iteration against the one that worked
- `.detail-toggle` — collapsed "+ Ver por qué esto importaba" asides for reasoning that shouldn't be visible by default
- closing `.pull` pull-quote block, then a `.close` section with a single `mailto:` CTA
- a reading-progress bar (`#progress`, updated on scroll) and `.reveal` fade-ins on scroll, same JS pattern in both files

Each case study picks its own accent color and heading font but shares `'IBM Plex Mono'` for body/eyebrow/UI chrome — keep that split for any new case study rather than reusing one project's palette:

| | accent | heading font |
|---|---|---|
| `habitanto.html` | `#1450e6` | Inter |
| `tangerine.html` | `#d9480f` | Space Grotesk |

## Assets

- Real screenshots live under `assets/<project-slug>/`, e.g. `assets/habitanto/residente-home.png`, with descriptive kebab-case names (`residente-…` / `admin-…` prefixes distinguish the two Habitanto apps; `proceso-…` marks process/working artifacts rather than final UI).
- Tab-card hero images for `work.html` live directly under `assets/` as `<slug>-hero.webp`.
- Never invent a screenshot reference. If a feature is described but no real capture exists for it, describe it in prose instead (see the "Fuera de esta página" list at the end of `habitanto.html` for the pattern) rather than pointing an `<img>` at a file that doesn't exist.

## Content accuracy

- Don't add specific metrics or numbers to case-study copy unless Cindy has explicitly confirmed them — describe impact qualitatively when a real figure isn't available.
- Known content debt: `tangerine.html`'s closing CTA still uses a placeholder `mailto:hola@ejemplo.com` instead of Cindy's real address (`uxclewis@gmail.com`, already correct in `habitanto.html`).
- Language is currently inconsistent by design: `work.html` tab teasers and `about.html` are in English; the full case studies (`tangerine.html`, `habitanto.html`) are in Spanish. Match whichever file you're editing rather than "fixing" this unless asked.

## Pending work

`p2` (Stone Art Precision) and `p3` (ProfitPeek) in `work.html` still route to placeholder anchors inside `El Ojo.dc.html` and have unfinished `details[]` copy. They need their own full case-study pages built from the template above once source material (Figma files, specs, etc.) is available.
