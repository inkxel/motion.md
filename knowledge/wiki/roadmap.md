# Roadmap — parking lot

The public open-standard repo. Spec-public; the *applied* CoSi tooling (real-brand `motion.md` files, AE introspection, client work) stays in a separate private repo and is not mirrored here.

## Near term
- [ ] Token vocabulary v0 — lock the primitive layer to DTCG; draft `spring` + `sequence` choreography types (`spec/tokens.md`).
- [ ] `motion.md` artifact template — prose principles + tokens + reference-library structure (`spec/motion-md-artifact.md`).
- [ ] **Cavalry reference adapter** — `.cv` is JSON-native + scriptable; fastest schema-validation target. Prove a known motion (settle / overshoot / stagger) round-trips into the token schema.
- [ ] **After Effects reference adapter** — ExtendScript → token JSON; same output contract as Cavalry. The diff between the two = the tool-neutrality proof.
- [ ] Validator — does a token file / `motion.md` conform?
- [ ] **DTCG upstream** — open the choreography-gap discussion issue (draft lives in the private applied repo); converge, don't fork.

## Decisions to make
- [ ] **License** — CC-BY-4.0 (spec text) + Apache-2.0 (reference tooling)? Confirm before first publish.
- [ ] **Naming** — public format = `motion.md` (here, `inkxel`); private applied repo currently `Collier-Simon/motion-md` — rename to avoid `motion.md` / `motion-md` confusion (touches `ae-render` references).
- [ ] Run the `knowledge-layer` scaffolder for the full build-docs setup (this is a hand-seeded stub).

## Context
- 2026-06-19 — repo seeded. Forked-out-the-neutral-spec move (Throughline precedent): the open standard goes public on `inkxel`; applied/brand/client work stays private. Landscape + prior-art research lives in the private applied repo's `knowledge/research/`.
