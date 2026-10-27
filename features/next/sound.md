# Sound: the ship, heard from inside

## Status

- **Status:** Approved (game-repo ledger D-101, 2026-10-20; built as R2-SOUND). Proposed 2026-10-20 (game-repo ledger D-98). After playing the bow-facing bridge, Mark said: "it looks really great. obviously we need animation on the captain a bit and sound effects and the level design but such good potential".
- **Priority:** P1 for R2.
- **Implementation:** game repo `design_docs/planned/r2/sound.md` and its sprint plan (R2-SOUND). This document holds the **what**, the canon the implementation must keep.
- **Builds on:**
  - [ship-structure.md](../phase1-data-models/ship-structure.md), The Spire: "Low constant hum, felt as much as heard."
  - [lore/archive/one-g.md](../../lore/archive/one-g.md): "Inside, the teacup on the table does not tremble. And when the drive is silent and the ship coasts between stars, you still weigh what you weigh."
  - [lore/archive/nothing-crosses.md](../../lore/archive/nothing-crosses.md) and [photon-drive.md](../../lore/archive/photon-drive.md): nothing material crosses the wall; the drive throws only light.
  - [lore/archive/weather-between-the-stars.md](../../lore/archive/weather-between-the-stars.md): dust grains flash on the forward dome.
  - [bubble-constraint.md](../future/bubble-constraint.md) Audio: "everything is mediated", "signal reception sounds".
  - Decisions D-7 and D-8 (2026-09-28): every crew line spoken in its own voice; no live voice; text-only always available.

---

## Game vision alignment

| Pillar | Alignment | Rationale |
|---|---|---|
| **Choices Are Final** | ✅ Supports | The commit sounds final: a tone that rises while you hold, and one sealing chord when the ship accepts it. There is no undo sound because there is no undo. |
| **The Game Doesn't Judge** | ✅ Supports | No fanfare and no failure stinger anywhere. Arrival, a crew concern and a hold limit are neutral instrument cues. |
| **Time Has Emotional Weight** | ✅ Supports | The coast is quiet: when the drive falls silent, the ship is its hum and its people. A home beacon (proposed) drops in pitch as Earth recedes. |
| **The Ship Is Home** | ✅ Supports | Footsteps, consoles, the lift, the spire, and the crew's voices, which always win the mix. |
| **Grounded Strangeness** | ✅ Supports | Hard physics heard honestly: nothing from space, no rumble from a drive you cannot feel. |
| **We Are Not Built For This** | ✅ Supports | A 100 m dome answers every footstep; between stars the only constant is a machine nobody can enter. |

---

## The canon (proposed; Mark rules on each in the game repo's OQ1–OQ9)

1. **Nothing is heard from outside the bubble.** Space carries no sound, and nothing material crosses the wall. Stars, planets, the starbow, black holes and the drive's beam are silent. Every sound has a source inside the ship: a machine, a surface, an instrument or a person.
2. **The teacup rule.** A boost is not felt, so it does not sound like a rocket: no rumble, no shake, no hull stress. The drive's machinery is heard instead, rising to a clean, high tone and settling. When the ship coasts, the drive is silent.
3. **No exhaust and no thrusters.** Nothing leaves the bubble but light, so a turn makes no hiss; a navigation tone tracks it.
4. **The spire always hums**, low and constant, from the mast. Its light dims when a star takes over (D-96); its hum does not.
5. **Instruments may make the outside audible**, because they are machines aboard:
   - the **dust counter**, which ticks for every grain that flashes on the dome, faster at speed and in the warm clouds;
   - the **home beacon** (proposed): Earth's carrier at the helm, its pitch following the Doppler factor, compressed so it stays audible; low as Earth recedes, true at a stop, rising on the way home.
6. **One quiet interface layer** for clicks, outside the fiction.
7. **No judgement.** Nothing sounds like a reward or a punishment.
8. **Voices win.** Crew and the Archive always sit on top of the mix; everything else ducks under them.

## The palette, in words

- **The bridge:** air under a large dome; a long, soft tail on footsteps on plating; consoles waking with a small servo and a glass hum; an old brass chime for the lift.
- **The drive:** clean, high and narrow; never low, never loud.
- **The instruments:** small, dry ticks and tones, like a laboratory's.
- **The spire:** low, slow, slightly beating; present everywhere, noticed rarely.
- **Music:** deferred. The ship's own sound is the score until level design decides whether each level has a voice.

## Out of scope

Music, generated voices (D-7/D-8 stand), sound for the lower levels beyond the lift (with level design), and any sound on the website that touches the endings.
