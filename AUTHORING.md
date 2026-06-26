# Authoring a `motion.md`

> How to write a brand's `motion.md` — for a human or an agent. **v0 / scaffold.** The token vocabulary (`spec/tokens.md`) is still settling; this guide grows with it. See `examples/` for well-formed files (format-by-example is the fastest way in).

A `motion.md` has two parts, deliberately separated:

1. **Tokens** — structured, machine-readable motion values (front-matter / a token block).
2. **Prose** — principles, intent, do/don'ts, and a reference library (Markdown body).

Tokens are the *what*; prose is the *why and when*. An agent briefs and QAs against both.

## Section order
1. **Identity** — one line on the brand's motion personality.
2. **Principles** — the motion philosophy; do/don'ts.
3. **Tokens** — durations, easings, springs, sequences, named patterns (see `spec/tokens.md`).
4. **Intent map** — which motion is used for what (clarify / guide / confirm / celebrate / reveal).
5. **Reference library** — exemplar clips/specimens tagged to principles.

## How to fill each layer
- **Primitives (duration, easing/cubicBezier)** — adopt the W3C DTCG types; don't reinvent.
- **Springs** — record the **physical params** (`response`/`bounce` or `stiffness`/`damping`/`mass`). See the sourcing rule below — this is the one people get wrong.
- **Sequences (keyframes)** — ordered `{position, value, easing}` stops, grouped per element into tracks (one element animates several properties at once), each track with a `base` (rest value). See the adapter rule below for the easing conversion.
- **Named patterns** — a brand's signature, composed from the above under one name.
- **Intent & principles** — prose; this is the layer no tool exports, and the layer that makes motion *branded*.

## The sourcing rule (the load-bearing discipline)
**Source motion values from a real motion source, never from a static guideline deck.** A scrollable guidelines deck shows a human *what* motion looks like but carries none of the actual values — duration, easing, the spring. Pull values from the design/animation tool (its motion export, source file, or a motion-aware MCP), not from screenshots.

**Keep springs as params, not as baked curves.** Tools commonly export a spring as a *flattened* easing curve — an analytic function or a sampled `linear()` list — which replays correctly but **throws away the spring's parameters**, so no one downstream can retune it. When a source only gives you the baked curve, record it as a *derived* value and recover/estimate the params; the params are canonical, the curve is derived. (Empirical basis: a tool's motion export gave the same spring as both a closed-form function *and* a 51-point `linear()` sample — neither preserved the params.)

## Adapter rules (tool → tokens)
The tool-specific conversions that keep extraction lossless. Validated against a real source, not assumed.

**After Effects easing → `cubicBezier`.** AE stores each keyframe tangent as `(speed, influence%)`, not a bézier. For a segment between keyframes A→B with `speed = 0` on both tangents (the common Easy-Ease case):

```
x1 = outInfluence_A / 100      y1 = 0
x2 = 1 − inInfluence_B / 100   y2 = 1
```

Non-zero speed adds the slope term `y = x · speed · dt / dv` (dt = segment duration, dv = value delta). Example: `out 60 / in 90` → `cubic-bezier(0.6, 0, 0.1, 1)`.

**Easing lives on the *arriving* keyframe** (incoming convention — matches Figma Motion and CSS `@keyframes`): the curve for segment A→B is stored on B, not A. The first keyframe's easing is inert. *(Confirmed by AE → Figma round-trip read-back.)*

**Positions:** AE keyframe times are absolute seconds; normalize to `0–1` if the target wants offsets. Translation is usually absolute in the source's coordinate space — record it as a delta from the rest keyframe so the mapping to a target frame is a single transform.

## Validating
A conformance validator/CLI is on the roadmap (`lint` / conform / export to DTCG). Until then, check against `spec/tokens.md` and the `examples/`.

## For agents
A generic authoring skill (procedure + gates) will live in `.agents/skills/` so it travels with the format. Until it ships, follow this guide + the sourcing rule above.
