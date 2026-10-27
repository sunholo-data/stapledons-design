# Living worlds: how life, minds and their messages look and sound

## Status

- **Status:** Approved 2026-10-27 (Mark, attended; the open questions are answered below). Proposed 2026-10-27 (Mark, attended: "we need the graphics for the planets with life civ etc, and fun with generating alien communication").
- **Priority:** P2. It follows the evidence, finder and console work, but its art direction should be agreed early.
- **Roadmap:** [r3-signals-and-contact.md](../../roadmap/r3-signals-and-contact.md), track V.
- **Builds on:**
  - [art/worlds-civilizations-blender-brief.md](../../art/worlds-civilizations-blender-brief.md) (species, kits, time-layered art, faceless portraits, communication-quality variants);
  - the shipped planet renderer (stapledons-godot M5: `sunholo/celestial`, photometric reflected light, rings, the TRAPPIST-1 planets);
  - [signals-and-instruments.md](signals-and-instruments.md) (what each instrument can resolve; the lens);
  - [contact-console.md](contact-console.md) (primers, decoding, the Listen and Decode panes);
  - design-decisions 2025-12-08 (the French 70s comic look) and D-7 (generated portraits, voices and assets, cached and carried across runs);
  - `sunholo/audio` (the game's synthesised sound, R2-SUNHOLO-AUDIO).

---

## Game vision alignment

| Pillar | Alignment | Rationale |
|---|---|---|
| **Choices Are Final** | N/A | Presentation only. |
| **The Game Doesn't Judge** | ✅ Supports | A ruined world is drawn as what it is, not as a warning. |
| **Time Has Emotional Weight** | ✅ Core | The same world drawn at several eras: the lights you saw in the light, and the dark you find on arrival. |
| **The Ship Is Home** | ✅ Supports | Wonder and grief at a living world are crew moments; the art has to earn them. |
| **Grounded Strangeness** | ✅ Core | Colours come from pigments under a real host star, lights only on the night side, and alien messages built on real primer design. |
| **We Are Not Built For This** | ✅ Supports | A face that is not a face; a message half understood. |

---

## Problem

The renderer draws planets from physics, but every world is sterile: geology, oceans and clouds at best. A biosphere, a civilisation and a ruin need to read differently from orbit, honestly, and the same world has to be drawable at several eras. Contact also needs a look and a sound: the console is where the strand's best moments happen, and "text on a panel" will not carry them.

---

## Design

### 1. What a living world looks like, and from where

Resolution decides what can be shown (signals-and-instruments §4):

| From | What is resolvable | What the game draws |
|---|---|---|
| Light-years away | A point | A point with its measured spectrum on the console. No picture. |
| Through the lens | Tens of kilometres at ~100 ly | **An instrument product:** a reconstructed, noisy, low-resolution image in measured bands. Never a beauty shot. |
| At the world, in orbit | Everything | The full render (§2) |

### 2. Layers on the planet renderer

Each layer is driven by the world's simulated state (planet-state machine, civ-mirror), and each has a physical reason:

| Layer | Physics | Varies with |
|---|---|---|
| **Biosphere cover** | A pigment reflectance curve evaluated under the host star's spectrum. Plants around M dwarfs may absorb into the near-infrared and look dark (Kiang et al. 2007); around F stars they may reflect more blue. | Host star temperature, biosphere coverage, season |
| **Oceans and glint** | Specular reflection at crescent phase | Phase angle |
| **Clouds and ice** | The existing atmosphere model | Climate state |
| **Night lights** | Artificial illumination, **night side only** | Population and technology level |
| **Industrial haze** | NO₂ and aerosols (a brownish, bluer-limb haze) | Industry, collapse |
| **Orbital works** | Satellite belts, mirrors, sails, megastructures (artificial transits seen from afar) | Technology level |
| **Scars** | Burned or dead regions, dark cities, ecocide hazes | Remnant and collapse types |

**Time layers:** the same world is drawn at every era its history passes through. The hindsight card can show "what the light said" beside "what you found".

**Generated surfaces:** surface maps for each world come from a seeded generator, refined with generated textures where D-7 allows. They are cached, labelled as generated, and join the cross-run content library.

### 3. Species: how they present

From the art brief: biology follows real constraints, body plans may be radically non-human, and faces may not exist. A species' "portrait" shows how it expresses state (posture, colour, pattern, an interpreter device). The game's emotion markers map to species-appropriate expressions, plus a **communication-quality** variant (clear, noisy, misunderstood) tied to the translation confidence in the console.

Most contacts never show a body. A civilisation 40 ly away is a voice, a glyph stream and a lens image. A body is seen only in orbit, and only if it can be resolved, or if they choose to show themselves.

### 4. The look and sound of alien messages

Each civilisation gets a **communication system** generated from its seed and its species traits:

| Element | How it is made |
|---|---|
| **Carrier** | Physical choice: radio band, optical pulses, a modulated starlight occulter. Drawn on the Listen pane as a real spectrum and waterfall. |
| **Glyphs** | A seeded grammar of strokes and shapes. Its rules fit the species (a radial script for a radially symmetric body, a time-sequence script for a species that thinks in rhythm). The same grammar always gives the same glyphs. |
| **The primer, drawn** | Counting dots, the prime ladder, the hydrogen-line diagram, the host star's spectrum: the shared physics the decoding starts from |
| **Sound** | Sonification through `sunholo/audio`: the carrier's pulses and structure rendered audible, deterministically, so a world has a sound you learn to recognise |
| **Translation overlay** | Decoded sentences under the glyphs, each with its confidence. Low-confidence words are greyed; contested readings show both. |
| **Voice** | Once decoded, replies are written by the model in the civilisation's voice (D-20), and voiced with a generated voice if the species "speaks" in a way a voice suits; otherwise read by the crew member decoding them |

The fun is in learning a system across a run. The first message is noise and structure. By the fourth letter, decades later, the crew can read most of it, and the glyphs have become familiar faces.

---

## Acceptance criteria (for the implementation design that follows)

1. A biosphere world's disc-integrated colour under a G2 star matches its pigment model within the golden tolerance, and moves as the model says under K and M hosts.
2. Night lights never render on the day side (a golden over all phases).
3. A lens image is generated from the instrument model's resolution and noise, never from the full render at full resolution.
4. Glyph and sonification generation is deterministic. The same seed gives byte-identical glyph sheets and audio.
5. Every generated asset is labelled as generated and recorded in the content library with its seed.

## Decisions (Mark, attended 2026-10-27)

1. **Art direction:** photometric planets (the "true sky"); console art, glyphs and species portraits in the comic style.
2. **Procedural first.** Species portraits and surface detail start procedural; D-7 generated images come in the slice's second pass.
3. **Untranslated messages are read aloud by the crew member decoding them,** in their own voice.
