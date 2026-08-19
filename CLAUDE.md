# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A static marketing website for Wrightwood Consulting, a UK building-safety/cladding-remediation consultancy. There is no build process, no package manager, and no JavaScript framework — every page is a single self-contained `.html` file with inline `<style>` and (where needed) inline `<script>`. There is nothing to install, build, lint, or test.

## Running / previewing

Open the `.html` files directly in a browser, or serve the directory with any static file server, e.g.:

```
python -m http.server 8000
```

Then visit `http://localhost:8000/index.html`.

## Deployment

The contact form (`contact.html`) posts to Netlify (`data-netlify="true"`, hidden `form-name` input), so this site is expected to be hosted on Netlify. There is no `netlify.toml` — deployment is effectively "push HTML, Netlify serves it as-is." Pushing to `main` is equivalent to a deploy; there is no CI/build step.

## Site structure

- `index.html` — the main landing page. One long single-page scroll with anchor-linked sections: `#services`, `#fraew`, `#design`, `#explainer`, `#ten-steps`, `#approach`, `#process`, `#contact`.
- `building-safety.html` — a standalone split-screen landing page (dark hero + lead-gen form) used as a secondary conversion page ("Get a quote" in the footer). Its form has no `name`/`action`/`data-netlify` wiring yet — it's presentational only, unlike `contact.html`.
- `contact.html` — the primary lead-gen page: a 5-step multi-step form (`#step-1`...`#step-5`) with sidebar step navigation, client-side step switching, and file upload preview, all driven by a single inline `<script>` at the bottom of the file. Submits via Netlify Forms (`method="POST" action="/" data-netlify="true"`).
- `thank-you.html` — static confirmation page shown after form submission.

## Conventions to preserve when editing

- **Design tokens**: every page redeclares the same CSS custom properties in `:root` (`--cream`, `--warm-white`, `--charcoal`, `--mid`, `--light`, `--border`, `--accent`, `--accent-light`). These are copy-pasted per file, not shared via a common stylesheet — if you change a color, update it consistently across all four HTML files.
- **Fonts**: `EB Garamond` (serif, headings/emphasis) + `DM Sans` (sans, body/UI), loaded from Google Fonts via the same `<link>` in every page's `<head>`.
- **No external JS libraries or bundlers**: any interactivity (multi-step form logic, file input previews) is written as plain inline `<script>` at the bottom of the relevant page. Keep new interactivity dependency-free and inline, consistent with the existing pattern.
- **Numbered/staged content blocks**: sections like services, the FRAEW review steps, the 10-stage remediation process, and design pillars all follow a repeated "numbered card" HTML/CSS pattern (`*-num`, `*-title`, `*-desc` classes per card). When adding a new item to one of these lists, copy the existing markup pattern for that section rather than introducing a new structure.
- **Netlify form requirements**: if you touch `contact.html`'s form, preserve the hidden `form-name` input and `data-netlify="true"`/`name` attributes on the `<form>` — Netlify's form detection is static-parse-based and breaks silently if these are altered or the form is made dynamic.
- Internal navigation between pages uses relative links (`index.html#services`, `contact.html`, `building-safety.html`) — keep cross-page anchors in sync when renaming section `id`s.
