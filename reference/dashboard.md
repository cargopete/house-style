# The dashboard dialect

Taken from [lodestar-dashboard.com](https://www.lodestar-dashboard.com) in October 2026, after
three passes on it: the house palette (lodestar#378), one accent instead of two (#384), and
the whole site in rem at a larger root size (#385).

The landing pages in this style are read once, top to bottom. A dashboard is worked in for an
hour: twenty-odd pages, a sidebar, tables of indexers, numbers that fall and checks that fail. Two
things in the landing rules do not survive that, and this dialect changes exactly those two.
Everything else in `SKILL.md` stands: the ground, the ink, the hairlines, mono as the
instrument label, no shadows, no em dashes.

Use it for an application someone operates. Use the landing palette for the page that
explains it. One site does not mix them: the dialect is switched on for the whole document.

```html
<html lang="en" data-palette="brick">
```

## 1. One accent, and a red that only means bad

The landing palette is a pair, slate and terracotta. On a dashboard the pair failed twice. The
slate blue was the only cool colour on a warm page and brighter than the terracotta, so the two
competed on every screen. And terracotta was doing two jobs, section labels and bad news, so a
falling price looked like a heading.

So the dialect has one accent and gives the bad news away.

| Token | Value | Job |
|---|---|---|
| `--accent` | `#d0704f` brick | kickers, stat labels and units, links, the active nav item, focus rings, the primary action, a chart's lead series |
| `--accent-ink` | `#f2cbbb` | text on `--pale-accent-bg` |
| `--bad` | `#e36b6b` | a negative number, a failing check, an error, a read that failed. Nothing else |
| `--bad-ink` | `#f5c4c4` | text on `--pale-bad-bg` |
| `--ok`, `--warn` | unchanged | washed status, as in the landing palette |

> **Brick labels things. Red reports things. Neither does the other's job.**

`--bad` never decorates, never marks a heading, never marks "ours". If a design wants a red
that is not bad news, it wants `--accent`. If a number in brick could be read as a loss, it
should not be in brick.

There is no `--rust` to spend. The token is aliased to `--accent` so that landing components
still render, but do not write it, and the landing rule about a dozen uses of it does not apply.

**Text on a wash is the `-ink` tone.** Brick on its own 12% wash measures 4.10 on `--bg-elev`
and fails AA; `--accent-ink` measures 9.41. The same holds for red. `tools/contrast.py` checks
every ink-on-wash pair, which is how this was found: Lodestar drew the hue on the wash in
28 places, badges included, until lodestar#387.

Dark only. There is no validated light inversion of brick, so do not combine
`data-palette="brick"` with `data-theme="light"`.

## 2. Type at 112.5%, and every size in rem

Labels on a data page live at 10 and 11 pixels, and people were zooming to 110% and 125% to
read them. The fix was not a bigger label but a bigger root:

```css
html { font-size: 112.5%; }   /* 18px against the browser default */
body { font-size: 0.875rem; line-height: 1.6; letter-spacing: -0.01em; }
```

Then **no pixel font sizes anywhere**, including chart ticks and inline styles. The sidebar
width and the topbar height are rem as well, so the frame grows with the text, and the site
follows the reader's own browser text size, which a pixel stylesheet ignores.

The scale, in rem against that root:

| Size | Used for |
|---|---|
| `0.5625` | sidebar section titles, the wordmark's tagline |
| `0.625` | kickers, stat labels, stat units, readout chrome, footnotes |
| `0.6875` | badges, table headers, the topbar label, chips |
| `0.75` | secondary copy, notes under a card |
| `0.8125` | nav links |
| `0.875` | body, table cells |
| `1.125` | a card or section title, weight 500, `-0.03em` |
| `1.875` | a page's h1, bold, `-0.04em` |
| `clamp(2.8rem, 5vw, 4.7rem)` | the overview hero only, weight 500, line-height 1.05 |

Mono labels keep the house treatment: uppercase, tracked `0.08em` to `0.14em`, and in brick.
Figures are `font-variant-numeric: tabular-nums` so a column of them lines up.

## 3. The frame

A dashboard has a shell the landing grammar does not, and it is the same on every page.

- **Sidebar**, `15rem`, fixed left. Lowercase wordmark at `-0.06em` with a mono tagline in
  brick beneath it. Sections titled in mono uppercase brick at `0.5625rem`. A link at rest is
  muted; hovered, brick; current, a `--pale-accent-bg` wash with a 2px brick bar on its left
  edge (`box-shadow: inset 2px 0 0 var(--accent)`) and full ink.
- **Topbar**, `4.5rem` (60px under 540px), fixed, `--bg` at 80% with a backdrop blur and a
  hairline under it. The page name on the left in brick mono, search in the middle, the
  network chip and the wallet on the right.
- **Bottom nav on mobile**, 64px plus the safe-area inset, with three tabs and a More sheet
  holding everything else. The main column pads its bottom by the same amount, and anything
  sticky at the bottom of a page sits above it, not under it.
- **The sidebar and the bottom nav list the same routes.** Two hand-kept lists drift: Lodestar
  shipped a page that phones could not reach. Keep a test that compares them.

## 4. Surfaces

- **Card.** `--bg-elev`, a 1px `--border`, `--radius` (10px), padding 1rem. On hover only the
  border changes, to `--border-hi`. Never a lift, never a shadow.
- **Stat grid.** Cells separated by a 1px gap over a `--border` background, so the grid is
  drawn by hairlines rather than by boxes. Each cell: a brick mono label, the figure in the
  display face at weight 500 with tabular numerals, the unit in brick mono beside it.
- **Readout.** The dashboard's terminal: `--bg-code`, a chrome row in mono muted text led by a
  6px brick dot, a large mono figure, a mono footnote. It shows a live value from the real
  source, under the same rule as the landing terminal.
- **Tables.** Headers in `0.6875rem` muted, rows divided by `--border`, cells at body size. A
  wide table scrolls inside its card; a pinned first column takes the card's own surface so
  nothing shows through it.
- **Badges.** The one place a pill radius is allowed. Mono, `0.6875rem`, a wash behind the
  `-ink` tone: default (`--bg-inset`, muted), accent, ok, warn, bad.

## 5. A failed read says so

This is a rule about honesty before it is a rule about colour, and on a dashboard it matters
more than anything above.

When a source cannot be reached, the panel says that it could not be loaded, in `--bad`, once,
at the top, and every figure it feeds reads "Unavailable". It never renders as `0`, an empty
table, "No revenue recorded" or a green "Stable". A page that shows a confident zero for data
it never received is the dashboard's version of the invented terminal transcript.

Loading is a `--bg-inset` shimmer pulsing to 45% opacity, and nothing else moves. All of it
stops under `prefers-reduced-motion: reduce`.

## 6. Focus and selection

`:focus-visible` is a 2px brick outline offset by 3px. Selection is `--pale-accent-bg` behind
full ink. The skip link is brick with `--bg` text.

## 7. Where Lodestar itself falls short

Copy the dialect, not these:

- The nav icons are Heroicons path data, which the banned list rules out.
- Graphy's illustration on `/ask` fills his spines with `#7a5a44`, a colour that is not a token.

## Before you call it done

- [ ] `data-palette="brick"` is on `<html>`, and `data-theme="light"` is not.
- [ ] `grep -c 'var(--rust)'` returns zero in your own code.
- [ ] Every use of `--bad` marks something bad.
- [ ] Every tag on a wash uses the `-ink` tone, or `--ok` and `--warn`, which clear on theirs.
- [ ] `grep -nE 'font-size: *[0-9.]+px|text-\[[0-9]+px\]'` finds nothing.
- [ ] Sidebar and mobile nav are compared by a test.
- [ ] Disconnect the data source: every panel says it failed, and no figure reads as zero.
- [ ] `python3 tools/contrast.py` passes.
