# My Lectures

Built with [Simplex](https://github.com/shlomi-perles/simplex).

## Quick start

1. Click **Use this template** on GitHub to create your own repo.
2. Enable Pages: **Settings -> Pages -> Source: GitHub Actions**.
3. Locally: `uv sync && uv run simplex new my-first-deck`.
4. Push to `main` -- the `Deploy` workflow publishes to GitHub Pages.

Optional: `uv run simplex serve --watch` for live reload during editing.

## What's in this template

```
.
|-- pyproject.toml            depends on `simplex>=0.2.0`
|-- site.toml                 brand, tagline, base_url
|-- ruff.toml                 lint config with carve-out for decks/
|-- .github/workflows/
|   `-- deploy.yml            build + actions/deploy-pages@v4
`-- decks/_example/           the starter deck -- replace or delete
    |-- deck.toml             slug, title, theme, entrypoints
    |-- manim.cfg             `plugins = simplex`, `save_sections = True`
    |-- notes.md              rendered into the portal as the deck page
    |-- refs.bib              optional BibTeX
    |-- assets/               figures, code blocks, ...
    `-- slides/intro.py       the Manim scenes
```

`decks/_example/` is named with a leading underscore so `simplex` skips
it during discovery once you add your own decks. Rename or delete it
when you're ready.

## Notes

- The Pages site comes from the GitHub Actions build artifact; there's
  no `gh-pages` branch. Flip **Settings -> Pages -> Source** to
  **GitHub Actions** once after creating your repo from this template.
- The first build is cold and can take ~20 minutes (texlive + manim
  cache). Subsequent builds reuse `site/decks/*/media` via
  `actions/cache@v4` and finish in a few minutes.

## License

Apply whatever license suits your lectures. The template itself is MIT.
