# The galaxy beyond measurement: measured core, synthesised remainder, one source of truth

## Status

- **Status:** Proposed 2026-10-06 (Mark, attended: "I'm looking at basically what the game play area will be and when we need to start making up star positions"). Not approved.
- **Priority:** P1. It sets the play area and the boundary between real and generated data.
- **Supersedes:** the reach figures in [starmap-data-model.md](../phase1-data-models/starmap-data-model.md) ("Local Bubble (0-1000 ly): Real Gaia DR3 stars"); see Problem.
- **Companions:**
  - [life-and-intelligence-parameters.md](life-and-intelligence-parameters.md) (what lives in this galaxy);
  - the rebuild's implementation docs `starmap-single-truth.md` and `starmap-large-tier-and-reach.md` (stapledons-godot, the measured core).

---

## Game vision alignment

Checked against [core-pillars.md](../../vision/core-pillars.md):

| Pillar | Alignment | Rationale |
|---|---|---|
| **Choices Are Final** | ✅ Supports | A playthrough's galaxy is fixed at world generation (seed plus data version) and never changes under the player. |
| **The Game Doesn't Judge** | ✅ Supports | Every star states whether it is measured or synthesised; nothing generated is passed off as observed. |
| **Time Has Emotional Weight** | ✅ Core | The whole galaxy is reachable at γ 707, so the play area must be the galaxy, and real distances drive the time costs. |
| **The Ship Is Home** | N/A | No change. |
| **Grounded Strangeness** | ✅ Core | Real stars wherever they are measured; beyond that, stars drawn from a published galactic model, not invented by eye. |
| **We Are Not Built For This** | N/A | No change. |

---

## Problem

**1. The play area is the galaxy.** These are already decided:
- the cruise reaches 0.999999c (γ ≈ 707, D-15);
- the player has 100 subjective years ([game-vision.md](../../vision/game-vision.md));
- the galactic centre is a destination (D-13: Sgr A* at ~27,000 ly; [black-holes.md](../future/black-holes.md) calls supermassive holes "pilgrimage destinations").

A γ-707 leg covers 1,000 ly in about 1.4 subjective years, and the measured demo already reaches Aldebaran at 74 ly in 38 ship days. So every region of the Milky Way can be visited in one life aboard.

**2. Real data does not reach 1,000 ly.** `starmap-data-model.md` assumes Gaia gives a complete local bubble to 1,000 ly. What is measured, as of 2026-10-06:

| Region | What is measured | Completeness |
|---|---|---|
| ≤ 25 pc (81 ly) | CNS5: 5,931 objects | essentially complete, including brown dwarfs |
| ≤ 100 pc (326 ly) | GCNS: 331,312 stars, in the repo | complete for stars down to the faintest M dwarfs (Gaia G ≈ 20.7) |
| 100 pc – ~2 kpc | Gaia DR3 parallaxes for luminous stars | **incomplete**: Sun-like stars yes; most M dwarfs too faint; distances 5–50 % uncertain by 1 kpc |
| ≳ 2 kpc | Gaia sees giants and bright stars; distances mostly from models, not parallax | a sample, not a census |
| Galactic bulge and centre | crowded and dust-extincted; sparse, mostly giants | a sample |

The real catalogue is **complete only to about 326 ly**. Beyond that, the bright minority is measured and the faint majority is not.

**3. "Fill the rest" needs care.** The cosmological principle (homogeneity) holds for the universe on scales above ~100 Mpc, but not inside a galaxy. Star density falls by about e per ~1,000 ly above the disc plane and per ~8,000 ly in radius. It rises steeply into the bulge, and there are arms, clusters and dust. Resampling the local stars uniformly would give a Milky Way with no disc and no bulge, and the Sgr A* pilgrimage would cross a featureless fog.

---

## Design

### 1. Four data regimes, one rule

The guiding rule: **augment, never replace.** A measured star is always kept; synthesis only adds the stars a survey is known to have missed.

| Regime | Contents | Provenance label |
|---|---|---|
| **Measured core** (≤ 100 pc) | every GCNS/CNS5 star, with the truth-table position (`starmap-single-truth.md`) | "measured (Gaia DR3 · ±σ)" |
| **Measured sample + completion** (100 pc – ~2 kpc) | every Gaia DR3 star with ϖ/σ ≥ 5, plus synthetic stars only where the survey's selection function says stars are missing (mostly faint M dwarfs) | real stars "measured"; added stars "synthesised (completeness)" |
| **Model galaxy** (≳ 2 kpc, bulge, halo) | Gaia's luminous stars where available, plus population synthesis from a published Milky Way model | "synthesised (model, seed)" |
| **Named landmarks, everywhere** | real objects of any distance, always measured: Sgr A*, open and globular clusters, nebulae, known black holes, exoplanet hosts | "measured" |

