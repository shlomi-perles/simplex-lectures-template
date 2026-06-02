# My Lectures

A starter repository for building a Manim lecture site with
[Simplex](https://github.com/shlomi-perles/simplex).

This repository is an application template, not a Python package to publish.
It depends on the published Simplex package set and is wired for GitHub Pages.

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

Render only one true slide theme during iteration or CI:

```bash
uv run simplex build --slide-theme dark
uv run simplex test --slide-theme dark
```

Push to `main` to publish with GitHub Pages.

Deck notes support label-based slide refs such as `[slide:key-idea]` and
BibTeX citations such as `\cite{KB15}` from `refs.bib`. When a TeX engine is
available, Simplex also generates a downloadable `<title>-note.pdf`.

## Repository Layout

```text
.
|-- pyproject.toml            project dependencies; not published to PyPI
|-- uv.lock                   locked app environment for local + CI parity
|-- site.toml                 brand, tagline, navigation
|-- ruff.toml                 lint config with relaxed deck-slide rules
|-- simplex_themes/           repo-local Theme Studio exports
|   |-- code_styles/          custom Pygments styles
|   |-- palette_styles/       custom Manim/iTerm palette exports
|   `-- themes/               custom Simplex theme JSON files
|-- LICENSE                   default MIT license; replace if needed
|-- .github/workflows/
|   |-- ci.yml                PR smoke render
|   `-- deploy.yml            build + deploy the static site to Pages
`-- decks/example/            starter deck; replace it with your lectures
    |-- deck.toml             slug, title, entrypoints, slide themes
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
quality = "high_quality"
entrypoints = ["slides.intro:Intro", "slides.intro:KeyIdea"]

[slide_themes]
enabled = true
dark = "simplex_dark"
light = "simplex_light"
default = "dark"

[slides."Key Idea"]
notes_anchor = "key-idea"
```

`entrypoints` points to scene classes relative to the deck directory.
The template enables true slide themes by default: Simplex renders
`simplex_dark` and `simplex_light`, including matching thumbnails, and the
player swaps between those compiled outputs. Set `[slide_themes] enabled =
false` in `site.toml` or in a deck's `deck.toml` to use the single-render
filter toggle instead.

The old top-level `theme = "..."` field is intentionally not used in new deck
files. With true slide themes, rendered slide pixels come from
`[slide_themes] dark` and `[slide_themes] light`.

## Theme Studio

Open the palette/code-style editor with:

```bash
uv run simplex theme-studio
```

Save exported code styles into `simplex_themes/code_styles/` and exported
palette JSON or `.itermcolors` files into `simplex_themes/palette_styles/`.
Save complete theme JSON files into `simplex_themes/themes/`.

Theme names are selected in `site.toml` or per deck:

```toml
[slide_themes]
enabled = true
dark = "simplex_dark"
light = "my_light"
default = "dark"
```

Example `simplex_themes/themes/my_light.json`:

```json
{
  "manim_palette": "simplex_light",
  "code_style": "simplex_solarized_light",
  "palette": {
    "background": "#EEEAD8",
    "font": "#3C313F"
  }
}
```

- `manim_palette` patches Manim constants such as `BLUE`, `BLUE_A`, and
  `WHITE` for rendered slides.
- `palette` overrides semantic rendered-slide colors such as `background`,
  `font`, `accent`, `vertex`, `vertex_stroke`, `edge`, `weight`, `visited`,
  `label`, and `distance`.
- `code_style` controls Manim slide `Code` objects.
- `[web] notes_code_style = "..."` controls markdown notes code blocks.
- `[web] background`, `[web] text_primary`, and related fields control the
  generated HTML shell, not rendered slide pixels.

The default dark theme preserves Manim's palette. The default light theme uses
Simplex's built-in `simplex_light` palette.

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
use the same dependency graph. Simplex release automation opens dependency PRs
for this template. To update manually:

```bash
uv lock --upgrade-package manim-simplex
uv sync --locked
uv run simplex test
```

Commit both `pyproject.toml` and `uv.lock` when dependency constraints change.

## License

Apply whatever license suits your lectures. The template itself is MIT.
