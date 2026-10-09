# R1: Foundations, from spike to first playable journey

**Status:** Proposed
**Target:** r1 (four milestones after the spike)
**Priority:** P0
**Dependencies:** [ADR 0001](../decisions/0001-engine-and-architecture.md), [relativity spec](../physics/relativity-spec.md), [Higgs bubble physics](../physics/higgs-bubble.md)
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
6. **Forward CMB glow** (added 2026-10-01; a renderer gap found by D-11). The
   CMB ahead is a blackbody at T_CMB·γ(1+β), with its temperature halving 1/γ
   rad from the forward pole. It is faintly visible from γ ≈ 146 and plainly
   from γ ≈ 275, and it reaches 3,853.7 K at γ 707
   ([higgs-bubble.md §7](../physics/higgs-bubble.md), HB-62 to HB-70; game
   charter row 6a). It needs a CPU check of HB-62, HB-63 and HB-68, and a
   golden case.

**Acceptance**
- All relativity-spec §2 check values pass on the CPU. GPU golden tests pass
  with free orientation, including off-axis velocity.
- Reference renders at 0, 0.5, 0.9, 0.99 and 0.999c are committed and diffed
  in CI.
- The forward CMB glow matches HB-63 and HB-68, with a reference render at
  γ 707.
- 60 fps at 1440p with the 330k catalogue and the background on M4 Max.

