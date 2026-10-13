# Bubble Resources: one closed ledger

## Status
- **Status:** Accepted in part: option B, m_eff = 1 kg, wall shape as a discovery (ledger D-69, Mark, attended 2026-10-13). The resource list and check values remain proposed.
- **Priority:** P1 (defines what every crew scenario, crisis and upgrade spends)
- **Supersedes in part:** [mass-budget.md](../future/mass-budget.md) (mass sinks and visibility stay; the resource list and the energy source are new)
- **Physics:** [physics/higgs-bubble.md](../../physics/higgs-bubble.md) §4, §5 and §8 (the "later energy ledger" this document proposes)
- **Companion:** [inner-shapes-outer.md](inner-shapes-outer.md) (how contact with civilisations changes these numbers)

> "Compute time is good but the others are abstract; we need more universal.
> The finite resources within the bubble... water, energy... anything that
> increases efficiency of the transfer between them is a critical thing for
> the bubble. Keep it real with the physics." (Mark, 2026-10-13)

## Game Vision Alignment

Scored by hand against [core-pillars.md](../../vision/core-pillars.md). The
`game-vision-designer` skill was not available in this session; score again
with it before a sprint.

| Pillar | Alignment | Rationale |
|--------|-----------|-----------|
| **Choices Are Final** | ✅ Core | Mass only leaves; every kilogram the drive turns to light is gone for good |
| **The Game Doesn't Judge** | ✅ Supports | Speed, children and machines are priced in one unit; no allocation is "right" |
| **Time Has Emotional Weight** | ✅ Supports | Going faster saves Earth years and costs atoms; the trade is explicit |
| **The Ship Is Home** | ✅ Core | Home is a closed loop of water, light and soil that the crew keep alive |
| **Grounded Strangeness** | ✅ Core | Every figure below is ordinary physics except the one named hand-wave |
| **We Are Not Built For This** | ✅ Supports | A century in a sealed sphere: every loss ends up as heat or sludge, never "away" |

## Prior Design Decisions

Checked against [design-decisions.md](../../vision/design-decisions.md):

- **Higgs Bubble Model (D-11, 2026-10-01).** No massive particle crosses; only light (and neutrinos) leaves. This document follows that rule through.
- **Finite Mass Budget (2025-12-06).** It stays. Mass becomes the single currency everything is priced in.
- **Internal Tension Model (2025-12-06).** No constant food or air meters. Tension comes from crises and decisions, so the resources here surface as capacities that fail, not bars that drain.
- **Player Location Freedom (2025-12-06).** No micromanagement of food or sleep.
- **Engineering Deck: Background Access (2025-12-06).** Engineering is where capacities live; it matters during crises.
- **Proto-Tech via Information (2025-12-06).** Blueprints cross the wall; building them costs mass. Here, a blueprint usually raises an efficiency.
- **Player Role: Perturbation Operator (2025-12-02).** The ship trades only information, so what the galaxy can give it is better efficiencies and new ideas, never more atoms.

## 1. The one rule

The bubble is a closed system. Mass is conserved inside it. The only thing
that leaves is light:

- the drive's beam (`higgs-bubble.md` §4);
- the boundary glow (§6);
- **waste heat**, radiated as infrared through a wall that is transparent to
  light of every energy.

Light carries energy, and energy is mass (E = mc²). **Every joule that leaves
the bubble takes 1/c² kg of the ship with it.** Nothing else ever leaves: not
ash, not brine, not spent fuel, not the dead. Everything else is a
transformation inside.

So every resource aboard is one of three kinds of quantity:

| Kind | What it is | Physics | Example |
|---|---|---|---|
| **Stock** | An amount of matter in a form | Conserved; moved between forms, never created | Water, free mass, people |
| **Capacity** | A rate that machines and geometry allow | Set by hardware and by the wall's area | Power, heat rejection, recycling throughput |
| **Efficiency** | The fraction of a transfer that lands where you want | Thermodynamics; the rest becomes heat or a sludge pool | Light to food, brine to water |

Stocks are what the ship *has*. Capacities are what it can *do* per day.
Efficiencies decide how much of each transfer is wasted. A crisis is almost
always a capacity failing, which turns a stable stock into a falling one.

