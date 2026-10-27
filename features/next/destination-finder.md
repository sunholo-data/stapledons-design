# The destination finder: from a sky of stars to reasons to go

## Status

- **Status:** Proposed 2026-10-27 (Mark, attended: "adding tools to the map so we can filter down to the likely destinations - we need reasons to go to systems, most often due to suspecting they hold a civilisation we will interact with"). Not approved.
- **Priority:** P1. It is the decision surface for the whole strand.
- **Roadmap:** [r3-signals-and-contact.md](../../roadmap/r3-signals-and-contact.md), track F.
- **Builds on:**
  - [signals-and-instruments.md](signals-and-instruments.md) (the evidence ledger and its beliefs);
  - [planet-state-transitions.md](../future/planet-state-transitions.md) (transition rates, `predictStateAtArrival`, arrival resolution);
  - [journey-planning-ui.md](../future/journey-planning-ui.md) (the retired-era mock-ups: detection colours, the predicted-state bars);
  - [galaxy-map.md](../phase2-core-views/galaxy-map.md) and the shipped galaxy map (M2.6b commit dialog, star names, inspection);
  - premise-and-loop §2 (the Earth clock is a light-year budget) and D-69 (the drive spends mass);
  - [voyage-life-loop.md](voyage-life-loop.md) (where to go next, with the crew's voices; roadmap G4).

---

## Game vision alignment

| Pillar | Alignment | Rationale |
|---|---|---|
| **Choices Are Final** | ✅ Core | The finder informs the commit and never previews the outcome. It shows a distribution, not an answer. |
| **The Game Doesn't Judge** | ✅ Core | It ranks by what the player asks for (evidence, cost, distance), never by "best". |
| **Time Has Emotional Weight** | ✅ Core | Every candidate shows what it costs Earth, and how much its picture will have aged by the time you arrive. |
| **The Ship Is Home** | ✅ Supports | The crew's opinions sit beside the numbers (G4). |
| **Grounded Strangeness** | ✅ Supports | Candidates come only from measured evidence with its stated uncertainty. |
| **We Are Not Built For This** | ✅ Supports | The hindsight card shows how wrong you were, and why light could not have told you. |

---

## Problem

The galaxy map shows hundreds of thousands of stars. Within reach of the doom clock there are thousands of planets, and the dial puts life on a few hundred of them and minds on a handful. Nothing helps the player find those few, or say why one is worth decades of Earth's time. A destination needs a reason, and the reason needs to be honest about its age and its odds.

---

## Design

### 1. Candidates

A **candidate** is any world with at least one evidence record above the noise (the ledger in signals-and-instruments §7). Candidates appear on their own as the automatic survey finds them; the player never has to search.

Each candidate has:
- **the evidence:** each record, with instrument, SNR and light age;
- **the belief at last light:** probabilities per planet state, and the evidence that moved them most;
- **the prediction at arrival** (§3);
- **the cost** (§4);
- **the crew's view** (§6).

### 2. Filters and sorts

The candidate list and the map share one filter set:

| Filter | Values |
|---|---|
| Evidence kind | habitable-zone planet · biosignature · surface pigment · technosignature: beacon / leakage / industrial gas / night lights / waste heat / artificial transit · lens image · message addressed to us |
| Belief | P(life) ≥ x · P(minds) ≥ x · P(technological) ≥ x |
| Light age | the picture is ≤ n years old |
| Reach | within the doom budget · within n ship-years · within the mass the drive can spend |
| Prediction | P(still alive at arrival) ≥ x · P(remnant at arrival) ≥ x |
| Network (later) | known to a world we have contacted · on a route we gave away |

Sorts: by Earth-time cost, ship-time cost, P(minds at arrival), light age, or distance. There is no "recommended" sort (Pillar 2).

### 3. The prediction at arrival

A belief is about the past. The finder moves it forward:
- **Interval:** light age plus travel time in the galaxy frame, from the journey core's plan for the leg being considered.
- **Propagation:** the planet-state transition rates (a seeded Markov model, from the dial's lifetimes) carry each state's probability across the interval.
- **Display:** bars, as in journey-planning-ui: "picture 212 years old; arrival in 655 years: 41 % technological, 33 % remnant, 15 % transcended, 11 % other".
- The distribution is **exactly what the model believes**. The true world was drawn at world generation, and arrival reveals it.

### 4. The cost, in the three currencies

| Currency | Source | Shown as |
|---|---|---|
| **Earth time** | the journey core's galaxy-frame time for the planned leg, against the doom clock | "costs Earth 655 years of the 16,000 left; 15,345 after" |
| **Ship time** | the same plan's proper time | "4.1 years of your 100" |
| **Mass** | the drive's mass-energy for the leg (D-69) | "3,100 kg of free mass" |

Map overlays carry the same numbers:
- **the doom-budget sphere**: where a round trip still leaves Earth time;
- **the ship-time sphere**;
- **light-age shading**: how stale each candidate's picture is.

### 5. The reason card

Selecting a candidate opens a card that states the case in plain words, from the record:

> **HD 40307 g, 42 ly.** Super-Earth in the habitable zone of a K dwarf.
> O₂ and CH₄ out of equilibrium (SNR 7). A red edge at 0.71 µm. Light 42 years old.
> Belief then: life 88 %, minds 6 %.
> At arrival (44 years from now, so the picture will be 86 years out of date): life 86 %, minds 7 %.
> Costs Earth 44 years · you 4 years · 1,200 kg.

The Archive can offer an interpretation in its own voice. It may be wrong, as everything the Archive says may be.

### 6. The crew's view

At the navigation station the crew's opinions appear beside the card (roadmap G4), from live crew state: the biologist wants the red edge; the engineer counts the mass; the homesick want the short leg. A destination the crew oppose can still be chosen; ordering it spends trust, as all orders do.

### 7. Hindsight

At arrival a hindsight card compares the prediction with the world found:
- what the light said, and how old it was;
- what was predicted;
- what is there, and what happened in between (from the world's own history, as far as the ship can learn it).

This is the epistemic gap made tangible (design-decisions 2025-12-02). Over a run, these cards record how often the player's bets came good.

### 8. Earth's beams: the home anchor on the map

(Mark, 2026-10-27: "we could then have a map graphic showing which direction earth is sending its signals as far as we know, and where they will point, I guess an important anchor for the ship.")

Earth's link runs in straight lines towards plans Earth has heard (signals-and-instruments §3.1). The map draws that link as the ship believes it to be:

| Element | What it shows | Drawn as |
|---|---|---|
| **Beams Earth should be sending** | Every plan whose message has had time to reach Earth: a cone from Sol towards that port | A solid cone, with Earth's light-front moving along it at c |
| **Beams Earth cannot send yet** | Plans whose message is still in flight to Earth | A dashed cone, plus the message's own light-front crawling back towards Sol, with the date it arrives |
| **Abandoned beams** | Plans the ship has left | A fading cone: that light is aimed at empty space |
| **Coverage along the line** | A beam covers its whole line from Sol, before and beyond its port | The cone runs from Sol past the port |
| **The link window at each port** | From the earliest arrival of Earth's light there to the ship's planned departure | A bar on the port: green when it overlaps the stay, with "wait n years to hear Earth" when it does not (the triangle excess) |
| **Silence** | No new Earth light since date X | The last known light-front, labelled with its age |

Rules:
- **It is a belief, not the truth.** The overlay shows what Earth *should* be doing given what it was told and when. If Earth has stopped, the ship finds out only when the silence arrives, d years later.
- **Changing course shows its cost before the commit.** Selecting a new destination previews the triangle excess at the new port, and which beams the change abandons.
- **The ship's own messages home** appear as light-fronts travelling towards Sol, so the player can see when Earth will hear what.

---

## Acceptance criteria (for the implementation design that follows)

1. Every candidate traces to ledger records. A test fails if a world with no record above the noise appears.
2. Filters and sorts are pure functions of the ledger and the plan. The same inputs give the same list.
3. For analytic test cases (two and three states, constant rates), the prediction at arrival matches the closed-form Markov solution to 1e-12.
4. The Earth-time and ship-time costs equal the journey core's plan for the same leg, exactly.
5. No candidate shows a "recommended" label or a single predicted state.
6. The hindsight card's "predicted" column equals the distribution shown at commit time, byte for byte.
7. Earth's beams: each cone exists only for a plan whose message has reached Earth by light travel time; the link window at each port equals the triangle excess d_A + d_B − |AB| for re-plans, and zero for a port on an existing beam's line (beyond its target or between it and Sol).

## Open questions for Mark

1. **How many candidates at once?** Proposed: show everything above the noise, filtered down to the ten nearest by Earth-time cost by default.
2. **Should the prediction use the dial's true rates or the ship's estimate of them?** Recommended: the ship's estimate. The ship does not know this universe's luck until the Archive works it out (premise §5), so early predictions are pessimistic. When the Archive reveals the luck number, the predictions shift.
3. **Messages addressed to us** (a beacon aimed at the ship's route, or a reply). Pin them to the top of the list? Recommended: yes, as their own filter, since they are the strongest reason to go.
