# Roadmap R3: Signals and Contact (the outward strand)

**Status:** Proposed 2026-10-27 (Mark, attended: "can we get some good design docs up and a roadmap for this part of the game?"). Not approved.
**Scope:** everything between the ship and other worlds:
- what reaches the ship, and what it can measure;
- how the player finds reasons to go somewhere;
- what is out there, set by the anthropic dial;
- what the ship can do at a world, and what comes back;
- how living worlds and alien messages look and sound.

**Releases:** begins once R1 lands, alongside R2 ([r2-voyage-life.md](r2-voyage-life.md)). R2 builds the ship's society; R3 points it outward. R2's track C (the civilisation simulation, in the lab) feeds this roadmap's contact work.

## The idea in one paragraph

Each run is a universe the loop chose because it is unusually kind to minds (premise §5). The dial sets how kind; physics sets everything else. The ship is the only interstellar vessel anywhere (D-71), a passive observatory with a voice: only light crosses its wall. It finds worlds from their old light, through spectra that hint at life and at industry, with honest odds and honest false positives. It commits centuries of Earth's time to go and see. On arrival, the world has moved on. The ship can watch a biosphere it can never touch, reveal itself to minds that will make a myth of it, or decode a civilisation's primer and trade the only thing that crosses: information. Every map, blueprint and philosophy it gives away perturbs a network that grows at light speed, may help save Earth, and may put someone else at Earth first. Meanwhile Earth's news reaches the ship at a rate set by its speed: frozen while it flies away, a torrent while it flies home.

## Design documents

| Doc | What it decides |
|---|---|
| [life-and-intelligence-parameters.md](../features/next/life-and-intelligence-parameters.md) | The realistic baseline, the dial as offsets from it, calibration by simulation, encounter kinds |
| [world-gen-settings.md](../features/future/world-gen-settings.md) | The New Voyage presets and sliders (to be revised to the offsets model) |
| [galaxy-beyond-measurement.md](../features/next/galaxy-beyond-measurement.md) | Which stars and planets are measured, which synthesised |
| [signals-and-instruments.md](../features/next/signals-and-instruments.md) | The light cone; Earth's signal and the Earth rate; the instrument suite; biosignatures and technosignatures with false positives; the gravitational-lens look; the evidence ledger |
| [destination-finder.md](../features/next/destination-finder.md) | Candidates, filters, the prediction at arrival, the three costs, the reason card, hindsight |
| [contact-console.md](../features/next/contact-console.md) | The comms station; light-lag regimes; the contact ladder from microbes to civilisations; primers and decoding as crew work; what can be exchanged; alien values |
| [living-worlds.md](../features/next/living-worlds.md) | Living-world layers on the planet renderer; lens images; species presentation; generated glyphs and sound |
| [civ-mirror.md](../features/next/civ-mirror.md), [inner-shapes-outer.md](../features/next/inner-shapes-outer.md) | Worlds run the same simulation one level up; how crew and worlds shape each other |
| [planet-state-transitions.md](../features/future/planet-state-transitions.md) | The planet-state machine, beliefs and arrival resolution |

## Who does what

