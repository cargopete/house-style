# cargopete style - voice

The writing is half the style. A page with the right palette and the wrong copy does
not read as this style; it reads as a dark theme applied to marketing.

The four exemplars agree on this more tightly than they agree on anything else, which
is the clearest evidence it is load-bearing.

---

## The shape of a heading

Sentence case. A full stop when it is a sentence. Short - three to eight words - and
carrying a claim rather than announcing a topic.

From the sites, unedited:

> Be your own indexer.
> Three front doors.
> The honest comparison.
> Two minutes. One binary. No one to ask.
> Write the subgraph once. Properly.
> The eject path is the whole bet.
> Skip the endpoints. Write the query.
> What it does, and what it doesn't.
> A community service. Be a good citizen.

Note what none of them do. None is a noun phrase ("Powerful Indexing Features"). None
is title case. None sells. Several are two sentences where the second one lands the
blow, and the second sentence is usually one or two words.

The best of them state a position the reader could disagree with. "The eject path is
the whole bet" is an argument. "Flexible and extensible" is not.

---

## The shape of a lede

One or two sentences under the heading, capped at 56ch, in `--text-muted`. It says
concretely what the thing is, in the first clause, with no wind-up.

> A fast, single-binary, polyglot task runner with a content-addressed cache that
> captures outputs and shares them across machines.

> camp serves raw, indexed event history. A few things are deliberately out of scope.
> Call them out so you can plan around them.

No "In today's fast-moving world". No "We believe". No question openers.

---

## Numbers instead of adjectives

This is the mechanism that does most of the work.

| Don't | Do |
|---|---|
| blazing fast | `0.43s`, and the cold run was `6.10s` |
| lightweight | 33MB resident following mainnet tip |
| minimal setup | two minutes, one binary |
| tiny bundle | 46KB transferred |
| a handful of lines | 31 lines, one file, down from 148 across four |

A number is checkable, which is the point. It puts the page at risk in a way an
adjective does not, and readers can feel the difference even when they never check.

Which means: every number must be true and current. A stale benchmark is worse than no
benchmark, because it converts the page's chief asset into its chief liability. If a
number cannot be verified today, delete it.

Where a figure is a design target rather than a measurement, say so in the same breath
- "design target, not yet measured" - and keep it out of the stat band, which is for
measurements only.

---

## Say what it does not do

The doctrine's fifth rule, and the one most often lost in a revamp because it feels
like a step backwards.

Three places it lives:

1. **The comparison table**, with at least one row the product loses, placed near the
   top of the table rather than buried at the bottom.
2. **The scope panel**, as an explicit "out of scope" column of roughly the same
   length as the "in scope" one.
3. **The prose**, in the product's own voice, at the moment the limitation is
   relevant rather than in a footnote.

> If you're building something that depends on chain data being available at a
> specific latency, run your own.

That sentence sends business away, and it is the single most persuasive line on the
page it appears on. Readers have no way to verify a claim of strength, and every way
to verify a claim of weakness; a page that volunteers one is telling them the rest can
be trusted.

---

## No em dashes

A house rule, and it is absolute. Not in headings, not in body copy, not in list items,
not in code comments, not in commit messages, not in the alt text.

An em dash is almost always a sentence that has not decided what it wants to be. The fix
is not a hyphen in its place; it is to pick the punctuation the sentence actually needs:

| Instead of | Write |
|---|---|
| Frontend as the main job — not what you want me for | Frontend as the main job. Not what you want me for. |
| Three things — speed, size, and honesty | Three things: speed, size, and honesty |
| It works — mostly | It works, mostly |
| a P2P protocol — millions of messages — across global nodes | a P2P protocol carrying millions of messages across global nodes |

Two full stops beat one em dash nearly every time, and the second sentence usually lands
harder for being short. Where a genuine aside is wanted, a comma pair or a plain hyphen
pair will carry it.

Check with `grep -rn '—' src/` before you call anything finished. The answer is zero.

---

## Banned vocabulary

Delete on sight, no substitution needed - the sentence is nearly always better without
the word:

elevate, seamless, unleash, next-gen, game-changer, delve, revolutionary, effortless,
cutting-edge, leverage (as a verb), empower, supercharge, robust, powerful,
best-in-class, world-class, enterprise-grade, blazing, buttery, magical, simply,
just (as in "just works"), everything you need, and more.

Also banned:

- **Exclamation marks.** Anywhere. Including in code comments and toast messages.
- **Emoji.** The one standing exception is a single 🇧🇬 in the nuthatch footer, which
  is a signature.
- **Placeholder names.** No Lorem Ipsum, no John Doe, no Acme Corp, no example.com. If
  you need a contract address, use a real one. If you need a repo, use the real repo.
- **Rhetorical questions as headings.** "Why nuthatch?" is a section that has not
  decided what it wants to say. The exception is redstart's "Would you rather write and
  maintain this", which is a question whose answer is the next 148 lines of real code,
  and which therefore earns it.
- **The word "just"** meaning "merely". It undersells the work and reads as a wince.
- **Em dashes**, as above.

---

## Microcopy

- Buttons say what happens: "Copy install command", "Read the docs". Not "Learn more",
  not "Get started" where something more specific fits.
- The copy hint on an install line is lowercase `copy`, in mono, in `--text-faint`. It
  flips to `--accent` for about 1.4 seconds on success and says nothing.
- Alt text describes what the graphic shows, in a sentence, ending with a full stop.
  The terminal's `aria-label` describes what the run did, not what the panel looks like.
- Error and empty states get the same treatment as everything else: plain, specific,
  no apology, no exclamation mark, and a next action.
- The footer carries the licence in mono and nothing sentimental.

---

## A test

Read the page aloud in a flat voice. Anywhere you would have to add enthusiasm to make
a sentence work, that sentence is doing the job the evidence should be doing. Cut it,
and put a number in its place.
