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
- [x] **License — DECIDED 2026-06-19:** spec/docs = **CC-BY-4.0** (`LICENSE`); reference tooling (adapters/validator) = **Apache-2.0** when added.
- [x] **Name — confirmed `motion.md`** (Tucker, 6/19). Note: if motion.md takes off, CoSi adopts it — public standard and CoSi's applied use converge, so there's no real conflict, just a clarity rename of the private repo when convenient.
- [ ] **Rename private applied repo** `Collier-Simon/motion-md` → e.g. `cosi-motion` to avoid `motion.md`/`motion-md` confusion (touches `ae-render` references). Not urgent.
- [ ] Run the `knowledge-layer` scaffolder for the full build-docs setup (this is a hand-seeded stub).

## Context
- 2026-06-19 — repo seeded **and pushed public: https://github.com/inkxel/motion.md** (default branch `main`, CC-BY-4.0). Forked-out-the-neutral-spec move (Throughline precedent): the open standard is public on `inkxel`; applied/brand/client work stays in the private `Collier-Simon/motion-md`. Landscape + prior-art research + the DTCG issue draft + Cavalry adapter scoping live in the private applied repo's `knowledge/research/`.
