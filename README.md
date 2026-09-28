# paromita-sg.github.io

My personal data science website, built with [Quarto](https://quarto.org/) and
published with GitHub Pages. It includes two computational blog posts, one in R
and one in Python, each with its own pinned environment (`renv` for R, `uv` for
Python) so the site can be rebuilt from a clean clone.

Live site: https://paromita-sg.github.io

## 1. What to install first

Install these before you clone. The versions are the ones I built the site with.

| Tool   | Version I used | Get it |
|--------|----------------|--------|
| Git    | X.Y.Z          | https://git-scm.com/downloads |
| Quarto | X.Y.Z          | https://quarto.org/docs/get-started/ |
| uv     | X.Y.Z          | https://docs.astral.sh/uv/getting-started/installation/ |
| R      | X.Y.Z          | https://cran.r-project.org/ |

- You do **not** need to install Python yourself. `uv` downloads Python 3.14
  (pinned in `.python-version`).
- You do **not** need to install `renv` yourself. It installs itself the first
  time R starts in this folder.

## 2. Build the site

Run these commands in order. Every command is run from the **top level** of the
repository (the folder that contains `_quarto.yml`).

**Shell:**

```bash
git clone https://github.com/paromita-sg/paromita-sg.github.io.git
cd paromita-sg.github.io
uv sync
```

**Shell:** restore the R packages listed in `renv.lock`. The first R start in
this folder bootstraps `renv`, which needs an internet connection.

```bash
Rscript -e 'renv::restore(prompt = FALSE)'
```

*(Equivalent from an interactive R session started in this folder:
`renv::restore()`.)*

**Shell:** render the whole site. `uv run` makes Quarto use the project's
`.venv`, and R picks up `renv` automatically from `.Rprofile`.

```bash
uv run quarto render
```

## 3. Where the built site lands

Quarto writes the finished site to the `docs/` folder. Open it locally with:

```bash
# macOS
open docs/index.html
# Linux
xdg-open docs/index.html
# Windows (PowerShell)
start docs/index.html
```

Or serve it with live reload: `uv run quarto preview`.

## 4. Where the data comes from

Neither post needs the network to fetch data. Both datasets ship inside
packages that the lockfiles install, and no data files are committed.

| Post | Dataset | Source | Licence |
|------|---------|--------|---------|
| `posts/penguins-r/` | Palmer Penguins (`palmerpenguins` R package) | [allisonhorst.github.io/palmerpenguins](https://allisonhorst.github.io/palmerpenguins/), Palmer Station Antarctica LTER | CC0 |
| `posts/penguins-python/` | Palmer Penguins (`palmerpenguins` Python package) | [allisonhorst.github.io/palmerpenguins](https://allisonhorst.github.io/palmerpenguins/), Palmer Station Antarctica LTER | CC0 |

The only network use during a fresh build is installing packages
(`uv sync` and `renv::restore()`).

## Repository layout

| Path | Purpose |
|------|---------|
| `_quarto.yml`, `*.qmd` | Site configuration and pages |
| `posts/` | Blog posts, one folder each |
| `pyproject.toml`, `uv.lock`, `.python-version` | Python environment (uv) |
| `renv.lock`, `.Rprofile`, `renv/activate.R` | R environment (renv) |
| `docs/` | Built site served by GitHub Pages |