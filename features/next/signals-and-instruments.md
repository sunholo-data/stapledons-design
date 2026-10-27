# Signals and instruments: what reaches the ship, and how it is measured

## Status

- **Status:** Proposed 2026-10-27 (Mark, attended: "we need some kind of session on how we get signals from earth and other suns/planets to the ship. what instrumentation we use"; "we will have pure physics for other things, so I expect something like we detect what looks like organic life or industry spectrum and other detections that make us confident to go to that planet spending our precious time resources"). Not approved.
- **Priority:** P1. Every reason to go somewhere starts here.
- **Roadmap:** [r3-signals-and-contact.md](../../roadmap/r3-signals-and-contact.md), track S.
- **Builds on:**
  - design-decisions 2025-12-02 "Detection Model: Epistemic Gap" (old light, three tiers, `last_light_year`) and "No Survey Budget: Auto-Discovery";
  - D-11 (only light crosses the bubble wall);
  - [planet-state-transitions.md](../future/planet-state-transitions.md) (`PlanetBelief`, detection vs reality);
  - [life-and-intelligence-parameters.md](life-and-intelligence-parameters.md) (what is out there to detect);
  - [bubble-resources.md](bubble-resources.md) (`signal`, `compute`, `power`);
  - the relativity spec §2 (Doppler and aberration, which also act on every observation).

---

## Game vision alignment

| Pillar | Alignment | Rationale |
|---|---|---|
| **Choices Are Final** | ✅ Core | You commit decades of Earth time on evidence that is old, noisy and sometimes a false positive. |
| **The Game Doesn't Judge** | ✅ Supports | The ledger shows measurements and their limits, not verdicts. "O₂ and CH₄ out of equilibrium, SNR 7, light 212 years old" is a fact; what it means is the player's call. |
| **Time Has Emotional Weight** | ✅ Core | Every observation is a picture of the past. Earth itself goes silent behind you: at your destination you hear Earth as it was the week you left (§3). |
| **The Ship Is Home** | ✅ Supports | The crew read the instruments, argue about the spectra, and decode the signals (crew work). |
| **Grounded Strangeness** | ✅ Core | Reach comes from photon budgets, apertures and real biosignature and technosignature science, including the false positives. Nothing is detected because the plot needs it. |
| **We Are Not Built For This** | ✅ Supports | The ship can never touch anything. Everything it will ever know about another world arrives as a few photons, years late. |

**Needs discussion:** deliberate observation (§6) against "No Survey Budget". The proposal keeps the survey automatic and adds a few large, deliberate acts, never a scan budget.

---

## Problem

The canon says the player sees only old light, in three tiers, with stale data labelled. It does not say:
- what the ship's instruments are, and how far each one can see a given thing;
- how a detection becomes confidence, including false positives;
- how Earth's signals reach a ship that moves at γ up to 707, and what that does to news from home;
- what the player can *do* to learn more about a candidate without reintroducing scan micromanagement.

Without these, "reasons to go" are authored flags, and the commit decision loses its weight.

---

## Design

### 1. The rule: a passive observatory with a voice

Only light crosses the wall (D-11). So:
- **No probes, no landers, no samples.** The ship cannot send anything material out or bring anything in. Every fact about another world is remote sensing.
- **The ship listens and looks** across the spectrum, and **transmits** (§3, and [contact-console.md](contact-console.md)).
- The ship *can* move its observatory anywhere quickly. That is its one great advantage over any planet-bound astronomer (§4.4).

### 2. The light cone: what the ship can know, and when

Everything the ship knows about a source at distance *d* is the source's state at galaxy time *t − d/c*, where *t* is the galaxy time of the observation. One pure function owns this:

```
seen(source, ship_position, galaxy_time) -> state of the source at (galaxy_time - distance)
```

