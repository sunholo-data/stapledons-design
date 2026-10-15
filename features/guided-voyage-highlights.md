# Guided voyage highlights

**Status:** Landed on main in [game PR #201](https://github.com/sunholo-data/stapledons-godot/pull/201), 2026-10-11, after the accepted D-65 ruling and complete local/CI validation.
**Scope:** The current Godot + AILANG guided voyage. This supersedes the older guided system-arrival/inward-hop itinerary and the route in the [Go-era cinematic arrival proposal](phase4-polish/cinematic-arrival-system.md).

## Game vision alignment

| Pillar | Design consequence |
|---|---|
| Choices Are Final | Each highlight is a real committed journey with a checked endpoint. |
| Time Has Emotional Weight | Ship and Earth clocks remain continuous across the whole tour. |
| Grounded Strangeness | Actual three-dimensional positions, finite bodies and existing physics determine every approach. |

## Accepted itinerary

Mark accepted an Earth start followed by seven travel legs:

| Leg | Destination | Arrival |
|---|---|---|
| Start | Earth | Rest at the existing D-54 Earth standoff. |
| 1 | Sun | Existing close Solar stop. |
| 2 | Jupiter | Existing close planet stop. |
| 3 | Callisto | Existing moon stop and genuine clearance leg before Saturn. |
| 4 | Saturn | Existing checked ring-clear stop. |
| 5 | Alpha Centauri A | One close finite-star arrival, at the existing D-54 stop. |
| 6 | TRAPPIST-1 | One close finite-star arrival, at the existing D-54 stop. |
| 7 | Aldebaran | One close finite-star arrival, at the existing D-54 stop. |

Interstellar cruise requests retain 0.999c, 0.9999c and 0.999999c respectively.
The existing drive, collision checks, ring checks and medium-dependent hold
limits remain authoritative. The Solar timed legs retain their existing
boost, braking and final-approach behavior.

## Arrival and pacing rules

The live tour selects `stop_rule = 54`. A catalogue-star request resolves to
the existing finite-star planner and its checked close endpoint. It does not
first stop 1,000 AU out and then schedule a second inward body journey.
The historical `stop_rule = 0` itinerary retains all ten legs for existing
replays and tests; its outer-system stops are historical behavior.

Every next leg begins at the actual previous endpoint. Positions and clocks
are never reset or teleported to shorten the tour. Distances are separations
between actual three-dimensional catalogue/body positions, not differences
between each star's distance from Sol. For this itinerary, the interstellar
legs are about 4.321, 41.756 and 74.422 light-years. The former 1,000-AU inward
legs represented real distances of about 5.8 light-days; removing them changes
the itinerary rather than repairing a coordinate error.

Boost and braking play in real ship time. Long cruise keeps the existing
10-second real-time cruise hold and one labelled interlude that advances to
the authoritative braking boundary. There is no extra timed inward hop for
the three destination stars. Their arrival views follow the actual braking
trajectory and end at rest with the target still visible.

TRAPPIST-1's direct stop is the existing D-54 finite-star stop. It is not the
former habitable-zone stop engineered beside planet e. Planet positions,
clearance, phase and apparent size come from the actual system feed; the
tour must not claim the older beside-e view.

## Optional GR demonstration

After the guided voyage completes, the player may open the clearly labelled
Sgr A* GR demo. This is a separate demonstration session placed at the black
hole, not an eighth continuous travel leg or an arrival from Aldebaran.
The existing nonrotating-hole and labelled Sol-background limitations apply.

## Validation and remaining gates

The scoped game implementation in [PR #201](https://github.com/sunholo-data/stapledons-godot/pull/201)
has the following local evidence:

- Actual seven-leg controller lifecycle and pacing: 134 checks, zero failures;
  one initialization, continuous clocks, exact checked endpoints and no eighth leg.
- Pure direct-journey chain: strict bytecode VM and interpreter outputs match.
- Native references: 18 final-braking/arrival frames and nine TRAPPIST-1
  views, with zero capture failures; final views were opened and inspected.
- Representative complete-tour clocks: Earth +120.507874 years and ship
  +0.889 years. These are simulation results, not a promised duration for
  other itineraries or a wall-time playback target.

Complete local and exact-source CI validation pass, including 423 ordinary
tests with zero failures or skips and the required strict/parity/replay gates.
Independent evaluation passes at 98/100. The review build is published and
the game change is merged on main. [Completion evidence](https://github.com/sunholo-data/stapledons-godot/blob/main/design_docs/implemented/r1/bubble-surface-and-moon-textures-report.md)
records the checks and native references. The remaining R1 visual and
console-journey gates stay open. No new physics formula, package pin or lore
check value is introduced by this ruling.
