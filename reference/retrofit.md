# Revamping an existing site into cargopete style

A procedure, in order. The order is the whole point: retrofitting components before
tokens produces a site that is half in each style and worse than either, and it is
very hard to climb back out of once the components have been touched.

Work on a branch. `pete/cargopete-style`.

---

## Step 0 - Audit before you change anything

Do not skip this. Fifteen minutes here saves an afternoon of chasing a hardcoded
`#3b82f6` through four component files.

```bash
# every colour literal in the codebase, by frequency
grep -rhoE '#[0-9a-fA-F]{3,8}\b|rgba?\([^)]*\)|hsla?\([^)]*\)|oklch\([^)]*\)' \
  --include='*.css' --include='*.scss' --include='*.astro' --include='*.tsx' \
  --include='*.jsx' --include='*.vue' --include='*.svelte' --include='*.html' . \
  | sort | uniq -c | sort -rn | head -40

# fonts in play
grep -rhoE "font-family:[^;]+" --include='*.css' --include='*.astro' --include='*.tsx' . | sort -u

# third-party requests to kill
grep -rnE "fonts\.googleapis|fonts\.gstatic|cdn\.|unpkg|jsdelivr|picsum|googletagmanager|analytics" \
  --include='*.html' --include='*.astro' --include='*.tsx' --include='*.js' . | head -30

# shadows and gradients, both of which are mostly going away
grep -rnE "box-shadow|linear-gradient|radial-gradient|backdrop-filter" \
  --include='*.css' --include='*.astro' --include='*.tsx' . | wc -l
```

Write the counts down. They are the before column of the report you will hand over.

Also record, from the browser: total transfer size of the landing page, number of
requests, and the number of distinct hosts contacted. Those are the three numbers the
revamp is judged on besides how it looks.

**Decide the texture now** - flat or atmospheric (SKILL.md section 9). Flat unless
there is a reason. Changing your mind at step 5 means redoing step 3.

---

## Step 1 - Tokens

Drop `reference/tokens.css` in as the single source of colour, and delete every other
colour definition in the project. Not "map the old tokens onto the new ones" - delete
them, so that a stray old name fails loudly rather than rendering a rogue blue.

- **Tailwind v3**: replace the `theme.colors` block wholesale; keep only the token
  names from tokens.css. Delete the default palette import so `bg-blue-500` stops
  resolving and shows up as an error rather than as blue.
- **Tailwind v4**: replace the `@theme` block. Same rule - the default palette goes.
- **CSS modules / plain CSS**: import tokens.css first, then let the audit list from
  step 0 drive a find-and-replace, literal by literal.
- **styled-components / emotion**: replace the theme object, keep the keys identical
  to the CSS custom property names so there is one vocabulary rather than two.

Then map, mechanically:

| Old role | New token |
|---|---|
| page background | `--bg` |
| card / panel background | `--bg-elev` |
| code and terminal background | `--bg-code` |
| inline code, kbd, chip | `--bg-inset` |
| any divider or card border | `--border` |
| hover / focus / interactive border | `--border-hi` |
| primary text | `--text` |
| secondary text | `--text-muted` |
| tertiary, labels, timestamps | `--text-faint` |
| brand colour, links, primary action | `--accent` |
| second brand colour, highlight, boundary | `--rust` |
| success | `--ok` |
| warning | `--warn` |
| **error red** | there isn't one - see below |

Two mappings need judgement rather than mechanics:

**The brand colour.** It becomes `--accent` regardless of what it was. This is the
part that feels wrong and is not. A house style whose accent varies per site is not a
house style. If the client's brand hue is contractually load-bearing, this skill is
the wrong skill for that job - say so rather than smuggling the hue in.

**Error red.** The resting palette has none. Genuine destructive-action and
validation states may use a red, derived at that moment and checked with
`tools/contrast.py`; it never enters tokens.css, never appears on a marketing page,
and never decorates. If your landing page has a red on it, you have found a bug in the
copy, not a missing token.

**Checkpoint.** The site will look broken now, and mostly grey-brown. That is
correct. Do not fix it by adding colour; fix it at step 3.

---

## Step 2 - Type

Three faces, three jobs (SKILL.md section 3). Usually this means deleting two faces
and a lot of weights.

1. Delete Inter, Roboto, Open Sans and any icon-font import.
2. Install Space Grotesk (variable, 300–700) and JetBrains Mono (400), self-hosted:

```bash
npm i -D fonttools   # or: pip install fonttools brotli
pyftsubset space-grotesk.ttf --flavor=woff2 --layout-features='*' \
  --unicodes='U+0020-007E,U+00A0-00FF,U+2010-2027,U+2190-21FF,U+2500-257F' \
  --output-file=public/fonts/space-grotesk.woff2
```

   Widen the unicode range if the copy needs it - check box-drawing and arrow glyphs
   if there is a terminal or a diagram. Both faces together should land near 40KB.

3. Set the scale: h1 `clamp(2.4rem, 6vw, 3.9rem)` at `-0.03em`, h2
   `clamp(1.6rem, 3.4vw, 2.3rem)`, body 17px at 1.6.
4. **Convert every label to the mono tier.** This is the single highest-leverage edit
   in the whole retrofit. Eyebrows, tags, badges, table units, timestamps, breadcrumbs,
   card meta, stat suffixes, footer legal - all of it goes mono, uppercase, 11–13px,
   tracked 0.08–0.14em, in `--text-faint`. When this step lands the site starts looking
   like the family, before a single component has been rebuilt.
