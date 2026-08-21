# cargopete style

A Claude Code skill that carries one frontend house style: warm near-black ground,
slate-blue and terracotta accents, monospace instrument labels, hairline borders,
macro whitespace, and a terminal showing a transcript that is actually true.

It was extracted from four sites that already ship it:

- [nuthatch-indexer.com](https://nuthatch-indexer.com) - canonical, and the source of
  the palette
- [yetanothertaskrunner.com](https://yetanothertaskrunner.com) - the atmospheric texture
- [redstart-lang.com](https://redstart-lang.com) - the before/after diptych
- [engine.camp](https://engine.camp) - the scope panel

All four began from the `minimalist-ui` protocol in
[Leonxlnx/taste-skill](https://github.com/Leonxlnx/taste-skill) and then diverged in the
same direction, which is what made the divergence worth writing down.

## What makes it different from a taste protocol

Most design skills hand you a method for deriving colour. This one hands you the
colour. The palette in `reference/tokens.css` is mandated: sixteen tokens, validated in
both themes, and not open for per-project reinterpretation. What varies between sites
is subject, structure, illustration and texture. Not colour.

That is the entire point of a house style. Three of the four exemplars above disagree
with each other about colour, which is precisely why the mandate exists now.

## Install

```bash
git clone https://github.com/cargopete/cargopete-style.git
cd cargopete-style
./install.sh
```

Then start a Claude Code session and ask for the house style, or ask to revamp an
existing site into it.

## Contents

| Path | What it is |
|---|---|
| `SKILL.md` | The doctrine, the palette, the page grammar, the artefact list, the banned list |
| `reference/tokens.css` | The mandated palette. Ship it verbatim |
| `reference/components.md` | Markup and CSS for all seven artefacts |
| `reference/retrofit.md` | An ordered procedure for bringing an existing site over |
| `reference/voice.md` | Copy rules, and the banned vocabulary |
| `tools/contrast.py` | Palette validator. Exits non-zero, drops into CI |
| `example/index.html` | Reference render: every artefact, one page, no build step |

## Validate

```bash
python3 tools/contrast.py
```

Checks every ink and accent against all four surfaces in both themes, not merely
against the page ground. It caught two AA failures in the shipped sites while this
skill was being written, both now corrected here.

`--text-faint` is listed DECORATIVE rather than failing. That is a contract: it carries
kickers, window titles and timestamps, and never body copy, a link, or the only
instance of a fact.

## Look at it first

```bash
open example/index.html
```

No build step, no dependencies, and the only colour literals in the file are inside the
code samples that are meant to show them.

## Licence

MIT.
