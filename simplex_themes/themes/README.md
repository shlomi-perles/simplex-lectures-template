# Custom Simplex themes

Save custom Simplex theme JSON files here.

The file name is the theme name used in `site.toml` or `deck.toml`, for
example `simplex_themes/themes/my_light.json` is selected with:

```toml
[slide_themes]
light = "my_light"
```

Theme JSON can combine an iTerm/Theme Studio palette with explicit semantic
overrides:

```json
{
  "manim_palette": "simplex_light",
  "code_style": "simplex_solarized_light",
  "palette": {
    "background": "#EEEAD8",
    "font": "#3C313F",
    "vertex": "#355561",
    "vertex_stroke": "#426A79"
  }
}
```

Any missing `palette` fields are derived from `manim_palette`. If
`manim_palette` is omitted, missing fields derive from Manim's default palette.
