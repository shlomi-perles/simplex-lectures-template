# My Lectures

A starter repository for building a Manim lecture site with
[Simplex](https://github.com/shlomi-perles/simplex).

This repository is an application template, not a Python package to publish.
It depends on the published `simplex-web` package, which installs the
`simplex` command and brings in `manim-simplex`.

## First Setup

1. Click **Use this template** on GitHub.
2. In the new repo, enable **Settings -> Pages -> Source -> GitHub Actions**.
3. Clone your repo locally.
4. Install system dependencies:

   ```bash
   # Ubuntu / Debian
   sudo apt-get install texlive-latex-extra texlive-fonts-recommended ffmpeg \
                        libcairo2-dev libpango1.0-dev
   ```

   ```powershell
   # Windows
   winget install MiKTeX.MiKTeX
   winget install Gyan.FFmpeg
   ```

5. Sync and check the environment:

   ```bash
   uv sync --locked
   uv run simplex doctor
   ```

6. Preview the starter site:

   ```bash
   uv run simplex build
   uv run simplex serve
   ```

## Daily Workflow

Create a new deck:

```bash
uv run simplex new algorithms/hash-tables
```

Render one deck:

```bash
uv run simplex render hash-tables
```

Build and serve the full portal:

```bash
uv run simplex build
uv run simplex serve --watch
```

Push to `main` to publish with GitHub Pages.

## Repository Layout

```text
.
|-- pyproject.toml            project dependencies; not published to PyPI
|-- uv.lock                   locked app environment for local + CI parity
|-- site.toml                 brand, tagline, navigation
|-- ruff.toml                 lint config with relaxed deck-slide rules
|-- LICENSE                   default MIT license; replace if needed
|-- .github/workflows/
|   |-- ci.yml                PR smoke render
|   `-- deploy.yml            build + deploy the static site to Pages
`-- decks/example/            starter deck; replace it with your lectures
    |-- deck.toml             slug, title, theme, entrypoints
    |-- manim.cfg             Manim plugin configuration
    |-- notes.md              rendered into the deck page
    |-- refs.bib              optional BibTeX references
    |-- assets/               figures, code samples, and other deck assets
    `-- slides/intro.py       Manim scene classes
```

The starter `decks/example/` deck is intentionally discoverable so the first
build produces a real site. Rename it, edit it, or delete it once you add your
own decks.

## Deck Configuration

Each deck has a `deck.toml`:

```toml
slug = "example"
title = "Example Deck"
summary = "A short description."
theme = "dastimator_dark"
quality = "high_quality"
entrypoints = ["slides.intro:Intro"]
```

`entrypoints` points to scene classes relative to the deck directory.

## GitHub Pages

The deploy workflow builds `site/` and uploads it with
`actions/deploy-pages`. There is no `gh-pages` branch.

By default, the workflow sets `SIMPLEX_BASE_URL` to `/<repository-name>`,
which matches normal GitHub project pages. For a custom domain or a root
`owner.github.io` site, set a repository variable named `SIMPLEX_BASE_URL` to
`/`.

Optional deployment settings:

- `SIMPLEX_BASE_URL`: URL prefix for generated links.
- `SIMPLEX_GA_TAG`: Google Analytics tag. The workflow reads this from a
  repository secret named `GA_TAG`.
- `SIMPLEX_BRAND`: override the site brand without editing `site.toml`.
- `SIMPLEX_PREVIEW`: set to `1` to suppress analytics during previews.

## Updating Simplex

This template tracks `uv.lock` on purpose so local builds and GitHub Actions
use the same dependency graph. To update Simplex:

```bash
uv lock --upgrade-package simplex-web
uv sync --locked
uv run simplex test
```

Commit both `pyproject.toml` and `uv.lock` when dependency constraints change.

## License

Apply whatever license suits your lectures. The template itself is MIT.
