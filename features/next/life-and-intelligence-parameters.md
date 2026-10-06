# Life and intelligence parameters: a realistic baseline, an honest tuning dial

## Status

- **Status:** Proposed 2026-10-06 (Mark, attended: "we are going to weight anthropic values such as density of life and intelligence etc. so will need to see how much we need to vary those from expected 'normal' Drake values to have an interesting game experience"). Not approved.
- **Priority:** P1. It sets how often the player meets life, and why.
- **Builds on:**
  - [world-gen-settings.md](../future/world-gen-settings.md) (presets and sliders);
  - [design-decisions.md](../../vision/design-decisions.md) ("Anthropic Luck Factor", "Three Distance Regimes");
  - [galaxy-beyond-measurement.md](galaxy-beyond-measurement.md) (the stars these parameters act on).

---

## Game vision alignment

Checked against [core-pillars.md](../../vision/core-pillars.md):

| Pillar | Alignment | Rationale |
|---|---|---|
| **Choices Are Final** | ✅ Supports | Parameters are fixed at world generation (existing decision); civilisations rise and fall on their own clock whatever the player does. |
| **The Game Doesn't Judge** | ✅ Supports | The game states how far this universe is tuned from the realistic baseline, as a number, instead of hiding the dial. |
| **Time Has Emotional Weight** | ✅ Core | Most encounters come *through* time dilation: you live long enough, in galactic time, to see civilisations appear and vanish. |
| **The Ship Is Home** | N/A | — |
| **Grounded Strangeness** | ✅ Core | Pillar 5: "Drake equation and real science inform the number and distribution". The baseline is cited distributions, not guesses. |
| **We Are Not Built For This** | ✅ Supports | Life is common, minds are rare, and contact usually arrives late, as ruins, signals or light from the dead. |

---

## Problem

The world-gen defaults (`f_life 0.3`, `f_complex 0.1`, `f_tech 0.05`, `civ_lifetime_mean 10,000 yr`, `anthropic_luck 0.7`) and the "8–15 civs within 1,000 ly" target have never been computed against each other. Four documents give different civilisation counts:
- "5–15 within 500–1,000 ly";
- "8–15 in 1,000 ly";
- "~12";
- "maybe 1 within 500 ly".

No document states a realistic reference value. So the game can't yet say how far it departs from reality, which Pillar 5 and "The Game Doesn't Judge" require.

## What the numbers imply (computed 2026-10-06)

The inputs:
- the measured local stellar density, 0.079 stars/pc³ (GCNS: 331,312 within 100 pc);
- a thin disc with scale height 300 pc;
- 20 % FGK stars;
- η⊕ = 0.4 for FGK (Bryson et al. 2021: 0.37–0.60, conservative habitable zone);
- technological civilisations arising uniformly over a 5 Gyr window.

| Within | Stars | Biospheres (f_life 0.3) | Tech civs ever | Alive at any instant | Exist at some point in a game window of 10³ / 10⁵ / 10⁶ yr |
|---|---|---|---|---|---|
| 1,000 ly | 6.7 × 10⁶ | 1.6 × 10⁵ | 805 | **0.0016** | 0.002 / 0.018 / 0.16 |
| 3,000 ly | 1.0 × 10⁸ | 2.5 × 10⁶ | 1.3 × 10⁴ | 0.025 | 0.03 / 0.28 / 2.5 |
| 10,000 ly | 1.4 × 10⁹ | 3.3 × 10⁷ | 1.7 × 10⁵ | 0.33 | 0.36 / 3.6 / **33** |

For comparison, under the same star counts:
- **Optimistic-expert** values (f_l 1, f_i 0.1, f_c 0.1, L 10⁶ yr) give about 1 civilisation alive within 1,000 ly at any instant, and about 220 within 10,000 ly.
- **Pessimistic** values (f_l 10⁻³, f_i 10⁻³, L 10³ yr) give 10⁻⁸ within 1,000 ly; the player is effectively alone.

The realistic baseline is therefore **not a number but a range spanning more than ten orders of magnitude.** Sandberg, Drexler and Ord (2018) put log-uniform uncertainty on each Drake factor and find a substantial probability that we are alone in the galaxy.

Three conclusions:
1. **Life can be common at realistic values; minds cannot be, nearby.** The defaults give 160,000 biospheres within 1,000 ly, so "many biospheres, maybe 1 civ" (the existing distance-regimes decision) is already realistic.
2. **"8–15 civs alive within 1,000 ly" needs a boost of about 10⁴** over the world-gen defaults. That's what `anthropic_luck` is silently doing.
3. **Time dilation is a better lever than luck.** The player's galactic-time window runs from 10³ years on a short voyage to 10⁶ (the Year 1,000,000 fast-forward). Over 10⁶ years and 10,000 ly, the *unboosted* defaults already give ~33 civilisations that exist at some point. Range and time make rarity playable, and that *is* the game's premise.

### Inside the doom window

The mission bounds both range and time. The useful volume is about 500–3,000 ly, and the window about 10⁴ Earth years (galaxy doc, problem 4). Within it, the **unboosted defaults** give:

| Within | Biospheres | Civilisations existing at some point in a 10,000 / 24,000-year window |
|---|---|---|
| 500 ly | 2.4 × 10⁴ | 0.0005 / 0.0008 |
| 1,000 ly | 1.6 × 10⁵ | 0.003 / 0.005 |
| 3,000 ly | 2.5 × 10⁶ | 0.05 / 0.09 |
| 5,000 ly | 7.8 × 10⁶ | 0.16 / 0.27 |

So the time-dilation lever only works for players who leave Earth behind. For the mission itself, **a few civilisations within reach before impact need about 10²–10³ over the world-gen defaults**, and far more over the realistic median.

**The fiction already explains the dial** (Mark, 2026-10-06; retired `game_loop_origin.md`: each failed voyager seeds a variation with "slightly different anthropic luck"). The runs are a selection across multiverse variations, weighted towards universes where the mission is possible. That's an observer-selection effect with a story: this universe is unusually rich in life and minds near Earth *because* it's one the loop chose. The ship's AI knows this is a re-run. So the luck number becomes a **diegetic reveal** rather than a settings-screen fact.

---

## Design

### 1. Separate the baseline from the dial

- **Realistic baseline:** a pinned table of cited distributions for each factor, with log-uniform or literature priors:
  - R★: Licquia & Newman 2015, about 1.65 M☉/yr;
  - f_p ≈ 1 (Kepler);
  - η⊕: Bryson 2021 for FGK, and Dressing & Charbonneau 2015 for M dwarfs, flagged as "habitability debated";
  - f_l, f_i, f_c: log-uniform per Sandberg et al. 2018;
  - L: log-uniform from 10² to 10⁸ yr.

  It's living data: new results (JWST biosignature limits, η⊕ updates) re-pin it.
- **The dial:** a world's parameters are written as **offsets from the baseline median, in orders of magnitude**, per factor. For example, "f_i +2.0 (100× the baseline median)". Presets become named offset sets. The existing "anthropic luck" becomes the sum of offsets, shown to the player as one number: *"This universe is 10^3.5 times kinder to minds than the median expert estimate."*

### 2. Count encounters by kind, not just living civilisations

An encounter in this game rarely means "alive and talking":

| Kind | Mechanism | Frequency at near-baseline values |
|---|---|---|
| **Biosphere** | a habitable world with life (spectra, then a visit) | common |
| **Remnant** | a civilisation that ended before you arrived: ruins, artefacts, a dead signal | common over a 10⁶-year window |
| **Signal / light** | detected by its old light; may be gone by arrival (`last_light_year`) | moderate |
| **Contact** | alive when you arrive | rare: the climax, not the routine |

Time dilation feeds all four. A civilisation 3,000 ly away seen in its youth may be ruins by the time you get there, or may still be waiting.

### 3. Calibrate by simulation, not by hand

An AILANG Monte Carlo ("life calibration"), seeded and deterministic:
- **Input:** a galaxy from `galaxy-beyond-measurement`, a parameter set and a set of player strategies:
  - a cautious local explorer (≤ 1,000 ly, few γ-707 legs);
  - a mid-range wanderer;
  - a Sgr A* pilgrim;
  - a "wait out the galaxy" player.
- **Civilisations:** emerge and end on their own clocks (rates from the parameters).
- **Output:** per-playthrough distributions of biospheres seen, remnants found, signals and contacts, plus the galactic-time window actually lived.
- **Presets** are chosen to hit design targets (open question 1) across strategies, and the doc records the resulting offsets from the baseline. Lonely and Teeming stay as honest extremes.

### 4. The player sees the dial

At world generation, the presets show:
- the expected encounters, by kind, for a typical voyage (from the calibration);
- the departure from the baseline, in orders of magnitude.

In play, the Archive can explain the factors as lore with real citations.

---

## Acceptance criteria (for the implementation design that follows)

1. The baseline table is pinned, with every factor cited and checked by the lore/canon checks.
2. The calibration is deterministic: VM and interpreter are byte-identical, and the same seed gives the same distributions.
3. At zero offset, the calibration reproduces the table above (within Monte Carlo error) for the analytic cases.
4. Each preset hits its encounter targets for every reference strategy, with its offsets recorded.
5. The world-gen screen shows the offset number and the expected encounters, and they are derived from the calibration output, never written by hand.

---

## Open questions for Mark

1. **Encounter targets** for the default preset, per typical 100-year voyage, **counted inside the doom window and range**, since those are the encounters that can help Earth. A starting proposal: 20–50 biospheres detected, 3–8 visited, 2–5 remnants, 1–3 signals, **0–2 living contacts**, with contact more likely the further and longer you go.
2. **Show the luck number to the player, and when?** It's a spoiler for the multiverse loop. Proposed default: hidden at world generation (presets are named only); the ship's AI reveals the number as part of its secret ("this universe is 10^3 kinder to minds than chance would give; that is not an accident"); fully visible on New Game+.
3. **M-dwarf habitability:** count M-dwarf habitable-zone planets (about 75 % of stars, so up to 4× more biospheres) or flag them as debated and off by default? Default: on, with the planet labelled "habitability debated".
4. **Reconcile the four existing civilisation counts** to whatever targets are chosen in question 1, and retire `gamma_max` (superseded by D-15).
