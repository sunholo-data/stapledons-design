# Brief: illustrated characters and mini avatars

**Status:** revised direction (2026-09-28). Mark rejected the Blender character
study and requested richer, expressive generated images with expression swaps
and mini avatars. The illustrated Medic direction and subsequent cast studies were accepted.
Departure ages and family connections are being revised for a generation ship;
see [generation-cast-plan.md](generation-cast-plan.md) for the proposed roster.
The filename is retained so existing brief links remain valid.
**Read first:** [art/README.md](README.md) (style, conventions, review), then
`features/future/crew-psychology.md`, `features/future/dialogue-system.md`,
`features/future/archive-system.md` and `features/future/bubble-society.md`.
**For:** an image-generation art agent working with the shared art bible.
**Needed by:** R1 M4 (conversations and "news from home").

---

## 1. What characters do in the game

- **In the world:** small static crew avatars occupy the **isometric play
  areas**, at consoles, in gardens and in doorways. They're small against cathedral-scale
  spaces (tiny humans; Pillar 4 "The Ship Is Home").
- **In conversation:** a **large portrait** fills a big part of the screen
  beside the dialogue text and choices (decision D-6; dialogue design). Most of
  the game's emotional weight sits in these faces.
- **Over time:** the voyage lasts **100 subjective years**. Crew **age**, some
  **die** (memorial dialogues), **children are born**, and **generations** turn
  over (bubble society). Personalities drift invisibly (OCEAN drift). The
  portraits must carry age honestly, because time having emotional weight is
  Pillar 3.

**Principle: one consistent illustrated identity, two image scales.** Each
named character has:
- **large illustrated portraits**, rich enough to carry subtle human emotion;
- a **matching static mini avatar**, readable in the isometric play area.

Generate and retain a reference image for each proposed identity, then use it
throughout the expression and age set. Match facial structure, clothing,
hairstyle and palette between portraits and avatar. Dialogue changes expression
by selecting another image; it does not animate a face. Blender character
models, humanoid rigs, shape keys and animation clips are no longer required.
This change concerns character assets; the environment remains Blender-built.


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
  The captain needs a **static mini avatar** that reads clearly in the isometric view
  (a distinctive silhouette and one accent colour), and **no expressive
  portrait**. The player's face is left to the player, so show at most a
  silhouette or back view in UI.
- The captain ages like everyone else: provide 4 age stages on the avatar (§3.3).

### 2.3 The Archive (the ship's AI)
- It's **a full NPC** with a personality that drifts and a memory that degrades.
  It's present in two forms (design decisions 2025-12-06 and 12-08):
  - **Terminals** beside the spire on every level. Design a terminal "face": an
    abstract, non-human presence (light, pattern, form), **not an android
    head**. Its portrait is this presence in 8 emotional states, expressed
    through light, pattern and composition. Its **degradation** shows as
    subtle glitch or irregularity variants: 3 levels, never labelled.
  - **Mobile robots:** small, gentle and a little odd, able to go wherever the
    crew go. Design one robot with 2 matching static avatar variants. The
    Archive speaks through its terminal-presence portrait.
- Organic and mechanical: the Archive should feel grown and ancient, tied to
  the spire's mystery.

### 2.4 Background population (~100 people, generations)
- A **static crowd avatar set**: 6 body silhouettes, with varied heads, hair,
  clothing layers and palettes. These dress the play areas without individual
  portrait sets or animation requirements.
- **Children and elders:** births and generations happen, so include child and
  elder silhouettes.
- **Cultural drift:** provide **3 clothing eras** (launch → middle years → late
  voyage), drifting from Earth-derived toward something the ship invented.

## 3. Specifications

### 3.1 Static mini avatars
- **Format:** transparent RGBA PNG, with a consistent foot anchor and viewing
  angle that sits naturally in the isometric play area.
- **Scale:** an adult represents about 1.75 m and must read at approximately
  60–80 px tall in the play view. Validate at 70 px on the bridge.
- **Design:** simplify detail while retaining the portrait's face, hairstyle,
  clothing, proportions and palette. Keep an identifiable silhouette.
- **Motion:** deliver static art. Rigged GLBs and idle, walk, talk or other
  animation clips are not required for this direction. Any engine positioning
  or image transitions are implementation work, separate from authored art.

