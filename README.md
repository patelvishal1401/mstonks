# mstonks design

Self-contained HTML files. No build step, no dependencies.

```bash
open mstonks/landing/v3.html    # landing, current
open mstonks/terminal/v1.html   # terminal, where Launch App lands
open mstonks/docs/v1.html       # docs, how the product works
```

Or open any `index.html` for that surface's version list. The three pages share
one nav and link to each other, so the whole MVP journey clicks through from
`landing/v3.html`.

## The nav, on every page

`Board` (the terminal) and `Docs` are live. `Predict` and `Rewards` are in the
nav as plain text with a **Soon** chip, because they are Beta and post-MVP: the
nav tells you the whole surface area and which parts of it exist. Both drop out
below 820px, and the whole bar tightens twice more on the way down to 360px.

Everything is inline: styles, the canvas or WebGL work and all interaction
logic. Fonts come from the Google Fonts CDN, so the page wants a connection the
first time; it degrades to system sans and mono offline. The hero card art in
`landing/assets/` is the only external file v2 and v3 load.

## Versions

| File | Theme | Notes |
|---|---|---|
| `landing/v3.html` | Misfits | **Current.** v2 plus the launch countdown and the hero trims below. |
| `landing/v2.html` | Misfits | The Claude Design canvas, rendered without a framework. |
| `landing/v1.html` | Ignition | First pass, kept for reference. |

`landing/v2-misfits.dc.html` is the untouched Claude Design export the Misfits
versions come from, with its `support.js` runtime beside it. It is the design
source of truth, not the page that ships: open it to keep editing on canvas,
open `v3.html` to see what ships.

## v3, what changed from v2

Six edits, everything else is v2 exactly as designed.

- **Nav.** `Creed` dropped from the top nav.
- **Hero.** The secondary `Read the creed` CTA dropped, so `Launch App` stands
  alone under the paragraph.
- **Close.** The secondary `See the board` CTA dropped, the same way.

  The Creed section itself still ships; nothing links to it now, so it reads as
  a scroll beat rather than a destination.

- **Hero rhythm.** The hero was a bottom-aligned grid, which parked the eyebrow
  and headline 258px below the nav on a desktop fold. It is now
  `grid-template-rows:auto 1fr` with a `clamp(118px,12vh,132px)` top pad, so the
  content starts 44px under the nav at every width and the torn tape still
  pins to the bottom of the fold.
- **Card fan.** Hovering a hero card lifts it forward (`translateZ(+70px)`,
  `scale(1.07)`, raised stacking) on top of the existing pointer tilt, and
  drops back on leave. Under `prefers-reduced-motion` it scales without tilting.
- **Countdown.** Launch clock in the hero under the CTAs, counting to
  **5 Oct 2026, 23:00 SGT** (`Date.UTC(2026, 9, 5, 15, 0, 0)`). Reads
  `Live now` once it passes. Same pill, border, mono and lime tokens as the
  eyebrow above it.

## Rendering the canvas design without a framework

The Claude Design export is a `<x-dc>` template with `{{ value }}` bindings that
its runtime renders through React. v2 and v3 keep that markup character for
character and swap the runtime for ~120 lines inline: it walks the DOM once,
records every `{{ }}` in text and attributes, binds `data-on-click` /
`data-on-input` to the state handlers, expands `<sc-for>` from a `<template>`,
and re-renders on state change. The reveal observer, pointer tilt and the hero
WebGL shader are ported verbatim from the export's `Component` class. Result:
one file, no React, no CDN, no build.

## Verified in a real browser

- Zero JS errors and zero horizontal overflow at 390, 768, 1024, 1440, 1920px
- Chain and fee toggles, the UP/DOWN market, the size slider, the live wire and
  the card hover all exercised with Playwright
- Reduced motion honoured: no reveals, no tilt, static shader time

---

# Terminal, `terminal/v1.html`

Where **Launch App** lands. One page, one job: get a scout to hold a launch slot
before the launchpad opens.

```
land  ->  read the board  ->  Reserve a launch  ->  agent drafts it
      ->  connect wallet  ->  one signature     ->  Telegram + X
```

## Screens in one page

