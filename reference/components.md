# cargopete style - components

Markup and CSS for the six artefacts, plus the scaffolding they sit in. Every rule
here assumes `reference/tokens.css` is already loaded. No component defines a colour;
they all reach for tokens.

---

## Section head

Every section opens this way. A section without a mono kicker is not in this style.

```html
<section id="how">
  <div class="wrap">
    <p class="kicker">How it works</p>
    <h2 class="narrow">One process, from RPC to a query you can trust.</h2>
    <p class="section-lede">
      No separate node, no separate database, no separate query layer. One binary
      follows the chain, seals segments to Parquet, and serves them.
    </p>
  </div>
</section>
```

```css
.section-lede { color: var(--text-muted); max-width: 56ch; font-size: 1.05rem; }
.section-head { margin-bottom: 52px; max-width: 720px; }
```

```css
/* The kicker is terracotta. It is the largest single employer of --rust on a page
   and the cheapest way to stop a build reading as monochrome-plus-blue. */
.kicker, .section-label { color: var(--rust); }
```

**The two accents have jobs.** If a shell printed it, it is `--accent`; if a person
wrote it, it is `--rust`. Slate takes prompts, the nav CTA, links, focus rings,
technical chips and counts inside a terminal. Terracotta takes kickers, the hero
eyebrow, stat units, list markers, the live marker on a timeline, hover states, and
tags for work that is yours and ongoing. See SKILL.md section 1 for the table and the
usage floor.

---

## Nav

```html
<a class="skip" href="#main">Skip to content</a>
<header>
  <nav class="nav">
    <a class="brand" href="/"><svg aria-hidden="true"><use href="#mark"/></svg>nuthatch</a>
    <div class="nav-links">
      <a href="#how">How it works</a>
      <a href="#compare">Compare</a>
      <a href="/docs">Docs</a>
    </div>
    <a class="nav-cta" href="https://github.com/…">GitHub</a>
  </nav>
</header>
```

```css
header {
  position: sticky; top: 0; z-index: 30; height: 72px;
  display: flex; align-items: center;
  backdrop-filter: blur(14px);
  background: color-mix(in srgb, var(--bg) 66%, transparent);
  border-bottom: 1px solid var(--border);
}
.nav { display: flex; align-items: center; gap: 28px; width: 100%;
       max-width: var(--maxw); margin-inline: auto; padding-inline: 24px; }
.brand { display: flex; align-items: center; gap: 11px; font-weight: 700;
         font-size: 19px; letter-spacing: -0.01em; color: var(--text); }
.brand svg { width: 30px; height: 28px; color: var(--accent); }
.nav-links { display: flex; gap: 26px; margin-left: auto; font-size: 15px; color: var(--text-muted); }
.nav-links a { color: inherit; transition: color 0.18s var(--ease); }
.nav-links a:hover { color: var(--text); text-decoration: none; }
.nav-cta {
  font-family: var(--mono); font-size: 13.5px; font-weight: 500;
  color: var(--bg); background: var(--accent);
  padding: 9px 15px; border-radius: 9px;
  transition: transform 0.18s var(--ease), background 0.18s var(--ease);
}
.nav-cta:hover { background: var(--accent-ink); transform: translateY(-1px); text-decoration: none; }
@media (max-width: 920px) { .nav-links { display: none; } }
```

The blur is the only glassmorphism permitted anywhere in the style.

---

## 1. The terminal

The hero artefact. Read SKILL.md section 5 before writing the transcript: the
transcript must be true.

```html
<div class="term term--hero" role="img"
     aria-label="Terminal recording of nuthatch init followed by nuthatch dev, ending
                 with a live API served on 127.0.0.1 port 8288.">
  <div class="term-bar">
    <span class="term-dots" aria-hidden="true"><i></i><i></i><i></i></span>
    <span class="term-title">nuthatch - zsh - 92×14</span>
  </div>
  <div class="term-body" aria-hidden="true">
    <div class="tline"><span class="tline-pre pre-cmd">$</span><span class="tline-body">nuthatch init 0xA0b8…eB48 --chain mainnet</span></div>
    <div class="tline tline--out"><span class="tline-pre pre-ok">✓</span><span class="tline-body">resolved ABI from Sourcify</span></div>
    <div class="tline tline--out"><span class="tline-pre pre-ok">✓</span><span class="tline-body">sealed segment 3f9c8a… → 2,497 rows to Parquet</span></div>
    <div class="tline tline--out" data-serve="1"><span class="tline-pre pre-serve">●</span><span class="tline-body">serving  http://127.0.0.1:8288</span></div>
  </div>
</div>
```

