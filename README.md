# README

[![pre-commit.ci status](https://results.pre-commit.ci/badge/github/smucclaw/internal_docs/main.svg)](https://results.pre-commit.ci/latest/github/smucclaw/internal_docs/main)

## Timeline

We're aiming to get most of the substantive material up by June 1.

## How to install / set up locally

1. Install `poetry`,  the dependency manager we're using, [by following the instructions here](https://python-poetry.org/docs/).

2. Use `poetry shell` to activate the virtual environment (see [the poetry docs](https://python-poetry.org/docs/basic-usage/) for more info)

3. Once you have done `poetry shell`, install dependencies with `poetry install --no-root`.

4. **To publish the site:** After making whatever changes you want to make, you can get it to show up on the GH pages site by pushing to `main` / making a PR and merging it in. *For substantive docs, you are encouraged to tag at least one of the rest for 'code' review.*

**PLEASE DO NOT PUSH TO THE `gh-pages` branch.**

To see what it'll look like, locally: Build the docs site with `mkdocs build`; serve it (with live-reloading) via `mkdocs serve`.

### If you are using VSCode

I recommend using [this extension for Markdown linting](https://marketplace.visualstudio.com/items?itemName=DavidAnson.vscode-markdownlint).

### Pre-commit

#### To install locally

If you are on a mac:

```bash
brew install pre-commit

# navigate to this repo
cd internal_docs
 # install the git hook scripts
pre-commit install
```

> Every time you clone a project using pre-commit, running `pre-commit install` should always be the first thing you do.

See docs at [https://pre-commit.com/](https://pre-commit.com/) for more info.

#### Then, to run the hooks against the files locally

```bash
pre-commit run --all-files
```

This is usually faster than waiting for the GH workflow.

### Why pre commit and markdown linting

Yongming noticed that parts of the site weren't rendering properly because the markdown was not valid. Using precommit and markdown linting should help with that.

## For more info on mkdocs

See [https://squidfunk.github.io/mkdocs-material](https://squidfunk.github.io/mkdocs-material) and [https://www.mkdocs.org/](https://www.mkdocs.org/).