## 2. The eight resources

These are universal. Every crew scenario, crisis and upgrade uses only these,
in these units. A scenario never invents a resource such as "paper stock";
paper is free mass that has been fabricated.

| Id | Resource | Kind | Unit | What draws on it | What happens when short |
|---|---|---|---|---|---|
| `mass` | Free mass | Stock | kg | Births, fabrication, repairs, **the drive** | Something must be dismantled (70% recovery, `mass-budget.md`) |
| `water` | Water in the loop | Stock (with a sludge pool) | t | Drinking, gardens, hygiene, cooling | Rationing; gardens wilt first |
| `power` | Generation headroom | Capacity | MW | Grow lights, recycling, fabrication, the Archive | Work queues; something is switched off |
| `heat` | Heat-rejection headroom | Capacity | MW | Everything that uses power, plus the forward sky at high γ | The ship warms; nothing may run harder |
| `garden` | Lit growing area | Capacity | m² at full light | Food and oxygen | Diet narrows; CO₂ creeps up |
| `fabrication` | Fabricator time | Capacity | bay-hours | Turning free mass into parts, medicine, tools | Repairs wait |
| `compute` | Archive compute | Capacity | slots | Decoding, simulation, the Archive's own memory | The Archive answers slower, or not at all |
| `signal` | Antenna time | Capacity | hours | Sending to and receiving from civilisations and home | Messages wait for a window |

Crew time is not on the list; crew time is fatigue and stress, which the crew
host already models.

### How the eight connect

```
                 (the generator: mass → drive light, §4)
  mass ────────────────────────────────────────────────▶ light out (gone)
   │  fabrication (bay-hours, yield < 1 → scrap pool)
   ├────────────▶ parts, medicine, tools ──wear──▶ scrap pool ──fabrication──▶ mass
   │
  fuel (deuterium) ──fusion──▶ power ──┬──▶ grow lights ──garden──▶ food, O₂
                                      ├──▶ recycling ──▶ water (brine pool)
                                      ├──▶ fabrication
                                      └──▶ compute
                every watt above ──▶ heat ──radiators (heat)──▶ infrared out (gone, E/c²)
```

Every arrow has an efficiency below 1. The loss goes one of two ways:
- **as heat**, which must leave through the wall and is limited by `heat`;
- **into a pool inside**: brine, ash, scrap, spent fuel. A pool can be
  recovered later, at a cost in power, fabrication and time. It can never be
  thrown away.

That is why **efficiency is the critical upgrade.** Raising one efficiency
shrinks both the heat load and the pools. It is also exactly what crosses the
wall as information (§6).

## 3. Check values (proposed)

These follow the HB-n convention: one number per row, so a parser can register
them. They are **proposed**. On acceptance they move into `higgs-bubble.md` §8,
and the formulas go into `sunholo/relativity` first. They were computed with an
independent oracle and agree with HB-49 (23.26 N) and HB-53 (1.370 × 10¹⁷ J).
R = 100 m and n = 0.1 cm⁻³ (uniform) unless stated.

### The wall and heat

The wall is transparent to infrared, so heat leaves as radiation. A radiator
inside can emit no more through the wall than a black body filling the wall's
area at the radiator's temperature: P ≤ σT⁴ · 4πR².

| ID | Quantity | Value | Unit |
|---|---|---|---|
| RES-1 | Wall area 4πR² | 1.257 × 10⁵ | m² |
| RES-2 | Interior volume | 4.189 × 10⁶ | m³ |
| RES-3 | Heat-rejection ceiling, whole wall at 300 K | 57.7 | MW |
| RES-4 | Heat-rejection ceiling, whole wall at 600 K | 924 | MW |
| RES-5 | Heat-rejection ceiling, whole wall at 1,000 K | 7.13 | GW |

Real radiators cover only part of the view out, so the game's `heat` capacity
starts well below RES-3. Room-temperature living is cheap in heat. Anything
that runs hot (fusion, fabrication) needs hot radiators.

### Household