```css
.term {
  background: var(--bg-code);
  border: 1px solid var(--border);
  border-radius: 12px;
  overflow: hidden;
}
.term-bar {
  display: flex; align-items: center; gap: 0.75rem;
  padding: 0.6rem 0.9rem;
  background: color-mix(in srgb, var(--text) 5%, var(--bg-code));
  border-bottom: 1px solid var(--border);
}
.term-dots { display: inline-flex; gap: 0.45rem; }
.term-dots i { width: 11px; height: 11px; border-radius: 50%; background: var(--border-hi); }
/* one warm lamp on the window chrome, so the panel is not wholly cool */
.term-dots i:first-child { background: color-mix(in srgb, var(--rust) 70%, transparent); }
.term-title {
  font-family: var(--mono); font-size: 0.72rem; color: var(--text-faint);
  margin-inline: auto; padding-right: 3rem;
}
.term-body {
  font-family: var(--mono);
  font-size: clamp(0.72rem, 1.9vw, 0.92rem);
  line-height: 1.85;
  padding: 1.1rem 1.2rem 1.4rem;
  color: var(--text);
  overflow-x: auto;
}
.tline { display: flex; gap: 0.6rem; white-space: pre; min-height: 1.85em; }
.tline--out { padding-left: 1.2rem; }
.tline-pre  { flex-shrink: 0; font-weight: 500; }
.pre-cmd    { color: var(--accent); }
.pre-ok, .pre-serve { color: var(--ok); }
.tline[data-serve] .tline-body { color: var(--text-muted); }

/* the one pulse in the whole motion budget */
.tline[data-serve] .pre-serve { animation: pulse 2.4s var(--ease) infinite; }
@keyframes pulse { 0%, 100% { opacity: 1; } 50% { opacity: 0.45; } }
```

Notes that matter:

- The transcript is static markup. It renders complete with JavaScript off and for a
  crawler. A typewriter effect, if you add one, sets `.term--anim .tline { opacity: 0 }`
  and reveals lines as typed - the reserved `min-height: 1.85em` means zero layout shift
  either way.
- The whole panel is `role="img"` with a prose `aria-label` describing what the run
  did, and the body is `aria-hidden`. A screen reader gets the meaning without being
  read a table of ASCII.
- Three dots, `--border-hi`, no traffic-light colours. The leftmost takes a
  70%-strength `--rust`: the one warm mark inside an otherwise cool panel.

---

## 2. The install line

A button, so it is focusable and works on Enter.

```html
<button class="install" id="install" aria-label="Copy install command">
  <span><span class="prompt">$</span> cargo install nuthatch</span>
  <span class="copy">copy</span>
</button>
```

```css
.install {
  display: inline-flex; align-items: center; gap: 14px;
  font-family: var(--mono); font-size: 15px;
  background: var(--bg-elev); border: 1px solid var(--border);
  border-radius: var(--radius-sm); padding: 13px 16px; color: var(--text);
  cursor: pointer;
  transition: border-color 0.18s var(--ease), background 0.18s var(--ease);
}
.install:hover { border-color: var(--accent); background: var(--bg-inset); }
.install .prompt { color: var(--accent); }
.install .copy {
  color: var(--text-faint); font-size: 12px;
  border-left: 1px solid var(--border); padding-left: 12px;
  transition: color 0.18s var(--ease);
}
.install.copied .copy { color: var(--accent); }
```

```js
document.querySelectorAll('.install, .cmd-line').forEach((el) => {
  el.addEventListener('click', async () => {
    const text = el.dataset.copy ?? el.querySelector('span').textContent.replace(/^\$\s*/, '');
    try {
      await navigator.clipboard.writeText(text.trim());
      el.classList.add('copied');
      setTimeout(() => el.classList.remove('copied'), 1400);
    } catch { /* clipboard denied: the command is still selectable on the page */ }
  });
});
```

The `catch` is not decoration. Clipboard access fails on insecure origins and under
some policies, and the failure must be silent rather than a thrown error in the
console of a marketing page.

---

## 3. The bento

```html
<div class="bento">
  <div class="cell c-feature">
    <div>
      <span class="tag">Content addressed</span>
      <h3>Hashes inputs, not clocks</h3>
      <p>The cache key is a BLAKE3 hash of sources, command, working directory and
         declared outputs. Same inputs, guaranteed hit.</p>
    </div>
    <div class="flow">
      <span class="chip">sources</span><span class="arrow">+</span>
      <span class="chip">command</span><span class="arrow">→</span>
      <span class="chip chip--hot">blake3</span>
    </div>
  </div>
  <div class="cell c-third">…</div>
  <div class="cell c-mid">…</div>
</div>
```