5. Add a mono kicker above every section heading that lacks one.

---

## Step 3 - Borders, elevation, whitespace

```bash
# every shadow in the project. In flat texture, all of them go.
grep -rn "box-shadow" --include='*.css' --include='*.astro' --include='*.tsx' .
```

- Delete every `box-shadow`. Card elevation becomes `--bg-elev` plus
  `1px solid var(--border)`. The only survivor is a hero terminal in atmospheric
  texture, using `--shadow-hero`.
- Delete every gradient in flat texture. In atmospheric, gradients may only
  interpolate between two existing surface tokens.
- Radii collapse to two values: `--radius` (10px) for cards, panels and terminals,
  `--radius-sm` (6px) for buttons and chips. `9999px` survives only on tags, eyebrow
  pills and terminal dots.
- Section padding becomes `clamp(4rem, 9vw, 7rem)`. This will feel like far too much
  the first time. Leave it for a day before deciding.
- Content caps at `68rem`, prose at `44rem`, ledes at 56ch.

**Checkpoint.** Now it should look like the family. If it does not, the problem is
almost always one of: a surviving cool-grey neutral, labels still in the sans tier, or
section padding that was quietly reduced because it looked odd.

---

## Step 4 - Section grammar

Restructure the page to the skeleton in SKILL.md section 4. This is content work as
much as layout work, and it is where a revamp stops being a reskin.

The usual findings:

- **There is no proof section.** Most sites put features first and evidence last, or
  nowhere. Move the hard numbers directly under the hero. If there are no hard
  numbers, go and measure some; that is the actual deliverable of this step.
- **The mechanism is missing.** Nearly every site explains what it does and never how.
  Add the diagram, the numbered walk, or the before/after diptych.
- **The comparison table is dishonest, or absent.** Add one, and add a row the product
  loses (components.md §4). Or add the scope panel (§7). On a free or community
  service, add both.
- **Too many sections.** Eight is the ceiling. Merge or cut.

---

## Step 5 - Artefacts

Now, and not before, build the components from `reference/components.md`:
terminal, install line, bento, comparison, stat band, diptych, scope panel.

The terminal is the piece that makes or breaks it. Get a real transcript:

```bash
script -q /dev/null your-command 2>&1 | tee transcript.txt
# then trim to the ~8 most load-bearing lines, keeping the real timings
```

Do not invent timings. Do not round `6.10s` to "6s" because it scans better. A reader
who checks one number and finds it true will believe the rest of the page; a reader
who finds it false will believe none of it.

---

## Step 6 - Copy

Run `reference/voice.md` over every string on the site, including alt text, meta
descriptions, button labels and 404 pages.

```bash
grep -rniE "elevate|seamless|unleash|next-gen|game.chang|delve|revolutionar|effortless|cutting.edge|leverage|empower|supercharge|robust|powerful" \
  --include='*.astro' --include='*.tsx' --include='*.md' --include='*.html' src/

grep -rn "Lorem|John Doe|Acme|example\.com|placeholder" --include='*.astro' --include='*.tsx' --include='*.html' src/

grep -rnP "[\x{1F300}-\x{1FAFF}\x{2600}-\x{27BF}]" --include='*.astro' --include='*.tsx' --include='*.md' src/
```

Every hit is a deletion or a rewrite. Headings go to sentence case with a full stop.
Adjectives become numbers.

---

## Step 7 - Assets and the performance contract

- Kill every third-party request found in step 0. Self-host the fonts, drop the
  analytics, inline the CSS.
- Replace stock photography and icon-library imports with SVG that depicts something
  real in the product.
- Add `llms.txt`, `robots.txt`, `sitemap.xml`, a real favicon, honest `og:` tags.

---

## Step 8 - Measure

```bash
python3 tools/contrast.py     # exits non-zero on any failure
```

Then, in the browser, with the network panel open and the cache disabled:

- Total transfer for the landing page. Target under 100KB.
- Number of distinct hosts contacted. Target: one.
- Toggle `prefers-reduced-motion: reduce` in devtools. The page must be complete,
  still and legible.
- Disable JavaScript. The page, including the terminal transcript, must still read.
- Tab through it. Skip link first, visible accent focus ring throughout, install line
  focusable and operable by Enter.
- 320, 540, 920, 1440. No horizontal scroll at any of them.

Finally, walk SKILL.md section 12 line by line, and put the before/after numbers from
step 0 in the pull request body. The transfer size and host count are the part that
survives the taste argument.

---

## What usually goes wrong

**Retrofitting components before tokens.** The most common failure and the most
expensive. Order matters.

**Keeping the brand colour "just for the logo".** It spreads. Within a week it is on
the primary button and the site belongs to neither style.

**Softening the section padding.** It looks excessive on a 27-inch monitor at step 3
and correct on every real reading device. Leave it.

**Using `--text-faint` for body copy** because it looks calmer. It measures 3.36 and
fails AA. Calm is what `--text-muted` is for.

**An invented terminal transcript.** Undoes the entire page. If the real output is
ugly, that is a product finding, not a design problem.

**A comparison table the product wins outright.** Reads as a lie, and costs more than
the features gain.
