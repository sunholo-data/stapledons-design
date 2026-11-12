# Dark lenses: hearing in gravity, then seeing further

## Status

- **Status:** Proposed 2026-11-12 (Mark, attended: "I guess we could use gravitational wave detection as well somehow?" and "we had an idea of using gravitational lensing to enhance our detection limits - so GR detection to find those, then use it to point in directions to get better data?"). Not approved. Every number below is a first estimate and is labelled; none is a check value yet.
- **Priority:** P2. It follows the evidence ledger and the finder (R3-REASONS), and gives the lens look (roadmap S4) and the Kerr black-hole work a place in the travel loop.
- **Roadmap:** [r3-signals-and-contact.md](../../roadmap/r3-signals-and-contact.md), track S (after S4).
- **Builds on:**
  - [signals-and-instruments.md](signals-and-instruments.md) §4.4 (the lens: focal distance, gain, one target per positioning) and §7 (the evidence ledger);
  - [destination-finder.md](destination-finder.md) (candidates, costs, the reason card);
  - [physics/observation.md](../../physics/observation.md) (the rule that an instrument's reach follows from a budget, and the registry these numbers must enter first).

---

## Game vision alignment

| Pillar | Alignment | Rationale |
|---|---|---|
| **Choices Are Final** | ✅ Supports | A lens look is one target, one positioning and a long stare. Going to a dead star to use it spends Earth's years for knowledge alone. |
| **The Game Doesn't Judge** | ✅ Supports | A dark lens is a tool, not a reward. Nothing says the detour was worth it. |
| **Time Has Emotional Weight** | ✅ Core | The player spends decades of home's time to buy a clearer picture of somewhere else, before spending more to go there. |
| **The Ship Is Home** | ✅ Supports | The crew argue about a detour that visits nobody. |
| **Grounded Strangeness** | ✅ Core | Gravitational waves, microlensing and the gravitational lens are all real astronomy. |
| **We Are Not Built For This** | ✅ Supports | The best telescope the ship will ever have is a dead star. |

---

## The idea

1. **Hear.** The ship listens in gravity as well as in light. Pairs of compact objects (white dwarfs, neutron stars, black holes) in close orbit radiate gravitational waves. A lone compact object does not; the ship finds those when one drifts in front of a background star and briefly brightens and shifts it (microlensing).
2. **Go.** A dark compact object becomes a mark on the map and a candidate destination: a reason to travel that has nothing to do with life there.
3. **See.** Parked on the far side of it from a target world, the ship uses the object as a gravitational lens. A dark lens has no starlight to block and can be more massive than any normal star nearby.
4. **Decide.** The lens image and its deep spectrum become records in the evidence ledger for worlds far beyond the direct telescope's range, and the finder's cards for those worlds sharpen.

## Why a dark object is a better lens

The feature doc's formula gives the nearest point of the focal line, F = R²c²/(4GM), and a gain that scales with the lens's mass.

| Lens | Radius, mass (assumed) | Focal line starts at | Starlight to block |
|---|---|---|---|
| Sun | 1 R☉, 1 M☉ | 548 AU | bright |
| White dwarf | 7,000 km, 0.6 M☉ | about 0.09 AU | very faint |
| Neutron star | 12 km, 1.4 M☉ | at the surface (the weak-field formula no longer holds this close) | almost none |
| Black hole | 10 M☉ | just outside it (the same caveat) | none |

- Estimates from the formula above; to be recomputed and pinned as OB rows.
- The gain row belongs in the registry too: about ten times the Sun's for a ten-solar-mass black hole, by the mass scaling.
- Strong-field lensing near a neutron star or black hole needs the exact geodesics the Kerr and Schwarzschild work already has in `sunholo/relativity`, not the weak-field deflection.

## How the ship hears

A detector's sensitivity depends on its size, and the ship is about 100 m across. Three routes, each to be checked against sources before any is adopted:

| Route | What it uses | What it could hear | Status |
|---|---|---|---|
| Pulsar timing | The radio dish the ship already has; pulsars are the detector's far ends | Very slow waves (supermassive pairs); also gives a position fix from pulsar signals | Unverified; pulsar navigation has flown as an experiment on the Space Station |
| Atom interferometer | A 100 m baseline aboard | The band between LIGO's and LISA's | Unverified; 100 m instruments are pathfinders, so this is likely a blueprint the ship earns |
| Ship and Earth as one detector | The Earth link | Low-frequency waves, while the ship is in Earth's beam | Unverified; a design idea, no source |

Sources to open and verify (none opened yet):
- the population of close white-dwarf pairs expected in the LISA band;
- the first isolated stellar-mass black hole found by microlensing (OGLE-2011-BLG-0462, reported 2022);
- the proposal that a very massive, hard-accelerating object would be detectable in gravitational waves (Sellers and colleagues, about 2022). In this game that is another bubble ship: quiet in radio, loud in gravity. It would be a new technosignature row.

## Caveats the design must keep

- **One line per look.** The ship must sit exactly on the line from the target through the lens. Each target is a separate positioning and months of integration (signals-and-instruments §4.4).
- **Pairs are messy lenses.** Gravity reveals pairs; the clean lenses are single objects, which gravity does not reveal. The two finders are complementary.
- **The trip was never the cost.** At the ship's speeds 548 AU is not far. A compact lens wins on darkness and mass, not on the shorter trip.
- **A lone white dwarf may already be in the catalogue.** Van Maanen's star, about 14 ly away, is a candidate first dark lens for an early voyage (to verify against the game's catalogue).

## What it would add to the game

- Map marks for dark objects, with their own evidence kind.
- A "signs of minds" record for heavy engineering heard in gravity.
- The lens look (S4) offered at dark lenses as well as stars, with the registry's gain and focal distance.
- Archive entries: hearing the universe; the telescope that is a dead star; how a black hole was first weighed alone.

## Acceptance criteria (for the implementation design that follows)

1. Every reach and gain is an OB row in `physics/observation.md` before any code, with its source opened.
2. A lone compact object never produces a gravitational-wave record; a pair never offers a clean single-lens image.
3. A lens record enters the same evidence ledger as every other observation, with its light age.
4. No dark object is marked on the map without a record above the noise.

## Open questions

- Which hearing route does the ship start with, if any, and which are earned?
- Are there dark objects within the play area in the real catalogues, or are they synthesised under galaxy-beyond-measurement's "augment, never replace" rule?
- Does another civilisation's heavy engineering exist in canon to be heard?