| Agent | Owns here |
|---|---|
| **Integration agent** (stapledons-godot) | The game side of every track: sim modules, protocol, consoles, map, renderer layers |
| **Packages** (ailang-packages) | `sunholo/observe` (new: photon budgets, link budgets, the lens, false-positive tables); additions to `sunholo/relativity` and `sunholo/celestial` |
| **Crew-lab agent** | Alien value spaces and world claims in the lab (track C here, after R2's C1–C4) |
| **Scenario agent** | Contact scenarios in the lab: crew arguments over a message, a world's reply decades later |
| **The mission loop** | Headless-verifiable items only (marked *loop* below). Anything visible, any art direction and any publish stays attended. |
| **Mark** | Rulings (ledger), canon, art direction, reviews |

## Tracks and milestones

### Track W: worlds and the dial

| ID | Milestone | Exit criteria | Depends |
|---|---|---|---|
| W1 *loop* | The baseline table (cited) and the dial as offsets; the calibration Monte Carlo in AILANG | Reproduces life-and-intelligence-parameters' tables at zero offset; VM = interpreter; presets hit their encounter targets for every reference strategy | — |
| W2 *loop* | World population: every system within the play area gets planets and a seeded history (planet states over galaxy time) from (seed, dial). Measured planets are kept and labelled. | Same seed gives the same galaxy; measured planets unchanged; a census of encounter kinds per preset matches W1 | W1 |
| W3 | The New Voyage screen: presets by name, the dial hidden (premise §5) | Presets choose offsets; nothing reveals the luck number | W1 |
| W4 | The Archive reveals the luck number as it works out the loop | A scripted reveal plus New Game+ visibility | W3, R2 Archive |

### Track S: signals and instruments

| ID | Milestone | Exit criteria | Depends |
|---|---|---|---|
| S1 *loop* | The light cone `seen()`, and Earth's signal on every leg: the news epoch generalised, the **Earth rate** D as a live value in the sim | signals-and-instruments ACs 1–2; M4.4's α Cen epoch unchanged | — |
| S2 *loop* | `sunholo/observe` 0.1: photon and link budgets, SNR, lens focal distance and gain, false-positive likelihoods; spec check values added first | Package tests and `pkg quality` clean; the §4.4 focal table to 0.1 % | — |
| S3 *loop* | The census, spectroscopy and technosignature instruments, writing the evidence ledger and beliefs | ACs 4–6; a desiccated M-dwarf O₂ world never reads as life | S2, W2 |
| S4 | The lens look: the trip to the focal line, months of integration, the lens image | A GPU golden of the lens product against the CPU model; renders reviewed (gate 2) | S3, V2 |
| S5 | The homecoming torrent on the console | Earth's compressed history plays at D on a return leg; reviewed in play | S1, K1 |
| S6 | Lens relays and Earth's own lens observatory: Earth builds relays towards the stars on the itinerary it was sent, and sends candidates it finds; the ship can listen from a destination's focal line | Link budgets with and without a relay match `sunholo/observe`; Earth's relays exist only for itineraries Earth received | S2, S4, Earth as a world (civ-mirror Q2) |

### Track F: the destination finder

| ID | Milestone | Exit criteria | Depends |
|---|---|---|---|
| F1 | Candidates and filters on the galaxy map and in a list | destination-finder ACs 1–2, 5 | S3 |
| F2 *loop* | The prediction at arrival and the three costs | ACs 3–4 | F1 |
| F3 | The reason card, with the Archive's interpretation | Reviewed in play | F2 |
| F4 | The crew's voices on candidates (= R2 G4) | Opinions from live crew state | F3, R2 G1 |
| F5 | The hindsight card at arrival | AC 6; reviewed in play | F2 |
| F6 | Earth's beams on the map: beams Earth should be sending, beams it cannot send yet, abandoned beams, link windows per port, silence | destination-finder AC 7; reviewed in play | S1, F1 |

### Track C: civilisations (continues R2 track C)

| ID | Milestone | Exit criteria | Depends |
|---|---|---|---|
| C5 | Worlds advance between visits in the game; light-speed messages (moved here from R2) | The Worlds pane shows last-seen state plus arrived light | R2 C4, S1 |
| C6 *loop* | Alien value spaces in the lab: per-species axes mapped to engine parameters | Claims for three species pass over seeds | R2 C2 |
| C7 *loop* | Non-technological worlds: an ecosystem response model (animals and light) and a pre-technological reaction model (the ship revealed) | Lab claims: e.g. "a revealed pre-tech world forms a lasting belief about the ship in most seeds, and the belief's content follows its values" | C6 |

### Track K: the contact console

| ID | Milestone | Exit criteria | Depends |
|---|---|---|---|
| K1 | The comms station: Listen (Earth first, with the Earth rate) and the Exchange log; Earth's link fails only by the physical causes in signals-and-instruments §3.1 (Earth stops, aims off-plan, band out of reach, distance, medium) | Earth's signal shown with Doppler, light age and rate on every leg; going off-plan loses exactly the photons already aimed at the plan | S1 |
| K2 | Primers and decoding as crew work; translation confidence | contact-console AC 3; crew events for contested readings | K1, C5, V3 |
| K3 | Compose, the voice and consent | Messages in a crew voice; refusal and orders work as aboard | K2, R2 G1 |
| K4 | Exchange: blueprints, maps, philosophies, warnings, favours, wired to the civ simulation | ACs 1–2, 5–6 | K3, C6 |
| K5 | Emergent travel: worlds build **sub-light** drives and **AI probes** from what we give them (agreed 2026-10-27), and bubbles only with our design and a reachable spinning black hole (decision 6); their ships and probes can reach Earth inside the doom window | A world's launch traces to a blueprint the ship gave; arrival times follow launch and speed | K4, decision 6 |

### Track V: living worlds and alien voices

| ID | Milestone | Exit criteria | Depends |
|---|---|---|---|
| V1 | Living-world layers on the planet renderer | living-worlds ACs 1–2; goldens; renders reviewed (gate 2) | W2 |
| V2 | The lens image as an instrument product | AC 3 | S2 |
| V3 *loop* (generation) | Communication systems: glyph grammars and sonification | AC 4; the sound reviewed attended | — |
| V4 | Species presentation: portraits and expressions, procedural then generated | Art brief; reviewed | V3 |

## The first target: the First Contact slice

The smallest version that is already the game:

1. A seeded universe at the default preset.
2. The finder narrows the sky to two candidates within ~50 ly: a biosphere (O₂ and CH₄, a red edge) and a technological world (an industrial-gas hint and a weak beacon).
3. The player commits; Earth's news slows to a trickle on the way.
4. At the biosphere: hindsight, the living-world render, wonder aboard.
5. At the technological world: the primer is decoded over weeks of crew work, one exchange is made in a chosen voice, and the reply arrives later as a ship event.

It needs W1–W2, S1–S3 (the subset for those two worlds), F1–F3 and F5, C5 for one world, C6 for one species, K1–K4 for one world, and V1 and V3 for those worlds. Mark reviews the slice in play before the tracks widen.

## Order of work (next)

1. **S1, K1 and F6** (integration). The light cone, Earth's signal with the live Earth rate, and Earth's beams on the map. They use physics the game already has, and make home the ship's anchor from day one.
2. **W1 then W2** (*loop*). Everything else needs a populated galaxy.
3. **S2 then S3** (packages, then integration). The evidence ledger.
4. **F1, F2, F3, F5.** Reasons to go.
5. **C6 and C7** in the lab (crew-lab agent), alongside R2's C1–C4.
6. **K2, K3, K4 and V3.** Contact.
7. **V1, V2, S4.** Seeing living worlds, and the lens.
8. **The First Contact slice review** (Mark).
9. Later: K5, S5, V4, W3 and W4, and the network in the legacy screens.

## Gates

- Every visual of SR or GR (Doppler on the Listen pane, observing at speed, the lens image) gets spec check values, a GPU golden and reviewed renders before it merges (definition of done, gate 2).
- New maths goes into packages first (gate 3): `sunholo/observe`, `sunholo/relativity`, `sunholo/celestial`.
- World and contact simulation stays pure and deterministic (gates 4 and 6); model output writes words only (D-20).

## Decisions needed

Collected from the design documents, each with a recommendation in its doc:

| # | Question | Doc | Recommendation |
|---|---|---|---|
| 1 | Deliberate observation (lens looks, deep stares, listening, paid in time) alongside "No Survey Budget"? | signals §6 | Yes |
| 2 | The ship's apertures | signals Q2 | 10 m class; larger as blueprints from worlds |
| 3 | Reception at cruise | signals Q3 | Low γ only, Doppler shown honestly |
| 4 | The homecoming torrent as a console feature | signals Q4 | Yes |
| 5 | Predictions use the ship's estimate of the dial, not the truth | finder Q2 | Yes, until the Archive's reveal |
| 6 | ~~Can worlds ever build a bubble?~~ | console Q1 | **Decided 2026-10-27: (c).** The design can be passed on; forming a bubble needs a spinning black hole's ergosphere |
| 7 | Unauthorised transmissions | console Q2 | Yes, mutiny-adjacent |
| 8 | Telling worlds about Earth's doom | console Q3 | Allowed from the first message |
| 9 | Encounter targets for the default preset | life-and-intelligence Q1 | 20–50 biospheres detected, 3–8 visited, 2–5 remnants, 1–3 signals, 0–2 living contacts |
| 10 | M-dwarf habitability on by default | life-and-intelligence Q3 | On, labelled "habitability debated" |
| 11 | Art direction for living worlds | living-worlds Q1 | Photometric planets, comic-style console art |

## Risks

| Risk | Mitigation |
|---|---|
| Detection reach makes candidates too rare or too common | Reach is computed, so tune the dial and the ship's apertures (decision 2), never the physics |
| The finder becomes an optimiser that tells you where to go | No "recommended" sort; distributions, never single answers (finder AC 5) |
| Decoding feels like a progress bar | It is crew work with arguments and wrong readings, and letters decades apart |
| Alien values collapse into OCEAN with new names | Each species declares its own axes and a mapping; lab claims must show behaviour OCEAN cannot (C6) |
| Generated content breaks determinism | Generation sits outside the pure sim; outcomes come from seeds; words and art are cached and labelled |
| The scope swallows R2 | R3 starts with S1 and K1, which are small and use existing physics. The lab work rides on R2's track C. |