### M2: Simulation protocol and the journey core (gameplay foundation)
**Status:** Landed 2026-10-02. Sprint `R1-M2-JOURNEY` in the game repo: 10
milestones, PRs #22–#35 (plus the catalogue fix #37), independent evaluations
89–96/100. `sunholo/relativity` 0.4.0 published; protocol v2; planner equal
to the closed form to 1e-9; the commit rule enforced in the simulation;
SplitMix64 named streams; a 10k-tick replay byte-identical on the VM and the
interpreter (goldens per architecture until ailang#1465); galaxy map with the
commit dialog. R1 charter bar clause 2 met. Report:
[m2-report.md](https://github.com/sunholo-data/stapledons-godot/blob/main/design_docs/implemented/r1/m2-report.md);
design:
[m2-journey-core.md](https://github.com/sunholo-data/stapledons-godot/blob/main/design_docs/implemented/r1/m2-journey-core.md).
Item 5 shipped SplitMix64 (`splitmix64-1`), not PCG.

**Goal:** the game's central mechanic, trading your years against the
galaxy's, runs in AILANG and can be tested with no Godot at all.

1. **Protocol v1.** Versioned NDJSON messages: `hello`/`version` handshake,
   `input` (tick, player intents), and `state` (change sets for ship, clock and
   journey). Hand-written AILANG codecs with round-trip tests. Message shapes
   follow AILANG's planned `Render`/`Input`/`Clock` host effects
   (`m-game-engine-effects.md`) so native effects can replace the pipe later.
2. **World clock and ship.** Galaxy time and ship proper time, and the
   journey phases **boost → cruise → brake** (D-11). Boost and brake take
   minutes of ship time; cruise is at the chosen speed. There's no flip and no
   zero-g coast: the generator holds 1 g. Exact rapidity integration, already
   proven in the spike. The mass-budget stub becomes an **m_eff plus
   energy-ledger readout**: boost and brake energy m_eff c² φ (HB-35, HB-36), and ISM
   drag energy n γβ m_p c² A d at a constant Local Bubble density (HB-51 to HB-56). It's
   a readout only; an enforced budget comes later.
3. **Journey planner.** Given a target star and a cruise speed (0.9c to
   0.999999c, within the γ cap), compute ship-years, Earth-years, arrival date,
   crew age and the energy ledger. Pure AILANG, with closed-form tests.
   Example: Sol → α Cen (4.37 ly) at 0.99c takes 4.414 Earth-yr and
   0.6227 ship-yr, or 227.4 ship-days (HB-20 to HB-22). The 1 g flip-and-burn (3.582
   ship-yr, 6.003 Earth-yr, peak 0.9517c; HB-27 to HB-29) is no longer the gameplay
   profile. It stays a `sunholo/relativity` journey check value.
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
- The planner matches the closed-form relativistic equations to 1e-9,
  including HB-20 to HB-22 and HB-27 to HB-29.
- The energy-ledger readout matches HB-35, HB-36 and HB-51 to HB-56.

### M3: Black holes (GR foundation)
**Status:** Landed 2026-10-08. Sprint `R1-M3-BLACK-HOLES` in the game repo
(approved D-53, executed attended), shipped in `v0.4.0-dev.23-black-hole`. Bar clause 3
**MET**: shadow vs Synge 0.001 / 0.010 / 0.006 px at 10 / 5 / 3 r_s; weak field 0.148 %
at b = 1000 and 2.68e-4 at b = 100; Einstein ring within 0.032 px; the geodesic
integrator ships in `sunholo/relativity` 0.10.0. Evidence: the game repo's
`design_docs/implemented/r1/m3-report.md`. Sgr A* is non-spinning with mass a scenario
parameter (D-53), so the canon's rogue hole can reuse it; spin (Kerr) and the true
Galactic-Centre sky are follow-ups.
**Goal:** the black hole looks exactly right, because it's where New Game+
begins and where the hard-SF promise is most visible.

1. Offline Schwarzschild null-geodesic integration → a deflection
   lookup-table texture (relativity spec §3).
2. A per-pixel lensed sky (background) plus per-star lensed image positions
   and magnification. The shadow comes from b_c.
3. Composed with SR: the observer's orbit or hover velocity is applied as
   aberration and Doppler in the local static frame. The observer's
   gravitational blueshift is applied to incoming light.
4. A black-hole demo scene in the simulation at **Sgr A*** (4.297 × 10⁶ M☉,
   about 27,000 ly; D-13). Sgr A* replaced Gaia BH1 because the wall doesn't
   shield tides (HB-72 to HB-86). The ship is placed there without a journey,
   and the lensed sky is the real sky in that direction. The scene covers
   approach and hover over r ∈ [2, 10⁶] r_s. The HUD shows the gravitational
   time dilation (the AILANG simulation computes √(1 − r_s/r)) and the tidal
   acceleration across the bubble. Near-horizon dives belong to the time-skip /
   New Game+ milestone.
5. For an observer at r, a 2D δ(ψ, r) table, from the integrator or the exact
   elliptic form, each cross-checking the other (spec §3).

**Acceptance**
- Shadow angular radius matches sin α = (b_c/r)√(1 − r_s/r) to 0.5 px at
  10, 5 and 3 r_s.
- Weak field (D-13): within 1% of 2r_s/b at b = 1000 r_s, and within 3e-4 of
  2r_s/b + (15π/16)(r_s/b)² at b = 100 r_s (RS-14, RS-15). The golden tests at
  10, 5 and 3 r_s are mass-independent.
- The HUD's tidal readout at Sgr A* matches HB-84 to HB-86.
- An Einstein ring appears for a star placed behind the hole, at the predicted
  angle.

### M4: First journey, a vertical slice
**Goal:** a first playable loop. Plan → commit → transit → arrive → consequence.

1. **Galaxy map:** pick α Centauri, see the plan, commit. The default cruise
   speed is **0.99c** (γ 7.0888: about 227 ship-days and 4.414 Earth-years each
   way; ISM readout 1.37 × 10¹⁷ J, or 1.52 kg; HB-20 to HB-22, HB-53, HB-54). The player can
   change it. Commit is one dialog showing both clocks (D-12).
2. **Transit:** the **D-6 interior**, an isometric three-layer view from inside
   the bubble (Blender play area, interior panorama, then the live relativistic
   sky through the panorama's exported camera). "Up" is the direction of
   travel throughout; there's no flip (D-14). Time warp; the HUD shows both
   clocks. Interior art waits for Mark's approval of the bridge style frame.
3. **Arrival:** brake to a **1,000 AU stand-off** from the target star, where
   α Cen A is about magnitude −12.2, with A and B about 1.3° apart (HB-91 to HB-94). The
   starbow relaxes to the normal sky.
4. **Consequence stub:** the Earth-side simulation advances by the elapsed
   Earth-years. One scripted "news from home" beat, as a text panel at the
   Archive terminal only, driven by the size of the time gap.
5. **Return trip:** home is now years older, and the legacy log records it.
6. **Archive physics lore:** the entries in
   [`lore/archive/`](../lore/archive/) are readable at the Archive terminal as
   they unlock. Their numbers are checked against HB/RS check values by test.

**Acceptance**
- The scripted, deterministic minimum-path proxy completes the loop in
  ≤ 360 s, with transit legs of 45–120 s at the default warp. The
  three-new-player human playtest moves to R2 (D-14).
- Every number shown matches the simulation.
- A replay of the session is byte-identical.
- Every lore entry's numbers match its cited check values (test).

## Out of scope for R1

Civilization simulation, the crew psychology model, dialogue with Gemini, the
Archive (except its physics lore entries and the news panel, M4), trade, the 1M-year endgame, TTS and music. They come in R2 and later,
on top of the M2 protocol.

## Decisions needed before or during R1

1. **Ship interior presentation.** *Resolved 2026-09-28 (D-6):* an isometric
   three-layer view from inside the bubble. See `vision/design-decisions.md`.
2. **Readability vs accuracy** (`open-questions.md`). Proposal: accuracy is the
   default and cannot be changed silently. Accessibility and readability aids
   (exposure clamps, "navigation view" overlays) are explicit, labelled
   player settings.
3. **AILANG version pinning.** Pin to a release per milestone, and track the
   v0.46+ `--strict-bytecode` flag and the VM parity fixes.
4. **Target platforms.** None are specified in the docs. Proposal: macOS first
   (the dev machine), then Windows/Linux, with web as a stretch goal (the
   sidecar would then need AILANG WASM that runs outside the browser).

## Decisions (attended rulings, 2026-10-01)

Recorded in `vision/design-decisions.md` and the game repo's charter ledger:
- **D-11:** the Higgs bubble model. One admitted hand-wave with three
  properties; exact physics for the rest
  ([physics/higgs-bubble.md](../physics/higgs-bubble.md)). The journey is
  boost → cruise → brake; the ISM elastic mirror; physics as Archive lore.
- **D-12:** M2's remaining questions. A relative calendar ("Earth +6.003 yr")
  with start_age 30; manual thrust only in diagnostic sessions, no pause; a
  commit dialog showing both clocks, with a 1.5 s hold.
- **D-13:** M3. The weak-field check pair; the δ(ψ, r) table allowance; the
  hover range [2, 10⁶] r_s; the demo hole Sgr A*.
- **D-14:** M4. No flip; 0.99c default; a 1,000 AU stand-off; news at the
  Archive terminal; art waits for style-frame approval; a scripted proxy in
  R1, with the human playtest in R2.
- **D-15:** shielding is real glazing inside (the wall passes light of every
  energy; dome and hull absorb UV/X-rays); the slider reaches 0.999999c with the
  ISM cost as the brake (the 10–20 default γ cap is retired); the boundary does
  not refract; bubble defaults m_eff 1 kg, boost 7.5×10⁵ g, ε 10⁻⁹ (now 10⁻¹⁰: D-29 and its follow-up).
- **D-29 / D-30:** the forward glow is faint at cruise and a cue from about
  0.997c (ε 10⁻¹⁰, chosen from a rendered comparison), and its spectrum is a
  blackbody at the impact temperature (K cos θ/σ)^¼, computed in
  `sunholo/relativity` 0.8.0 (higgs-bubble.md §6).
- **D-60 / D-61:** the medium is the real one (status 2026-10-09: PR A in
  review). New games fly `lism-1` ([ism-structure.md](../physics/ism-structure.md)):
  the Local Interstellar Cloud, 14 Redfield & Linsky clouds and the Local
  Bubble's hot gas, driving the glow, drag and ledger; a route above the
  drive-hold limit is refused with the limit shown; dust grains flash on the
  wall (the labelled afterglow G-AG). `sunholo/celestial` 0.4.0 and
  `sunholo/relativity` 0.12.0 published. Next: the HUD rows (after the ship UI)
  and PR B, the Edenhofer 2024 dense clouds and the Local Leo Cold Cloud.

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