```css
.bento { display: grid; grid-template-columns: repeat(6, 1fr); gap: 16px; }
.cell {
  background: var(--bg-elev);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 28px;
  position: relative; overflow: hidden;
  transition: border-color 0.22s var(--ease), transform 0.22s var(--ease);
}
.cell:hover { border-color: var(--border-hi); transform: translateY(-3px); }
.cell h3 { font-family: var(--sans); font-weight: 700; font-size: 1.16rem; margin: 0 0 9px; }
.cell p  { color: var(--text-muted); font-size: 0.97rem; margin: 0; }
.cell .ic { width: 30px; height: 30px; margin-bottom: 18px; color: var(--accent); }

.c-third   { grid-column: span 2; }
.c-mid     { grid-column: span 3; }
.c-wide    { grid-column: span 3; }
.c-feature { grid-column: span 4; min-height: 230px;
             display: flex; flex-direction: column; justify-content: space-between; }
.c-feature .big-num {
  font-family: var(--display); font-size: 3.4rem; line-height: 1;
  letter-spacing: -0.02em; color: var(--accent);
}

/* mini diagram inside the feature cell */
.flow { display: flex; align-items: center; gap: 10px; flex-wrap: wrap; margin-top: 8px;
        font-family: var(--mono); font-size: 12px; color: var(--text-muted); }
.flow .chip { background: var(--bg); border: 1px solid var(--border);
              border-radius: 8px; padding: 7px 11px; }
.flow .chip--hot { color: var(--accent); border-color: var(--border-hi); }
.flow .arrow { color: var(--rust); }

@media (max-width: 920px) {
  .bento { grid-template-columns: repeat(2, 1fr); }
  .c-third, .c-mid, .c-wide, .c-feature { grid-column: span 2; }
  .c-feature { min-height: auto; }
}
@media (max-width: 540px) {
  .bento { grid-template-columns: 1fr; }
  .c-third, .c-mid, .c-wide, .c-feature { grid-column: span 1; }
}
```

Tags and badges:

```css
.tag {
  display: inline-block; font-family: var(--mono);
  font-size: 0.66rem; font-weight: 500;
  letter-spacing: 0.08em; text-transform: uppercase;
  padding: 0.2rem 0.55rem; border-radius: 9999px;
  background: var(--pale-blue-bg); color: var(--accent-ink);
}
.tag--rust { background: var(--pale-rust-bg); color: var(--rust); }
```

The two tag colours are not decoration, they are a split: **`.tag--rust` for work that
is yours and ongoing, plain `.tag` for past employment and things somebody else owns.**
On a page with five cards that reads instantly and without a legend, which is the whole
point of keeping a second accent.

`border-radius: 9999px` is legal here and on the eyebrow pill and the terminal dots.
Nowhere else.

---

## 4. The honest comparison

```html
<table class="matrix">
  <thead>
    <tr class="head-divider">
      <th></th><th>Subgraphs</th><th class="us">nuthatch</th><th>Substreams</th>
    </tr>
  </thead>
  <tbody>
    <tr><td>Runs on your own box</td><td class="no">&ndash;</td><td class="col-us yes">✓</td><td class="no">&ndash;</td></tr>
    <tr><td>Decentralised serving network</td><td class="yes">✓</td><td class="col-us no">&ndash;</td><td class="yes">✓</td></tr>
  </tbody>
</table>
```

```css
.matrix { width: 100%; border-collapse: separate; border-spacing: 0; }
.matrix thead th { font-family: var(--sans); font-weight: 500; font-size: 0.95rem;
                   color: var(--text-muted); text-align: center; padding: 0 0 18px; }
.matrix thead th:first-child { text-align: left; }
.matrix thead th.us { color: var(--accent); font-weight: 700; }
.matrix .head-divider th { border-bottom: 1px solid var(--border); }
.matrix tbody td { padding: 16px 12px; text-align: center; font-size: 0.95rem; color: var(--text-muted); }
.matrix tbody td:first-child { text-align: left; color: var(--text); font-weight: 500; }
.matrix tbody tr:nth-child(odd) td { background: color-mix(in srgb, var(--text) 2%, transparent); }
.matrix tbody tr td:first-child { border-radius: 10px 0 0 10px; }
.matrix tbody tr td:last-child  { border-radius: 0 10px 10px 0; }
.matrix .col-us { background: var(--pale-blue-bg) !important; color: var(--text); }
.matrix .col-us.yes { color: var(--accent); }   /* the tint must not steal the accent tick */
.yes  { color: var(--accent); font-size: 1.1rem; }
.no   { color: var(--text-faint); }
.part { color: var(--text-muted); }
@media (max-width: 920px) { .matrix { font-size: 0.86rem; } .matrix .hide-sm { display: none; } }
```

