# Voyage Life: the loop between destinations

## Status
- **Status:** Proposed (Mark, attended 2026-10-13: "I'm hoping that the actual game loop in between destinations is this aspect of the game, but also decisions on where to go next. We will be able to skip ahead through days/weeks/months/years as well, so need some kind of simulation on where that leaves the crew")
- **Priority:** P1 for R2. This is the game's main loop between arrivals.
- **Roadmap:** [roadmap/r2-voyage-life.md](../../roadmap/r2-voyage-life.md)
- **Builds on:**
  - [crew-psychology.md](../future/crew-psychology.md);
  - [bubble-society.md](../future/bubble-society.md);
  - [bubble-resources.md](bubble-resources.md) (D-69);
  - [narrative-orchestrator.md](../future/narrative-orchestrator.md);
  - [journey-system.md](../phase3-gameplay/journey-system.md) (Journey Events mode, "What happened aboard");
  - [ui-modes.md](../future/ui-modes.md);
  - art/generation-cast-plan.md (households);
  - the crew host `sunholo/crew_core` and its game integration (stapledons-godot R2-CREW, D-70).

## Game Vision Alignment

Scored by hand; score again with `game-vision-designer` before a sprint.

| Pillar | Alignment | Rationale |
|---|---|---|
| **Choices Are Final** | ✅ Core | A skip cannot be undone; its outcome is journalled. Standing orders are a choice made before you look away |
| **The Game Doesn't Judge** | ✅ Core | The report says what happened, never whether you did well |
| **Time Has Emotional Weight** | ✅ Core | Skipping a year means missing a year of people's lives. The report is the cost |
| **The Ship Is Home** | ✅ Core | Between destinations the ship *is* the game: households, work, arguments, children |
| **Grounded Strangeness** | ✅ Supports | The length of a skip is ship time. The physics sets how much life fits into a leg |
| **We Are Not Built For This** | ✅ Core | Long skips under hard orders are where people break |

## 1. What the player does between destinations

Two modes, using the same simulation:

| Mode | Time | What the player does |
|---|---|---|
| **Live** | One crew turn per ship-day (D-70), at a chosen pace (canon `TimeSpeed`) | Walk the ship and use the consoles: offer work, grant or deny relief, answer concerns. Hear the crew. Watch resources and ship events |
| **Skip** | Ahead by a day, a week, a month, a year, or "until arrival" | Set **standing orders**, choose how sensitive interrupts are, look away, then read the report |

**Where to go next** is part of the same loop. At the navigation station the
choice of destination and speed shows:
- the leg's ship time, which is how much life it holds;
- its Earth time;
- its mass bill (D-69);
- what the crew think.

The crew's opinions use the canon vote formulas (crew-psychology §5), fed by
their current state from the crew host. A crew who are exhausted, divided or
grieving argue differently about a 40-ship-year leg than a rested one. The
decision is the player's; the crew only argue.

Speed sets presence (journey-planning-ui): a high-γ leg is a short ship time,
so a short skip and little life between stops; a lower γ means more ship time
and more life.

## 2. Skipping ahead: where it leaves the crew

### Standing orders

A skip is the crew host running without the captain. **Standing orders** say
what the captain would have done:

| Order | Values | Effect |
|---|---|---|
| Captaincy | consent / orders | as in the host (D-70: consent by default) |
| Rest | rest the tired / only on request / never | when the stand-in captain sends people to rest |
| Relief | grant / deny / grant only when overextended | the answer to relief requests |
| Priorities | ordered list of work kinds | what gets offered first when resources are short |
| Arguments | let the crew settle it (default) / keep the most senior | lost-capacity disputes (crew-strain) |

These are the lab's `crew-sim` captain strategies, generalised and made
player-facing. The lab already measures what each one does to a crew over
many runs.

### Interrupts

A skip stops early at a moment that needs the captain. The player sets the
sensitivity (Few, Some or Most):
- a major event (`loss`, `deep_time`, `first_contact`, `betrayal`);
- a birth or a death;
- someone overextended for N turns in a row;
- relief requests left unanswered;
- an argument over lost capacity;
- trust in the captain falling below a floor;
- a mutiny warning sign (bubble-society);
- any resource reaching zero.

The interrupt shows the moment and returns the player to Live. They can resume
the skip.

### The report: "What happened aboard"

