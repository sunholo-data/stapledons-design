# Star surfaces: what a star looks like up close, and what an eclipse reveals

## Status

- **Status:** Proposed 2026-10-20 (Mark, attended, ledger D-99): "another strand is we could have more impressive textures for the stars/sun - we are dimming them anyway, so we can see structure?"
- **Priority:** P2.
- **Implementation design:** stapledons-godot `design_docs/planned/r2/star-surfaces.md` (R2-STAR-SURFACES), awaiting Mark's approval.
- **Spike frames (throwaway shader, real game views):** https://storage.googleapis.com/stapledons-voyage-assets/refs/star_surfaces/contact_sheet.png
- **Builds on:**
  - [physics/relativity-spec.md](../../physics/relativity-spec.md) §2: each surface element is a blackbody at its own temperature, seen at D·T. This feature adds check values RS-24 and RS-25.
  - The finite-destination-stars stop rules (D-47, D-54): they set how big each star looks at its stop.
  - The Auto view (D-38), which already dims a star's disc so its colour shows; D-59 (no glare); D-96 (authored eclipse and transition framings).

---

## Game vision alignment

| Pillar | Alignment | Rationale |
|---|---|---|
| **Choices Are Final** | ✅ Neutral | Presentation only. |
| **The Game Doesn't Judge** | ✅ Neutral | Description, not comment. |
| **Time Has Emotional Weight** | ✅ Supports | The surface moves on the star's own clock as received; ahead at speed it visibly races. The Sun's spots differ when you come home. |
| **The Ship Is Home** | ✅ Neutral | Seen from the bridge. |
| **Grounded Strangeness** | ✅ Core | Each star type looks like itself for physical reasons; every size, contrast and lifetime is cited. |
| **We Are Not Built For This** | ✅ Supports | One convection cell on a giant is three Suns wide. |

---

## What the player sees

**The rule:** a structure appears only if its real size would resolve at the star's apparent size at that moment. Nothing is drawn larger than physics allows, and the star's total light never changes.

| Star | At its stop | What shows (Auto view) |
|---|---|---|
| **A giant** (Aldebaran, K5 III) | fills 61° of sky | about thirty huge, slowly churning convection cells, each about three solar radii across; strong limb darkening; an orange made of many oranges |
| **A Sun-like star** (α Centauri A) | 11° | a limb-darkened disc; its granulation is far too fine to see; a large spot group just shows as a few dark pixels |
| **A red dwarf** (TRAPPIST-1, Barnard's Star) | 3–4° | big dark starspots and bright faculae near the limb; deep red at the edge |
| **The Sun from home** | 0.5° | a small limb-darkened disc; nothing else resolves (as it should) |

**In the Realistic view** a star's disc stays a white, clipped disc, as the eye sees it.

**Time:** the surface evolves on the star's clock as received. At a stop, the Sun's granules turn over in minutes; a giant's cells over weeks. Seen ahead at 0.9c, any surface runs √19 = 4.36 times faster (RS-25). Spots come and go with the star's activity cycle, so the Sun at home after a long voyage wears a different face.

**Eclipses (with D-96):** when a planet or moon covers a star's photosphere, the corona appears (about a millionth of the disc's brightness, Baumbach 1937), with pink hydrogen-alpha prominences at the limb; just before and after totality, the last sliver of photosphere makes the "diamond ring". Hydrogen-alpha is line emission, so at speed its colour shifts as a line, not as a blackbody. An eclipse reads only when the star is big in the sky (a few degrees, as TRAPPIST-1 is from its planets); the Sun from Earth's distance gives a corona ring only 10–25 px across at the game's view.

## What is real and what is chosen

- **Real (cited):** limb-darkening laws per temperature and gravity; cell sizes (about ten pressure scale heights); intensity contrasts; spot temperatures, sizes and latitude bands; differential rotation; corona brightness profile; prominence heights.
- **Chosen (seeded, labelled):** where each cell and spot sits. The pattern is not a record of any real day on any star.
- **Not shown:** supergranulation (below 1% contrast in white light), rotational Doppler across a disc (a few parts per million), flares (a later event feature).

## Open questions

See the implementation design's OQ1–OQ8 (resolution gate, procedural or baked, fixed total light, supergranulation, eclipse layers now, flares later, an Archive entry, a bloom for the diamond ring).
