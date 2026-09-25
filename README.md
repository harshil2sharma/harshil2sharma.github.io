# harshil2sharma.github.io

Personal website and blog for Harshil Sharma, built with [Quarto](https://quarto.org).
Source lives at the top level (`index.qmd`, `about.qmd`, `blog.qmd`, `posts/`); the
rendered site is published from `docs/` via GitHub Pages, live at
<https://harshil2sharma.github.io>.

Two of the posts are computational: one in R, one in Python, both using the
[Palmer Penguins](https://allisonhorst.github.io/palmerpenguins/) dataset (CC0), which
ships inside the `palmerpenguins` package for each language. A third, bonus post runs R
and Python in the same document via `reticulate`. None of the posts need network access to
rebuild — the data is bundled in the packages, not downloaded.

## What to install first

Versions used to build this site:

- [Quarto](https://quarto.org/docs/get-started/) 1.10.18
- [uv](https://docs.astral.sh/uv/getting-started/installation/) 0.12.7 (manages Python and
  the Python packages; installs Python 3.14 itself, so a separate Python install isn't
  needed)
- [R](https://cran.r-project.org/) 4.6.1

`renv` bootstraps itself on the first `quarto render` — you don't need to install it
first.

## Build instructions

Run every command below from the top level of the repository (the folder containing
`_quarto.yml`). `quarto render` has to be run from here, not from inside `posts/`, because
that's how it finds `.Rprofile` and turns `renv` on, and how `uv run` finds `pyproject.toml`.

```sh
# 1. Clone the repository
git clone https://github.com/harshil2sharma/harshil2sharma.github.io.git
cd harshil2sharma.github.io

# 2. Install the Python environment (creates .venv from pyproject.toml + uv.lock)
uv sync

# 3. Install the R environment (restores packages from renv.lock into renv/library)
#    Run this from an R session started at the top level of the repo:
Rscript -e "renv::restore()"

# 4. Render the site (uv run so Quarto uses this project's .venv for Python chunks)
uv run quarto render
```

`renv::restore()` will prompt to confirm before installing — accept it. It reads
`renv.lock` and installs into a project-local `renv/library`, without touching any other R
library on your machine.

If a render picks up the wrong Python environment (for example, if you ran `quarto render`
without `uv run`), clear the Jupyter kernel cache and re-render:

```sh
rm -r .quarto
uv run quarto render
```

## Where the built site lands

`quarto render` writes the built HTML into `docs/`. Open `docs/index.html` directly in a
browser to view it locally, or run `quarto preview` for a live-reloading local server.

## Data

Both computational posts use the Palmer Penguins dataset (Horst, Hill, Gorman 2020,
CC0 1.0), loaded through the `palmerpenguins` package in each language — no CSV is
committed and no network call happens at render time. Full attribution and the licence
link are given inside each post.

## Repository layout

```
├── _quarto.yml
├── pyproject.toml, uv.lock, .python-version   # Python environment (uv)
├── renv.lock, .Rprofile, renv/activate.R      # R environment (renv)
├── index.qmd, about.qmd, blog.qmd
├── posts/
│   ├── my-path-into-data-science/
│   ├── penguin-body-mass/                     # R post
│   ├── penguin-species-classifier/            # Python post
│   └── r-and-python/                          # bonus post: R + Python via reticulate
└── docs/                                      # rendered site (published, not edited by hand)
```
