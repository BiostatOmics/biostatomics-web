# BiostatOmics Lab website

Source of the [BiostatOmics Lab](https://biostatomics.com) website, built with [Quarto](https://quarto.org/) and published automatically to GitHub Pages.

For details on how to add content (a new tool, a new person, a new page...) and our commit message conventions, see [`_CONTRIBUTING.md`](_CONTRIBUTING.md).

## Requirements

- [Quarto CLI](https://quarto.org/docs/get-started/) installed locally.
- [Git](https://git-scm.com/downloads).

## Get the code

```bash
git clone git@github.com:BiostatOmics/biostatomics-web.git
cd biostatomics-web
```

## Preview your changes locally

Before pushing anything, check how the site looks with:

```bash
quarto preview index.qmd
```

This opens a local preview in your browser that updates automatically as you edit files. Always preview `index.qmd` specifically (not any other file) — see [`_CONTRIBUTING.md`](_CONTRIBUTING.md) for why.

## Submitting your changes

1. Create a branch for your change (don't commit directly to `main`):

   ```bash
   git checkout -b my-change
   ```

2. Make your edits, then commit them following our [commit message conventions](_CONTRIBUTING.md#commit-messages).

3. Push the branch:

   ```bash
   git push -u origin my-change
   ```

4. Open a pull request against `main` and ask a maintainer to review it.

Once a pull request is merged into `main`, a GitHub Action automatically renders the site and publishes it.
