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
- [lodestar-dashboard.com](https://www.lodestar-dashboard.com) - the dashboard dialect

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

The palette is a **pair**, and both halves have a job: if a shell printed it, it is the
slate blue; if a person wrote it, it is the terracotta. Kickers, eyebrows, stat units
and list markers are the terracotta half, and a page that skips them comes out reading
as blue on warm grey. There is a grep for it in the checklist.

## The dashboard dialect

A dashboard someone works in for an hour is not a landing page read once, and two of the
landing rules fail there. `data-palette="brick"` swaps the pair for one brick accent and a
red that only ever means bad news, and `reference/dashboard.md` carries the rest: type in rem
at a 112.5% root, the sidebar-and-topbar frame, and panels that say when a read failed rather
than showing zero. It was taken from Lodestar, and `example/dashboard.html` renders it.

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
| `reference/dashboard.md` | The dashboard dialect: brick palette, rem type, the frame, honest failure |
| `reference/retrofit.md` | An ordered procedure for bringing an existing site over |
| `reference/voice.md` | Copy rules, and the banned vocabulary |
| `tools/contrast.py` | Palette validator. Exits non-zero, drops into CI |
| `example/index.html` | Reference render: every artefact, one page, no build step |
| `example/dashboard.html` | Reference render of the dashboard dialect |

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
