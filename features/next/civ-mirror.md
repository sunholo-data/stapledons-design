# The Civilisation Mirror: worlds that run the same simulation

## Status
- **Status:** Proposed (Mark, attended 2026-10-13: "perhaps they run a similar simulation on their planets, then influenced by our visits and communication once it gets there between systems, that will mirror the interpersonal dynamics of internals"; "what are our limits for crew size, can we run it with tens, hundreds, thousands or even millions of citizens to represent planets? I guess we don't do it all deterministically and use statistics at some point")
- **Priority:** P2 (design now; build after the crew loop, roadmap track C)
- **Extends:** [inner-shapes-outer.md](inner-shapes-outer.md). That doc covers the coupling: crew state shapes messages, and civs answer with efficiencies and minds. This doc covers how a civilisation is simulated, and at what scale.
- **Builds on:**
  - [civilization-trade.md](../future/civilization-trade.md) (civ data, philosophies);
  - design-decisions 2025-12-02 (Perturbation Operator; Three Distance Regimes; launch order = arrival order);
  - premise-and-loop (the doom clock; what you build changes the odds).

## Game Vision Alignment

| Pillar | Alignment | Rationale |
|---|---|---|
| **Choices Are Final** | ✅ Core | A message cannot be recalled; a gift propagates through a society you will not see again for centuries |
| **The Game Doesn't Judge** | ✅ Core | You perturb; the simulation shows what followed |
| **Time Has Emotional Weight** | ✅ Core | Centuries pass for them between your visits; the society you meet again is not the one you left |
| **The Ship Is Home** | ✅ Supports | The ship's own society is the lens; its divisions shape what you send |
| **Grounded Strangeness** | ✅ Core | Light-lag, dilation and arrival order are real constraints on the exchange |
| **We Are Not Built For This** | ✅ Supports | Contact can break a world as easily as help it |

## 1. The mirror

A civilisation runs the same kind of simulation as the ship, one level up:

| Aboard the ship | On a world |
|---|---|
| A crew member (OCEAN traits, values) | A **faction or institution** (temperament, philosophy) |
| The captain | The world's **governance**: consensual, hierarchical, or none |
| Work offered and accepted | **Projects**: megastructures, launches, reforms |
| The eight bubble resources | **Energy, materials, attention, territory, knowledge** |
| Overextension, then refusal and relief | **Crisis**, then unrest and collapse or reform |
| Regard between crew members | **Regard between factions** |
| An argument over lost capacity (crew strain) | **Civil conflict over scarcity**: the least powerful faction is squeezed first |
| Trust in the captain | Trust in the **Ship** (the strangers), and in other worlds |
| A ship event the whole crew hears | **News** that reaches every faction, possibly late or distorted |
| Consent or orders | Who decides: a broad coalition, or a ruling faction |

The engine ideas are the same: requests, commitments, trust, resources, and
claims checked over seeds. Content and scale differ.

## 2. Scale: how many people can we simulate?

Measured on today's crew host (2026-10-13, one seed, one run):

| Crew | 12 turns | 30 turns |
|---:|---:|---:|
| 5 | 0.9 s | 1.9 s |
| 10 | 2.1 s | 10.3 s |
| 15 | 3.9 s | 24.2 s |

The scenario format allows 1–15 crew. Cost grows with crew size × accumulated
history: every crew member hears every event, and finished work is not yet
archived. **Individual agents stop being practical in the tens.** People in
the hundreds or millions need a different representation, and that
representation is statistical.

**Scale tiers:**

| Tier | Who | Count | Representation |
|---|---|---|---|
| **T1 Individuals** | featured crew; a civ's named figures (a speaker, a rival) | up to about 15 per host (about 30–50 with archiving and optimisation) | crew host actors: full OCEAN, indicators, relationships, words |
| **T2 Groups** | households and factions aboard; a world's factions and institutions | about 5–40 | host actors whose traits are **group averages** and whose capacity scales with size |
| **T3 Populations** | the ~100 founders as cohorts; a world's billions | any | **distributions**: counts per state per cohort (age, faction, belief), moved by rates |

### Statistics without losing determinism

"Statistical" does not mean "non-deterministic". A T3 population is a set of
**compartments**: for example, 4.1 billion content, 0.6 billion unrest and
0.2 billion fled. Each turn moves fractions between compartments:
- **Expected-value mode** uses fixed rates: exact, smooth, and byte-identical
  on replay.
- **Sampled mode** draws seeded binomials from a named stream (as the game's
  `rng.ail` does): noisy but still byte-identical on replay.

Both are pure functions of (seed, inputs). Neither ever simulates the
billions one by one.

