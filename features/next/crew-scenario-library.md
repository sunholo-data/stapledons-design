# Crew Scenario Library: a growing set of situations aboard

## Status
- **Status:** Proposed (Mark, attended 2026-10-13: "make a design doc for a new agent to take on, that can iterate and make lots of new scenarios that we can grow over time")
- **Owner:** a dedicated **scenario agent** working in the crew lab. Game integration is a separate agent (stapledons-godot R2-CREW, D-70).
- **Roadmap:** [roadmap/r2-voyage-life.md](../../roadmap/r2-voyage-life.md), track L
- **Consumers:** the narrative orchestrator ([voyage-life-loop.md](voyage-life-loop.md) §4) and the game's `data/crew/` (R2-CREW AC7)

## Game Vision Alignment

| Pillar | Alignment | Rationale |
|---|---|---|
| **Choices Are Final** | ✅ Supports | Every scenario offers choices whose outcomes are journalled |
| **The Game Doesn't Judge** | ✅ Core | Claims describe what happens, never what is right; scenarios have no "correct" answer |
| **Time Has Emotional Weight** | ✅ Core | A large share of the library is about time: news decades late, children growing up, people aging |
| **The Ship Is Home** | ✅ Core | The library is the ship's daily life |
| **Grounded Strangeness** | ✅ Supports | Only the eight bubble resources; physics moments drive events |
| **We Are Not Built For This** | ✅ Core | Strain, grief and breakdown are first-class subjects |

## 1. What a scenario is

A **scenario** is a small, testable situation aboard. It is a set of files:
- **`<id>.json`** (`crew_scenario/1`) has:
  - the cast: who is involved, with traits, values and starting state;
  - the work each person may be offered, with costs and yields in the eight
    resources;
  - rest and overextension;
  - trust;
  - the resources;
  - ship events with their turns.
- **`<id>.claims.json`** (`crew_claims/1`) holds what the designer expects to
  happen, **written before running**, over many seeds, for the consent and
  orders captains.
- **`review/<id>.*`** holds the measured claims, the sampled words, and notes.

A **template** (planned, L3) is a scenario with slots: which roles, how much
of each resource, and when events fire. The orchestrator binds a template to
the current crew and ship. Most of the library should end up as templates.

## 2. Ground rules (canon)

- **A job, an archetype and a person are separate** (generation-cast-plan).
  - A scenario's crew ids are **roles in the scene** (`engineer`,
    `quartermaster`).
  - Each role is played by a **canon archetype**: the 11 featured adults
    (Engineer, Scientist, Medic, Diplomat, Pilot, Quartermaster, Zealot,
    Dreamer, Skeptic, Fantasist, Analyst), with crew-psychology's baseline
    OCEAN profiles ±10.
  - The first pack used non-canon roles (counselor, navigator, botanist).
    Migrating them is task L1.
- **Households are canon proposals** (generation-cast-plan, DD 2025-12):
  - the Engineer is the parent of the Pilot and the Skeptic;
  - the Scientist and the Medic are partners with a child;
  - the Diplomat has a grown child and a grandchild;
  - the Zealot is the Dreamer's older sibling;
  - the Quartermaster has a partner and a child;
  - the Fantasist co-parents;
  - the Analyst joined alone.

  Use them; flag any conflict.
- **Only the eight bubble resources** (D-69): `mass`, `water`, `power`,
  `heat`, `garden`, `fabrication`, `compute`, `signal`. Stocks are consumed;
  capacities are reserved. `crew-sim --bubble` enforces this.
- **No constant meters** (DD 2025-12-06). Scenarios are moments and crises,
  not upkeep.
- **Names are still open.** Ids are roles; display names come later from cast
  files (D-70).
- **The model writes words, never outcomes.** Reactions are authored and
  seeded.
- **Nothing fixed:** no scripted deaths or guaranteed births.

## 3. Taxonomy and coverage

Every scenario is tagged on five axes. `scenarios/stapledon/COVERAGE.md` is a
generated matrix, and the agent fills the gaps:

