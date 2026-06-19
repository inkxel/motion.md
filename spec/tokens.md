# motion.md — Token Vocabulary (DRAFT)

> Stub. The token vocabulary is being validated against real extracted motion data (Cavalry first, then After Effects) before it firms up. Structure shown; values TBD.

## Layer 1 — Primitives (adopt W3C DTCG, don't reinvent)

Use the [W3C Design Tokens](https://www.designtokens.org/tr/drafts/format/) types verbatim so motion tokens interoperate with the rest of a design system:

- `duration` — `{ "value": 240, "unit": "ms" }`
- `cubicBezier` — `[ p1x, p1y, p2x, p2y ]`
- `transition` (composite) — `{ duration, delay, timingFunction }`

## Layer 2 — Choreography (the superset DTCG leaves out of scope)

The part that carries brand identity. Proposed types (names/shapes provisional):

- **`spring`** — physics easing: `{ stiffness, damping, mass }` (or `{ response, bounce }`). Not expressible as a `cubicBezier`.
- **`sequence`** (keyframes) — ordered stops: `[ { offset, value|ref, easing|spring }, … ]`. Captures overshoot-and-settle, anticipation→action→follow-through, staggers.
- **`pattern`** — a named, reusable choreography (a brand's signature reveal): bundles duration + easing/spring + optional sequence under one name.
- **`intent`** (metadata) — why it moves: `clarify | guide | confirm | warn | celebrate | reveal`. Brief/QA signal, not a render value.
- **`pacing`** (range) — tempo guidance (e.g. cuts/sec band, entrance vs. exit duration bands) for timeline-based / broadcast motion.

## Open questions
- One schema must absorb AE bézier handles **and** Cavalry easing **and** springs without flattening — that diff is the tool-neutrality test (see the Cavalry/AE adapter work).
- Where does `intent` live — on the token, or only in the `motion.md` artifact?
- How much of Layer 2 should be pushed upstream into DTCG vs. kept as a documented superset?
