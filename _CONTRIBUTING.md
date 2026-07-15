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

Next step is allowing users to access this website. For that, we need to modify the `_quarto.yml` file and add, at least the `text` and `href` keys to the navigation panel (whether `navbar` or `sidebar`) for that page. 

> For example, if I want to create the "People" page and make it accesible at the top navigation bar, I should: 1) create `people.qmd` 2) go to `_quarto.yml` 3) in the `navbar` key, add `name: "People"` and `href: people.qmd`. 

`text` key is optional (by default, `text` is the linked (`href`) document's title), but is a good practice to always add it (except for pages that only require an icon).

That's the basic setup, for more info check <https://quarto.org/docs/websites/website-navigation.html>.


#### Change design and style of a page

As pages are `.qmd` files, those can be modified and designed with great flexibility. The following resources contain detailed information about it:

- [About layout of pages](https://quarto.org/docs/authoring/article-layout.html#screen-column). Also, [this](https://quarto.org/docs/output-formats/page-layout.html#grid-customization). The layout of a page/site is essential as landing pages, for example, require a different layout than a "text only" page. 
- [About shortcodes (quarto native "snippets")](https://quarto.org/docs/authoring/shortcodes.html)

### Avoid rendering (publishing) of private files/dirs

By default, Quarto renders all valid Quarto files (``.qmd``, ``.md``, ``.ipynb``, ``.Rmd``) to the website. 

To avoid rendering private documents to the website (such as `_CONTRIBUTING.md`) use the `_` (for files) or the `.` (for folders) prefixes in the filename. 

More info in <https://quarto.org/docs/websites/#render-targets>.


### Preview the website

When developping in local, use `quarto preview index.qmd` to check the result. Importantly, only preview/render the `index.qmd` file, avoiding other "previewable" files (such as `_CONTRIBUTING.md`) to avoid generation of unnecesary files.

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