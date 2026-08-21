# Fonts for the reference render

The two faces this style uses are **Space Grotesk** (variable, 300–700) and
**JetBrains Mono** (400). Both are SIL Open Font Licence 1.1.

They are **not committed here.** Font binaries carry their own licence terms, including
reserved-name conditions on modified versions, and a subset is a modified version. Rather
than guess, this repo ships the recipe and you produce the files.

Until you do, the example renders on the fallback stack in `reference/tokens.css` and
still demonstrates the palette, the layout and every component. Only the type is
approximate.

## Producing the subsets

```bash
pip install fonttools brotli

# Space Grotesk: https://github.com/floriankarsten/space-grotesk (OFL 1.1)
pyftsubset SpaceGrotesk[wght].ttf --flavor=woff2 --layout-features='*' \
  --unicodes='U+0020-007E,U+00A0-00FF,U+2010-2027,U+2190-21FF,U+2500-257F' \
  --output-file=space-grotesk.woff2

# JetBrains Mono: https://github.com/JetBrains/JetBrainsMono (OFL 1.1)
pyftsubset JetBrainsMono-Regular.ttf --flavor=woff2 --layout-features='*' \
  --unicodes='U+0020-007E,U+00A0-00FF,U+2010-2027,U+2190-21FF,U+2500-257F' \
  --output-file=jetbrains-mono.woff2
```

Drop both files in this directory. Together they should land near 35–40KB, which is
the whole font budget for a page in this style.

Widen the unicode range if your copy needs it. Check the arrow block (U+2190–21FF) and
box drawing (U+2500–257F) specifically - the terminal and the flow diagrams use both,
and a missing glyph there shows up as a tofu box in the one component that most needs
to look credible.