### 3.2 Large portraits
- **Framing:** bust or three-quarter, **2048×2048 RGBA**, transparent
  background, consistent eye line and head size across the cast. Native-size
  generated images may be used for style review; record their actual dimensions
  and any remaining gap to the final delivery target.
- **Emotions:** the dialogue system's 8: **Neutral, Happy, Sad, Angry, Fearful,
  Curious, Loving, Grieving**. Supply separate images. Show expressive human
  faces with subtle changes in eyes, mouth, posture and gesture.
- **Style:** rich illustrated science-fiction portraits, coherent with the
  French 70s comics direction and the bridge palette. Preserve expressive
  detail at dialogue size; the simplified avatar serves the distant play view.
- **Consistency:** use the approved identity reference for each variant.
  Keep lighting direction, framing and recognizable facial structure stable.
  Banded Blender shading and Grease Pencil Line Art are not portrait requirements.

### 3.3 Age progression
- **Up to 4 relevant age stages per named character**, spanning their life (e.g. +0, +25, +50
  and +75 years from each character's starting age). Supply separate images:
  grey hair, skin, posture and clothing wear should carry time honestly while
  retaining identity. Emotions must work at every delivered age. Named crew
  who die before the end need the stages they reach.
- Children and descendants have **separate identities**. Begin with two recurring
  children from the generation cast plan, then age those same people into later
  portraits. Do not represent a descendant by de-ageing their parent.
- Match avatar age variants to the portrait reference set.

## 4. First deliverables, in order

1. **Character style frame:** one character (**the Medic** is proposed) as a
   portrait in Neutral, Grieving and Loving at age +0, plus one at age +50, and
   a matching static mini avatar on the spike's bridge play area, captured
   in-engine. Use generated illustrations for this revised style review.
   **Stop for Mark's approval.**
2. **Named crew:** all 11 approved identities, each with a portrait set
   (8 emotions × 4 ages) and matching static mini avatars.
3. **The Archive:** the terminal presence (8 states and 3 degradation levels)
   and one robot avatar with 2 variants.
4. **The captain:** static mini avatar with 4 ages, and a UI silhouette.
5. **Crowd kit:** static silhouettes, heads, clothing in 3 eras, children and elders.

## 5. Delivery

```
exports/stapledon/characters/<id>/
  avatar_<id>_age<0|25|50|75>.png       # static RGBA mini avatar, foot anchored
  portrait_<id>_age<0|25|50|75>_<emotion>.png   # 2048x2048 RGBA
  manifest.json                        # proposed identity, ages, emotions, palette, avatar anchor
  generation_notes.md                  # reference images, prompts, variants and selection notes
  preview_<id>.png                     # in-engine capture on the spike bridge (review only)
exports/stapledon/characters/archive/  # terminal presence states + robot avatar images
exports/stapledon/characters/crowd/    # static avatar images + era manifest
```

**Checks:**
- Portraits share head size, eye line and light (build a contact sheet per
  emotion).
- PNGs have the requested dimensions and clean transparency, with no clipped
  features or accidental background fragments.
- Facial identity is consistent across emotion and age variants.
- Mini avatars read at 70 px in the spike capture and match their portraits.

Hand-off is the same as the art README: zips on stapledons-godot issue #1, with
GitHub links for review.

## 6. At runtime: voice, emotion markers and a growing library (D-7)

Ledger D-7 (Mark, 2026-09-28) adds the runtime layer this art feeds. Full
design: `features/ai-showcase.md`.
- **Generated dialogue carries emotion markers** (the 8 above). Each marker
  **selects the matching portrait image** as the line streams: selection, not
  animation, as in §1.
- **Every line is spoken** with generated audio in the character's own voice.
  The same markers drive delivery. Pick one voice per featured identity (a TTS
  voice plus a style prompt) and record it in `manifest.json`.
- **The library grows during play.** New identities (births, successors),
  later life stages and aliens are generated when the story needs them, from
  the recorded references and prompts. So **keep `generation_notes.md`
  complete**: the reference image, prompt, model and settings for every file
  are what let the game regenerate consistently.
- **The AI is in the ship.** The Archive's core is fused into the spire's base,
  and its terminal presence (§2.3) is how that AI appears.

## 7. Mark decides
Every character's look, name and gender; the Archive's presence design; the
captain's silhouette; the clothing eras.