**Narrative board.** Ten narratives from the agents, each card carrying the
PRD's own fields: momentum score, lifecycle state (Emerging, Accelerating,
Breakout, Peaking, Fading), baseline multiple, velocity 1h, ignition age,
X posts 24h, likes, reposts, unique authors, posts per hour, news sources and
sampled posts, plus a trend sparkline. Filter by lifecycle state, sort by
momentum / posts / velocity / baseline / newest ignition, search by stock or
keyword.

**Scout agent, from a board narrative.** Clicking *Reserve a launch* opens a drawer, not a form. The
agent locks the narrative, auto-pairs the canonical stock token, writes a name,
ticker and one-line description, draws the token art, and defaults the terms to
a $25,000 start market cap and a 2% fee on Base with liquidity locked and the
30 / 50 / 20 split fixed. Everything is editable by chip or by typing: new
image, different name, "call it Dronefather", "make the market cap 60k",
"switch chain", "2% fee", or "why did this score 91". Say *ship it* and the
draft becomes an approval card.

**Scout agent, from your own story.** The nav CTA is **Launch token**, and it
opens the same agent with nothing locked in. Ask it to *search the wire* and it
returns the matching scored narratives to pick from, or describe a thesis it has
never seen ("quantum error correction finally works in production") and it names
it, tickers it, draws it and infers the stock to pair against, which you can
override with *pair with IONQ*. A custom thesis launches unscored; the engine
starts tracking it from launch.

**Approval and signing.** Connect wallet (MetaMask / Coinbase / Rainbow), then
one signature. The card is explicit that signing records a reservation, mints
nothing and moves nothing. Success shows the reservation number, queue
position, the 24-hour hold rule, and routes to Telegram and X.

**Launch waitlist.** A tab showing every confirmed reservation as a token card:
art, name, ticker, stock pair, chain, start market cap, fee, scout wallet and
queue number. A **Yours** tab filters to the connected wallet and the board
marks a narrative you already hold. Header stats carry reserved count and slots
left out of 250. The connected wallet shows as its own chip in the nav, beside
the Launch token CTA.

## Token art

No upload step in the MVP flow, so the art is generated in the page: a seeded
SVG of a state-coloured glow, halftone field, a trend squiggle and the ticker
set in the brand display face, with the landing's zig-zag mark in the corner.
The seed is the narrative id, so a token always draws the same card, and *new
image* reseeds it.

## Style

Every token comes from the landing page's Misfits theme, nothing new was
invented: `#0e1014` ground, `#f6ebd6` ink, lime `#d4f84a` as the instrument,
`#f0665a` / `#b56cf5` / `#7fb3ff` carrying lifecycle state, Bricolage Grotesque
for display, Familjen Grotesk for body, JetBrains Mono for every number and
eyebrow, 1.5px hairlines, pill controls, 18-22px panel radii, and the torn
lime tape running the reservation terms. The PRD supplied field names and
journey only; it supplied no styling.

## Verified in a real browser

- Zero JS errors and zero horizontal overflow at 390, 768, 1024, 1440, 1920px,
  reduced motion included
- Full reserve flow driven with Playwright: filter, sort, search, open agent,
  rename, reseed art, "make the market cap 60k", ship it, connect wallet, sign,
  success card, waitlist and Yours tabs, and the board marking the held
  narrative

---

---

# Docs, `docs/v1.html`

The product explained in its own theme, written from the PRD and reachable from
the nav on every page.

Eleven sections behind a sticky table of contents that follows the reader:
overview and the four-beat loop, how the narrative engine finds and scores a
story, a definition for every number on the board, what a launch actually sets,
how reservations work before launch day, stock rewards, prediction markets,
the X agent, a what-ships-when table, risk and limits, and a FAQ.

Two rules shaped it. Anything not in the MVP is labelled in place, so
`Prediction markets` and `X agent` carry a **Beta** tag and the status table
splits **Live now / At launch / Beta / Deferred** rather than implying it all
exists. And the honest caveats sit next to the mechanism they qualify, not
buried in the footer: a lifecycle state is attention, not a forecast;
permissionless means unvetted; tokenized-stock access is geofenced.

Same theme as everywhere else, plus what a docs page needs: mono table headers,
hairline rows, the lifecycle colours reused as state pills, `code` chips, a
details/summary FAQ and the torn lime tape across the top.

# Landing v1, Ignition

## Theme: Ignition