### 2. Synthesis: real local stars, real galactic structure

Mark's instinct, that the local neighbourhood is representative, is used where it holds: **for what stars are like**, not where they are.

- **Where (positions):** from a published Milky Way density model: thin disc, thick disc, bulge/bar and stellar halo, with their scale lengths and heights. Candidates are the Besançon model (Robin et al. 2003, updated 2014–2022) or the parameters behind TRILEGAL or Galaxia (Sharma et al. 2011). The model is pinned with its citation and version. Spiral arms and known clusters are optional overlays.
- **What (properties):** resampled from the measured core. The GCNS ≤ 100 pc sample is the best unbiased census of stellar types, binaries and brown dwarfs we have. Each synthetic star copies the type mix of the measured core, reweighted by the target population's age and metallicity: thick disc and halo stars are older, so fewer hot stars survive.
- **Determinism:** each synthetic star comes from the world seed, a sky-cell hash and the model version, through SplitMix64 streams (as `reference/rng-determinism.md` intends). The same seed and data version always give the same galaxy.
- **Lazy, by cell:** the galaxy is generated in cells on demand (an octree of galactocentric cells). A 10¹¹-star galaxy is never stored. What is stored is what the player has seen or visited.

### 3. The source of truth: a star registry with provenance

One registry holds every star the game has ever shown.

- **Measured rows** (the truth table): the identity, position, source and σ.
- **Synthetic rows:** the cell id, seed, model version and generated properties. They are created on demand and cached deterministically.
- **Each row carries `provenance` and `data_version`.**

**When better data arrives** (Gaia DR4 is expected December 2026; DR5 later; new exoplanets at any time), it enters as a **new data version**:

- New measured stars join the measured regimes.
- The completion step re-runs, so synthetic completions are removed where real stars now fill the gap. The count of synthetic stars in a cell falls as the real count rises.
- Named landmarks and exoplanet systems update in place (the D-41 living-data rule).

**A playthrough pins its data version at world generation** (Choices Are Final). A new version affects new playthroughs only. The galaxy never changes under a player mid-voyage.

### 4. What the player sees

- Star inspection shows the provenance label from the registry, never as written text.
- The galaxy map can tint by provenance (a measured / synthesised overlay), so players can see the edge of human knowledge. That's a strong "Grounded Strangeness" moment: on leaving the 326 ly sphere, the map tells you you're now among stars no one has catalogued.

---

## Acceptance criteria (for the implementation design that follows)

1. A deterministic cell generator: the same seed and data version give byte-identical cells on the AILANG VM and interpreter.
2. The synthetic star counts per cell reproduce the pinned model's density, within Poisson error, over 10⁴ test cells.
3. Within the measured core, synthesis adds **zero** stars. In the completion regime it adds stars only below the survey's completeness limit.
4. Type mix: synthetic stars from thin-disc cells near the Sun match the measured core's type histogram within sampling error.
5. A data-version bump that adds real stars removes the matching synthetic completions in those cells, and the regression shows no other cell changing.
6. Rendered evidence: the galaxy map at 100 ly, 1,000 ly, 10,000 ly and galaxy scale, plus the view from Sgr A*, with the provenance overlay.

---

## Open questions for Mark

1. **Which Milky Way model?** Default: Besançon (best documented, regularly updated), pinned by citation and version.
2. **Show the provenance overlay on the map by default?** Default: yes. "The edge of the known" is part of the experience.
3. **How to bridge the 326 ly → ~2 kpc band?** Default: the measured sample plus completion (no wholesale synthesis), which needs the Gaia DR3 selection function (Cantat-Gaudin et al. 2023) in the pipeline.
4. **Real exoplanet systems beyond TRAPPIST-1:** pull all confirmed systems within the measured core from the NASA archive (about 1,000 hosts within 100 pc), each with labelled assumed appearance?

## Reconciles

- `starmap-data-model.md` "Local Bubble (0-1000 ly) real" → measured core to 100 pc; sample plus completion to ~2 kpc.
- `starmap-data-model.md` "Smooth blending over 100 ly transition" → no blending: augment-only completion, using the survey selection function.
- The circular dependency between starmap and world-gen: the galaxy depends on (seed, data version); life parameters are layered on top (companion doc).