Assumptions: 100 people, as in `mass-budget.md`. Food energy is the NASA
Baseline Values and Assumptions Document (BVAD) nominal 11.82 MJ per person per
day. Gardens are 40 m² per person at 600 µmol m⁻² s⁻¹ for 16 h a day, under LEDs
at 3.0 µmol J⁻¹ (current horticultural LEDs). "Everything else" (recycling,
fabrication, Archive, lighting) is taken as half the gardens again.

| ID | Quantity | Value | Unit |
|---|---|---|---|
| RES-6 | Food energy per person | 137 | W |
| RES-7 | Grow-light electric power per person | 5.33 | kW |
| RES-8 | Electricity to food energy, overall (RES-6 / RES-7) | 2.6 | % |
| RES-9 | Household power, 100 people | 0.80 | MW |
| RES-10 | Household energy over 100 years | 2.53 × 10¹⁵ | J |
| RES-11 | The same, as mass-energy | 0.028 | kg |

RES-8 is the bubble's most important efficiency. Of every watt of electricity,
97% becomes heat in the gardens at once (the rest becomes heat later, in the
crew). Better light recipes, better crops and better
LEDs each raise it, and each lowers `power` and `heat` together.

### Fuel

What fraction of a fuel's mass a reaction releases as energy:

| ID | Quantity | Value | Unit |
|---|---|---|---|
| RES-12 | Fission, ²³⁵U (~200 MeV) | 9.1 × 10⁻⁴ | — |
| RES-13 | D–T fusion (17.59 MeV) | 3.75 × 10⁻³ | — |
| RES-14 | D–³He fusion (18.35 MeV) | 3.92 × 10⁻³ | — |
| RES-15 | Catalysed D–D fusion (43.2 MeV per 6 D) | 3.84 × 10⁻³ | — |
| RES-16 | p–¹¹B fusion (8.68 MeV) | 7.8 × 10⁻⁴ | — |
| RES-17 | Matter–antimatter annihilation | 1 | — |
| RES-18 | Deuterium mass fraction of ordinary water (VSMOW) | 3.48 × 10⁻⁵ | — |
| RES-19 | Deuterium burned for RES-10 (catalysed D–D) | 7.3 | kg |
| RES-20 | Ordinary water holding that deuterium | 210 | t |

A century of household power needs about 7 kg of deuterium, but extracting it
from ordinary water would take 210 t, more than the ship holds. **The household
fuel is therefore carried as concentrated deuterium (heavy water), about 37 kg
of D₂O.** Fuel is not what limits the household: `power` and `heat`
capacities are.

The bubble adds two twists that a planet doesn't have:
- **Neutrons can't escape.** The wall reflects every massive particle,
  neutrons included. D–T and D–D fusion make neutrons, which then activate the
  ship's own structure. Low-neutron fuels (D–³He, p–¹¹B) are worth more aboard
  than on Earth.
- **Nothing is ever disposed of.** Spent fuel, activated parts and helium ash
  stay aboard forever, as pools.

### The drive

The guided tour's three interstellar legs (`guided-voyage-highlights.md`):
4.321 ly at 0.999c, 41.756 ly at 0.9999c and 74.422 ly at 0.999999c, with
`GUIDED_DRIVE` m_eff = 10 kg. Boost plus brake is 2 m_eff c² φ (HB-35);
drag is n γβ m_p c² A d (HB-51); the power to hold cruise is drag force × c.

| ID | Quantity | Value | Unit |
|---|---|---|---|
| RES-21 | α Cen A leg: boost + brake, as mass-energy | 76 | kg |
| RES-22 | α Cen A leg: ISM drag, as mass-energy | 4.8 | kg |
| RES-23 | TRAPPIST-1 leg: boost + brake, as mass-energy | 99 | kg |
| RES-24 | TRAPPIST-1 leg: ISM drag, as mass-energy | 147 | kg |
| RES-25 | Aldebaran leg: boost + brake, as mass-energy | 145 | kg |
| RES-26 | Aldebaran leg: ISM drag, as mass-energy | 2,616 | kg |
| RES-27 | Three interstellar legs, total | 3,088 | kg |
| RES-28 | The same, as catalysed D–D fuel | 8.0 × 10⁵ | kg |
| RES-29 | Drive power to hold 0.99c | 6.97 | GW |
| RES-30 | Drive power to hold 0.999999c (Aldebaran cruise) | 70.8 | TW |