A narrative behaves like a wildfire. A spark in dry fuel, most die unseen, a
few catch and spread, peak, then burn to ash. The PRD already uses the word
(`ignition_at`, ignition age), and it maps onto the five lifecycle states.

Colour is split by role, not by taste:

- **Green is the instrument.** mstonks itself, the scanner, every CTA.
- **The heat ramp is the subject.** A narrative's life, cold to hot to cold:
  spark `#5b9bf0`, catching `#22c55e`, firestorm `#f0b429`, peak `#f2701f`,
  ash `#5c646e`.

Type is Space Grotesk with JetBrains Mono for every number. Shape is a single
2px radius on chips and buttons, zero on surfaces. Hairlines carry structure,
so there are no glows, no gradient meshes and no decorative grids.

## Structure

Five chapters, built around the two things that separate mstonks from any
other memestock launchpad.

| Chapter | Headline | Job |
|---|---|---|
| Hero | Trade the narrative before the ticker. | Live scanner beside the claim. |
| Scout | Every stock narrative, scored before it trends. | **Differentiator 1a.** |
| Launch | Launch it early. Get paid in the stock. | **Differentiator 1b.** |
| Predict | Then trade daily UP/DOWN on the pair. | **Differentiator 2.** |
| Close | Find the narrative while it is still cheap. | One CTA. |

Copy names the product, not the metaphor. The fire lives in the colour ramp and
the canvases; the words use the vocabulary a trader already has. Lifecycle
labels are the PRD's own states (Emerging, Accelerating, Breakout, Peaking,
Fading), not invented ones.

Cut from the previous version because it explained nothing about what makes
the product different: the six-beat loop, the engine pipeline diagram, the
board as its own marketing section, fee economics as a full screen, the share
card gallery, the chain comparison table, the roadmap and the API section.
Page height went from 12,385px to 4,517px.

## Motion

Three canvas systems, each sized to its own box, each paused by
`IntersectionObserver` when off screen, each disabled under
`prefers-reduced-motion`.

**Ember field** (hero, close). Sparks lift off a dark floor. Most cool and die
on the way up. Roughly one in eleven catches and climbs the heat ramp. The
product thesis as an idle loop.

**Ignition cascade** (scout). A field of dormant posts with proximity links.
One node ignites, a spread front expands, neighbours catch only if they are
close enough, then the whole thing cools and resets. The counters under it are
reading the real simulation, not a script.

**Burn curve** (launch). The interactive centrepiece. Drag your entry along the
narrative's life and the state, the baseline multiple, the entry market cap,
the projected stock rewards and the verdict sentence all recompute together.
Rewards integrate the flow *ahead* of your entry, so arriving late collapses
them. Keyboard accessible with arrow keys, `aria-valuetext` announces the stage.

| Entry | Cap | Rewards |
|---|---|---|
| Emerging | $4.6k | $1,800 |
| Accelerating | $64.8k | $1,700 |
| Breakout | $494k | $997 |
| Fading | $11.8k | $32 |

The interesting result is that waiting from Emerging to Accelerating barely
costs you reward flow, but it costs you a 14x higher entry. The page argues
that with its own numbers.

## Design rules

Built against the published skills, cloned and applied:
[pbakaus/impeccable](https://github.com/pbakaus/impeccable),
[leonxlnx/taste-skill](https://github.com/leonxlnx/taste-skill),
[emilkowalski/skill](https://github.com/emilkowalski/skill),
[devmartinese/awwwards-animations-skill](https://github.com/devmartinese/awwwards-animations-skill).

Audited clean: zero em-dashes, zero middle-dot separators, zero section-number
eyebrows, zero eyebrows above headlines, zero decorative status dots, zero
scroll cues, no `transition: all`, no `100vh`, no raw scroll listener (a
sentinel `IntersectionObserver` drives the nav instead), one radius system, one
accent, no fake window chrome on any panel.

## Verified

- Zero JS errors and zero horizontal overflow at 390, 768, 1024, 1440, 1920px
- Drag, keyboard, stage-strip clicks and the market card all exercised in a
  real browser
- Canvas work pauses off screen and stops entirely under reduced motion

## Next

The terminal screens are itemised in [`DESIGN-CHECKLIST.md`](./DESIGN-CHECKLIST.md).
