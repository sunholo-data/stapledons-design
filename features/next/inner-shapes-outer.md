# Inner Shapes Outer: the crew loop and the civilisation loop

## Status
- **Status:** Proposed (draft for Mark's review, 2026-10-13)
- **Priority:** P1 for design, P3 for build (after crew integration and the resource ledger)
- **Builds on:**
  - [bubble-resources.md](bubble-resources.md) (what the ship has);
  - [crew-psychology.md](../future/crew-psychology.md);
  - [civilization-trade.md](../future/civilization-trade.md);
  - [archive-crew-trust.md](../future/archive-crew-trust.md);
  - `vision/premise-and-loop.md` (Mark's rulings of 2026-10-06, on branch `design/galaxy-and-life`).
- **Prototype:** the crew host `sunholo/crew_core` and the lab `crew-lab` (ailang-packages), where the crew loop already runs with AI-written words

> "I'm thinking we will extend this internal microcosm of the interior to the
> macroscopic interactions between civilisations in a similar manner. This is
> a gameplay loop: micro internal influencing macro external." (Mark,
> 2026-10-13)

## Game Vision Alignment

Scored by hand; score again with `game-vision-designer` before a sprint.

| Pillar | Alignment | Rationale |
|--------|-----------|-----------|
| **Choices Are Final** | ✅ Core | A message sent at light speed cannot be recalled, and the answer comes back decades later |
| **The Game Doesn't Judge** | ✅ Core | You perturb the network; it shows what followed (Perturbation Operator, 2025-12-02) |
| **Time Has Emotional Weight** | ✅ Core | A civilisation lives centuries between your visits; the crew live days |
| **The Ship Is Home** | ✅ Core | What the ship can say to the galaxy depends on who aboard is willing to say it |
| **Grounded Strangeness** | ✅ Supports | Only light crosses; contact is signals, light-lag and dilation, never a handshake |
| **We Are Not Built For This** | ✅ Supports | The crew carry the galaxy's news, and the galaxy's grief, inside a 100 m sphere |

## 1. One engine, two tempos

The crew host is a general engine for **actors with traits and values**. Its
pieces:
- actors who make **requests**;
- **commitments** made and kept;
- **trust** that builds or erodes;
- **resources** that are reserved and spent;
- **claims** that check whether a design behaves as intended.

Nothing in it is specific to people on a ship. A civilisation fits the same
shape at a different tempo:

| | Crew (inner) | Civilisation (outer) |
|---|---|---|
| Actor | A crew member: OCEAN traits, values, fatigue, stress, morale | A civilisation: values, philosophy, stability, contact openness, existential risks (`civilization-trade.md`) |
| One step | Hours to days of ship time | Years to centuries of its own time |
| Request | "Take the night watch in Processing" | "Send us your drive" / "Tell us where the others are" |
| Commitment | Agreed work, kept or broken | A promise across light-years, kept or broken generations later |
| Trust | In the captain | In the ship, and in each other |
| Resources | The eight bubble resources at crew scale | Population, energy use, knowledge held |
| Words | AI-written, in character, checked | AI-written, in the civilisation's voice, checked the same way |

The two tempos are linked by physics the game already computes:
- **time dilation:** ship time against Earth and galaxy time;
- **light-lag:** an answer takes at least the distance in years;
- **arrival order** (2025-12-02): ships launched earlier arrive earlier.

## 2. Inner shapes outer

What the ship can do to the galaxy depends on the state of the ship:

- **Who speaks.** Every transmission goes out in a crew member's voice, chosen
  by the captain, or by whoever volunteers when the captain is not trusted.
  Their traits colour it. A skeptic's message reads differently from a
  zealot's, and a civilisation's reaction depends on both the content and the
  voice.
- **What the crew will share.** Sharing the drive or the maps is a decision the
  crew can refuse (consent and orders captains, as in the lab). A divided crew
  sends a hedged message. A captain who orders it spends trust.
- **What the Archive holds.** Blueprints and maps exist only if the Archive kept
  them (`archive-crew-trust.md` memory health). A degraded Archive gives away
  wrong maps.
- **What the ship can afford.**
  - Every transmission costs `signal` and `power`.
  - Every visit costs `mass`, because the drive burns it (`bubble-resources.md` §4).
  - A ship that has spent its mass on children cannot also visit three more
    stars.

## 3. Outer shapes inner

Only light crosses the wall, so nothing material ever comes back. What does
come back changes the ship in exactly two ways.

**Efficiencies (the resource link).** A civilisation's science arrives as a
blueprint that raises one transfer in the bubble's resource graph. Examples:
- a better photosynthetic pigment (light to food, RES-8);
- a brine membrane (water recovery);
- a low-neutron fusion catalyst;
- a radiator coating (`heat`).

Building the blueprint costs `mass` and `fabrication`. **This is the main way
the macro loop pays the micro loop.** The galaxy can never give the ship more
atoms. It can only teach it to waste fewer.

**Minds.** Everything else that returns changes the crew:
- philosophies, which shift values (crew-psychology drift);
- news and loss: a civilisation you helped collapsed; one you warned survived;
- maps, which open choices;
- reframings of the spire, through the Archive.

Each arrives as a crew event, and the crew host plays it out. Two scenarios in
the review pack, "light-from-the-dead" and "news-decades-late", are already
this.

**The loop closes:**
1. Crew state decides the message.
2. The message perturbs a civilisation.
3. Centuries pass for it while days pass aboard.
4. Its answer, or its silence, returns as efficiency and as grief.
5. That changes the crew state.

## 4. The civilisation model (outline)

> Developed in [civ-mirror.md](civ-mirror.md): worlds run the same simulation one level up, with scale tiers (individuals, groups, populations) and deterministic statistics. The roadmap is [r2-voyage-life.md](../../roadmap/r2-voyage-life.md).

- **Actors:** each civilisation is an actor with:
  - values and a philosophy (with strength);
  - stability, energy use, knowledge held and contact openness;
  - trust towards the ship and towards each other civilisation it knows.
- **Between visits:** a deterministic, seeded, pure simulation advances each
  civilisation by the years that pass for it. It runs the same commitments and
  trust rules the crew host uses, at the civilisation tempo. Given the same
  seed and the same transmissions, the galaxy evolves byte-identically.
- **The network:** civilisations learn of each other only through maps and
  transmissions, including the ones the ship gives away. The sim is "how does
  the contact graph evolve when I poke it?" (2025-12-02).
- **The doom clock:** what the network learns changes the odds of saving Earth
  ("What you build changes the odds", 2026-10-06). Sharing the drive can mean
  someone else reaches Earth first.
- **Words:** civilisation messages are written by the model in the
  civilisation's voice. They go through the same pipeline as crew words: a
  fixed decision, generated words, checks for leaked identifiers, and saved
  wordings. The model never decides what a civilisation does.

## 5. One memory across scales

The memory system is owned by [semantic-memory.md](semantic-memory.md)
(stapledons-design #19): stages S0–S5, determinism and replay rules, and the
cross-run library. This section covers only how that memory bridges the two
scales. Local embeddings (EmbeddingGemma 2 on Ollama, on the player's machine)
give the game a shared memory space:

- **Crew memory:** "this is like the reclaimer failure at Tau Ceti." A crew
  member recalls an earlier moment of their own when something similar happens.
- **The Archive's unreliable recall:** the Archive returns the nearest record,
  and when two records are nearly as close it can return the wrong one. Its
  unreliability then comes from how it searches, not from a script
  (`archive-voice.md`).
- **Contact history:** a civilisation's message recalls what the ship said to
  it, or to a neighbour, generations ago.
- **Déjà vu across runs:** premise-and-loop §6 says generated content carries
  over between runs and memories never do. An embedding library of generated
  civilisations, voices and lore is that carry-over: "I have seen this species
  before." Recall finds the near-match from a previous universe and offers it
  as a variation.

The crew-lab spike (2026-10-13, measured data for semantic-memory S2) ran on the 60 sampled crew moments:
- Embeddings are fast and local: 300+ in 16 s.
- They are good at **who is speaking**: a crew member's own lines sit together.
- They are good at **near repeats**: a crew member asked the same thing twice.
- They are weak at telling **what happened** apart. Two wordings of the same
  moment score no higher than the same person on a different day.

So recall needs a margin before it says "this is like when…", and should be
combined with structured matching on the event's own data (the resource that
failed, the values touched). Small margins are exactly where the Archive is
allowed to be wrong.

**Rule:** memory shapes words, never outcomes. Retrieval is an effect outside
the pure simulation, like AI words, so determinism is untouched.

## 6. What to build first

1. **Crew memory in the lab.** Variety check plus recall with a margin, in the
   reply worker process; Ollama failures stay in the worker (ailang#1798). See
   ailang-packages `design_docs/planned/crew-memory.md` (PR #133).
2. **The resource vocabulary in the lab.** Done in ailang-packages #133: the
   scenario pack uses only the eight bubble resources (`bubble-resources.md`
   §6), with every claim verdict unchanged.
3. **A contact scenario in the lab.** One civilisation as an actor alongside
   the crew: a request arrives by signal, the crew argue over whether to share
   the drive, the reply goes out in a chosen voice, and the answer returns 40
   years later. Claims check that a divided crew sends hedged messages and that
   sharing costs trust with the crew members who refused.
4. **A galaxy step spike.** A pure function that advances N civilisations by
   T years from a seed and a list of transmissions, with a claims file of
   intended behaviour ("an isolationist civilisation given a map of an
   expansionist one becomes less stable within 300 years in most seeds").

## 7. Open questions for Mark

1. Is a civilisation's tempo one step per visit, or does the galaxy run on its own clock between visits regardless of where you are?
2. Who chooses the voice of a transmission: always the captain, or can crew send unauthorised messages (a mutiny-adjacent act)?
3. Can civilisations talk to each other without you once they know of each other? This decides whether the network is a graph you edit or a society you seed.
4. Should the déjà-vu library cross runs from day one, or wait for the loop reveal?
5. Efficiency blueprints as the main reward: agreed, or should philosophies sometimes outrank them?