The Solar legs are not counted, and the sim's `lism-1` medium (lower density
in hot gas) will lower RES-22, RES-24 and RES-26. The sim's ledger already
reports the exact figures; enforcing them is the open step.

A photon drive whose light comes from heat must radiate its power from a hot
surface. With the radiator facing aft over the cross-section πR², holding
speed against drag needs σT⁴ = n (γβ)² m_p c³. R cancels, so this is a limit
on speed for any ship size:

| ID | Quantity | Value | Unit |
|---|---|---|---|
| RES-31 | Thermal drive: highest γβ holdable, radiator at 2,000 K | 14.2 | — |
| RES-32 | Thermal drive: highest γβ holdable, radiator at 3,000 K | 31.9 | — |
| RES-33 | Thermal drive: highest γβ holdable at tungsten's melting point (3,695 K) | 48.4 | — |
| RES-34 | Radiator temperature to hold the Aldebaran cruise thermally | 14,100 | K |

## 4. The finding: the drive is a second hand-wave

Two facts above don't fit the canon as written:

1. **No fuel can pay for the tour.** The tour costs about 3,100 kg of
   mass-energy (RES-27). With the best real fusion fuel, that is 800 t of
   deuterium (RES-28), 40 times the ship's entire internal mass. Only total
   conversion of mass to light (RES-17) pays for it, and nothing aboard can
   do that today.
2. **No material can radiate it.** Holding 0.9999c or faster with heat-driven
   light needs radiators hotter than any solid (RES-33, RES-34). A ten-minute
   boost is worse: 76 kg of mass-energy in 600 s is about 11 PW.

The canon already assumes both, silently. Making them explicit is the honest
fix. Options:

| Option | Rule | Consequences |
|---|---|---|
| **A. Physics wins** | Real fusion, thermal drive | Cruise speed tops out near γβ ≈ 30–50 (0.9995c) at 0.1 cm⁻³, about 4.5 times higher in hot gas; 0.999999c and the Aldebaran leg go. Reverses D-15 "arbitrary γ" and the accepted itinerary |
| **B. The generator is the drive** (recommended) | Extend property 2: the generator turns interior mass directly into drive light at the wall, at 100%, with no waste heat. It does nothing else | Mass becomes the price of speed: the Aldebaran leg costs about 2,800 kg, about forty people's worth of atoms. The household still runs on ordinary fusion and radiators, so `power`, `heat` and fuel stay real |
| **C. Total conversion for everything** | The generator also powers the household | Energy stops being scarce (a century costs 0.03 kg). Only heat and mass remain. This loses the household energy game |

**Recommendation: B.** It keeps the canon's one bubble, adds one sentence to
it, and makes the closed-mass rule bite where it matters most:

- The speed slider becomes a mass decision as well as an Earth-years decision.
  0.99c to α Cen costs about 76 kg; pushing a long leg to 0.999999c costs tonnes.
- "Everything competes for the same atoms" (mass-budget) now includes the
  journey itself.
- It fits the spire mystery. The generator is the only machine aboard that
  nobody understands, and it eats mass.

Under B, m_eff matters a great deal: boost and brake scale with it, but drag
does not. The canon value is 1 kg; the guided drive uses 10 kg. Choosing m_eff
is choosing how much of the ship each departure burns.

**Decided (D-69, 2026-10-13): option B, with m_eff = 1 kg.** The guided drive's 10 kg is revisited when the ledger is enforced.

## 4a. Endurance: the reach law (under B)

Drag burns mass in proportion to γβ, while ship time per light-year is
1/γβ years. Their product doesn't depend on speed:

  **mass burned × ship-years = n m_p πR² × (1 ly) × (1 yr) per ly²**

At n = 0.1 cm⁻³ and R = 100 m, that is 0.0497 kg·yr per ly². The ship can
spend mass to save time, or time to save mass, at a fixed rate. Covering a
distance D with mass M and ship time T needs D ≤ √(M T / 0.0497).

