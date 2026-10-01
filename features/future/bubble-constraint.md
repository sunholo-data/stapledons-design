# Bubble Constraint System

## Status
- **Status:** Planned
- **Sprint:** Vision Integration - Sprint 2
- **Priority:** P1 (Defines core game physics)
- **Source:** [Interview: Game Loop Origin](../../vision/interview-log.md#2025-12-06-session-game-loop-origin--bubble-constraint)
- **Physics:** [physics/higgs-bubble.md](../../physics/higgs-bubble.md) (normative; ledger D-11, 2026-10-01)

## Game Vision Alignment

| Pillar | Score | Notes |
|--------|-------|-------|
| Choices Are Final | ✅ Strong | Can't undo what you brought/didn't bring |
| Game Doesn't Judge | ⚪ N/A | Physics constraint, not moral |
| Time Has Emotional Weight | ⚪ N/A | Enables isolation |
| Ship Is Home | ✅ Strong | Defines the boundary of "home" |
| Grounded Strangeness | ✅ Strong | Hard sci-fi constraint |
| We Are Not Built For This | ✅ Strong | Permanent separation from universe |

## Feature Overview

The Higgs-bubble creates an **absolute boundary** between the ship and the universe:

> **Only information crosses the boundary. Mass cannot.**

The wall is the game's one admitted hand-wave (D-11). It blocks every massive
particle in both directions and is transparent to light and neutrinos. **No
massive particle crosses at all**, so the mass budget is closed. Everything that
follows from this is exact physics, in
[physics/higgs-bubble.md](../../physics/higgs-bubble.md).

This single constraint shapes the entire game:
- You are "memetic travelers" - carrying ideas, not cargo
- Alien tech is absorbed as blueprints, fabricated internally
- No physical rescue or supply is possible
- The bubble is self-contained or it dies

## What Crosses the Boundary

### ✅ CAN Cross (Inward)

| Type | Mechanism | Gameplay Impact |
|------|-----------|-----------------|
| **Light/EM** | Transparent to visible spectrum | See the universe |
| **Radio signals** | Low-energy EM passes | Communication with civs |
| **Data/blueprints** | Encoded in light | Proto-tech acquisition |
| **Neutrinos** | Pass by rule (they barely interact with anything) | None; they pass through the ship anyway |
| **Philosophical frameworks** | Ideas, not matter | Unlock new interpretations |

### ❌ CANNOT Cross (Inward)

| Type | Explanation | Gameplay Impact |
|------|-------------|-----------------|
| **Physical objects** | Higgs field blocks mass | No cargo, no gifts, no rescue |
| **People** | Mass cannot enter | Starting crew is all you have |
| **Alien artifacts** | Physical tech cannot enter | Must reverse-engineer from specs |
| **Resources** | No material resupply | Finite mass budget |
| **ISM gas, stellar wind, cosmic rays** | Massive particles; the wall is an elastic mirror | Drag and a faint glow, never mass gain (see below) |

### ⬆️ CAN Cross (Outward)

| Type | Mechanism | Gameplay Impact |
|------|-----------|-----------------|
| **Light/signals** | Transparent both ways | Broadcast to civs |
| **Data transmission** | EM radiation | Share your archives |
| **Drive light** | The photon drive (the only thing that pushes) | Boost, brake and drag cost energy |

### ❌ CANNOT Leave

| Type | Explanation | Gameplay Impact |
|------|-------------|-----------------|
| **Crew members** | Permanent containment | No EVA, no away missions |
| **Physical samples** | Mass trapped inside | Can't send probes |

## Proto-Tech Acquisition

When encountering alien technology:

1. **Receive specifications** - Blueprints, equations, principles cross as data
2. **Analyze with Archive** - AI helps interpret alien concepts
3. **Fabricate internally** - Use existing mass to build implementation
4. **Mass cost applied** - Each upgrade costs finite mass budget

```
Alien Civ → Data Transmission → Archive Analysis → Internal Fabrication → Working Tech
           (crosses boundary)                      (uses internal mass)
```

## The Interstellar Medium: an Elastic Mirror

There is no trace-hydrogen absorption (removed 2026-10-01, D-11). At γ 707 the
ISM's hydrogen arrives in the ship frame as a 663.5 GeV proton beam (HB-59), so
a wall that let it through would be lethal.

Instead, the wall is an **elastic mirror** for massive particles. A particle
meeting a barrier it cannot climb reflects elastically, so the wall feels drag
and is not heated:
- **Drag:** F = n γ²β² m_p c² A on the sphere. Holding cruise speed costs drive
  energy n γβ m_p c² A d per trip, which grows as about γ·d. Sol → α Cen at
  0.99c costs 1.37 × 10¹⁷ J, about 1.5 kg of mass-energy (HB-49, HB-53, HB-54).
- **Plume:** the reflected protons stream ahead as an ultra-relativistic
  forward plume.
- **Glow:** a small inelastic fraction ε of impacts becomes faint light at the
  wall. This is the boundary glow, brightest at the bow.

In R1 the simulation reports drag energy and glow as readouts, from a constant
Local Bubble density (0.1 cm⁻³).

## Design Decisions

From [design-decisions.md](../../vision/design-decisions.md):

| Decision | Summary |
|----------|---------|
| Proto-Tech via Information | Alien tech absorbed as blueprints, built internally |
| Finite Mass Budget | Competition between population and upgrades |
| ~~Slow Mass Absorption~~ | Superseded 2026-10-01: no mass crosses |
| Radiation Shielding Automatic | In tension with D-11 (the wall passes all light); open |
| Higgs Bubble Model (D-11, 2026-10-01) | One hand-wave, three properties; everything else exact |

## Boundary Physics

### Light of Every Energy Passes

> The earlier energy-dependent table (X-rays filtered, gamma blocked) conflicts
> with D-11, which makes the wall transparent to light of every energy.
> Resolved by D-15 (2026-10-01): the crew is protected by real glazing and hull
> inside the bubble, which absorb UV and X-rays; the wall filters nothing.

- **Massive radiation is blocked:** cosmic rays, stellar-wind protons and ISM
  gas.
- **Photons all pass**, which is why the crew sees exactly the relativistic sky
  (relativity spec §2). At very high γ the forward sky is blueshifted into soft
  X-rays: about 16 W/m² from starlight at γ 707, an approximate figure. Any
  glazing absorbs these within micrometres, so the ship's own structure is the
  shield (`higgs-bubble.md` §7).

### Mass Threshold

The boundary has an effective "particle size" filter:
- **Photons:** Always pass (massless)
- **Neutrinos:** Pass. They have tiny masses, so letting them through is part of the rule, not a consequence
- **Electrons:** Blocked (massive particles)
- **Atoms:** Blocked, always (no trace infiltration)
- **Molecules:** Blocked
- **Macroscopic objects:** Absolutely blocked

## Narrative Implications

### The Memetic Traveler Identity

You don't carry cargo - you carry:
- Ideas from civilizations
- Philosophies that reframe understanding
- Scientific principles that enable new technology
- Art, music, stories (digitized)
- Memories (in Archive and crew minds)

This makes every encounter about **exchange of meaning**, not trade of goods.

### Permanent Isolation

Once inside the bubble:
- You can never physically touch the universe again
- EVA is impossible
- If the ship breaks, no one can help
- You are truly alone, together

This serves Pillar 6: **We Are Not Built For This**

### First Contact Dynamics

When meeting aliens:
- They cannot board your ship
- You cannot board theirs
- All interaction is mediated by signals
- Trust must be built without physical presence

## Edge Cases

### Q: What about the spire?

The spire predates the bubble and may not obey the same rules. This is part of the mystery.

### Q: Can crew members leave and return?

No. No mass crosses in either direction. Once inside, you stay inside.

### Q: What about births?

Babies are born inside the bubble, from mass already inside. Population growth uses internal mass.

### Q: What about death?

Bodies are recycled. Mass is conserved. This is both practical and thematically significant.

## AILANG Types

```ailang
type BoundaryTransfer =
    | LightSignal(string)           -- EM data
    | DataPacket(bytes)             -- Encoded information
    | BlockedMass(string)           -- Rejected with reason

type ImpactSource =                -- reflected, never absorbed (D-11)
    | InterstellarMedium
    | StellarWind(star_id: int)
    | Nebula(density: float)

type TransferResult = {
    success: bool,
    type: BoundaryTransfer,
    mass_delta: float,
    archive_data: Option(string)
}
```

## Engine Integration

### Visual Representation
- Subtle shimmer at bubble boundary
- Incoming signals show as light touches
- Blocked objects show rejection effect (for clarity)

### Audio
- Muffled external sounds (everything is mediated)
- Signal reception sounds
- Glow hum (when near dense regions)

### UI
- Mass budget display (see mass-budget.md)
- Drag energy and glow readout (R1: readout only)
- Signal log for received data

## Testing Scenarios

1. **Signal Reception:** Receive alien transmission, verify data crosses
2. **Mass Rejection:** Attempt to "receive" physical gift, verify blocked
3. **No Absorption:** Long journey in ISM, verify internal mass is unchanged and the drag energy matches HB-51 to HB-56
4. **Proto-Tech Build:** Receive blueprints, fabricate tech, verify mass cost

## Success Criteria

- [ ] Boundary constraint is clear and consistent
- [ ] Proto-tech acquisition feels meaningful
- [ ] Internal mass never changes from outside; ISM drag energy is reported
- [ ] Massive radiation is always blocked; photons are absorbed by the ship's glazing and hull (D-15)
- [ ] Player understands they are "memetic travelers"
- [ ] Isolation creates appropriate emotional weight
