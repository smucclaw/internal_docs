# Timeline

We're aiming to get most of the substantive material up by June 1.

# How to install / set up locally

1. Install `poetry`,  the dependency manager we're using, [by following the instructions here](https://python-poetry.org/docs/).

2. Use `poetry shell` to activate the virtual environment (see [the poetry docs](https://python-poetry.org/docs/basic-usage/) for more info)

3. **To publish the site:** After making whatever changes you want to make, you can get it to show up on the GH pages site by pushing to `main` / making a PR and merging it in. *For substantive docs, you are encouraged to tag at least one of the rest for 'code' review.*

**PLEASE DO NOT PUSH TO THE `gh-pages` branch.**

To see what it'll look like, locally: Build the docs site with `mkdocs build`; serve it (with live-reloading) via `mkdocs serve`.

# For more info on mkdocs

See [https://squidfunk.github.io/mkdocs-material](https://squidfunk.github.io/mkdocs-material) and [https://www.mkdocs.org/](https://www.mkdocs.org/).
