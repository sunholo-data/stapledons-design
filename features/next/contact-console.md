# The contact console: talking to worlds across light-lag

## Status

- **Status:** Proposed 2026-10-27 (Mark, attended: "we communicate maybe with just bacteria or animals or a species etc. and we use our social dynamics system and resources to communicate - need a new pane for that I guess for ship to planet communication - and setup possibly trade of information and resources with other civs all with the aim to help our game goals. and we have made the interior social dynamics extendable to also simulate these planets interacting, but with alien values, perhaps"; "fun with generating alien communication"; "we are going to be in a universe where we are the first fraction of light speed ship, but perhaps by game end others learn it from us"). Not approved.
- **Priority:** P1 for design, P2 for build (after the civilisation lab, R2 track C).
- **Roadmap:** [r3-signals-and-contact.md](../../roadmap/r3-signals-and-contact.md), track K.
- **Builds on:**
  - [inner-shapes-outer.md](inner-shapes-outer.md) (who speaks; efficiencies and minds come back);
  - [civ-mirror.md](civ-mirror.md) (worlds run the same simulation one level up; scale tiers);
  - [civilization-trade.md](../future/civilization-trade.md) (civilisation data, philosophies, trade value; retired-era UI);
  - design-decisions 2025-12-02: Perturbation Operator, Launch Order = Arrival Order, Alien Biosphere: Science Not Tropes, Wanderer Mythology, Earth Contact: Emergent, Fermi Answer: Temporal Fragmentation;
  - D-11 (only light crosses), D-20 (the model writes words; decisions are authored and seeded), D-71 (the first interstellar ship; worlds talk once they know of each other);
  - [bubble-resources.md](bubble-resources.md) (`signal`, `power`, `compute`).

---

## Game vision alignment

| Pillar | Alignment | Rationale |
|---|---|---|
| **Choices Are Final** | ✅ Core | A transmission cannot be recalled. A map given away propagates through a network you will not see again for centuries. |
| **The Game Doesn't Judge** | ✅ Core | Revealing yourself to a pre-technological species, or giving a warlike world a map, is shown as what followed, never scored. |
| **Time Has Emotional Weight** | ✅ Core | A conversation across 40 light-years is one letter per 80 years. The people who asked the question are not the ones who hear the answer. |
| **The Ship Is Home** | ✅ Core | The crew decode, argue, consent or refuse. Every message goes out in a crew member's voice. |
| **Grounded Strangeness** | ✅ Core | Contact is light, primers and decoding, with alien values that are not human values. |
| **We Are Not Built For This** | ✅ Core | The ship can watch a living world forever and never touch it. Understanding is partial, and mistakes are made in a language nobody fully knows. |

---

## Problem

The game has worlds with life and minds (the dial), ways to see them (signals and instruments) and reasons to go (the finder). It has no way to *act* on a world: no surface to land on (D-11), no handshake, no trade window that makes sense when only light crosses. The retired civilization-trade screen assumed drag-and-drop goods. What can a ship that can only send light actually do at a world, and what comes back?

---

## Design

### 1. The console

A comms station on the bridge, beside the navigation station, with five panes:

| Pane | What it does |
|---|---|
| **Listen** | What is arriving: beacons, leakage, replies, Earth (signals-and-instruments §3), each with Doppler, light age and SNR |
| **Decode** | Work in progress on each world's signals: the primer, the vocabulary found, translation confidence, who aboard is working on it |
| **Compose** | Write a message: content blocks (§4), the voice (§5), the cost in `signal`, `power` and crew time |
| **Exchange log** | Everything sent and received, per world, with the dates in both frames: "sent ship-year 31 (galaxy year 1,212); arrives galaxy year 1,254; reply earliest galaxy year 1,296" |
| **Worlds** | Each world as last seen, plus what light has arrived since (civ-mirror C5) |

### 2. Two regimes, set by light-lag

| Regime | Light-lag | Feels like |
|---|---|---|
| **In system** (the ship stopped at the world) | seconds to hours | A conversation with pauses. The crew wait at the console; the world answers in its own time, by its own decision rules. |
| **Interstellar** | years to centuries | Letters. You send and leave; the answer comes back as a ship event wherever you are, decades later, or never. |
| **Remnant** | — | No reply is possible. You read what a dead civilisation left in light: a beacon still running, an archive broadcast on a loop. |

