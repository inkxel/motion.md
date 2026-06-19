---
date: 2026-06-19
session: repo-seeded
status: shipped
related: [[roadmap]]
---

# motion.md — public spec repo seeded

## Context
Spun out of a research thread that started from LottieFiles' new "Motion System." Decision (Tucker, 2026-06-19): the **open standard** goes public on **`inkxel`** (personal), while the **applied** CoSi work — real-brand `motion.md` files, AE introspection, client AE sources — stays in the private `Collier-Simon/motion-md` repo. Same "fork the neutral spec out, keep the applied work private" move as Throughline (out of the private knowledge-layer skill).

## What changed
- Seeded `inkxel/motion.md` (local; not yet pushed to GitHub — Tucker's trigger).
- `README.md` — thesis, layered model (DTCG primitives + choreography superset), tool-neutral adapter/consumer model, non-goals, status.
- `spec/tokens.md` — token vocabulary stub (Layer 1 = DTCG primitives; Layer 2 = spring / sequence / pattern / intent / pacing).
- `knowledge/` — lean hand-seeded layer (this journal + roadmap). Full `knowledge-layer` scaffold TBD.

## Decisions made
- **Public host = inkxel** (open standard → personal OSS thought-leadership, like Throughline + fonts). Applied tooling stays Collier-Simon (private).
- **Tool-neutral is the founding constraint** — not AE/Premiere/Lottie/Cavalry-bound; each tool is an adapter, the shared token JSON is the contract.

## Open threads
- [ ] Tucker: create + push `inkxel/motion.md` (public) — outward action, held for his go.
- [ ] License decision (CC-BY-4.0 spec + Apache-2.0 tooling).
- [ ] Naming: rename private `Collier-Simon/motion-md` → e.g. `cosi-motion` to kill `motion.md`/`motion-md` ambiguity (touches `ae-render` refs).
- [ ] Token vocabulary v0 validated via the Cavalry adapter, then ported to AE.
