---
date: 2026-06-25
session: ae-roundtrip-validation
status: shipped
related: [[2026-06-25-figma-adapter]], [[../research/2026-06-25-figma-motion-adapter]]
---

# AE → motion.md → Figma round-trip — three spec refinements from real data

## Context
Ran a full manual round-trip on a real source comp (a client After Effects project,
specifics withheld): AE extraction → motion.md token mapping → authored into Figma Motion
via the Plugin API → read back. The point was to stress the token vocabulary against
actual extracted motion, which `spec/tokens.md` was explicitly waiting on. Three concrete
refinements fell out, all tool-neutral.

## What changed in the spec

1. **`sequence` gained a `base` (rest value).** Figma Motion keyframes are a *timeline
   overlay* — the node's static geometry is unchanged; the animation plays on top. So the
   resting/design pose is a distinct value the keyframes deviate *from*. If an adapter only
   records keyframes, the rest state is lost. Every track now carries `base`.
   → `spec/tokens.md`.

2. **Added `track group` — multi-property grouping per element.** A lone `sequence` is one
   property, but one element almost always animates several properties in lockstep (the test
   comp moved scale + translate together). Group sequences under the element:
   `{ element, tracks: { scale, translate, … } }`, or the lockstep relationship is lost.
   → `spec/tokens.md`.

3. **Documented the AE `(speed, influence) → cubicBezier` adapter rule** + the
   *easing-on-the-arriving-keyframe* (incoming) convention, both confirmed by read-back.
   → `AUTHORING.md` ("Adapter rules").

## The conversion (for the record)
AE tangents are `(speed, influence%)`. For `speed = 0` on both ends of a segment A→B:
`x1 = outInf_A/100, y1 = 0, x2 = 1 − inInf_B/100, y2 = 1`. Non-zero speed adds
`y = x · speed · dt/dv`. The resulting bézier is stored on the **arriving** keyframe B.

## Why this matters
This is the AE leg of the tool-neutrality test (`spec/tokens.md` open question): AE bézier
handles + springs now demonstrably survive into motion.md and back out into a real motion
tool without flattening. Cavalry easing is the remaining leg.
