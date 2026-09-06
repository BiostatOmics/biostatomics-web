# Contributing guidelines
This document defines the conventions to follow when contributing (changing, updating) the website.

## Overview
The website (`.html`) is created using Quarto and hosted on GitHub Pages. Useful references to check are:

- [Quarto documentation: how to build websites](https://quarto.org/docs/websites/)
- [Quarto documentation: how to publish websites using GitHub Pages](https://quarto.org/docs/publishing/github-pages.html)
- [All Quarto yaml options available for websites](https://quarto.org/docs/reference/projects/websites.html) 
- [GitHub Pages documentation for details](https://docs.github.com/en/pages)


## Inspiration
Some users have shared their website repos so anyone can dive into them and get ideas/inspiration!

- <https://github.com/spcanelon/silvia>
- <https://github.com/andrewheiss/ath-quarto>
- <https://github.com/jhelvy/jhelvy_quarto>
- <https://github.com/AtlasOfLivingAustralia/ala-labs>
- <https://github.com/MasielloGroup/MasielloGroupWebsite>

## Building the website

### Create a new page

To create a new page the only thing needed is to create a Quarto valid file (``.qmd``, ``.md``, ``.ipynb``, ``.Rmd``). Quarto will render it and include it in the website. 

Next step is allowing users to access this page. For that, we need to modify the `_quarto.yml` file and add, at least the `text` and `href` keys to the navigation panel (whether `navbar` or `sidebar`) for that page. 

> For example, if I want to create the "People" page and make it accesible at the top navigation bar, I should: 1) create `people.qmd` 2) go to `_quarto.yml` 3) in the `navbar` key, add `text: "People"` and `href: people.qmd`. 

`text` key is optional (by default, `text` is the linked (`href`) document's title), but is a good practice to always add it (except for pages that only require an icon).

That's the basic setup, for more info check <https://quarto.org/docs/websites/website-navigation.html>.


#### Change design and style of a page

As pages are `.qmd` files, those can be modified and designed with great flexibility. The following resources contain detailed information about it:

- [About layout of pages](https://quarto.org/docs/authoring/article-layout.html#screen-column). Also, [this](https://quarto.org/docs/output-formats/page-layout.html#grid-customization). The layout of a page is essential as landing pages, for example, require a different layout than a "text only" page. 
- [About shortcodes (quarto native "snippets")](https://quarto.org/docs/authoring/shortcodes.html)

### Add a new tool

The [Tools](../tools/index.qmd) page is not built from individual `.qmd` files like other pages: it's a Quarto [listing](https://quarto.org/docs/websites/website-listings-custom.html) that renders cards automatically from a single metadata file. This keeps every card the same style.

To add a new tool, add a card to `tools/tools.yml`:

```yaml
- title: "My Tool"
  image: images/my-tool-logo.png # optional, path relative to tools/
  # If a tool has no logo, the alternative is to plot its GitHub preview (via opengraph)
  # image: https://opengraph.githubassets.com/1/username/repo
  date: 2026-01-01 # release/publication date
  section: "Tools" # heading the card appears under: "Tools" or "Web apps"
  categories: [package, R] # filterable tags shown on the card and in the sidebar
  description: "One or two sentences describing the tool."
  links:
    - label: "GitHub"
      icon: github # any Bootstrap Icons name
      url: "https://github.com/BiostatOmics/my-tool"
```

No changes to `_quarto.yml` or the navbar are needed as the card appears automatically the next time the website is rendered.

`section` and `categories` control two different things and are easy to confuse:

- `section` decides which heading the card is grouped under ("Tools" vs "Web apps"). Only these two exist today.
- `categories` are shown on the card (e.g. `R`, `shiny`, `package`) and power the clickable filter in the right sidebar. A tool can have as many as needed.

> For example, adding a new Python package would use `section: "Tools"` (same heading as the R packages) and `categories: [package, python]` (its own filterable tags).

If you need to change how cards look (not just their content), edit `tools/tools-cards.ejs.md` — see Quarto's [custom listing templates](https://quarto.org/docs/websites/website-listings-custom.html) docs.

#### About tool thumbnails

- The card thumbnail is shown at a fixed 120px height and scales the image to fit, so exact pixel dimensions aren't critical. 
- However, for a good result generate the image around 900px wide (height following the image's own aspect ratio, trying to be close to 16:9). 
- Save the image with transparent background.
- Optimize the image converting it to `.webp` using [Squoosh](https://squoosh.app/) or similar.

### Add a new person

The [People](../people/index.qmd) page is not a single page either: it's a Quarto [listing](https://quarto.org/docs/websites/website-listings-custom.html) that gathers one `.qmd` per person and renders a card for each, grouped under the right heading automatically.

To add a new person, copy `people/_people-template.qmd` into a new folder named `lastname-firstname/`, rename it to `index.qmd`, and fill in your own data: the template has a comment above every field explaining what it's for and how it's used, including the photo, the `people_group` (which heading your card appears under), and `education` (a list you fill in once — the group listing card automatically shows a short version of it).

No changes to `_quarto.yml` or the navbar are needed as the card appears automatically the next time the website is rendered.

#### Predoctoral researchers

Active predoctoral researchers (`people_group: "phd"`) get two extra optional fields on top of the regular template: `institution` (a small badge, eg. `"UPV"` or `"UPV-CSIC"`) and `thesis_directors` (their thesis director(s), written once as HTML — so a director with their own page on this site can be linked — and reused both in a hover tooltip on the card and in a "## Thesis directors" section on their own page, via `{{< meta thesis_directors >}}`).

A predoctoral researcher who has already defended their thesis doesn't get their own file at all: add a row directly to the plain markdown table under "## Former Predoctoral Researchers" in `people/index.qmd` (name, thesis title, directors, institution, date, grade). Column widths and Bootstrap table classes (striped, hover, sm, responsive...) are set via the `{...}` attribute line right after the table — see [Quarto's table docs](https://quarto.org/docs/authoring/tables.html#pipe-tables). Each name links to that person's LinkedIn profile.

If you need to change how cards look (not just their content), edit `people/people-cards.ejs.md` — see Quarto's [custom listing templates](https://quarto.org/docs/websites/website-listings-custom.html) docs.

### Avoid rendering of private files/dirs

By default, Quarto renders all valid Quarto files (``.qmd``, ``.md``, ``.ipynb``, ``.Rmd``) to the website. 

To avoid rendering private files to the website (such as `_CONTRIBUTING.md`) use the `_` (for files) or the `.` (for folders) prefixes in the filename. 

More info in <https://quarto.org/docs/websites/#render-targets>.


### Preview the website

When developping in local, use `quarto preview index.qmd` to check the result. Importantly, only preview the `index.qmd` file, avoiding other "previewable" files (such as `_CONTRIBUTING.md`) to avoid generation of unnecesary files.

### Icons

For icons, we use [Bootstrap icons](https://icons.getbootstrap.com/).


## Commit messages

We follow [Conventional Commits](https://www.conventionalcommits.org/) notation.

### Format

`type(scope): short description`

- `type`: one of the allowed types below (required)
- `scope`: the affected section, e.g. `team`, `publications`, `homepage` (optional)
- `description`: imperative mood, present tense ("add", not "added"/"adds"), no trailing period

### Types

| Type | Description | Examples |
|------|-------------|----------|
| `feat` | A new feature or content addition | `feat: add publications page`<br>`feat(team): add new PhD student profile` |
| `fix` | A bug fix or correction | `fix: broken image link in about page`<br>`fix(team): correct typo in member bio` |
| `docs` | Documentation-only changes | `docs: update contributing guidelines`<br>`docs(readme): fix instructions` |
| `style` | Formatting changes with no logic impact (whitespace, CSS, layout) | `style: fix inconsistent indentation`<br>`style(homepage): align navbar items` |
| `chore` | Other tasks (init, dependencies, config, build tools) | `chore: update dependencies`<br>`chore(ci): fix GitHub Actions workflow` |

A complete cheatsheet with more types and examples is available [here](https://gist.github.com/qoomon/5dfcdf8eec66a051ecd85625518cfd13).