# Brief: characters (crew, captain, Archive, background people)

**Status:** approved scope (2026-09-28). Starts after the interior style frame
is approved, so the characters match that look.
**Read first:** [art/README.md](README.md) (style, conventions, review), then
`features/future/crew-psychology.md`, `features/future/dialogue-system.md`,
`features/future/archive-system.md` and `features/future/bubble-society.md`.
**For:** an agent in `~/dev/blender`. **Needed by:** R1 M4 (conversations and
"news from home").

---

## 1. What characters do in the game

- **In the world:** crew walk the **isometric play areas**, standing at
  consoles, in gardens, in doorways. They're small against cathedral-scale
  spaces (tiny humans; Pillar 4 "The Ship Is Home").
- **In conversation:** a **large portrait** fills a big part of the screen
  beside the dialogue text and choices (decision D-6; dialogue design). Most of
  the game's emotional weight sits in these faces.
- **Over time:** the voyage lasts **100 subjective years**. Crew **age**, some
  **die** (memorial dialogues), **children are born**, and **generations** turn
  over (bubble society). Personalities drift invisibly (OCEAN drift). The
  portraits must carry age honestly, because time having emotional weight is
  Pillar 3.

**Principle: one model per character, two outputs.** Each named character is
**one Blender model** that yields both:
- a low-poly **play figure** (GLB, walks the isometric areas; the engine
  toon-shades it);
- **large portraits** (rendered plates, with emotions and ages).

The same face, clothes and palette in both places are what make the crew feel
like people you know.

## 2. The cast

### 2.1 Named crew: 11 archetypes (from `crew-psychology.md`)

| Archetype | OCEAN (O,C,E,A,N) | In one line | Visual direction (proposal; Mark approves at the style frame) |
|---|---|---|---|
| Engineer | .2 .8 .2 .5 .2 | practical, laconic, fiercely competent | work-worn practical layers, tool harness, steady posture |
| Scientist | .8 .8 .2 .5 .5 | analytical, future-oriented | neat, many pockets and notes, looks past you at an idea |
| Medic | .5 .5 .5 .8 .5 | warm, stabilizing | soft layers, open stance, the kindest eyes on the ship |
| Diplomat | .5 .5 .8 .8 .2 | reads people, persuasive | considered elegance, expressive hands |
| Pilot | .2 .2 .8 .5 .2 | decisive, thrill-driven | light, fast silhouette, restless |
| Quartermaster | .2 .9 .5 .3 .2 | craves structure and rules | orderly uniform, badges of office |
| Zealot | .8 .5 .5 .3 .5 | passionate, morally intense | a ritual detail (a mark, a garment) that grows over the years |
| Dreamer | .9 .2 .5 .5 .8 | sensitive, poetic, sometimes overwhelmed | mismatched, colourful, a little dishevelled |
| Skeptic | .5 .8 .2 .3 .8 | doubts motives, challenges assumptions | closed stance, sharp features, a watchful look |
| Fantasist | .8 .1 .8 .5 .5 | creative chaos agent | unpredictable accessories, bright asymmetry |
| Analyst | .5 .8 .2 .5 .2 | quiet pattern-seeker | understated, still, sees what others miss |

**Rules:**
- The **archetype is a role and a temperament, not a costume.** Personality
  should read from posture, expression and small choices, not labels.
- **Diversity:** a mixed, plausible human crew (ages, bodies, ethnicities,
  genders). Nobody is a caricature.
- **Names, genders and appearance are Mark's call.** Propose, don't decide.

### 2.2 The captain (the player)
- The player is "The Self": personality emerges from play and is never declared.
  The captain needs a **play figure** that reads clearly in the isometric view
  (a distinctive silhouette and one accent colour), and **no expressive
  portrait**. The player's face is left to the player, so show at most a
  silhouette or back view in UI.
- The captain ages like everyone else: provide 4 age stages on the figure (§3.3).

### 2.3 The Archive (the ship's AI)
- It's **a full NPC** with a personality that drifts and a memory that degrades.
  It's present in two forms (design decisions 2025-12-06 and 12-08):
  - **Terminals** beside the spire on every level. Design a terminal "face": an
    abstract, non-human presence (light, pattern, form), **not an android
    head**. Its portrait is this presence in 8 emotional states, expressed
    through light, pattern and motion stills. Its **degradation** shows as
    subtle glitch or irregularity variants: 3 levels, never labelled.
  - **Mobile robots:** small, gentle and a little odd, able to go wherever the
    crew go. Design one robot with 2 variants. They're figures only; the
    Archive speaks through its terminal-presence portrait.
