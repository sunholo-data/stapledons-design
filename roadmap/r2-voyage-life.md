# Roadmap: Voyage Life and the Living Galaxy (the social strand)

**Status:** Approved 2026-10-27 (Mark, attended). Proposed (Mark, attended 2026-10-13: "let's create a roadmap doc for this strand of work"). The canon conflicts it raised were ruled on as **D-71**, below.
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
| stapledons-godot `design_docs/implemented/r2/crew-integration.md` | The crew host aboard: R2-CREW, D-70. Includes auto-run with standing orders, escalation rules and `crew_hold` (CI-5) |
| stapledons-godot `design_docs/planned/r2/crew-population.md` | The ~1,000 aboard as 8–12 cohort actors with mood buckets (R2-CREW-POP): the ship's group and population tiers |

## Who does what

| Agent | Works in | Owns |
|---|---|---|
| **Crew-lab agent** (the attended session line that built the lab; takes ailang-packages#137) | ailang-packages `packages/crew-core`, `examples/crew-lab` | the engine (`crew_core`, `social_dynamics`), claims, evaluations, publishing |
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
| E1 ✅ | R2-CREW CI-0, ailang-packages#137 (crew_core 0.3.0): archiving, run-time `ship_event`, `policy` switch (D-70), standing orders, escalation rules, `advance(n)` | lab validate; claims unchanged; 500 turns grow linearly; `crew-sim --auto` escalations per year; 10-year interlude < 2 s in game | E0 |
| E2 | Kinship dimensions | household claims hold | E1 |
| E3 | Life cycle (join and leave), age, seeded death hazard, births tied to mass and morale | lab claims on births and deaths over long runs | E1, E2 |
| E4 (game half ✅) | The skip report ("What happened aboard") built from the journal; skip claims (standing orders and escalation are now in E1). **Game half done** (R2-INTERLUDE, game [#264](https://github.com/sunholo-data/stapledons-godot/pull/264), 2026-10-17): the cruise digest in the pure sim (counts, movement, up to seven moments by a local salience rule) feeds the crossing review. **Engine half open:** the journal-built report in crew_core; the game swaps its local ranking for it (I3b) | `crew-sim --auto --turns 365` report and escalation claims | E1 |
| E5 | Scenario templates bound to crew and ship state | the orchestrator can draw and bind | E1 |
| E6 ✅ | Cohort actors (crew_core 0.4.0, pinned in game #231) (CP-0/CP-1 in ailang-packages#137; crew-population.md): headcount, OCEAN mix, mood and trust buckets, seeded transitions per bucket, yearly births and deaths | a founders scenario of ~1,000 growing to ~1,500 (D-75); cohort claims | E1 |
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
| G1 ✅ | R2-CREW: the crew aboard (landed, game [#224](https://github.com/sunholo-data/stapledons-godot/pull/224), 2026-10-14; Crew console [#239](https://github.com/sunholo-data/stapledons-godot/pull/239)); `last-messages-home` first; consent by default; mass bill as a readout (D-70) | its ACs (`make crew-parity`, `make crew-vendor-check`, …) | E1 |
| G2 ✅ | Skip from the Voyage console; interrupts; "What happened aboard" in the Archive's voice. Landed for the cruise case (R2-INTERLUDE, game [#264](https://github.com/sunholo-data/stapledons-godot/pull/264), 2026-10-17; evaluation 89/100): the crossing goes in chunks the sim publishes (protocol 2.10), a crew hold stops it on its check-in day, and the crossing review (crossing, home picture per tier, aboard, moments with faces, who changed, the medium, waiting) is read at the player's pace; the Archive frames it with authored lines and, with live AI, tells it once, recorded and voiced | one ship-year skip < 5 s in game (critical_only decade 0.89 s, slowest chunk 0.11 s); a report shown | E4, G1 |
| G3 | The orchestrator draws library scenarios by beat and tension | scenarios appear in play; parity holds | E5, L3, G1 |
| G4 | Where to go next with the crew's voices at the navigation station | crew opinions shown from live state | G1 |
| G5 | Households and generations aboard (births, deaths, coming of age) | portraits and new identities (DD 2025-12) | E3, G2 |
| G6 ✅ | The ship as a stage: travel along the deck (D-92) and a fixed Bow view that slides with the captain and never turns (D-91). Landed (R2-CAMERA-FORWARD, game [#282](https://github.com/sunholo-data/stapledons-godot/pull/282) and [#286](https://github.com/sunholo-data/stapledons-godot/pull/286), 2026-10-20; evaluation 88/100; Mark accepted F1 in play, D-94): the starbow, the destination and the deck in one frame, Astern on 9, consoles zoom; a guided stop turns the body 30° off the bow, and the Sgr A* hole sits on the bow. Next: the bridge deck re-planned facing the bow (D-90), then per-level 2.5D views with parallax and per-view set dressing | `make camera-forward-test`, `make camera-views-capture` | G1 |
| G7 ✅ | Star surfaces you can see when the disc is dimmed (D-105, D-111, D-114): per-type limb darkening; cells, spots, faculae and flares as a temperature map, gated by real resolution and flux-preserving; a close pass of Sun-like stars; the helm's telescope; eclipse layers (corona, prominences, diamond ring). Landed (R2-STAR-SURFACES, game [#302](https://github.com/sunholo-data/stapledons-godot/pull/302), [#315](https://github.com/sunholo-data/stapledons-godot/pull/315), 2026-10-21; evaluation 77/100; `sunholo/celestial` 0.5.0; spec RS-24, RS-25) | goldens G-SS-1..8; flux = e_v within 0.5 %; unresolved Sun byte-identical | the D-49 colours, finite stars |
| G8 ✅ | The bridge as one illustrated world (R2-BRIDGE-STYLE, game [#300](https://github.com/sunholo-data/stapledons-godot/pull/300) and the ST3–ST6 PR, 2026-10-22; D-100, D-104, D-106, D-110): the spire lights the deck in deep space and hands the light to a near star; banded toon shading and ink lines; grown, curved props (D-104); the deck and mast painted; painted foliage cards (D-106); the whole HUD on one theme. Next: the Commons and lower levels get their own passes (R2-LEVELS) | `make style-test`, `make dressing-test`, `make ui-theme-check`, `make style-capture` | G6 |

### Track C: civilisations (crew-lab agent, then integration)

| ID | Milestone | Exit criteria | Depends |
|---|---|---|---|
| C1 | T3 compartments (expected-value and seeded) | tests; the founders by cohort | — |
| C2 | T2 groups and tier coupling | faction claims | C1, E6 |
| C3 | One world end to end in the lab, with light-lag news | world claims over seeds | C2 |
| C4 | The contact scenario (crew argue, voice, a 40-year answer) | claims; words | C3 |
| C5 | Worlds advance between visits in the game; light-speed messages. **Moved to R3** ([r3-signals-and-contact.md](r3-signals-and-contact.md) track C, 2026-10-27) | the galaxy view shows last-seen state plus arrived light | C4, G1 |

## Order of work (next)

1. **E1** (crew-lab agent): it unblocks G1, and every long run depends on it.
2. **L1, then L2** (scenario agent), in parallel with E1. Each is data-only.
3. **G1** (integration agent), on E1.
4. **E4, then L6, then G2:** skipping, which is the heart of the loop.
5. **E2 and E3, then L5, then G5:** households and generations.
6. **C1 to C4** in the lab once E6 exists. **C5** follows R2, in [R3](r3-signals-and-contact.md) with the contact console.

## Decisions (D-71, Mark, attended 2026-10-13)

The canon conflicts found while drafting, as ruled:

1. **Crew and population.** "Crew is a % of the population I guess? not sure
   what is practical." The featured crew are a share of those aboard: ~1,000 growing to ~1,500, per D-75. The
   practical split comes from the scale tiers: individuals for the featured
   crew, cohorts for the rest (crew-population.md, civ-mirror §2).
2. **Births are a captain's duty:** "part of captain's duties to control and
   encourage population". This supersedes bubble-society's "cannot prevent
   births".
3. **Mutiny.** Losing the captaincy for good is game over. Before that there
   may be an interregnum: the captain has no power and the ship is
   mob-ruled. A mutiny may end with the captain murdered, "an extreme
   consequence of decisions".
4. **100 years is the captain's lifespan.**
5. **Skips:** the player chooses how long, with a sensible default.
6. **The galaxy clock always runs:** "a core concept of the game".
7. **Worlds can talk to each other,** but the bubble ship is how they discover
   each other and the technology to talk: "we are the first interstellar
   spaceship for all worlds".

## Risks

| Risk | Mitigation |
|---|---|
| Individual simulation does not scale (measured: 15 crew × 30 turns takes 24 s) | Archiving (E1); scale tiers, with individuals only for featured people (civ-mirror §2) |
| Scenario content drifts from canon | Canon citations in every brief (library §4); archetypes and households (L1); review pages |
| Tuning to pass claims | Claims written first, failures recorded as findings (a rule in every gate) |
| The lab and the game diverge | R2-CREW byte parity; the engine changes only upstream |
| Registry blocked for crew_core (ailang#1811) | Vendoring in the game; one-line switch later |