**The same tiers aboard:** the game's crew-population.md (R2-CREW-POP) makes
the ~100 founders 8–12 cohort actors with mood and trust buckets and seeded
transitions per bucket. That is T3 with T2 groupings, built as crew_core
cohort actors (ailang-packages#137, CP-0/CP-1). Worlds should reuse the same
cohort actor, at a different tempo.

**Coupling between tiers:**
- T3 → T2: a faction's strength comes from the size of its compartments.
- T2 → T3: a faction's decision changes the rates; a reform slows unrest.
- T1 → T2: a named figure's choices weigh on their faction.
- **The ship uses the same tiers.** The featured crew are T1, households and
  factions are T2, and the founders by cohort are T3. That is how
  voyage-life-loop's "wider ship" works.

**Time steps per tier:** a crew turn is a ship-day (D-70). A world's T2 and T3
turn is a year or a decade of *its* time. Between visits it runs for the
galaxy-frame time that has passed: hundreds of turns, cheap at these sizes.

## 3. What reaches a world, and when

Only light crosses (D-11). Everything arrives with light-lag and is delivered
as **news** to the world's factions. It is appraised by their temperament, as
crew appraise ship events.

| We do | It arrives | It becomes, in their simulation |
|---|---|---|
| **Visit** (arrive in-system) | at once, while we are there | a major event to every faction (first contact, or return) |
| **Transmit** from elsewhere | after d years (light-lag) | news delivered to factions. It is appraised by the voice it came in: which crew member spoke, and whether the crew were divided (inner-shapes-outer §2) |
| **Give a blueprint** | with the message | a project that raises an efficiency, changing their resource rates |
| **Give a map** | with the message | knowledge of another world, and a possible new relationship (and arrival-order risk to Earth) |
| **Share a philosophy** | with the message | a value shift in the factions that accept it, and a schism with those that reject it |

Their answers come back the same way:
- light-lag replies, as ship events for our crew (inner-shapes-outer §3);
- their transmissions to other worlds they know of, which builds the network
  (Perturbation Operator: the contact graph you poke).

## 4. A world's simulation

- **State:**
  - T2 factions (a temperament like OCEAN, a philosophy, power, regard for
    each other, trust in the Ship);
  - T3 compartments;
  - resources (energy, materials, attention, territory, knowledge);
  - projects in progress;
  - known worlds;
  - existential risks (civilization-trade).
- **Turn:** factions propose and commit to projects; resources flow; scarcity
  starts arguments (squeeze the weakest); compartments move by rates; news is
  appraised; projects complete.
- **Seeds:** each world takes a stream derived from (game seed, world id), so
  two worlds never share draws and a world replays identically.
- **The model writes words only:** a world's messages and its figures' lines,
  in its own voice. Decisions are authored and seeded, as aboard (D-20).
- **Lab first:** world scenarios and claims in the lab, checked over seeds
  before the game. For example: "an isolationist world given a map of an
  expansionist one has unrest above 30% within 300 years in most seeds".

## 5. Plan

| Step | What | Where |
|---|---|---|
| C1 | A T3 compartment model (pure, expected-value and seeded modes) with tests; also the ~100 founders by cohort | lab, a new package (proposed name `sunholo/society`) |
| C2 | T2 groups: factions as host actors with group traits and size-scaled capacity; the coupling between tiers | lab |
| C3 | One world end to end: a scenario, light-lag news in, a reply out, claims over seeds | lab |
| C4 | The contact scenario from inner-shapes-outer §6.3: the crew argue, the message goes out in a voice, and the world's answer returns 40 years later | lab |
| C5 | In the game: worlds advance between visits; messages travel at light speed; the galaxy view shows each world as you last saw it, plus what light has reached you since | game, after R2 |

## 6. Decided (D-71) and still open

**Decided (Mark, 2026-10-13):**
- **The galaxy clock always runs.** Every world advances for the time that
  passes, wherever the ship is. This is a core concept.
- **The bubble ship is the first interstellar ship for all worlds.** Worlds can
  talk to each other, but they learn that others exist, and gain the
  technology to reach them, from the ship: maps and blueprints are the
  Perturbation Operator's gifts. Before contact, a world's network is empty.
  After contact it grows at light speed, and every link traces back to
  something the ship gave.

**Still open:**
1. **Sampled or expected-value populations?** Sampled (seeded) gives
   surprise, expected-value gives smoothness. Recommended: expected-value for
   the ship's founders, sampled for worlds.
2. **Do we model Earth itself as a world** in this simulation, so the doom
   clock and the news you receive come from the same model?
