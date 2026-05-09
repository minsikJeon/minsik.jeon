# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository purpose

Personal academic website for Minsik Jeon, deployed via GitHub Pages to the custom domain in `CNAME` (msjeon.me). It is a pure static site — no build step, no package manager, no tests. Editing equals deploying once committed and pushed.

## Architecture

Two independent static sites coexist in this repo:

1. **Root site** (`index.html` + `stylesheet.css`) — the personal homepage, forked from Jon Barron's academic template. Layout is hand-rolled HTML tables (intentional, matches the template). Sections are: bio, Education, Publications, Projects. Each publication/project entry is a `<tr>` block with a thumbnail cell and a description cell. Several entries use a hover image-swap pattern: a `.one`/`.two` div pair plus a small inline `<script>` defining `xxx_start()` / `xxx_stop()` functions wired to `onmouseover` / `onmouseout` on the `<tr>`. Image IDs (`smerf_image`, `gb_image`, `iac_image`, `hynix_image`, `nica_image`) are referenced from those inline scripts — **renaming an ID requires updating its script, and reusing an ID across rows breaks the swap because `getElementById` returns the first match**. The active row using `id='smerf_image'` is the DA-RAW row (the ID name is a leftover from the template).

2. **`OWODRep/`** — a separate project page for the WACV 2026 paper, based on the Nerfies template (Bulma + bulma-carousel + bulma-slider, all vendored under `OWODRep/static/`). Self-contained; does not share assets with the root site.

Assets: `images/` (root site), `data/CV_MinsikJeon_*.pdf` (linked from homepage — when updating CV, update the filename in `index.html` too), `OWODRep/static/{css,js,images,videos,interpolation}` (project page).

## Working on this repo

- No build, no lint, no tests. To preview locally: `python3 -m http.server` from the repo root, then open `http://localhost:8000/`.
- Edit `index.html` directly. Adding a publication or project = duplicate an existing `<tr>` block and swap the image, title, authors, venue, and links. Keep the surrounding HTML-comment markers (`<!-- VENUE YEAR : NAME -->` ... `<!-- /VENUE YEAR : NAME -->`) — they're how entries are visually delimited in source.
- If adding a new hover image-swap row, give it a **unique** id and matching unique start/stop function names. Don't reuse existing ids.
- Deployment is automatic via GitHub Pages on push to the default branch. The `CNAME` file controls the custom domain — don't delete it.
