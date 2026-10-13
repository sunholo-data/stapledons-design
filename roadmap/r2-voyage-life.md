# Roadmap: Voyage Life and the Living Galaxy (the social strand)

**Status:** Proposed (Mark, attended 2026-10-13: "let's create a roadmap doc for this strand of work").
**Scope:** everything social:
- the crew between destinations;
- skipping time and what it does to people;
- households and generations;
- the growing library of situations;
- civilisations that run the same kind of simulation.

**Releases:** begins in R2 (the crew host comes aboard, D-70) and runs alongside the voyage and physics roadmap ([r1-foundations.md](r1-foundations.md)).

## The idea in one paragraph

Arrivals are the spectacle; the game is what happens between them. Aboard, the
crew live, work, argue, grieve and grow up, drawing on a closed ship (D-69).
The captain decides where to go next, how fast, and how to lead. They can look
away for days or years and come back to find out what that cost. Out there,
each world runs the same kind of simulation at the scale of factions and
populations. The ship can only reach those worlds with light, late, in the
voice of whoever aboard was willing to speak.

## Design documents

| Doc | What it decides |
|---|---|
| [voyage-life-loop.md](../features/next/voyage-life-loop.md) | Live and Skip modes, standing orders, interrupts, the "What happened aboard" report, households and generations, where-to-go-next with the crew's voices |
| [crew-scenario-library.md](../features/next/crew-scenario-library.md) | The scenario agent's brief: rules, taxonomy, loop, gates and first tasks |
| [civ-mirror.md](../features/next/civ-mirror.md) | Civilisations as the same simulation one level up, with scale tiers (individuals, groups, populations) and deterministic statistics |
| [inner-shapes-outer.md](../features/next/inner-shapes-outer.md) | How the crew and the worlds affect each other |
| [bubble-resources.md](../features/next/bubble-resources.md) | The eight resources, D-69 |
| [semantic-memory.md](../features/next/semantic-memory.md) | Crew recall, the Archive's misremembering, déjà vu across runs |
| stapledons-godot `design_docs/planned/r2/crew-integration.md` | The crew host aboard: R2-CREW, D-70 |

## Who does what

| Agent | Works in | Owns |
|---|---|---|
| **Crew-lab agent** (the attended session line that built the lab) | ailang-packages `packages/crew-core`, `examples/crew-lab` | the engine (`crew_core`, `social_dynamics`), claims, evaluations, publishing |
| **Scenario agent** (new) | the crew lab's `scenarios/` | the scenario library, COVERAGE.md, review pages for Mark |
| **Integration agent** | stapledons-godot | R2-CREW and its successors: vendoring, sim wiring, protocol, consoles |
| **Mark** | — | rulings (ledger), canon, reviews |

The rule that keeps it fast: **social behaviour is designed and proven in the
lab; the game proves parity.** The lab answers in seconds per scenario, with
no game build.

## Tracks and milestones

### Track E: the engine (crew-lab agent)

| ID | Milestone | Exit criteria | Depends |
|---|---|---|---|
| E0 ✅ | Crew host, verification loop, resources that flow, crew strain (crew_core 0.2.0, social_dynamics 0.4.0 published) | Done 2026-10-13; four sprints, each evaluated (80, 77, 80, 80) | — |
| E1 | R2-CREW CI-0: archiving, run-time `ship_event`, run-time policy switch (D-70) | lab validate; claims unchanged; 500 turns grow linearly (< 10 s) | E0 |
| E2 | Kinship dimensions | household claims hold | E1 |
| E3 | Life cycle (join and leave), age, seeded death hazard, births tied to mass and morale | lab claims on births and deaths over long runs | E1, E2 |
| E4 | Standing orders as data, a skip driver with interrupts, and the report journal | `crew-sim --skip 365` < 5 s; interrupt claims | E1 |
| E5 | Scenario templates bound to crew and ship state | the orchestrator can draw and bind | E1 |
| E6 | Society layer: T2 groups and T3 cohorts (`sunholo/society`, proposed) | founders by cohort; faction claims | E3 |
| E7 | Crew memory (semantic-memory S2) in the reply worker | variety and recall claims | E1 |