### 3. The contact ladder: what "contact" means for each kind of world

| What is there | What the ship can do | What it costs | What comes back | What happens to them |
|---|---|---|---|---|
| **Microbial life** | Observe: deep spectra, the lens, seasonal monitoring from orbit | Time at the world, `compute` | Biochemistry: pigments, catalysts, membranes, as **efficiency blueprints** (light to food, water recovery). Wonder, as a crew event. | Nothing. The ship cannot contaminate what it cannot touch. |
| **Complex life** (animals, ecosystems) | Observe behaviour at orbit and lens resolution. **Signal with light** to light-sensitive species (patterns, illumination). | Time; `power` for signalling | Ecology, behaviour, sometimes a response to the light. Morale, grief, argument. | Their ecosystem simulation registers the light as a new stimulus. Usually nothing; sometimes a behaviour shifts. |
| **Pre-technological minds** | Watch, or **reveal**: a new light in their sky, patterns they can see | `power`; trust aboard if the crew disagree | Their reaction, seen over years at the world or on return | Their civilisation simulation takes the event: a myth, a fear, a science, a schism (Wanderer Mythology: no canonical reaction). |
| **A technological civilisation** | The primer, then conversation and **exchange** (§6) | `signal`, `power`, crew decoding time, and what you give away | Replies, blueprints, maps, philosophies, favours (§6), each a ship event | The full civ-mirror simulation: factions appraise every message by their values. |
| **A remnant** | Decode its archive or beacon | `compute`, crew time | Knowledge without dialogue, and the story of how it ended | Nothing; it is over. |

The rungs follow the planet-state machine. A world can climb or fall between your visits, since its clock keeps running (D-71).

### 4. First words: primers and decoding