When a skip ends, the journal becomes a report (journey-system's "What happened
aboard"):
- **Counts:** work done, refusals, relief, arguments, births, deaths.
- **Movement:** who changed most in stress, trust and regard.
- **Moments:** the 3 to 7 most salient journal entries (majors, halts, a
  refusal overridden, a first word from a new child).

The facts are authored. The model writes them as prose, in the Archive's
voice, through the recorded AI service. Prose is checked like crew words: no
traits, scores or identifiers. When **Memory Health** is low, the Archive's
report may misremember (archive-system, semantic-memory S3): the facts behind
the report stay true in the journal, but its telling may not.

### Scale and speed

A whole playthrough is at most about 60 to 100 ship-years, so at most about
22,000–36,500 crew turns at one per ship-day. A skip has to be fast:
**target, one ship-year in under 5 s headless**. That requires crew_core's
archiving (R2-CREW CI-0), and a budgeted, interruptible batch with progress
shown, like the light-speed loading view. If day-level turns cannot meet the
target, the fallback is coarser turns during long skips: a week per turn, with
work durations scaled.

## 3. Households and generations

Canon (generation-cast-plan) has about 100 founders in households: partners,
children, carers. Eleven featured adults are the window onto them. A job, an
archetype and a person are separate concepts. Descendants are new people,
never age variants of a parent. Nothing about births or deaths is fixed.

**Two tiers:**
- **Featured people** are crew host actors with OCEAN traits, indicators,
  trust and regard. Today that is the 11 featured adults; later, ship-born
  adults who become featured (children aged 23–28 by year 20).
- **The wider ship** is a society layer: households and factions as aggregate
  actors (bubble-society). It is fed by featured outcomes, and it feeds
  morale and tension back to them.

**What the host needs** (engine backlog; each item is built and verified in the
crew lab first):
1. **Kinship.** Relationship dimensions such as `kin_parent`, `kin_child`,
   `kin_sibling` and `kin_partner`, from scenario data. Events reach kin more
   strongly; for example, a loss to a sibling counts double.
2. **Life cycle.** People can join and leave the host mid-session: births,
   deaths, and children coming of age into featured adults. Each change is
   journalled, with new person ids.
3. **Age.** An age indicator that advances with ship time. Age shapes fatigue
   and recovery, and a death hazard (stress and age) that is seeded and
   authored. Deaths are never scripted dates (DD 2025-12, generation-cast-plan).
4. **Births.** A household-level process that depends on mass (70 kg each,
   D-69), morale and stability (bubble-society: about 2–4% a year in good
   conditions), and the household's values. Canon conflicts on whether the
   captain can limit births (Q2).
5. **Drift.** OCEAN drift within ±0.2 of baseline, using the
   crew-psychology trigger table, through `social_dynamics` personality
   (already present).

## 4. Pacing: the orchestrator decides what happens

Between arrivals, ship events come from two sources:
- **The ship's physics:** heat on an approach, news from home, signal windows,
  the drive's mass bill. These are R2-CREW CI-2.
- **The narrative orchestrator:** a seeded director that draws from the
  **scenario library** (crew-scenario-library.md) by beat and tension
  (Quiet, Rising, Crisis, Resolution; target tension by game stage).

A scenario in the library is a template: crew roles, preconditions and
events. The director binds it to the current crew and ship state. The player
never sees the director (DD 2025-12).

## 5. Open questions for Mark

1. **Skip granularity:** day turns with a speed target (recommended), or week
   turns during long skips?
2. **Births and the captain:** design-decisions lists "limit births" as a
   choice, but bubble-society says the captain cannot prevent births. Which?
3. **Mutiny:** game over and reset (DD, premise-and-loop), or "the ship
   changes course" (black-holes)?
4. **The 100 years:** the captain's lifespan (death near 130 at a start age of
   30), or a budget of 100 ship-years from departure?
5. **Crew size:** about 100 (canon) or the "~50 crew max" in
   crew-assignment-system?
6. **Interrupt default:** Some (recommended), or Most for a first playthrough?

## 6. Acceptance (when a sprint takes this up)

- **Lab:**
  - standing orders as `crew_response_policy`-style data;
  - `crew-sim --skip 365` returns a report JSON and an interrupt list;
  - claims on skips, for example "a year under orders with rest = never
    overextends the engineer in most seeds".
- **Engine:** kinship, life cycle and age in `crew_core`, each with lab tests
  and claims.
- **Game:** skip from the Voyage console, the report shown, interrupts
  returning to Live. One ship-year skip under 5 s (`make crew-perf`).