| Axis | Values |
|---|---|
| **Trigger** | ship physics (approach, signal, drive, failure) · news and loss · household · Archive · contact · society (faction, ritual, vote) · work and strain |
| **Voyage phase** | departure · cruise · approach · arrival · deep time (long skip) · black-hole park |
| **Time scale** | moment (days) · season (months) · years · generation |
| **Pillar** | the six |
| **Resource** | which of the eight it stresses |

Early families to grow (from canon):
- **Departure and last messages** (D-70 brings `last-messages-home` aboard
  first).
- **News decades late and grief.**
- **Processing failures and repair.**
- **Black-hole dread and refusal.**
- **Light from the dead.**
- **The Archive misremembering.**
- **Births:** mass and morale.
- **A death:** recycling, rituals in the Garden Cathedral.
- **Children coming of age.**
- **Generations who remember Earth, and those who do not.**
- **Faction rise:** Progressives and Preservers.
- **Meaning crisis.**
- **Rituals:** Founding Day.
- **Crew votes on the next destination.**
- **A crew member sending an unauthorised message** (inner-shapes-outer Q2).

## 4. The agent's loop

For each batch of about 6 scenarios:

1. **Pick** gaps from COVERAGE.md and canon. Write a one-paragraph brief per
   scenario, with canon citations.
2. **Write the claims first:** at least 4 per scenario, including clean runs
   under both captains and one claim per intended tension.
3. **Author** the scenario JSON. The model may draft the purpose and event
   text; ids, numbers and reactions are authored.
4. **Gates:**
   - `crew-sim --bubble` (resources);
   - seeds 1..3 play clean;
   - `crew-sim --check` over 20 seeds with `--policy scenarios/stapledon/policy.json`;
   - `crew-sample` words with no leaked traits or ids;
   - `make scenario-pack-test`.
5. **Record:** add the results to REVIEW.md, **failures included**. A failing
   claim is a finding about the design. Never tune a scenario to pass a claim
   without saying so.
6. **Review page:** publish a review artifact for Mark: situation, crew,
   claims, words and findings.
7. **Engine backlog:** if a scenario needs something the host cannot express
   (kinship, births, age, factions), write it up. Don't hack around it.

## 5. What the agent needs from the engine (backlog)

These are built and verified in `crew_core` with lab tests and claims, then
re-vendored into the game:

| # | Capability | Needed by |
|---|---|---|
| E1 | crew_core 0.3.0 (ailang-packages#137): archiving, run-time ship events, policy switch, standing orders, escalation, `advance(n)` | everything long; the game |
| E2 | Kinship relationship dimensions with stronger effects among kin | household scenarios |
| E3 | Life cycle: join and leave mid-session (births, deaths, coming of age) | births, deaths, generations |
| E4 | Age and a seeded death hazard | deep-time scenarios |
| E5 | The skip report built from the journal (standing orders and escalation come with E1) | voyage-life-loop |
| E6 | Templates: slots bound to the current crew and ship | the orchestrator |
| E7 | Cohort actors (#137 CP-0/CP-1; crew-population.md) and factions | population, votes, mutiny warnings |
| E8 | Crew memory (semantic-memory S2) | callbacks, the Archive misremembering |

## 6. Tasks to start with

| Task | What |
|---|---|
| L1 | Migrate the pack to canon archetypes: map counselor, navigator, botanist and others to the 11 featured adults or to jobs they hold; use the households; rerun claims; record changes |
| L2 | Give the ergosphere scenario a third heat user (D-70 OQ6); rerun and record whether the argument now happens |
| L3 | Template format (E6) and a generator that instantiates many variants with claims |
| L4 | COVERAGE.md generator, and the first two batches (12 scenarios) on canon families |
| L5 | Household scenarios once E2 lands |

## 7. Acceptance (per batch)

- `make scenario-pack-test` passes.
- Every new scenario passes `crew-sim --bubble`.
- Claims are recorded with the 20-seed result. REVIEW.md has a section per
  batch, listing failures and their causes.
- COVERAGE.md is regenerated, and gaps shrink.
- Sampled words are present for each scenario, with no identifier leaks.
- Scenarios meant for the game are listed in a sync manifest that the game's
  `make crew-sync-check` reads (R2-CREW AC7).
