# motion.md — Knowledge Layer (AGENTS.md)

> **What motion.md is.** An open, tool-neutral standard for describing a brand's motion language: a token vocabulary (built on top of the W3C Design Tokens standard) plus a per-brand `motion.md` artifact, so a person *or* an AI can match a brand's real choreography instead of animating it like generic motion.

This is the **canonical** agent-instructions file for this knowledge layer, per the cross-tool [AGENTS.md](https://agents.md) standard — any agent (Claude Code, Cursor, Codex, Aider, …) reads it. The sibling `knowledge/CLAUDE.md` is a thin pointer back here; keep the orientation in this file, not duplicated there. Claude Code discovers this layer when a session opens this repo's `knowledge/` directory (the `CLAUDE.md` pointer triggers progressive loading, which leads here). Read it before doing architectural work on the standard itself — the token schema, the artifact template, an adapter's output contract — not before simply *reading* `spec/tokens.md` or `AUTHORING.md`, which are self-contained for that.

---

## What this knowledge layer is

A mini wiki + per-session journal + decision records + roadmap, living inside this repo at `knowledge/`. It exists because the real substance of this project is empirical and easy to lose: which token shape survives a round-trip through a real tool (After Effects, Cavalry, Figma Motion), which one doesn't, what got tried and dropped. The whole bet — that one schema can absorb an AE bézier handle, a Cavalry easing, and a spring without flattening them into mush — is being tested one adapter at a time, and the reasoning behind each validation pass surfaces mid-session and gets lost if it isn't captured there and then. This layer makes that legible session-to-session.

### The doc surfaces (don't confuse them)
- **`knowledge/`** (this layer) — *build history*: decisions made while shaping the standard, a per-session journal of validation passes (the AE round-trip, the Figma Motion round-trip), and a curated wiki of how the schema actually behaves once it meets real tool data.
- **`README.md`** — the pitch and the standard's public shape: the problem, the wager, the layered token/artifact model, the open questions, the bars that would prove or kill the bet. Read for *why this exists and what it claims*; `knowledge/` records *what we actually found when we tested the claim*.
- **`AUTHORING.md`** — the how-to for writing a brand's `motion.md` artifact (section order, the sourcing rule, the "springs as params not baked curves" discipline). A user-facing guide, not a build log.
- **`spec/tokens.md`** — the token schema itself, the product. `knowledge/` explains *why* a token shape ended up the way it is; `spec/` states the shape as it stands today.
- **`examples/`** — format-by-example: well-formed `motion.md` files and adapter output. Not build rationale.
- **The private applied repo (`Collier-Simon/motion-md`)** — real-brand `motion.md` files, client AE introspection, and competitive/prior-art research live there, not here (see `knowledge/wiki/roadmap.md`). This public repo carries the tool-neutral spec only and never absorbs client or brand-identifying data.

## How it's organized

```
knowledge/
  AGENTS.md          this file (canonical) — orientation, the three rules, the formats
  CLAUDE.md          thin pointer to AGENTS.md (so Claude Code discovers the layer)
  wiki/              curated reference, one article per subsystem/topic — append-only context logs
  journal/           per-session ADR-style entries — YYYY-MM-DD-slug.md, written continuously
  decisions/         atomic decision records — YYYY-MM-DD-slug.md, ADR format
  research/          research, findings, evaluations, option-analysis — YYYY-MM-slug.md
  wiki/roadmap.md    parking lot — "at some point we should…"
```

`wiki/` is the curated layer. `journal/` and `decisions/` are the firehose. `research/` holds findings. `roadmap.md` is the parking lot.

## README & research conventions

- **The README is WHAT / WHY / DIRECTION / NAVIGATION only.** It's the canonical one-page outline — what this is, why it exists, where it's going, how to navigate.
- **Research never goes in the README.** Findings, evaluations, round-trip validation results, adapter feasibility studies, option-analysis → `knowledge/research/<YYYY-MM>-<topic>.md`. The README states the *decision* that came out of it and links; it never reproduces the research. Test: *if it reads like findings, it's research; if it reads like direction, it's README.*
- **No client/brand names in this public repo.** Real-brand validation work stays in the private applied repo; this repo only carries tool-neutral findings ("AE bézier handles survive the round-trip," never which client's AE file proved it).

## Claim provenance tags (optional, use sparingly)

Beyond article-level `confidence:`, an individual claim inside a wiki article or ADR can carry a provenance tag for *how it was known*, not just how confident you are:
- **`EXTRACTED`** — sourced directly from a real tool export or round-trip (ground truth for the current state, not asserted)
- **`INFERRED`** — assembled from reading or extrapolation (verify before acting)

Use these inline, only when the distinction matters for the reader — it matters a lot here, since the project's whole credibility rests on claims being tool-validated rather than asserted.

---

## The three rules — no more

### Rule 1 — Journal *continuously*, wiki at the end

The journal is the firehose — write to it **as you go, not at session end.** Batching journal writes to the end loses information: you forget, you compress it away, or the session crashes and it's gone. The journal must survive a crash at any point.

**Checkpoint cadence — append to today's `journal/YYYY-MM-DD-slug.md` after any of these, while fresh:**
- a larger move lands (a feature works, a subsystem changes, a milestone closes)
- a longer code run completes (a big edit pass, a refactor, a tricky debug)
- **right after each `git commit`** — the commit is the natural "larger move" marker; journal the *why* the commit message doesn't capture (a breadcrumb hook automates the prompt)
- before any risky/irreversible operation (so intent is recorded even if it goes wrong)

A rough running entry beats a perfect one that never gets written. The entry is append-only — keep adding sections as the session progresses.

**Wiki, by contrast, is end-of-session only.** At session end an **explicit compile pass** promotes mature journal entries into wiki updates (per Rule 2). Without an explicit trigger the wiki stays untouched — that keeps it intentional and the prompt cache stable. *Journal hot and often, compile to wiki cold and deliberately.*

### Rule 2 — Wiki updates require a real trigger — only three
1. A new subsystem/capability got added → write a new article or new section.
2. A previously-documented decision is now contradicted → update the article + log the contradiction.
3. The user explicitly said "document this in the wiki."

No "just in case" updates. No proactive rewrites of articles that aren't broken.

### Rule 3 — Append-only, wiki-links everywhere
Wiki articles are append-only context logs with dated entries. Journal and decision entries are append-only. All cross-references use `[[wiki-links]]`. Never edit prior entries; if something is wrong, append a correction with a link to the contradicting source.

---

## Journal entry format — ADR-flavored

Each `journal/YYYY-MM-DD-slug.md`:

```markdown
---
type: Journal                      # OKF-required concept type (keep it)
date: YYYY-MM-DD
session: short-slug
status: in-progress | shipped | abandoned | superseded-by [[YYYY-MM-DD-slug]]
related: [[article-1]], [[article-2]]
---

# Session Title — What we worked on

## Context
Where we were when we started. What problem this session was responding to.

## What changed
- Paths touched: `src/...`
- Subsystems affected: [[article]]
- Behavior shipped: brief description

## Decisions made
- **Decision** — short statement. Rationale + link: [[decisions/YYYY-MM-DD-slug]]

## What was tried and abandoned
- Tried X — dropped because Y. Saves the next teammate from re-litigating.

## Open threads
- [ ] Next-session item with [[wiki-link]]

## Related
- Touched articles: [[article]]
```

## Decision record format — standard ADR

Each `decisions/YYYY-MM-DD-slug.md`:

```markdown
---
type: Decision                     # OKF-required concept type (keep it)
date: YYYY-MM-DD
status: accepted | proposed | deprecated | superseded-by [[YYYY-MM-DD-slug]]
deciders: <names>
related: [[article-1]]
---

# Decision: One-sentence statement (verb-led, decisive)

## Context
The forces at play. What we were deciding against. Why now.

## Decision
What we're doing.

## Consequences
- **Positive:** what gets easier
- **Negative:** what gets harder, what we're trading away
- **Neutral:** what changes that's neither good nor bad

## Dissent / Alternatives Considered
Options weighed *before* the decision and why each lost. If there were none, say
"None — only viable path given X" so absence is explicit.

## Sources
- [[journal/YYYY-MM-DD-slug]] — session where this surfaced
```

## Wiki article frontmatter convention

```yaml
---
name: short-kebab-slug
type: subsystem | topic | meta | roadmap
created: YYYY-MM-DD
last_updated: YYYY-MM-DD
confidence: high | medium | low | speculative
related: [[other-article-1]], [[other-article-2]]
---
```

**`confidence:`** — **high** (in code / ratified in a decision / repeatedly confirmed) · **medium** (said once, one source, not contradicted) · **low** (inference / single source, provisional) · **speculative** (extrapolation, revisit before acting). Add it when an article is touched for a real reason, not in a bulk pass.

---

## Context log

### 2026-07-01 — AGENTS.md + CLAUDE.md added
This repo's `knowledge/` folder already had `journal/`, `decisions/`, `research/`, and `wiki/roadmap.md` (populated across the AE-round-trip and Figma-Motion-validation sessions) but no orientation file — the roadmap itself had flagged this gap ("Run the `knowledge-layer` scaffolder for the full build-docs setup — this is a hand-seeded stub"). Added `AGENTS.md` (canonical) + `CLAUDE.md` (thin pointer) per the standard convention, sourced from Throughline's own `assets/knowledge-AGENTS.md.tmpl`. No existing journal, decision, or roadmap content was changed.