| ID | Quantity | Value | Unit |
|---|---|---|---|
| RES-35 | Reach constant n m_p πR² ly · yr, n = 0.1 cm⁻³ | 0.0497 | kg·yr/ly² |
| RES-36 | Drag mass per ly at 0.99c | 0.349 | kg |
| RES-37 | Drag mass per ly at 0.999999c | 35.2 | kg |
| RES-38 | Reach with 5,000 kg and 60 ship-years, n = 0.1 cm⁻³ | 2,457 | ly |
| RES-39 | The same, n = 0.3 cm⁻³ (warm cloud) | 1,418 | ly |
| RES-40 | The same, n = 0.005 cm⁻³ (hot gas) | 10,986 | ly |
| RES-41 | 1,000 ly in 60 ship-years with 20 stops, m_eff 1 kg | 969 | kg |
| RES-42 | The same with m_eff 10 kg | 2,232 | kg |

The reach matches the canon play area (500–3,000 ly, premise-and-loop §2)
without tuning. Mark's condition, "so long as we have enough to have a good
game run", is met with margin. Long voyages are paid for in drag. Stops are
paid for in m_eff.

### What m_eff is, and lowering it

m_eff is **not** the ship's weight. The bubble still holds its structure, its
20 t of contents and its people. m_eff is the inertia the whole pocket shows
to the outside (property 2), tunable but never zero. It sets three things:

- **The cost of each stop** (boost plus brake, 2 m_eff c² φ). At m_eff 1 kg
  a stop costs 5–15 kg; at 0.1 kg, 0.5–1.5 kg.
- **The drive's largest force,** m_eff × the boost acceleration, and so the
  fastest cruise the drive can hold against drag (`driveHoldMaxPhi`). At the
  default boost of 7.5 × 10⁵ g, m_eff 0.1 kg still holds γβ ≈ 1,250 at
  0.1 cm⁻³ (above 0.999999c), but only γβ ≈ 720 in a 0.3 cm⁻³ cloud. At
  0.01 kg it falls to γβ ≈ 400.
- **Hover power near black holes** (HB-90, m_eff a c).

Drag does not depend on m_eff, so lowering it barely changes the reach:

| Voyage | m_eff 1 kg | m_eff 0.1 kg |
|---|---|---|
| 1,000 ly, 60 yr, 20 stops | 969 kg | 843 kg |
| 1,000 ly, 60 yr, 100 stops | 1,530 kg | 899 kg |
| 200 ly, 20 yr, 100 stops (a dense neighbourhood) | 699 kg | 159 kg |

**Proposal:** start at 1 kg (D-69). Make "tuning the pocket lighter" a spire
discovery that lowers m_eff towards 0.1 kg. It pays off for a player who
makes many short hops, and costs some ability to hold top speed in dense gas.

## 4b. Technology tree: first basis

Mark (D-69): "these are all good candidates for a tech tree we discover. This
will evolve over time." Rules:
- Nothing in known physics adds atoms to the bubble.
- Every discovery arrives as information (a blueprint, a map, a spire reading).
- Building it costs `mass` and `fabrication`.
- Each one has a catch.

| Branch | Discovery | Physics | Effect | Catch |
|---|---|---|---|---|
| Drag | **Hot-gas maps** | ISM density varies ~60× | Route through 0.005 cm⁻³: travel ~20× cheaper, reach ×4.5 | Longer paths; maps come from civilisations |
| Drag | **Shaping the wall** (spire) | Specular reflection at a glancing angle | A pointed bow: drag ÷16 at a 10° point, ÷65 at 5° | Spire knowledge; generator time to reshape |
| Drive | **Lighter pocket** (spire) | Property 2 | m_eff 1 → 0.1 kg: stops 10× cheaper | Lower hold limit in dense gas |
| Energy | **Basking** | Absorbed light adds mass-energy, never atoms | Near a star, refills the drive's energy: about 1.5 kg a year at 0.1 AU from a Sun-like star | Heat ceiling; storing energy densely needs antimatter-scale storage |
| Energy | **Beamed boosts** | Light pressure from a civilisation's laser | They pay for your boost | Doppler: at 0.99c only 0.5% of the beam arrives; good for departures |
| Energy | **Superradiance** | Light amplified by a spinning black hole's ergosphere, up to ~4% per pass | A black-hole civilisation's power plant | Needs a mirror cavity around a black hole |
| Energy | **Antimatter storage** | Pair production from light | Dense storage for basked or beamed energy | Very low production efficiency |
| Household | **Electro-food** | Microbes fed by electrolysis | Several times RES-8 | Fabrication and mass to build |
| Household | **Brine membranes** | Better water recovery | Smaller sludge pool, less `power` | — |
| Household | **Low-neutron fusion** | D–³He, p–¹¹B | Less activation of the ship | Fuel scarcity |
| Household | **Hot radiators** | σT⁴ | More `heat` headroom | Materials |
| Crew | **Torpor / longevity** | Metabolic suppression | More ship-years; reach grows with √T | Crew psychology |

