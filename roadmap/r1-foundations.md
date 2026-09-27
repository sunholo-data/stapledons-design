# R1: Foundations, from spike to first playable journey

**Status:** Proposed
**Target:** r1 (four milestones after the spike)
**Priority:** P0
**Dependencies:** [ADR 0001](../decisions/0001-engine-and-architecture.md), [relativity spec](../physics/relativity-spec.md)
**Repos:** design (this repo), `sunholo-data/stapledons-godot` (game)
**Implementation:** milestone design docs, sprints and the mission charter
draft live in `stapledons-godot/design_docs/`. The first is
[M1 relativistic sky](https://github.com/sunholo-data/stapledons-godot/blob/main/design_docs/planned/r1/m1-relativistic-sky.md).

## Game vision alignment

| Pillar | Relevance | Score | Notes |
|---|---|---|---|
| Choices Are Final | + | +1 | Journeys are committed and irreversible from M2 |
| The Game Doesn't Judge | 0 | 0 | |
| Time Has Emotional Weight | ++ | +2 | The ship-years vs Earth-years gap is the core of M2 and M4 |
| The Ship Is Home | + | +1 | A placeholder deck scene arrives in M4 |
| Grounded Strangeness | ++ | +2 | The universe looks exactly as it would at 0.99c or near a black hole (M1, M3) |
| **Net** | | **+6** | **Go** |

## Problem

The spike proved the architecture: AILANG simulation as a child process,
Godot rendering, and accurate relativity. But it's one ship on one axis,
looking at 3,802 stars against a black background. The game's central claim is
"travel as fast as you like, live with the consequences". To test that claim we
need one complete journey: plan it, commit to it, live through it, and arrive
to a changed galaxy. Every layer has to be built to the standard the design
docs demand.

## Milestones

### M0: Spike (done, 2026-09-27)
- AILANG `sim/ship.ail`: exact 1 g kinematics via rapidity, run as a child
  process over NDJSON; about 50 µs per tick round trip.
- Godot starfield: per-star aberration, blackbody Doppler colour, point-source
  beaming, HDR.
- Tests: 27 physics reference checks, 9 GPU-vs-CPU golden cases (under 0.1 px),
  simulation vs closed form, VM/interpreter parity.

### M1: The relativistic sky (visual foundation)
**Goal:** what the player sees out any window is correct in every direction, at
any speed and orientation.

1. **Full 3D motion.** Heading and velocity become arbitrary vectors in the
   simulation, and the camera orientation is free. Stars at infinity use
   direction only; nearby stars keep positions, with inverse-square flux (done
   in the spike).
2. **Real star colours.** Re-import CNS5 with Gaia BP−RP and G, and convert to
   T_eff with a published colour–temperature relation. This fixes the current
   class-letter temperatures. It also fixes the catalogue's suspicious class mix
   (2,294 K vs 717 M stars, where real M dwarfs dominate).
3. **Bigger catalogue.** GCNS (about 330k stars within 100 pc), instanced on the
   GPU. Budget: under 2 ms per frame on M4 Max for 330k stars.
4. **Milky Way background.** An HDR equirect or cubemap sky sampled per pixel
   with inverse aberration. The Doppler shift of an RGB panorama needs a
   spectral model: fit a colour temperature per texel offline, store
   (T, luminance), and recolour as a blackbody at D·T with D⁴ radiance. The
   stars in the panorama must be removed (or it must be star-free) so catalogue
   stars aren't counted twice.
5. **Exposure model.** Physically based: set by the eye/camera on scene
   luminance, with auto-exposure that can be clamped by the player.
   Readability options from `open-questions.md` are *explicit settings*, never
   silent fudges.

**Acceptance**
- All relativity-spec §2 check values pass on the CPU. GPU golden tests pass
  with free orientation, including off-axis velocity.
- Reference renders at 0, 0.5, 0.9, 0.99 and 0.999c are committed and diffed
  in CI.
- 60 fps at 1440p with the 330k catalogue and the background on M4 Max.

### M2: Simulation protocol and the journey core (gameplay foundation)
**Goal:** the game's central mechanic, trading your years against the
galaxy's, runs in AILANG and can be tested with no Godot at all.

1. **Protocol v1.** Versioned NDJSON messages: `hello`/`version` handshake,
   `input` (tick, player intents), and `state` (change sets for ship, clock and
   journey). Hand-written AILANG codecs with round-trip tests. Message shapes
   follow AILANG's planned `Render`/`Input`/`Clock` host effects
   (`m-game-engine-effects.md`) so native effects can replace the pipe later.
2. **World clock and ship.** Galaxy time and ship proper time, a mass budget
   stub, and burn/coast/flip/decelerate phases. Exact rapidity integration,
   already proven in the spike.
3. **Journey planner.** Given a target star and a peak speed or acceleration
   profile, compute ship-years, Earth-years, arrival date and crew age. Pure
   AILANG, with closed-form tests. Example: Sol → α Cen (4.37 ly), accelerating
   at 1 g to the midpoint and then decelerating at 1 g, takes 3.582 ship-yr and
   6.003 Earth-yr, with a peak speed of 0.9517c.
4. **Commit.** A committed journey can't be cancelled (Pillar 1). The
   simulation owns this rule, not the UI.
5. **Determinism.** A pure PCG/SplitMix generator in AILANG with named
   streams. Seed plus input log gives byte-identical state on both the VM and
   the interpreter. A replay harness (`make replay`) re-runs a recorded input
   log and diffs the state.
6. **Godot galaxy map (first UI).** A 3D starmap of the catalogue: select a
   target and see the planner's numbers. Crew-age projections are placeholders.

**Acceptance**
- The simulation runs headless from a file of inputs and is covered by
  `ailang test` plus golden state logs.
- The VM and interpreter give identical state for a 10k-tick session with
  journeys.
- The planner matches the closed-form relativistic rocket equations to 1e-9.

### M3: Black holes (GR foundation)
**Goal:** the black hole looks exactly right, because it's where New Game+
begins and where the hard-SF promise is most visible.

1. Offline Schwarzschild null-geodesic integration → a deflection
   lookup-table texture (relativity spec §3).
2. A per-pixel lensed sky (background) plus per-star lensed image positions
   and magnification. The shadow comes from b_c.
3. Composed with SR: the observer's orbit or hover velocity is applied as
   aberration and Doppler in the local static frame. The observer's
   gravitational blueshift is applied to incoming light.
4. A black-hole demo scene in the simulation: approach, hover, and the
   gravitational time dilation shown on the HUD (the AILANG simulation computes
   √(1 − r_s/r)).

**Acceptance**
- Shadow angular radius matches sin α = (b_c/r)√(1 − r_s/r) to 0.5 px at
  10, 5 and 3 r_s.
- The weak-field limit matches 2r_s/b to 1% at b = 100 r_s.
- An Einstein ring appears for a star placed behind the hole, at the predicted
  angle.

### M4: First journey, a vertical slice
**Goal:** a first playable loop. Plan → commit → transit → arrive → consequence.

1. **Galaxy map:** pick α Centauri, see the plan, commit.
2. **Transit:** a placeholder deck scene (one painted image with a masked
   window) with the live relativistic sky composited into the window through a
   SubViewport (`viewport-compositing.md`). Time warp; the HUD shows both
   clocks.
3. **Arrival:** decelerate, and the sky relaxes to normal.
4. **Consequence stub:** the Earth-side simulation advances by the elapsed
   Earth-years. One scripted "news from home" beat, text only, driven by the
   size of the time gap.
5. **Return trip:** home is now years older, and the legacy log records it.

**Acceptance**
- A new player finishes the loop in under 10 minutes.
- Every number shown matches the simulation.
- A replay of the session is byte-identical.

## Out of scope for R1

Civilization simulation, the crew psychology model, dialogue with Gemini, the
Archive, trade, the 1M-year endgame, TTS and music. They come in R2 and later,
on top of the M2 protocol.

## Decisions needed before or during R1

1. **Ship interior presentation.** The docs contradict each other:
   - isometric tiles;
   - first-person 3D (design-decisions, 2025-12-18);
   - painted 2D/2.5D scenes with parallax and a deck selector
     (`scene-based-interior-navigation.md`, 2025-12-20; the newest).

   R1 assumes the newest (painted scenes) for the M4 placeholder. Please
   confirm, and add the entry to `vision/design-decisions.md`.
2. **Readability vs accuracy** (`open-questions.md`). Proposal: accuracy is the
   default and cannot be changed silently. Accessibility and readability aids
   (exposure clamps, "navigation view" overlays) are explicit, labelled
   player settings.
3. **AILANG version pinning.** Pin to a release per milestone, and track the
   v0.46+ `--strict-bytecode` flag and the VM parity fixes.
4. **Target platforms.** None are specified in the docs. Proposal: macOS first
   (the dev machine), then Windows/Linux, with web as a stretch goal (the
   sidecar would then need AILANG WASM that runs outside the browser).

## AILANG upstream asks (tracked via `ailang messages`)

Sent 2026-09-27 from `stapledons_godot`:
- **Bug:** `ailang run` prints its progress banner to stdout, which corrupts
  stdio protocols.
- **Bug:** MOD010 ignores the ailang.toml it finds upward from the file unless
  `--package-dir` is passed.
- **DX:** `run --package-dir` vs `check --package` are inconsistent flags for
  the same concept.
- **Feature:** hyperbolic functions and `expm1`/`log1p` in `std/math`.

Also sent 2026-09-27:
- **Bug:** whole-number float literals (`4.0`) evaluate as Int inside `test`
  blocks.
- **Verifier:** `exp`/`log`/`sqrt` calls leak into SMT as undeclared
  constants, giving ERROR instead of SKIPPED.
- **Feature:** a `--bytecode-report` listing calls bridged to the interpreter
  (a coverage map for Phase 2E).

Published: `sunholo/relativity@0.1.0`, the pure physics core used by the
simulation (PR sunholo-data/ailang-packages#80).

Likely future asks, once they block: automatic JSON codecs for records and
ADTs; splittable random numbers in the stdlib; non-blocking AI calls; host
effects (v1.1).

## Risks

| Risk | Mitigation |
|---|---|
| Bytecode VM gives silently wrong results | VM vs interpreter parity check in CI on every simulation change |
| The per-tick protocol grows into draw calls, as the old `DrawCmd` did | Protocol carries game state only; all presentation decisions stay in Godot |
| The design docs assume the Go engine | Go-specific docs moved to `legacy/`; each feature doc is revised when its milestone starts |
| Performance of large catalogues or histories in AILANG | Arrays and maps, not lists; heavy history runs offline in batches; spatial queries are precomputed or passed in from Godot |
