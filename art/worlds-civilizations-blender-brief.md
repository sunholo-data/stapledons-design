# Brief: worlds and civilizations (R2 — scoping, do not start before R1 ships)

**Status:** **scoping.** It records the direction so R1 work stays compatible.
Mark schedules it after R1.
**Read first:** [art/README.md](README.md), then:
- `vision/core-pillars.md` (Pillar 5, Grounded Strangeness);
- `vision/design-decisions.md` ("Alien Biosphere: Science Not Tropes", Wanderer
  Mythology, Earth's fate);
- `features/future/exploration-modes.md` (planet surface and ruins);
- `features/future/civilization-trade.md`;
- `features/future/endgame-legacy.md`;
- `features/future/dialogue-system.md` (alien speakers).

---

## 1. What this covers

| Asset | Where it appears | Made by |
|---|---|---|
| **Alien species:** portraits and figures | first contact, negotiation and trade dialogues; surface modes | **you** |
| **Civilization kits:** surface play areas (spaceport, city, wilds) | Planet Surface mode (the isometric play-area pipeline, as for interiors) | **you** |
| **Ruins kits:** extinct civilizations | Ruins mode, environmental storytelling | **you** |
| **Artifacts:** logs, relics, tech | discovery and collection | **you** |
| **Alien ships and megastructures** | arrival views, CivDetail | **you** (exterior pipeline) |
| **Earth, changed:** variants of the home surface | return and endgame | **you** (surface kit) |
| Planets from orbit, rings and moons | system arrival, orbit | **engine** (NASA textures, procedural) |
| Galaxy-scale history, Year 1,000,000 | legacy screens | **engine and UI** (data visualisation) |

## 2. Principles

- **Scientifically plausible, maximally diverse** (Pillar 5):
  - **Biology:** follow real constraints (gravity, atmosphere, chirality,
    energy), not rubber-forehead humanoids or Hollywood monsters. Body plans may
    be radically non-human; faces may not exist at all.
  - **Portraits:** for species without faces, the "portrait" shows how that
    species expresses state (posture, colour, pattern, an interpreter device).
    The 8 dialogue emotions map to species-appropriate expressions, plus a
    **communication-quality** variant (clear, noisy, misunderstood).
- **Deep time is the story:**
  - **Time-layered kits:** one civilization appears at several eras (rise, peak,
    decline, ruin) because you revisit centuries later. Build kits with **era
    variants** from the same pieces, so the player recognises the place and
    feels the time.
  - **Ruins must read as the same culture's remains,** weathered by the
    elapsed centuries.
- **The game doesn't judge:** no "evil" or "good" alien aesthetics. Cultures
  are strange, not coded.
- **Grown, not built** (the house style): even the alien architecture should
  feel organic and mechanical, in the Moebius spirit, varied per culture.
- **Extensible:** kits and species use documented templates so new cultures can
  be added (the design anticipates community additions).

## 3. When it starts

After R1 (M1–M4) ships and Mark schedules R2, turn this into a full brief with
the same structure as the interior brief: style frame first, then canon,
physics contract, kit specifications, delivery and checks. The first proposed
style frame is **one civilization at three eras**: a surface play area at its
peak, the same place in ruin, plus one species portrait.

## 4. Constraints to respect now (so R1 work stays compatible)

- **Surface play areas use the same isometric pipeline as interiors:** a
  toon-shaded GLB play area, a panorama with an exported camera, a foreground
  plate. The **sky above a planet is the engine's**: a real star field and the
  local star, and atmosphere later.
- **Characters and species use the characters brief's model:** one model,
  figure plus portraits.
- **Never author the sky, stars, black holes or planets seen from orbit.**