Not in the tree, with reasons the Archive can give:
- **Zero-point energy:** the vacuum is already the lowest state.
- **Matter from virtual particles:** the dynamical Casimir and Schwinger
  effects need energy put in.
- **Atoms from light:** baryon number is conserved, so light makes only
  matter–antimatter pairs.
- **Warp:** it needs negative energy, which would be a third invented thing.
- **Unruh heating:** 5 × 10⁻¹⁵ K at boost acceleration.

Combined, the drag and drive branches can make travel nearly free. Atoms
never get cheaper: bodies, children and repairs stay scarce for the whole game.

## 5. How resources appear in play

Consistent with "no constant meters":

- **Stocks are shown abstractly** (`mass-budget.md`'s Comfortable / Tight /
  Critical), with kilograms on demand.
- **Capacities are ship systems.** You see them at Engineering, and they matter
  when one fails: a reclaimer breaks, so the `water` processing capacity
  drops, so the inventory starts draining into the brine pool.
- **Efficiencies are the upgrade path.** A blueprint raises a number in the
  transfer graph; building it costs `mass` and `fabrication`.
- **Crises are capacity failures.** Each one offers the same three answers:
  - repair: spend mass and fabricator time;
  - ration: crew stress, and the gardens suffer first;
  - re-route: switch something else off, which spends `power` or `heat`
    headroom.

  `processing-cascade` in the crew scenario pack is exactly this.
- **The drive's bill is presented at plan time.** "This leg will turn 140 kg
  of the ship into light."

## 6. Crew scale: what a scenario may use

Crew scenarios (`crew_scenario/1`, crew-lab) use whole-number units of the eight
resources, with fixed ids and unit sizes, so scenarios compose and later read
straight from the ship's ledger:

| Id | Crew-scale unit | Consumed or reserved |
|---|---|---|
| `mass` | 10 kg block | Consumed |
| `water` | 1 t | Consumed (returns later through recycling) |
| `power` | 50 kW of headroom | Reserved while the work runs |
| `heat` | 50 kW of headroom | Reserved while the work runs |
| `garden` | 100 m² of lit beds | Reserved |
| `fabrication` | One bay for one shift | Reserved |
| `compute` | One Archive slot | Reserved |
| `signal` | One antenna window | Reserved |

The current pack uses fourteen ad hoc resources ("paper stock", "clinic hours",
"diagnostic cartridges"). They were mapped onto the eight in ailang-packages #133,
and every claim verdict over 20 seeds was unchanged (33 of 36 hold).

## 7. Open questions for Mark

1. ~~Option B, A or C?~~ B (D-69).
2. ~~m_eff?~~ 1 kg (D-69); lowering it is a discovery (§4a).
3. Show the drive's mass cost at plan time in kilograms, or as people-equivalents?
4. Start the household on D–T (neutrons, simplest) or D–³He (cleaner, but where did the ³He come from)?
5. Should the scrap, brine and ash pools be visible places in the ship, for example on the industrial level?

## 8. Acceptance (when a sprint takes this up)

- `sunholo/relativity` gains `heatCeiling(T, R)`, `thermalHoldMaxGammaBeta(n, T)`
  and `massEnergy(E)`, with RES-1…5 and RES-31…34 as tests, published before
  the sim pins them.
- `make test` in the game asserts RES-21…27 from the sim's own ledger (uniform
  medium), and the `lism-1` figures are recorded alongside.
- The crew-lab scenario pack uses only the eight ids, and `crew-sim --check`
  still holds its claims.
