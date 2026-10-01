# Stapledon's Voyage: design

*Travel as fast as you like. Live with the consequences.*

This repo is the source of truth for the game's design, independent of any
engine. The game is being rebuilt from it:

- **Rendering:** Godot 4.
- **Simulation:** AILANG.

See [ADR 0001](decisions/0001-engine-and-architecture.md) for the reasons.

| Where | What |
|---|---|
| [vision/](vision/) | Game vision, core pillars, the design-decision log, interview log, open questions |
| [features/](features/) | Feature designs by phase (data models, core views, gameplay, polish, future backlog) |
| [physics/relativity-spec.md](physics/relativity-spec.md) | **Normative** accuracy spec for the SR/GR visuals, with check values (RS-n) and the audit of the old implementation |
| [physics/higgs-bubble.md](physics/higgs-bubble.md) | **Normative**: the Higgs bubble as the one admitted hand-wave, and every consequence derived exactly, with check values (HB-n) |
| [lore/archive/](lore/archive/) | Player-facing physics entries for the in-game Archive; their numbers are checked against HB/RS check values |
| [decisions/](decisions/) | Architecture decision records |
| [roadmap/r1-foundations.md](roadmap/r1-foundations.md) | Current roadmap: spike → relativistic sky → journey core → black holes → first playable journey |
| [reference/](reference/) | Cross-cutting references: AI capabilities, RNG and determinism, eval system, performance |
| [rejected/](rejected/) | Rejected directions, and why |
| [legacy/](legacy/) | Documents tied to the retired Go/Ebiten engine, kept for history |

## Provenance

These documents were split out of
[`sunholo-data/stapledons_voyage`](https://github.com/sunholo-data/stapledons_voyage)
at commit `930eca1` (2026-03-16):

| Original path | Moved to |
|---|---|
| `docs/vision/`, `docs/game-vision.md` | `vision/` |
| `design_docs/planned/` | `features/` |
| `design_docs/rejected/` | `rejected/` |
| Engine-independent parts of `design_docs/reference/` | `reference/` |
| Everything Go-engine-specific | `legacy/` |

Feature docs still contain implementation notes written for the Go engine.
Each doc is revised for Godot + AILANG when its roadmap milestone starts.

## Known contradictions to resolve

- **Ship interior presentation.** *Resolved 2026-09-28 (D-6):* an isometric
  three-layer view from inside the bubble (`vision/design-decisions.md`).
- **Radiation shielding.** "Radiation Shielding Automatic" (2025-12-06)
  filters photons by energy, but the D-11 wall passes light of every energy
  (`physics/higgs-bubble.md` §7). This needs Mark's ruling.