The second row in the example is not an oversight. Ship at least one row your product
loses, and put it above the fold of the table rather than last. `.no` is the only
place `--text-faint` may carry meaning, and it is paired with a `&ndash;` glyph so the
meaning does not rest on contrast alone.

---

## 5. The stat band

```html
<section class="speed">
  <div class="wrap speed-grid">
    <div class="stat"><div class="n">0.43<span>s</span></div>
      <div class="l">Warm run of the full pipeline. The cold run was 6.10s.</div></div>
    <div class="stat"><div class="n">33<span>MB</span></div>
      <div class="l">Resident memory following mainnet tip, measured not projected.</div></div>
    <div class="stat"><div class="n">1<span> binary</span></div>
      <div class="l">No node, no database, no query layer to operate beside it.</div></div>
  </div>
</section>
```

```css
.speed { background: var(--bg-elev);
         border-block: 1px solid var(--border); }
.speed-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 28px; }
.stat .n { font-family: var(--display); font-size: clamp(2.6rem, 5vw, 3.8rem);
           line-height: 1; letter-spacing: -0.02em; color: var(--text); }
.stat .n span { color: var(--rust); }   /* number in ink, unit in terracotta */
.stat .l { color: var(--text-muted); margin-top: 12px; font-size: 0.98rem; max-width: 30ch; }
@media (max-width: 920px) { .speed-grid { grid-template-columns: 1fr; gap: 36px; } }
```

Every number here is measured. "Measured not projected" in the label is a promise; if
you cannot keep it, delete the stat rather than soften the wording.

---

## 6. The before/after diptych

redstart's argument, and the strongest section type in the style when the product
replaces something verbose.

```html
<div class="section-head">
  <p class="kicker">The comparison</p>
  <h2>Would you rather write and maintain this?</h2>
</div>
<div class="diptych">
  <figure class="panel panel--before">
    <figcaption><span class="mono-label">Before</span><span class="lines">148 lines, 4 files</span></figcaption>
    <pre class="astro-code">…</pre>
  </figure>
  <figure class="panel panel--after">
    <figcaption><span class="mono-label">After</span><span class="lines">31 lines, 1 file</span></figcaption>
    <pre class="astro-code">…</pre>
  </figure>
</div>
```

```css
.diptych { display: grid; grid-template-columns: 1fr 1fr; gap: 16px; align-items: start; }
.panel { margin: 0; background: var(--bg-code); border: 1px solid var(--border);
         border-radius: var(--radius); overflow: hidden; }
.panel figcaption {
  display: flex; align-items: baseline; justify-content: space-between;
  padding: 0.7rem 1rem; border-bottom: 1px solid var(--border);
  font-family: var(--mono); font-size: 0.7rem;
  letter-spacing: 0.12em; text-transform: uppercase;
}
.panel--before figcaption .mono-label { color: var(--text-faint); }
.panel--after  figcaption .mono-label { color: var(--accent); }
.panel figcaption .lines { color: var(--text-faint); letter-spacing: 0.06em; }
.panel--before .astro-code { opacity: 0.72; }   /* the loser is dimmed, not shrunk */
.panel pre { margin: 0; padding: 1rem 1.2rem; overflow-x: auto;
             font-family: var(--mono); font-size: 0.82rem; line-height: 1.7; }
@media (max-width: 920px) { .diptych { grid-template-columns: 1fr; } }
```

The line counts in the caption are the whole argument, so they must be real counts of
the real code shown. Dim the "before" panel rather than shortening it: the length is
the point.

Highlighting is build-time. Astro ships Shiki; use `defaultColor: false` with a
light/dark theme pair so the token colours follow the theme toggle:

```css
.astro-code, .astro-code span { color: var(--shiki-dark); }
:root[data-theme='light'] .astro-code,
:root[data-theme='light'] .astro-code span { color: var(--shiki-light); }
```

Never a client-side highlighter. It costs more than the rest of the page.

---

## Buttons, kbd, inline code

