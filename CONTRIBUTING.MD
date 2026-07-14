# Contributing guidelines
This document defines the conventions to follow when contributing (changing, updating) the website.

## Overview
The website (`.html`) is created using Quarto and hosted on GitHub Pages. Useful references to check are:
- [Quarto documentation: how to build websites](https://quarto.org/docs/websites/)
- [Quarto documentation: how to publish websites using GitHub Pages](https://quarto.org/docs/publishing/github-pages.html)
- [GitHub Pages documentation for details](https://docs.github.com/en/pages)

## Commit messages

We follow [Conventional Commits](https://www.conventionalcommits.org/) notation.

### Format

```
type(scope): short description
```

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