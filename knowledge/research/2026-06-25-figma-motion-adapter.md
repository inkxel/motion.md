# Figma Motion as a motion.md adapter — schema + first real validation

> 2026-06-25. Figma shipped **Figma Motion** at Config 2026 (June 24) — a native keyframe/timeline system. It exposes motion through two surfaces, and the difference between them is instructive for motion.md. Findings below are from Figma's public Plugin API + its own "Introducing Motion" sample file.

## Two surfaces, very different fidelity

| Surface | What you get | Fidelity for tokens |
|---|---|---|
| **Plugin API** (`use_figma` / `node.manualKeyframeTracks`) | the **authoring model** — keyframes with typed values + easing **including springs as parameters** | **lossless** |
| **`get_motion_context`** (Dev Mode export) | pre-baked CSS `@keyframes`, motion.dev, JSON for handoff | **lossy** — springs flattened to a sampled `linear()` / analytic function |

**The same spring** read two ways: the Plugin API returns `{ type: "CUSTOM_SPRING", easingFunctionSpring: { bounce: 0.674 } }`; the export returns a 51-point CSS `linear(0, 0.0875, …)`. The export plays correctly but the spring's parameters are gone — you can't retune it.

**→ A motion.md adapter for Figma must read the authoring model (Plugin API), not the export.** The export is for handoff/codegen, not extraction.

## The Figma authoring schema (read off a real animated node)

```
node.manualKeyframeTracks = {
  <PROPERTY>: {                          // e.g. "TRANSLATION_XY", "SCALE_XY"
    baseValue: { type: "VECTOR", value: { x, y } },
    keyframes: [
      {
        timelinePosition: <seconds>,     // ABSOLUTE seconds (not normalized 0–1)
        value: { type: "VECTOR", value: { x, y } },
        easing: {
          type: "LINEAR" | "CUSTOM_CUBIC_BEZIER" | "CUSTOM_SPRING",
          easingFunctionCubicBezier?: { x1, y1, x2, y2 },
          easingFunctionSpring?: { bounce }
        }
      }, …
    ]
  }
}
```
- Timelines are owned by a **container frame** (`frame.timelines: [{ id, duration }]`), not a loose layer.
- Easing is **per-keyframe-segment** and heterogeneous: a single track mixes `LINEAR`, cubic-bézier, and spring.

## What this validates for motion.md

- **`sequence` (keyframes)** ✅ — Figma's `keyframes[]` of `{ timelinePosition, value, easing }` is exactly the proposed `sequence` type. Confirmed against real data.
- **`cubicBezier`** ✅ — present as `{x1,y1,x2,y2}` per segment. DTCG-aligned.
- **`spring`** ✅ and **params are canonical** — Figma's *own internal model* stores the spring as a parameter (`bounce`), not a baked curve. This independently confirms motion.md's decision: **spring = params primary, curve derived.** A vendor with every incentive to ship a simple export still keeps params as the source of truth.
- **Spring param model** — Figma uses **`bounce` + duration** (the modern designer-facing model, à la SwiftUI `spring(duration:bounce:)`), not stiffness/damping/mass. motion.md should offer the designer-facing `{ bounce, duration }` form as the primary spring representation, with physics params optional.

## Refinements folded into `spec/tokens.md`
- `spring` primary form → `{ bounce, duration }` (physics `{ stiffness, damping, mass }` kept as an optional alternate).
- `sequence` keyframe stop → confirmed `{ position, value, easing }`; note position can be absolute time (Figma) or normalized offset (motion.dev export) — the schema should normalize one way and record which.

## Open
- Map Figma's full animatable-property enum (confirmed `TRANSLATION_XY`, `SCALE_XY`; need rotation, opacity, etc.).
- Decide motion.md's canonical position unit (absolute seconds vs. normalized 0–1) — adapters convert.
