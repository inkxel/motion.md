---
date: 2026-06-25
session: figma-adapter
status: shipped
related: [[research/2026-06-25-figma-motion-adapter]]
---

# Figma Motion — first real schema validation

## Context
Figma shipped Figma Motion at Config 2026 (6/24). Probed its Plugin API + read motion off its own sample file to validate the motion.md token vocabulary against real tool data.

## What changed
- Added [[research/2026-06-25-figma-motion-adapter]] — the Figma authoring schema + the lossless-vs-lossy finding.
- Updated `spec/tokens.md`: `spring` primary form → `{ bounce, duration }` with **params canonical / curve derived** (validated by Figma's own model); `sequence` stop shape confirmed.

## Findings
- motion.md's **`sequence`, `cubicBezier`, and `spring` types all appear in Figma's real authoring model** — first validation against a shipping tool.
- **Springs are stored as params** (`{ bounce }`) in the authoring model; the *export* flattens them to a sampled `linear()`. → read via the **Plugin API** (lossless), not the export (lossy). A vendor keeping params as the source of truth confirms the "params canonical" call.

## Open
- Map Figma's full animatable-property enum.
- Decide motion.md's canonical position unit (absolute seconds vs normalized 0–1).