```css
.btn {
  display: inline-flex; align-items: center; gap: 0.5rem;
  font: inherit; font-weight: 500; font-size: 0.95rem;
  padding: 0.62rem 1.1rem; border-radius: var(--radius-sm);
  border: 1px solid transparent; cursor: pointer; text-decoration: none;
  transition: transform 0.12s var(--ease), background-color 0.18s var(--ease),
              border-color 0.18s var(--ease);
}
.btn-primary { background: var(--text); color: var(--bg); }
.btn-primary:hover { opacity: 0.9; text-decoration: none; }
.btn-primary:active { transform: scale(0.98); }
.btn-ghost { background: transparent; color: var(--text); border-color: var(--border-hi); }
.btn-ghost:hover { background: var(--bg-elev); text-decoration: none; }

kbd {
  font-family: var(--mono); font-size: 0.82em;
  padding: 0.1rem 0.4rem;
  border: 1px solid var(--border-hi); border-bottom-width: 2px;
  border-radius: 5px; background: var(--bg-inset); color: var(--text);
}

:not(pre) > code {
  font-family: var(--mono); font-size: 0.86em;
  padding: 0.12rem 0.36rem; border-radius: 5px;
  background: var(--bg-inset); color: var(--text);
}
```

The primary button is `--text` on `--bg`, inverted - not accent-filled. The accent is
too scarce a resource to spend on a large solid fill, and the inversion is stronger
anyway. The nav CTA is the one exception, because it is small and needs to win against
a blurred background.

---

## Footer

```css
footer { border-top: 1px solid var(--border); padding-block: 40px; }
.foot { display: flex; align-items: center; justify-content: space-between;
        gap: 20px; flex-wrap: wrap; color: var(--text-faint); font-size: 14px; }
.foot .license { font-family: var(--mono); font-size: 12.5px; letter-spacing: 0.06em; }
```

Brand, licence in mono, a link row. Nothing else. No newsletter, no social wall, no
"made with love".

---

## 7. The scope panel

engine.camp's contribution, and the purest form of the doctrine's fifth rule. Two
mono-labelled columns under one heading: what the thing does, and what it deliberately
does not.

```html
<div class="section-head">
  <p class="kicker">Use it well</p>
  <h2>What it does, and what it doesn't.</h2>
  <p class="section-lede">
    camp serves raw, indexed event history. A few things are deliberately out of
    scope. Call them out so you can plan around them.
  </p>
</div>
<div class="scope">
  <div class="scope-col">
    <p class="scope-label scope-label--in">In scope</p>
    <ul>
      <li>Raw <code>POST /v1/sql</code> for arbitrary SELECTs</li>
      <li>Event history for any contract, any block range</li>
      <li>Decoded Transfer events for ERC-20 and ERC-721</li>
    </ul>
  </div>
  <div class="scope-col">
    <p class="scope-label scope-label--out">Out of scope</p>
    <ul>
      <li>Token balances. Events, not state</li>
      <li>USD prices: bring your own oracle</li>
      <li>Chains other than Arbitrum One</li>
    </ul>
  </div>
</div>
```

```css
.scope { display: grid; grid-template-columns: 1fr 1fr; gap: 16px; }
.scope-col {
  background: var(--bg-elev); border: 1px solid var(--border);
  border-radius: var(--radius); padding: 28px;
}
.scope-label {
  font-family: var(--mono); font-size: 0.7rem; font-weight: 500;
  letter-spacing: 0.12em; text-transform: uppercase; margin-bottom: 1.1rem;
}
.scope-label--in  { color: var(--ok); }
.scope-label--out { color: var(--rust); }
.scope-col ul { list-style: none; margin: 0; padding: 0;
                display: flex; flex-direction: column; gap: 0.7rem; }
.scope-col li { color: var(--text-muted); font-size: 0.97rem; padding-left: 1.2rem;
                position: relative; line-height: 1.5; }
.scope-col li::before {
  /* Every list marker on the page is terracotta. Individually tiny, collectively
     the thing that stops a long list of prose feeling like a plain document. */
  content: ""; position: absolute; left: 0; top: 0.62em;
  width: 7px; height: 1px; background: var(--rust); opacity: 0.75;
}
@media (max-width: 920px) { .scope { grid-template-columns: 1fr; } }
```

The out-of-scope column is `--rust`, not a failure red. Nothing here is a failure; it
is a boundary, and the second accent exists precisely so a boundary can be marked
without alarm.

Both columns must be roughly the same length. A three-item "out of scope" beside a
twelve-item "in scope" is the table-that-wins-every-row problem wearing a different
hat, and readers spot it just as quickly.

The natural companion is a short closing paragraph in the same section saying how to
use the thing well - rate limits, what to do if you need more, where to self-host.
engine.camp titles it "A community service. Be a good citizen." and then links both
repos, which is the whole argument for trusting it.
