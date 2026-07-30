# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

The source for the BiostatOmics Group website (https://biostatomics.com), built with [Quarto](https://quarto.org/) and published to GitHub Pages via GitHub Actions. There is no application code, package manager, or test suite — this is a static site defined by `.qmd`/`.yml` content files and a Quarto project config.

## Commands

- Preview locally: `quarto preview index.qmd` — always preview `index.qmd` specifically, not any other file, to avoid generating unnecessary render artifacts for other pages.
- Render the full site: `quarto render` (normally not needed locally; GitHub Actions does this on push to `main` via `.github/workflows/*.yml`, publishing to the `gh-pages` branch).
- No lint/test/build scripts exist beyond Quarto itself.

## Architecture

### Site structure and navigation

`_quarto.yml` is the project's single source of truth for site-wide config: navbar entries, footer, theme (`cosmo` + `brand`), and global HTML includes. Every top-level page (`index.qmd`, `research_lines.qmd`, `publications.qmd`, `blog.qmd`, `contact.qmd`, `tools/index.qmd`, `people/index.qmd`) must be linked from the `navbar` block here to be reachable — creating a `.qmd` file is not enough on its own.

Files/folders prefixed with `_` or `.` are excluded from rendering (e.g. `_CONTRIBUTING.md`, `_quarto.yml` itself, `_people-template.qmd`). Use this prefix for any file that should exist in the repo but not become a page.

### Tools and People pages are data-driven listings, not hand-written pages

Both `tools/index.qmd` and `people/index.qmd` use Quarto's [custom listings](https://quarto.org/docs/websites/website-listings-custom.html) feature: a `listing:` block in the YAML frontmatter points at a data source and an `.ejs.md` template, and Quarto generates the cards at render time.

- **Tools** (`tools/index.qmd`): data lives in `tools/tools.yml` (packages/tools) and `tools/webapps.yml` (web apps), rendered through `tools/_tools-cards.ejs.md`. Each entry: `title`, `href` (card click-through target — set independently of `links`), `image`, `date`, `section` (`"Tools"` or `"Web apps"` — which heading it appears under), `categories` (filterable tags), `description`, `links` (buttons with `label`/`icon`/`url`). Adding a tool = adding a YAML entry; no `_quarto.yml` or navbar change needed.
- **People** (`people/index.qmd`): each person is their own folder `people/lastname-firstname/index.qmd`, created by copying `people/_people-template.qmd`. The listing page has one `listing:` block per group (`pi`, `phd`, etc., filtered via `include: { people_group: ... }`), each paired with a heading + `:::{#id-listing}` div in the body — both halves (YAML block and body div) must be added/removed together to show or hide a group. Cards render via `people/_people-cards.ejs.md`. The `education` YAML field is an HTML `<ul>` list written once; the card template auto-extracts a short summary from it for the compact group-listing card.

To change the visual layout of cards (not their content), edit the `.ejs.md` template, not the `.yml`/`.qmd` data files. These use Quarto's EJS templating powered by Lodash (note: its escaping/raw print tags are reversed vs. typical `.ejs` docs — see comments in the template files).

### Styling

`styles.css` plus the Quarto `brand` theme (layered with `cosmo` in `_quarto.yml`) control appearance. Card grid layouts (`.tool-card`, `.person-card`, etc.) are defined here and consumed by the `.ejs.md` templates above.

### Rendering/build behavior

`execute: freeze: auto` in `_quarto.yml` means any executable code chunks are run locally and cached in a `_freeze` directory (committed), so GitHub Actions can publish without installing R/Python — don't remove this without understanding the CI impact.

## Content conventions (from `_CONTRIBUTING.md`)

- **Commit messages** follow Conventional Commits: `type(scope): short description`, imperative mood, no trailing period. Types used in this repo: `feat`, `fix`, `docs`, `style`, `chore`.
- **Branching**: changes go on a branch and are merged via PR into `main`, not committed directly to `main`.
- **Icons**: use [Bootstrap Icons](https://icons.getbootstrap.com/) names (e.g. `icon: github`) for consistency; academic icons (ORCID, Google Scholar) are available via the `academicons` pack loaded in `_quarto.yml`.
- **Tool thumbnails**: ~900px wide, transparent background, `.webp` format (optimize via Squoosh or similar); displayed at a fixed 120px height.
- **Person photos**: square (equal width/height) so cards display consistently.
