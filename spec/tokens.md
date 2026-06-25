# motion.md — Token Vocabulary (DRAFT)

> Stub. The token vocabulary is being validated against real extracted motion data (Cavalry first, then After Effects) before it firms up. Structure shown; values TBD.

## Layer 1 — Primitives (adopt W3C DTCG, don't reinvent)

Use the [W3C Design Tokens](https://www.designtokens.org/tr/drafts/format/) types verbatim so motion tokens interoperate with the rest of a design system:

- `duration` — `{ "value": 240, "unit": "ms" }`
- `cubicBezier` — `[ p1x, p1y, p2x, p2y ]`
- `transition` (composite) — `{ duration, delay, timingFunction }`

## Layer 2 — Choreography (the superset DTCG leaves out of scope)

The part that carries brand identity. Proposed types (names/shapes provisional):

- **`spring`** — physics easing. Primary (designer-facing) form: `{ bounce, duration }` (à la SwiftUI / Figma Motion); `{ stiffness, damping, mass }` kept as an optional physics alternate. **Params are canonical, any baked curve is derived** — validated against Figma's own authoring model, which stores springs as `{ bounce }`, not a flattened curve ([research](../knowledge/research/2026-06-25-figma-motion-adapter.md)). Not expressible as a `cubicBezier`.
- **`sequence`** (keyframes) — ordered stops: `[ { position, value|ref, easing|spring }, … ]`. `position` is a normalized offset or absolute time (adapters convert). Per-stop easing is heterogeneous (linear / cubicBezier / spring). Captures overshoot-and-settle, anticipation→action→follow-through, staggers. *(Shape confirmed against Figma Motion's real keyframe model.)*
- **`pattern`** — a named, reusable choreography (a brand's signature reveal): bundles duration + easing/spring + optional sequence under one name.
- **`intent`** (metadata) — why it moves: `clarify | guide | confirm | warn | celebrate | reveal`. Brief/QA signal, not a render value.
- **`pacing`** (range) — tempo guidance (e.g. cuts/sec band, entrance vs. exit duration bands) for timeline-based / broadcast motion.

## Open questions
- One schema must absorb AE bézier handles **and** Cavalry easing **and** springs without flattening — that diff is the tool-neutrality test (see the Cavalry/AE adapter work).
- Where does `intent` live — on the token, or only in the `motion.md` artifact?
- How much of Layer 2 should be pushed upstream into DTCG vs. kept as a documented superset?