There is no shared language. Real interstellar-message work is the model (Freudenthal's Lincos, 1960; the Arecibo message, 1974; the Dutil–Dumas Cosmic Call messages, 1999 and 2003): **mathematics first, then physics, then everything else.**

1. **Notice.** A signal is artificial when it is narrowband, pulsed, or structured beyond any natural source.
2. **Primer.** Counting, primes, arithmetic, then shared physics: the hydrogen line, the elements, the host star's spectrum.
3. **Vocabulary.** Symbols bound to meanings by context, one at a time.
4. **Content.** Messages the ship can mostly read, with a confidence per sentence.

Decoding is **crew work**. It needs the scientist and the linguist (crew roles), `compute`, and time. Progress raises translation confidence. Disagreements are crew events: two readings of the same sentence, argued in character. Wrong readings are possible, and the Archive's unreliable recall can make them worse.

The ship's own messages use the same ladder in reverse. A world that has not decoded the primer reads nothing but "artificial, structured, from there".

### 5. The voice, and consent

From inner-shapes-outer §2:
- Every message goes out in a crew member's voice. Their traits colour the wording, and the receiving factions appraise both content and voice.
- **Sharing is a decision the crew can refuse.** A captain who orders it spends trust. A divided crew sends a hedged message, and the world can read the hedge.
- Whether a crew member can send an unauthorised message is an open question (Q2).

### 6. What can be exchanged

Nothing material crosses either way, so **every exchange is information** (Proto-Tech via Information Only):

| Give or receive | What it is | Its effect |
|---|---|---|
| **Blueprints** | Designs and science | Raise an efficiency in the receiver's resource graph. For the ship: food, water, heat, **instruments** (seeing further), compute. For them: whatever they lack. |
| **Maps** | Where other worlds are, and what we saw there | Their network grows (Perturbation Operator). They may contact others, or send sub-light ships and AI probes there, and may reach Earth before the doom if they launch early enough (Launch Order, under review 2026-10-27). |
| **Philosophies** | Values, argued | Shift the factions that accept them; schism with those that reject them |
| **Warnings** | Earth's doom; a dangerous neighbour | They may help, exploit it, or ignore it, by their values |
| **Favours** | A commitment across light-years: relay our messages, beam to Earth, prepare for our return, act at Earth when they can | Kept or broken generations later, by people who never met us. Commitments use the crew engine's rules at the civilisation tempo. |
| **Their knowledge toward saving Earth** | Stellar engineering, GR physics, evacuation logistics, another way of thinking about the problem | The premise §3 salvation paths are assembled from these, across runs |

"Trading resources" therefore means **trading what resources can do**: an efficiency that saves mass, or a world that spends its own energy on your behalf. The galaxy can never give the ship more atoms, only teach it to waste fewer (inner-shapes-outer §3).

### 7. Their side: alien values in the same engine

Each world runs the civ-mirror simulation. What is new here is **alien values**:
- **The engine stays general.** A crew_core actor needs a small set of behavioural parameters: how it weighs requests, how fast trust moves, how it keeps commitments, its appetite for risk, how decisions aggregate.
- **Each species declares its own value axes,** not OCEAN, plus a published mapping from those axes to the engine's parameters. Examples:
  - a long-lived species whose decisions take generations (slow trust, very durable commitments);
  - a collective that answers only in consensus (every message is appraised by the whole);
  - a species for whom silence is courtesy (replies are rare and meaningful);
  - an expansionist culture that reads any map as an invitation.
- **Claims first, in the lab:** "a consensus species given a divisive philosophy takes ten times longer to reply, in most seeds" is checked over seeds before any world reaches the game, as the crew lab does today.
- **Words are generated; decisions are not** (D-20). A world's replies are written by the model in its voice, from a decision the simulation already made. Generated species, glyphs and voices join the cross-run content library (premise §6): déjà vu across universes.

### 8. What the console is for, in the run

Every exchange serves the game's goals:
- **The ship:** efficiencies that stretch the closed ledger (D-69), and instruments that see further.
- **Earth:** knowledge toward the salvation paths, allies who may act at Earth, relays that carry news home.
- **The galaxy:** a contact network that exists because the ship made it (D-71), shown in the legacy screens.
- **The crew:** wonder, grief, argument, faith and doubt, all as crew events.

---

## Acceptance criteria (for the implementation design that follows)

1. Every message's send, arrival and earliest-reply dates follow from light-lag and both frames' clocks, and match the journey core to the tick.
2. Nothing material appears in any exchange type. A test enumerates exchange kinds and fails on a mass transfer.
3. Decoding progress is a pure function of (signal, crew assignment, `compute`, time, seed), and translation confidence is shown for every decoded sentence.
4. Each species' value axes map to engine parameters through a declared table. Lab claims for at least three species pass over seeds before they reach the game.
5. World decisions are seeded and authored; model output never changes an outcome (the existing words pipeline check).
6. Replay: the same seed and the same transmissions give byte-identical world histories.

## Open questions for Mark

1. **Can worlds ever build a bubble?** Sub-light drives and AI probes are agreed (Mark, 2026-10-27). The bubble is **reopened** (Mark, attended 2026-10-27: "im not decided about the bubble technology. i guess it does make sense as humans invented it in the first place and so we should know how to pass it on. its not divine"). The premise (§1) currently says no one can replicate it. Options:
   - **(a) Unique:** keep the premise; only the ship ever has a bubble.
   - **(b) Transferable:** humans designed it, so it can be passed on. Earth could build more too, which changes the doom (evacuation becomes easy), and uplifted worlds reach Earth at any γ.
   - **(c) Engineering plus a rare condition:** the design is human and can be passed on, but forming a stable bubble needs a real extreme environment, such as a spinning black hole's ergosphere, where the ship itself emerged. Not divine, just rare. The crossing erased the crew's memories, so the Archive must rebuild the design over the run, unreliably. The rogue black hole approaching Earth is itself an ergosphere, so the doom may carry the means of escape.

   Recommended: (c). Until Mark rules, the docs assume (a).
2. **Unauthorised transmissions.** Can a crew member send a message the captain forbade? Recommended: yes, as a mutiny-adjacent act with its own consequences.
3. **Telling worlds about Earth's doom.** Allowed from the first message? Recommended: yes. Whether to tell, and whom, is one of the run's biggest choices.
4. **How long decoding takes.** Proposed: a primer takes weeks of crew work at a stop, and a working vocabulary takes months; long gaps between letters let both sides decode more.
5. **Light signalling to animals and pre-technological minds.** In scope for the first contact slice, or later? Recommended: watching first, revealing in the slice (it is the strongest pre-tech choice), animal signalling later.
