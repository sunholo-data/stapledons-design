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
| [physics/relativity-spec.md](physics/relativity-spec.md) | **Normative** accuracy spec for the SR/GR visuals, with check values and the audit of the old implementation |
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

- **Ship interior presentation.** Three versions exist: isometric; first-person
  3D (`vision/design-decisions.md`, 2025-12-18); and painted 2D/2.5D scenes
  with a deck selector (`features/scene-based-interior-navigation.md`,
  2025-12-20, the newest). The decision log has no entry for the newest one.
  See the R1 roadmap, "Decisions needed".
