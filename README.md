# motion.md

**An open, tool-neutral way to describe a brand's motion language — so a person *or* an AI can match it.**

> Status: **early draft / request-for-comment.** The thesis and shape are here; the token vocabulary is being validated against real motion data. Issues and disagreement welcome.

---

## The problem

Design systems govern color, type, and spacing with **tokens** — portable, named, machine-readable values that any tool and any AI can read. Motion is the one foundation that never got the same treatment. It lives in a PDF, a Notion doc, or a 47-slide deck: it *describes* motion, but it doesn't **govern** it.

That gap is why off-the-shelf AI motion "never works" for a brand with a real motion identity — with nothing to match against, it animates a careful brand *like PowerPoint*. The fix isn't a better model. It's giving motion the same legible, portable substrate the rest of the design system already has.

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
   ┌──────────┴───────────┐   ┌─────────────────────────┐
   │ adapters (per tool)  │   │ consumers (per tool/AI)  │
   │ AE · Cavalry · …     │   │ review skills · gen · …  │
   └──────────────────────┘   └─────────────────────────┘
```

Each tool gets an **adapter** that reads its motion (keyframes, easing, timing) and emits the same neutral token JSON. If one schema cleanly absorbs, say, an After Effects bézier handle *and* a Cavalry easing *and* a spring, the vocabulary is proven tool-neutral. That shared output contract is the whole game.

## Non-goals

- **Not a renderer or a player.** It describes motion; it doesn't draw it.
- **Not an interchange or runtime format.** Delivery formats — a tool's native scene file, an exported clip, a vector animation — carry one *finished* animation. motion.md carries the *language* a thousand of them should share. It composes with whatever you deliver in; it doesn't compete with it.
- **Not AI-generated motion-from-nothing.** The near-term value is making motion legible (so humans stay consistent and AI can brief/QA against it). Generation is a downstream maybe, not the premise.

## Status & roadmap

- [x] Thesis + layered model (this doc)
- [ ] Token vocabulary v0 — primitives (DTCG-aligned) + first choreography types (spring, keyframe sequence)
- [ ] `motion.md` artifact template (prose + tokens + reference library)
- [ ] Reference adapter: **Cavalry** (`.cv` is JSON-native + scriptable — the fastest schema-validation target)
- [ ] Reference adapter: **After Effects** (ExtendScript → token JSON)
- [ ] Validator (does a `motion.md` / token file conform?)
- [ ] DTCG upstream: choreography token-type discussion

## Contributing

This is a request for comment as much as a spec. If you build motion systems — in any tool — the disagreement is the useful part. Open an issue: where does this break for your pipeline, your tool, your brand?

## License

The specification and documentation are **[CC-BY-4.0](LICENSE)** — implement it freely in any tool; attribution appreciated. Reference *tooling* added later (adapters, validator) will carry **Apache-2.0** — the conventional spec/code split for an open standard.

---

*`motion.md` is the motion sibling of `design.md`. Knowledge is the asset.*
