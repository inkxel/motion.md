# motion.md

**An open, tool-neutral way to describe a brand's motion language — so a person *or* an AI can match it.**

> Status: **early draft / request-for-comment.** The thesis and shape are here; the token vocabulary is being validated against real motion data. Issues and disagreement welcome.

---

## The problem

Design systems govern color, type, and spacing with **tokens** — portable, named, machine-readable values that any tool and any AI can read. Motion is the one foundation that never got the same treatment. It lives in a PDF, a Notion doc, or a 47-slide deck: it *describes* motion, but it doesn't **govern** it.

That gap is why off-the-shelf AI motion "never works" for a brand with a real motion identity — with nothing to match against, it animates a careful brand *like PowerPoint*. The fix isn't a better model. It's giving motion the same legible, portable substrate the rest of the design system already has.

**The wager.** The defensible bet isn't *documenting* motion — proprietary tools already do that. It's that **one tool-neutral token schema can capture brand choreography across radically different motion engines** — an After Effects bézier, a Cavalry easing, a spring — *without flattening them into mush* — and that an AI briefed on it then matches brand motion where vanilla AI fails. This can lose: if no single schema holds across tools without lossy compromise, motion.md collapses into just another per-tool format and the premise is dead. The whole project is an attempt to prove that bet true (or kill it cheaply).

**Appetite.** An open standard, but built in **days-scale probes, not a spec committee** — one adapter at a time, easiest tool first (Cavalry, because `.cv` is already JSON), proving the schema on real motion before widening. If the round-trip doesn't hold early, that's the signal to stop, not to spec harder.

## What motion.md is

Two layers, deliberately separated:

1. **A token vocabulary** — named, machine-readable motion values: durations, easings, springs, multi-step sequences, transition patterns, pacing ranges. Built **on top of the [W3C Design Tokens](https://www.designtokens.org/) standard** where it already covers the primitives (`duration`, `cubicBezier`, `transition`), and extending it where the standard stops.
2. **The `motion.md` artifact** — a per-brand document (the motion sibling of `design.md`) that pairs those tokens with **prose principles, do/don'ts, intent, and a reference library.** It's written to brief a junior editor and an AI equally well.

Tokens are the *what*; the artifact is the *why and when*.

## Why a new standard instead of an existing one

The W3C Design Tokens spec (stable, 2025) defines the motion **primitives** — a single property easing from A→B. It **explicitly leaves out of scope** the layer where brand motion actually lives:

- **Springs / physics-based motion** (stiffness/damping, not a bézier)
- **Keyframe / multi-step sequences** (anticipation → action → follow-through; overshoot-and-settle; staggers)
- **Named motion patterns** (a brand's signature reveal — a reusable choreography, not a raw curve)
- **Intent** (why something moves: clarify, guide, confirm, warn, celebrate, reveal)

`motion.md` is a **superset**: adopt DTCG for the primitives so tokens stay interoperable, and standardize the **choreography layer** on top. (We're also raising this gap upstream in the DTCG working group — the goal is convergence, not a fork.)

## Tool-neutral on purpose

This standard is **not pinned to any one app or runtime.** Not After Effects, not Premiere, not Lottie, not Cavalry, not whatever AI-native motion tool ships next year. The motion stack is being reshuffled fast; a standard that hard-codes one tool's internals inherits that tool's mortality.

The model:

```
        motion.md  (tokens + brand artifact — tool-neutral)
              ▲                         │
   extract    │                         │   apply / QA / generate
              │                         ▼
   ┌──────────┴───────────┐   ┌──────────────────────────┐
   │ adapters (per tool)  │   │ consumers (per tool/AI)  │
   │ AE · Cavalry · …     │   │ review skills · gen · …  │
   └──────────────────────┘   └──────────────────────────┘
```

Each tool gets an **adapter** that reads its motion (keyframes, easing, timing) and emits the same neutral token JSON. If one schema cleanly absorbs, say, an After Effects bézier handle *and* a Cavalry easing *and* a spring, the vocabulary is proven tool-neutral. That shared output contract is the whole game.

## The hard part

Three things could sink this:

- **The schema might not hold.** AE handles, Cavalry's easing, and springs are genuinely different math. The bet is that one representation absorbs all three without flattening — but it might not, and the *rabbit hole* is "fixing" that by quietly lowering every motion to a bézier, which would technically validate while destroying the thing that made the motion branded. If the schema can't stay faithful across tools, the honest move is to say so.
- **Choreography may be irreducibly per-brand.** DTCG punted on named patterns and sequences for a reason — they're hard to generalize. It's possible the choreography layer only ever expresses *one* brand's motion and doesn't standardize. That would still be useful (a per-brand `motion.md` is valuable on its own) but it wouldn't be a *standard*.
- **A standard with no users is a doc.** Adoption isn't in our control. The DTCG-convergence path is the hedge, but the working group may decline, and external implementers may never show up. We treat that as a real risk, not an afterthought.
- **A native tool format could become the de-facto standard.** Figma now ships motion with timeline, easing **variables**, and CSS/JSON/[motion.dev](https://motion.dev) export (Config 2026) — the way their *design* variables quietly became the design-token default. If their motion JSON becomes the format everyone targets, a separate open spec gets steamrolled. The hedge is the founding constraint: stay tool-neutral, align primitives to DTCG, treat Figma as **adapter #1** (ride the rails), and own the cross-tool, brand-choreography layer a single-app vendor has no reason to build.

## Non-goals

- **Not a renderer or a player.** It describes motion; it doesn't draw it.
- **Not an interchange or runtime format.** Delivery formats — a tool's native scene file, an exported clip, a vector animation — carry one *finished* animation. motion.md carries the *language* a thousand of them should share. It composes with whatever you deliver in; it doesn't compete with it.
- **Not AI-generated motion-from-nothing.** The near-term value is making motion legible (so humans stay consistent and AI can brief/QA against it). Generation is a downstream maybe, not the premise.

## How we'll know it worked

Concrete bars, not vibes:

1. **The round-trip holds.** A known motion — a settle, an overshoot, a staggered entrance — authored *both* in Cavalry and in After Effects, extracted through their adapters, produces the **same token JSON**, with no lossy flattening. This is the tool-neutrality proof; without it, nothing else matters.
2. **The brief beats vanilla.** In a blind review, an AI given a brand's `motion.md` catches motion violations that the same AI *without* it misses. That's the whole reason-for-being, made testable.
3. **Someone else picks it up.** An adoption signal outside our own walls: the DTCG working group engages the choreography proposal, or at least one external implementer files an issue or uses the format. A standard is only a standard if it travels.

## Open questions

- Does the choreography layer (named patterns, intent) **generalize across brands**, or is it irreducibly per-brand? (Determines whether this is a *standard* or a great *per-brand artifact*.)
- Where exactly is the line between motion.md and DTCG — how much pushes **upstream** vs. stays a documented superset?
- **Spring representation:** adopt which model — `response`/`bounce` (designer-facing) or `stiffness`/`damping`/`mass` (physics)? Whichever every tool can map to without loss.
- Is **intent** a token property, or only an annotation in the artifact layer?
- Does **Figma's motion JSON** become the de-facto format — and if so, do we converge on it the way we converge on DTCG, or stay deliberately above it? Does Figma Motion read/write DTCG-style tokens, or only its own variables?

## Status & roadmap

- [x] Thesis + layered model (this doc)
- [ ] Token vocabulary v0 — primitives (DTCG-aligned) + first choreography types (spring, keyframe sequence)
- [ ] `motion.md` artifact template (prose + tokens + reference library)
- [ ] Reference adapter: **Figma Motion** (JSON / [motion.dev](https://motion.dev) export, MCP-readable — now the fastest schema-validation target, and the proving ground for the agentic round-trip)
- [ ] Reference adapter: **Cavalry** (`.cv` is JSON-native + scriptable)
- [ ] Reference adapter: **After Effects** (ExtendScript → token JSON) — the non-token-native case that proves neutrality
- [ ] Interop: map the token schema cleanly to/from **[motion.dev](https://motion.dev)** (Figma's web-motion export target)
- [ ] Validator / CLI (does a `motion.md` / token file conform? `lint` · export to DTCG)
- [ ] Authoring guide ([`AUTHORING.md`](AUTHORING.md), v0) + `examples/` (format-by-example)
- [ ] Generic authoring skill in `.agents/skills/` (procedure + gates that travel with the format)
- [ ] DTCG upstream: choreography token-type discussion

## Contributing

This is a request for comment as much as a spec. If you build motion systems — in any tool — the disagreement is the useful part. Open an issue: where does this break for your pipeline, your tool, your brand?

## License

The specification and documentation are **[CC-BY-4.0](LICENSE)** — implement it freely in any tool; attribution appreciated. Reference *tooling* added later (adapters, validator) will carry **Apache-2.0** — the conventional spec/code split for an open standard.

---

*`motion.md` is the motion sibling of `design.md`. Knowledge is the asset.*