### Track L: the scenario library (scenario agent)

| ID | Milestone | Exit criteria | Depends |
|---|---|---|---|
| L1 | Migrate the pack to canon archetypes and households | claims rerun; changes recorded | — |
| L2 | Ergosphere: a third heat user (D-70 OQ6) | the argument occurs in most seeds, or the finding is recorded | — |
| L3 | Templates and a generator | variants are instantiated with claims | E5 |
| L4 | COVERAGE.md and batches 1–2 (12 scenarios) | gates pass; review page published | L1 |
| L5 | Household, birth, death and coming-of-age families | claims hold, or findings are recorded | E2, E3 |
| L6 | Long-skip scenarios: a year aboard under each standing order | report and interrupt claims | E4 |

### Track G: in the game (integration agent)

| ID | Milestone | Exit criteria | Depends |
|---|---|---|---|
| G1 | R2-CREW: the crew aboard; `last-messages-home` first; consent by default; mass bill as a readout (D-70) | its ACs (`make crew-parity`, `make crew-vendor-check`, …) | E1 |
| G2 | Skip from the Voyage console; interrupts; "What happened aboard" in the Archive's voice | one ship-year skip < 5 s in game; a report shown | E4, G1 |
| G3 | The orchestrator draws library scenarios by beat and tension | scenarios appear in play; parity holds | E5, L3, G1 |
| G4 | Where to go next with the crew's voices at the navigation station | crew opinions shown from live state | G1 |
| G5 | Households and generations aboard (births, deaths, coming of age) | portraits and new identities (DD 2025-12) | E3, G2 |

### Track C: civilisations (crew-lab agent, then integration)

| ID | Milestone | Exit criteria | Depends |
|---|---|---|---|
| C1 | T3 compartments (expected-value and seeded) | tests; the founders by cohort | — |
| C2 | T2 groups and tier coupling | faction claims | C1, E6 |
| C3 | One world end to end in the lab, with light-lag news | world claims over seeds | C2 |
| C4 | The contact scenario (crew argue, voice, a 40-year answer) | claims; words | C3 |
| C5 | Worlds advance between visits in the game; light-speed messages | the galaxy view shows last-seen state plus arrived light | C4, G1 |

## Order of work (next)

1. **E1** (crew-lab agent): it unblocks G1, and every long run depends on it.
2. **L1, then L2** (scenario agent), in parallel with E1. Each is data-only.
3. **G1** (integration agent), on E1.
4. **E4, then L6, then G2:** skipping, which is the heart of the loop.
5. **E2 and E3, then L5, then G5:** households and generations.
6. **C1 to C4** in the lab once E6 exists. **C5** follows R2.

## Decisions needed from Mark (canon conflicts found while drafting)

1. **Crew size:** about 100 people (generation-cast-plan, bubble-resources),
   or "~50 crew max" (crew-assignment-system)?
2. **Births:** can the captain limit births (design-decisions), or not
   (bubble-society)?
3. **Mutiny:** game over and a spire reset (design-decisions,
   premise-and-loop), or "the ship changes course" (black-holes)?
4. **The 100 years:** the captain's lifespan, or a budget of ship-years from
   departure?
5. **Skip granularity:** day turns with a speed target, or week turns during
   long skips (voyage-life-loop Q1)?
6. **The galaxy clock:** do worlds advance while you are far away (civ-mirror
   Q1)?
7. **Worlds talking to each other** without you (civ-mirror Q2)?

## Risks

| Risk | Mitigation |
|---|---|
| Individual simulation does not scale (measured: 15 crew × 30 turns takes 24 s) | Archiving (E1); scale tiers, with individuals only for featured people (civ-mirror §2) |
| Scenario content drifts from canon | Canon citations in every brief (library §4); archetypes and households (L1); review pages |
| Tuning to pass claims | Claims written first, failures recorded as findings (a rule in every gate) |
| The lab and the game diverge | R2-CREW byte parity; the engine changes only upstream |
| Registry blocked for crew_core (ailang#1811) | Vendoring in the game; one-line switch later |