- Organic and mechanical: the Archive should feel grown and ancient, tied to
  the spire's mystery.

### 2.4 Background population (~100 people, generations)
- A **modular crowd kit**: 6 body bases, heads, hair, and clothing layers with
  palette variants. It dresses the play areas and panoramas, as walking or idle
  figures with no portraits.
- **Children and elders:** births and generations happen, so include child and
  elder bases.
- **Cultural drift:** the bubble becomes its own civilization over 100 years.
  Provide **3 clothing "eras"** (launch → middle years → late voyage), drifting
  from Earth-derived toward something the ship invented. This is a strong,
  quiet way to show time passing.

## 3. Specifications

### 3.1 Play figures (GLB)
- **Scale:** about 1.75 m adults, in metres, Y up in glTF. The camera shows
  about 16 m of height, so figures are small: silhouette and colour must read
  at about 60–80 px tall.
- **Geometry:** low-poly and stylised (proportions a touch elongated,
  Moebius-like), flat Principled base colours, no baked lighting. The engine
  toon-shades and outlines them.
- **Rig:** simple humanoid (spine, head, limbs; a mixamo-like joint set is
  fine). Actions: `idle`, `walk`, `talk`, `work_console`, `sit`, and later
  `grieve` and `celebrate`.
- **Budget:** 3–6k triangles for named crew, 1.5–3k for crowd pieces.

### 3.2 Large portraits (rendered plates)
- **Framing:** bust or three-quarter, **2048×2048 RGBA**, transparent
  background, consistent eye line and head size across the cast.
- **Emotions:** the dialogue system's 8: **Neutral, Happy, Sad, Angry, Fearful,
  Curious, Loving, Grieving**. Use shape keys on the face and posture tweaks.
  Keep them subtle; these are people, not emoji.
- **Style:** banded toon plus Grease Pencil Line Art ink, matching the approved
  interior style frame. Light from a consistent key direction with a cool rim
  (the space light), so portraits sit together.

### 3.3 Age progression
- **4 age stages per named character**, spanning the voyage (e.g. +0, +25, +50
  and +75 years from each character's starting age). Drive them with shape keys
  and a texture or colour ramp: grey, lines, posture. The emotions must work at
  every age. Named crew who die before the end still need the stages they
  reach.
- Children are born aboard, so give 2 of the named crew a **child** variant
  (for generational stories).

## 4. First deliverables, in order

1. **Character style frame:** one character (**the Medic** is proposed) as a
   portrait in Neutral, Grieving and Loving at age +0, plus one at age +50, and
   the play figure standing on the spike's bridge play area, captured in-engine.
   **Stop for Mark's approval.**
2. **Named crew:** all 11 models, each with a portrait set (8 emotions × 4 ages)
   and a rigged play figure.
3. **The Archive:** the terminal presence (8 states and 3 degradation levels)
   and one robot with 2 variants.
4. **The captain:** play figure with 4 ages, and a UI silhouette.
5. **Crowd kit:** bases, heads, clothing in 3 eras, children and elders.

## 5. Delivery

```
exports/stapledon/characters/<id>/
  char_<id>.glb                        # play figure, rigged, actions included
  portrait_<id>_age<0|25|50|75>_<emotion>.png   # 2048x2048 RGBA
  manifest.json                        # id, archetype, name (proposed), ages, emotions, palette, height_m
  preview_<id>.png                     # in-engine capture on the spike bridge (review only)
exports/stapledon/characters/archive/  # terminal presence states + robot GLBs
exports/stapledon/characters/crowd/    # kit GLBs + era manifest
```

**Checks:**
- Portraits share head size, eye line and light (build a contact sheet per
  emotion).
- Every GLB re-imports with its rig and actions (`make validate-glb ANIMATED=1`).
- Figures read at 70 px in the spike capture.

Hand-off is the same as the art README: zips on stapledons-godot issue #1, with
GitHub links for review.

## 6. Mark decides
Every character's look, name and gender; the Archive's presence design; the
captain's silhouette; the clothing eras.