- Every readout carries `last_light_year` (existing decision), and the UI says how old it is.
- **Arrival reveals.** At arrival, light age falls to light-hours, and the gap between the belief and the world becomes visible (the [destination finder](destination-finder.md)'s hindsight card).
- The galaxy clock always runs (D-71), so the true state keeps moving while the light is in flight. The ship only ever sees the past.

### 3. Earth's signal

Earth is a source like any other, and the physics is stark (computed 2026-10-27):

| Leg | Arrive (Earth years after leaving) | The Earth light reaching you then left Earth at |
|---|---|---|
| α Cen, 4.37 ly at 0.99c | 4.41 | 16 days after you left |
| 40 ly at 0.999c | 40.04 | 15 days after you left |
| 1,000 ly at γ 707 | 1,000.001 | 9 hours after you left |

- **You outrun the news.** At any destination, the Earth you hear is the Earth of your departure. Waiting there catches up at one Earth year per year: to learn what happened in Earth's last 1,000 years, you would wait 1,000 years.
- **At cruise, the link fails.** Receding at γ 707, Earth's signal arrives with Doppler factor D = 1/(γ(1+β)) ≈ 7 × 10⁻⁴. A 10 GHz carrier arrives at 7 MHz, and an hour of broadcast takes two months to arrive. Approaching at γ 707, D ≈ 1,414: the same carrier arrives at 14 THz.
- **The homecoming torrent.** Flying home at γ 707 from 1,000 ly, the ship receives everything Earth broadcast since it left (about 2,000 years for the round trip) in about 1.4 years of ship time, blueshifted 1,414×. The comms console plays home's history at high speed, with what the instruments can recover from it. This is the one way to catch up with Earth, and it shows the cost of every year away.
- **The Earth rate, live.** At every moment, Earth's history plays aboard at D Earth-seconds per ship-second, the same Doppler factor that colours the sky (Mark, 2026-10-27: "it will be crazy to receive updates from earth at different rates according to distance and speed traveling towards or away"). The comms console shows it as a live readout beside the date of the newest Earth light:

  | Situation | D | What the crew experience |
  |---|---|---|
  | Cruising away at γ 707 | 7 × 10⁻⁴ | Earth nearly frozen: a day of Earth news takes about 4 years to arrive |
  | Cruising away at γ 7 (0.99c) | 0.071 | A trickle: an Earth year of news takes 14 ship-years to arrive |
  | Stopped anywhere | 1 | Real time, but as old as the distance: the news is d years stale |
  | Heading home at γ 7 | 14.1 | Fast-forward: two weeks of Earth per ship-day |
  | Heading home at γ 707 | 1,414 | The torrent: four Earth years per ship-day |

  Reception at the extreme rates depends on the receivers coping with the shifted carrier (§5), so the console shows both the rate and what fraction of it the instruments can recover.
- **Earth hears you late too.** A message from a destination *d* light-years out reaches Earth *d* years later. Earth only knows your itinerary if you sent it.
- **Letters at the next port.** Earth (or a world you gave a map to) can beam messages ahead to a port on your itinerary. They wait there in the beam until you arrive, provided they were sent early enough and you stop long enough to receive them. Worlds can act as relays once they know the route.

The game's shipped "news from home" (M4.4) already uses the light-delayed epoch for α Cen (t_e = 0.044 yr). This section generalises it to every source and every leg.

### 4. The instrument suite

Each instrument is defined by physics parameters (aperture, band, resolving power, detector noise, integration time). Its reach for a given target follows from a photon budget, not from an authored range. The suite grows when worlds give the ship better designs (efficiency blueprints, [inner-shapes-outer §3](inner-shapes-outer.md)), and costs `mass` and `fabrication` to build.

#### 4.1 Tier 1: the census (always on)

- **What:** wide-field photometry and astrometry. It finds planets by transit, by the star's wobble (radial velocity and astrometry) and by direct imaging of young giants.
- **Gives:** planet radius or minimum mass, orbit, equilibrium temperature, whether the planet is in its star's habitable zone.
- **Real worlds:** measured exoplanets come from the pinned NASA Exoplanet Archive snapshot (living data, as TRAPPIST-1 already does), labelled "measured". Synthesised planets around synthesised or under-surveyed stars are labelled "synthesised" (galaxy-beyond-measurement's rule: augment, never replace).

#### 4.2 Tier 2: spectroscopy (life)

- **What:** transit spectroscopy and direct-imaging spectroscopy behind a coronagraph. It reads atmospheres and surfaces.
- **Biosignatures,** with their standard false positives:

| Signal | Why it suggests life | False positives the confidence must model |
|---|---|---|
| O₂ or O₃ together with CH₄ (chemical disequilibrium) | Life keeps both in an atmosphere that would destroy one within decades | Abiotic O₂ from water photolysis on desiccated worlds around active M dwarfs; CO₂ photolysis |
| N₂O, CH₃Cl, DMS | Mostly biological on Earth | Weak alone; small abiotic sources |
| A surface reflectance edge (Earth's vegetation red edge near 0.7 µm) | Photosynthetic pigment | Minerals with edges; the edge moves with the host star (plants around M dwarfs may absorb into the near-infrared, Kiang et al. 2007) |
| Seasonal change in gases or colour | A biosphere breathing with the seasons | Seasonal abiotic chemistry |
| Ocean glint at crescent phase | Liquid water (habitability, not life) | Glint from other liquids, or glint mimicked by clouds |

- **Reach scales with aperture:** spectroscopy of an Earth twin is a tens-of-light-years task for a 6–15 m class space telescope (the HWO and LUVOIR studies). It becomes a hundreds-of-light-years task only with apertures of order 100 m, or with the lens (§4.4).

#### 4.3 Tier 3: technosignatures (minds)

| Signal | Physics | Reach (order of magnitude; pinned in the implementation) |
|---|---|---|
| **Intentional beacons** (narrowband radio, optical laser pulses) | Directed power; reach ∝ √(EIRP × receiving area) | Across the galaxy if they are aimed at you |
| **Leakage** (radio and radar) | Undirected; Earth's TV leakage is weak, planetary radar is strong | Light-years for TV-like leakage; up to hundreds of light-years for radar-like leakage with a large receiver |
| **Industrial gases** (CFCs, NF₃, NO₂) | No known abiotic source for CFCs | Tens of light-years for a large telescope (Kopparapu et al. 2021 for NO₂; Lin, Gonzalez Abad & Loeb 2014 for CFCs) |
| **Artificial light on the night side** | City illumination | Very close only, or with the lens |
| **Waste heat** (mid-infrared excess) | Energy use must be radiated (Wright et al. 2014) | A Dyson swarm shows across the galaxy |
| **Artificial transits** (megastructures, swarms) | Non-planetary light-curve shapes | Wherever transits are seen |

So technological minds are seen mostly when they are loud (beacons), large (waste heat), or near (gases and lights). Most civilisations near their beginnings are invisible. The anthropic dial sets how many minds exist; it never makes a quiet one loud.

#### 4.4 Tier 4: the lens (a deliberate look)

Every star is a gravitational lens. A telescope on the far side of a star, on the line from a target through the star, sees the target magnified by a factor of ~10¹¹ (the solar gravitational lens; Turyshev & Toth 2020). The minimum focal distance is F = R²c²/(4GM):

| Lens star | F | Light-time from the star |
|---|---|---|
| Sun | 548 AU | 3.2 days |
| K dwarf (0.7 R☉, 0.75 M☉) | 358 AU | 2.1 days |
| M dwarf (0.2 R☉, 0.2 M☉) | 110 AU | 0.6 days |
| Proxima Centauri | 107 AU | 0.6 days |
| Sirius A | 777 AU | 4.5 days |

- **What it gives:** a reconstructed image of another world's surface (continents, oceans, cloud bands, night-side lights), with resolution down to tens of kilometres at about 100 ly after long integration. It also gives deep spectra of that one world.
- **What it costs:** a short in-system trip to the focal line (the M5.5 in-system navigation already flies this), then **months of integration**, while the galaxy clock runs. One target per positioning.
- **Why it matters for play:** this is the Bubble ship's own instrument. No planet-bound civilisation can easily reach a focal line, so it is the ship's edge. It turns "a faint O₂ line at 300 ly" into "a picture of a living world", before you spend centuries going there. Red dwarfs make short lenses, and they are the most common stars.

### 5. Observing at speed

Aberration and Doppler act on every observation (spec §2, D = 1/(γ(1 − β cos θ′))):
- Spectral lines shift by D, and the sky crowds forward. Fine spectroscopy and the lens are **rest-frame activities**: the ship looks properly when it stops.
- At cruise, the census keeps running coarsely (bright sources, forward-shifted bands). The forward sky at high γ is blueshifted far beyond the instruments' design bands, so it is labelled as such rather than read.

### 6. Automatic survey, few deliberate acts

The existing decision stands: **no survey budget and no telescope-time management.** Whatever the passive suite finds lands in the evidence ledger on its own, at every stop and in coarse form at cruise.

What the player chooses is rare and large:
- **a lens look:** a trip and months of integration on one target;
- **a deep stare at a stop:** stay longer to integrate, which is crew work (`compute`) and time (the galaxy clock);
- **listening:** point the receivers at a candidate's beacon frequency while stopped (`signal` hours).

Each is a commitment of time, not a scan-points economy, so it serves Pillar 1 rather than adding friction.

### 7. The evidence ledger

Every observation becomes a record:

```
Observation = { world, instrument, quantity, value, snr, last_light_year, false_positive_notes }
```

A pure function turns a world's observations into a belief over planet states (`PlanetBelief` in planet-state-transitions):
- **Inputs:** priors from the anthropic dial ([life-and-intelligence-parameters](life-and-intelligence-parameters.md)), the observations, and false-positive likelihoods per signal and host star.
- **Output:** the probability of each state (sterile, microbial, complex, pre-tech mind, technological, remnant, ecocide) **at last light**, plus the evidence that moved it most.
- The finder propagates it forward to arrival.

Everything is seeded and pure. The same seed and the same observations give byte-identical beliefs.

### 8. Where the maths lives

Following the project rule (physics maths lives in packages first), the photon budgets, signal-to-noise, link budgets, lens focal distance and gain, and the false-positive likelihood tables go into a package, proposed as **`sunholo/observe`**. Doppler and aberration stay in `sunholo/relativity`. Spec check values for each go into the relativity spec's test section before any visual uses them.

---

## Acceptance criteria (for the implementation design that follows)

1. `seen()` returns the state at t − d/c for every source. On the shipped α Cen plan (`planBurnCoastBurn`) the news epoch matches M4.4's pinned t_e = 0.043984 yr, and the §3 coast-limit table reproduces to three significant figures.
2. The Doppler factors at γ 7.09, 22.4 and 707, receding and approaching, match the package to 1e-9 relative.
3. The focal distances in the §4.4 table reproduce from F = R²c²/(4GM) to 0.1 %.
4. Every instrument's reach for its reference target comes from the photon budget. A test fails if any reach is a literal.
5. Every false positive in the §4.2 table is modelled: a desiccated M-dwarf world with abiotic O₂ and no CH₄ never reaches more than the configured confidence for life.
6. The evidence ledger is deterministic: VM and interpreter are byte-identical, and the same seed gives the same beliefs.

## Open questions for Mark

1. **Deliberate acts vs "No Survey Budget".** Is §6 (lens looks, deep stares, listening, all paid in time) acceptable alongside the automatic survey? Recommended: yes. They are commitments, not a budget.
2. **The ship's apertures.** What can the ship carry? A 100 m-class sparse aperture across the bubble would make life detection a hundreds-of-light-years task without the lens. A 10 m class keeps the lens essential. Recommended: start at 10 m class, with larger apertures as blueprints from worlds (a reward from contact).
3. **Listening at cruise.** Allow coarse reception at low γ, or only at rest? Recommended: low-γ reception allowed, with Doppler shown honestly; nothing at γ ≳ 20.
4. **The homecoming torrent** (§3). Play Earth's compressed history on the way home as a feature of the comms console? Recommended: yes. It is the strongest Pillar 3 moment the physics offers for free.
