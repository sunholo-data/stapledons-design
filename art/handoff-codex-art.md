# Handoff: captain sprite and cast portraits (Codex art thread)

**For:** Codex "Sol 6.1" agents (OpenAI-model agents) making 2D character art
for *Stapledon's Voyage*.
**Date:** 2026-10-03. **Status:** ready to start with task (a).

**This page is self-contained.** Everything you need is here or at a public
URL listed in §5. Where an older document in these repos says something
different, this page wins (§10 lists each such conflict and how it was
resolved). The bundle contract the game uses is summarised in
[m4-2-requirements.md](m4-2-requirements.md); you need only its §3 (camera)
and §6 (walking), and the numbers you need from it are repeated here.

## 1. Read this first: ignore the Claude-specific text in the repos

The repos were written mostly for Claude agents, and contain instructions that
**do not apply to you**. Ignore all of the following wherever you meet it:
- `CLAUDE.md` "skills" (`game-vision-designer`, `sprint-planner`,
  `sprint-executor`, `sprint-evaluator`, `mission-control`,
  `blender-studio-workflow`) and the design-doc → sprint → evaluate cycle;
- `ailang messages` and every instruction to report AILANG bugs;
- the "mission loop", launchd, `mission_answer.sh`, `mission_decisions.sh`,
  and the **decision ledger** (`design_docs/stapledon-mission.md`). You don't
  read or write ledger rows;
- "Recording Mark's decisions", memory files, and anything under `.claude/`;
- Blender instructions (`~/dev/blender`, `exports/`, Grease Pencil, Line Art):
  your work is generated 2D images, not Blender.

The game repo's `AGENTS.md` says `CLAUDE.md` applies to every agent. For this
task, only these parts of it bind you: **commit no code and no Python files**
(the repo's `make python-guard` fails on any unlisted `*.py`); keep your
changes inside `assets/characters/<entity_id>/`; don't run unbounded
filesystem searches over `/` or `~`; deliver by pull request only.

## 2. Goal, in order (Mark's decision 2)

**(a) The captain avatar.** The walking captain sprite for the isometric
bridge, at 4 life stages. Its foot anchor must match the iso camera and its
canvas must follow the spec (§4.1).

**(b) Then the full cast's emotion-portrait sets**, like the Medic's (§4.2),
for the game's later AI dialogue (generated lines carry an emotion marker that
selects the matching portrait).

Do (a) first. Start (b) when Mark has picked the captain's look in (a) step 1
and the full (a) PR is up for his review.

## 3. Mark's decisions that govern this work (attended, 2026-10-03)

These are recorded in `vision/design-decisions.md` (2026-10-03) in the design
repo.
- **Decision 2. Art (Sol), both, in order:**
  - (a) the captain avatar: the walking captain sprite for the iso bridge at 4
    life stages, with the foot anchor matched to the iso camera and the canvas
    spec defined;
  - (b) then full-cast emotion-portrait sets like the Medic's, for later AI
    dialogue.
- **Decision 3. Delivery:**
  - A PR to the game repo `sunholo-data/stapledons-godot`, into the agreed
    character asset path (§8).
  - Large binaries belong in the public bucket `gs://stapledons-voyage-assets/`,
    content-addressed as `<sha256>.<ext>`. **You can't run gcloud**, so include
    the files in your PR branch; the Claude side moves them to the bucket.
  - Review happens on the PR, with renders attached.
- **Decision 4. Portraits:**
  - Use the generator's native resolution (about 1254 px), never upscaled.
  - Each set gets a `generation_notes.md` recording the generator, model, full
    prompt, date and seed (if any).
  - Project art is owned by Sunholo and shipped with the Apache-2.0 codebase.
  - Third-party references must not be traced or copied.

## 4. Deliverable specs

### 4.1 Captain avatar sprite

**What the game does with it:**
- The captain is the player. On the isometric bridge the game moves one static
  captain image over the walkable floor; there is **no walk cycle** and no
  animation. The image's **foot anchor** is placed on the floor point where the
  captain stands.
- The game picks the image for the captain's life stage, and for whether the
  captain is moving toward or away from the viewer. It mirrors the image
  horizontally for the other two diagonals.

**The camera you are drawing for:**
- Orthographic (no perspective), looking down **14°** below horizontal, rotated
  45° around the vertical, showing 16 m of screen height.
- A standing 1.75 m adult spans about **115 px** on a 1080-line screen, **96 px**
  in the 1600×900 bridge captures, and **67 px** in the 1120×630 Medic preview.
  The "reads at about 70 px" check in older docs refers to that last size.
- So draw a **full-body standing figure seen from just above eye level**:
  - feet visible, soles on one ground line;
  - a thin ground ellipse (not drawn, just implied by the feet);
  - no perspective foreshortening (feet the same scale as the head);
  - no floor, no ground shadow (the engine may add a contact shadow).

**Canvas, anchor and scale (the contract):**

| Field | Value |
|---|---|
| Canvas | **1024 × 1536 px RGBA** (portrait orientation, the generator's native size for full-body images; the accepted Medic avatar is exactly this) |
| Foot anchor | **(512, 1440)**: x = the horizontal centre, y = 96 px above the bottom edge. The anchor is the floor point under the body's centre: the midpoint between the soles of a relaxed standing pose |
| Scale | **720 px per metre**, the same for every sprite of the captain (±2%). A 1.75 m figure is 1260 px from the anchor to the top of the hair, so the head top sits near y = 180 |
| Margin | at least 16 px of alpha-0 border on all four sides; nothing touches the edge |
| Background | alpha exactly 0 everywhere outside the figure; no backdrop, glow, haze, halo or dark fringe; the antialiased edge is at most 3 px wide |
| Light | soft warm key from the upper left, delicate cool rim (as in the Medic) |
| Mirror-safe | no text, letters, numbers or one-sided insignia; the design must read correctly when flipped horizontally |

To meet the anchor and scale, you may **translate, pad, crop the empty margin,
and downscale** with a high-quality filter. **Never upscale.** If a generated
figure is too small to reach 720 px/m without upscaling, regenerate it.

**The set: 4 life stages × 2 facings = 8 sprites.**

| `age_stage` (years since departure) | Biological age at the default start age 30 | Height |
|---|---|---|
| 0 | 30 | 1.75 m (or the chosen design's height) |
| 20 | 50 | same |
| 40 | 70 | up to 2 cm less, posture a little settled |
| 60 | 90 | up to 5 cm less, visibly old, still upright and capable |

The game shows the stage with the greatest `age_stage` ≤ the captain's years
aboard. The start age is a scenario setting, so the images must read as "young
adult, mid-life, older, old" rather than as exact ages.

| Facing | `variant` | What |
|---|---|---|
| `front` | `"0"` | three-quarter view toward the viewer, body turned to screen-left |
| `back` | `"1"` | three-quarter view away from the viewer, body turned to screen-left |

**Identity rules for the captain:**
- The player's face is left to the player. The captain never has an
  expressive portrait. At sprite size the face is a few pixels; keep it calm
  and non-specific.
- The captain needs a **distinctive silhouette and one accent colour**, so the
  player can find themselves at a glance among the crew. **Ochre** (`#D9A340`)
  is proposed: the bridge uses ochre for human touchpoints, and the captain
  was ochre in the review mannequins.
- Keep the clothing, palette and silhouette consistent across all 4 stages,
  with honest wear and greying over time.
- **The captain's look (silhouette, build, gender presentation) is Mark's
  decision.** Hence the step order below.

**Steps for (a):**
1. **Silhouette proposals:** 3 different captain designs, stage 0, facing
   `front` only, each to the full canvas spec. Open a **draft PR** (§8) and
   post on issue #1. **Stop until Mark picks one** (he replies on the PR or
   on issue #1).
2. **The full set:** the chosen design at all 4 stages × 2 facings, generated
   from the chosen stage-0 image as the identity reference. Mark the PR ready
   for review.

### 4.2 Cast emotion-portrait sets

**The model is the accepted Medic set** (§5): a square bust portrait per
emotion and life stage, swapped in the dialogue UI, plus one static mini
avatar.

**Portrait spec:**

| Field | Value |
|---|---|
| Size | **the generator's native square size, about 1254 × 1254**, recorded in the manifest. Never upscaled; don't request a size the generator would reach by upscaling |
| Format | PNG, RGBA, genuine transparency |
| Background | alpha exactly 0 along the top, left and right edges and everywhere outside the figure. Only the bust may meet the bottom edge, cut at mid-chest. No scenery, stars, gradient, glow, frame, caption, name or watermark |
| Framing | bust from mid-chest up; mild three-quarter turn; gaze just off-camera; eye line on the upper third; full head and hair inside the frame with a little margin; both shoulders in; hands out of frame |
| Consistency | within one character, the crop, head scale, head position, eye line, clothing and light are the same in every image, so swapping emotions doesn't make the head jump. Check with an overlay (§4.5) |
| Emotions | exactly these 8 ids: `neutral`, `happy`, `sad`, `angry`, `fearful`, `curious`, `loving`, `grieving`. Subtle, believable faces: changes in the eyes, brow, mouth and posture. No theatrical crying, no emoji faces |

**Avatar spec (one per character, stage 0):** the same canvas, anchor and
scale as the captain (§4.1), facing `front` only, with face, hair, clothing
and palette matching the portraits. The crew don't walk, so one facing is
enough.

**Cast and what each needs.** `entity_id` is the game's key. Departure ages
are **proposals** from the generation-cast plan; record them as proposed. The
ten identity descriptions are in §11.

| `entity_id` | Departure age (proposed) | Identity reference | Work |
|---|---|---|---|
| `medic` | 35 | Medic set (§5) | add `happy`, `sad`, `angry`, `fearful`, `curious` at stage 0 to match the existing `neutral`, `loving`, `grieving`; re-key the existing four portraits and the avatar (bring the avatar to the anchor and scale by translating and padding only, or regenerate it) |
| `engineer` | 58 | cast_marker_v1 portrait (shows departure) | it becomes stage-0 `neutral`; add the other 7; avatar |
| `diplomat` | 62 | same | same |
| `pilot` | 28 | same | same |
| `dreamer` | 24 | same | same |
| `fantasist` | 31 | same | same |
| `scientist` | 31 | cast_marker_v1 portrait (shows age 41 = stage 10) | first a **younger stage-0 identity** (31) from it; review; then the other 7 at stage 0; avatar. The existing image becomes stage-10 `neutral` |
| `quartermaster` | 29 | portrait shows 49 = stage 20 | same pattern; existing → stage-20 `neutral` |
| `zealot` | 26 | portrait shows 36 = stage 10 | same; existing → stage-10 `neutral` |
| `skeptic` | 26 | portrait shows 46 = stage 20 | same; existing → stage-20 `neutral` |
| `analyst` | 32 | portrait shows 57 = stage 25 | same; existing → stage-25 `neutral` |

**Order for (b):**
1. The Medic (it proves the 8-emotion swap on the accepted identity).
2. The five that already show departure.
3. The five younger departure identities: **one PR with all five
   stage-0 neutrals beside their existing older images, then stop for Mark's
   review**, then their emotion sets.

Later life stages beyond those listed come only when Mark asks for them.

**Out of scope here:** the Archive's terminal presence, children and
descendants, the crowd kit, and the captain's UI silhouette. Ask on issue #1
before starting any of them.

### 4.3 Filenames

```
assets/characters/<entity_id>/
  portrait_<entity_id>_y<age_stage>_<emotion>.png    e.g. portrait_medic_y0_curious.png
  avatar_<entity_id>_y<age_stage>_<facing>.png       e.g. avatar_captain_y40_back.png
  manifest.json
  generation_notes.md
  review/contact_sheet.png        (and any other review images)
```

- `y<age_stage>` is **whole years since departure**, not biological age.
  Biological age goes in the manifest.
- Add `_v<n>` before `.png` only when you replace a file that was already
  delivered (`_v2`, `_v3`, and so on).
- lowercase `snake_case`, no spaces.
- Existing files you re-key (the Medic's `portrait_medic_age0_neutral_v2.png`,
  `portrait_<role>_age0_neutral_v1.png`) are not renamed in their old homes.
  Copy them in under the new pattern and record their original name and
  sha256 in the manifest's `source` field.

### 4.4 `manifest.json` (one per character folder)

```json
{
  "entity_id": "captain",
  "status": "proposed",
  "role": "captain",
  "departure_age": 30,
  "departure_age_status": "scenario default",
  "identity_note": "one paragraph: the look, clothing, accent colour",
  "palette": ["#D9A340", "#F2E3C7", "#3B2959"],
  "rights": "Copyright Sunholo. Shipped with the Apache-2.0 stapledons-godot codebase.",
  "avatar_spec": {
    "canvas_px": [1024, 1536],
    "foot_anchor_px": [512, 1440],
    "px_per_m": 720,
    "view_pitch_deg": -14,
    "mirror_safe": true
  },
  "files": [
    {
      "file": "avatar_captain_y0_front.png",
      "kind": "avatar", "emotion": "-", "age_stage": 0, "variant": "0", "facing": "front",
      "biological_age": 30, "height_m": 1.75, "figure_height_px": 1260,
      "width": 1024, "height": 1536,
      "sha256": "<lowercase hex of the file>",
      "source": "generated (see generation_notes.md#avatar_captain_y0_front)"
    },
    {
      "file": "portrait_medic_y0_neutral.png",
      "kind": "portrait", "emotion": "neutral", "age_stage": 0, "variant": "0",
      "biological_age": 35, "width": 1254, "height": 1254,
      "sha256": "93a02944772201d3e854a283342fd189409995e85a91796d5197a9e5f0be0017",
      "source": "re-keyed: medic_v2/portrait_medic_age0_neutral_v2.png"
    }
  ]
}
```

- `kind`, `entity_id`, `emotion`, `age_stage` and `variant` together are the
  game's asset key (`kind/entity_id/emotion/age_stage/variant`).
  - `emotion` is `-` for avatars.
  - Portrait `variant` is `"0"` unless Mark asks for alternates.
  - Avatar `variant` is `"0"` for front and `"1"` for back.
- `entity_id` matches `^[a-z][a-z0-9_]{0,31}$`.

### 4.5 `generation_notes.md` (one per character folder; decision 4)

Write one section per delivered file, headed by the filename:

```markdown
## avatar_captain_y0_front.png
- Generator: <product/tool name>      Model: <exact model id/version>
- Date: 2026-10-0x                     Seed: <seed, or "none exposed">
- Mode: text-to-image | edit of <input file> (sha256 <hex>)
- Prompt (full, verbatim):
  > ...
- Post-processing: background removal (how), translate/pad/crop/downscale (by how much); never upscaled
- Candidates: <n> generated; chosen because ...; rejected because ...
```

These notes let the game regenerate consistently later, so nothing is
summarised: paste the full prompt every time, even when it repeats.

**Self-check before every PR** (a throwaway script; **don't commit it**):

```python
from PIL import Image; import sys, hashlib
for p in sys.argv[1:]:
    im = Image.open(p); assert im.mode == "RGBA", p
    w, h = im.size; a = im.getchannel("A")
    edges = [a.crop((0, 0, w, 1)), a.crop((0, 0, 1, h)), a.crop((w - 1, 0, w, h))]
    if "avatar_" in p: edges.append(a.crop((0, h - 1, w, h)))
    print(p, w, h, "edges alpha max", max(e.getextrema()[1] for e in edges),
          hashlib.sha256(open(p, "rb").read()).hexdigest())
```

Every edge alpha max must be 0 (for an avatar, also check the 16 px margin).
For each character, also make `review/contact_sheet.png`: all images side by
side, plus an overlay of neutral at 50% over each emotion, to show the head
doesn't move. For sprites, also include:
- each sprite downscaled to 115 px and to 67 px tall, to judge readability;
- a mock-up of the stage-0 front sprite at 96 px tall, pasted on the deck of
  the 1600×900 bridge capture (§5) with the foot anchor on the floor.

The mock-up is a review aid. The Claude side makes the in-engine capture.

## 5. References

The design repo and the game repo are public. The Blender repo is
**private**: if a `sunholo-data/blender` link returns 404 for you, ask on
issue #1 and the Claude side will publish those files to the public bucket.
The ten identity descriptions are copied in §11, so you can start without the
images.

| What | URL |
|---|---|
| Medic set index (keys, sizes, sha256) | <https://github.com/sunholo-data/stapledons-godot/blob/main/data/ai/core/index.ndjson> |
| Medic neutral, stage 0 | <https://storage.googleapis.com/stapledons-voyage-assets/ai/93a02944772201d3e854a283342fd189409995e85a91796d5197a9e5f0be0017.png> |
| Medic loving, stage 0 | <https://storage.googleapis.com/stapledons-voyage-assets/ai/3c23ffe2b657f9045a998ab7416af0bee667a91dbb806f54d09f167172075b29.png> |
| Medic grieving, stage 0 | <https://storage.googleapis.com/stapledons-voyage-assets/ai/2a68ffbec262bae0e48607ab097a9f7747a1e2ae085bf91274d4fce15eebb0ca.png> |
| Medic neutral, stage 50 | <https://storage.googleapis.com/stapledons-voyage-assets/ai/bde097f5e6ca7165cd2b46dfc54ee0fd7aee8d919f5b3396c7a5f463674ce1a4.png> |
| Medic static avatar (1024×1536) | <https://storage.googleapis.com/stapledons-voyage-assets/ai/581719375bd36532ad1ce92d34224eecaa5c1a41efd99ba19613980865e4ff2e.png> |
| Medic avatar on the bridge (1120×630) | <https://github.com/sunholo-data/stapledons-godot/blob/spike/iso-bridge/spike/out/medic_image_v2/preview_medic.png> |
| Medic sources and prompts (private) | <https://github.com/sunholo-data/blender/tree/main/assets/stapledon/characters/medic_v2> |
| cast_marker_v1: ten identity portraits (private) | <https://github.com/sunholo-data/blender/tree/main/docs/stapledon/cast_marker_v1>, images in <https://github.com/sunholo-data/blender/tree/main/assets/stapledon/characters/cast_proposals_v1> |
| Bridge build-out v1, at rest (1600×900) | <https://github.com/sunholo-data/stapledons-godot/blob/spike/iso-bridge/spike/out/bridge_buildout_v1/preview_rest.png> |
| Bridge build-out v1, at 0.99c | <https://github.com/sunholo-data/stapledons-godot/blob/spike/iso-bridge/spike/out/bridge_buildout_v1/preview_099c.png> |
| Bridge parallax sheet | <https://github.com/sunholo-data/stapledons-godot/blob/spike/iso-bridge/spike/out/bridge_buildout_v1/bridge_parallax_sheet.png> |
| Art bible (style, who draws what) | <https://github.com/sunholo-data/stapledons-design/blob/main/art/README.md> |
| Character brief (older; §10 says what is superseded) | <https://github.com/sunholo-data/stapledons-design/blob/main/art/characters-blender-brief.md> |
| Generation-cast plan (ages, households) | <https://github.com/sunholo-data/stapledons-design/blob/main/art/generation-cast-plan.md> |

On GitHub `blob` pages, add `?raw=true` to download the image.

## 6. Style (pasted, so you don't need the art bible)

**The game:** you captain a ship inside a bubble that can travel close to the
speed of light, for about 100 years of your own life. Each journey costs the
galaxy centuries. About 100 people live aboard, and they become their own small
civilisation, ageing and having children. The game is conversations and
consequences. Tone: **vast, beautiful, lonely, bittersweet.**

**Direction:** French 1970s science-fiction comics: the ligne claire and
*Métal Hurlant* tradition of Moebius (Jean Giraud) and Philippe Druillet. Take
the qualities, never their images (§7):
1. organic and mechanical blended: technology that looks grown;
2. saturated colour against vast emptiness;
3. tiny humans in massive structures;
4. clean, flowing curves;
5. cathedral-like spaces.

**For characters, in practice** (this is what made the Medic work):
- a sophisticated hand-drawn 1970s European science-fiction graphic-novel
  illustration;
- delicate, expressive ink contours with fine, selective cross-hatching;
- textured gouache and watercolour colour planes, with subtle facial
  modelling and richly observed eyes;
- real, individual human faces and bodies, with visible age;
- tactile cloth with convincing folds and small signs of wear: grown,
  handmade fastenings, not armour;
- soft warm key light, delicate cool rim, restrained violet shadows;
- role shown through posture and small personal details, not costume or
  labels.

**Palette.** The values are the bridge layout's colour triples written as hex.
The game shows them a little lighter after shading, so match the bridge
captures by eye, not the numbers.

| Name | Hex | Use on the bridge | Use on characters |
|---|---|---|---|
| cream | `#F2E3C7` | deck | base cloth layers, linings |
| coral | `#E8694A` | captain ring | seams, small accents |
| teal | `#1F8A8A` | helm floor, consoles | shawls, collars, uniforms |
| ochre | `#D9A340` | human touchpoints, inlays | the captain's accent (proposed); brass-like fastenings |
| violet | `#3B2959` | ink lines, rails | shadows, collars, cords |
| mint | `#9EE0C7` | screens | rarely; small light accents |
| leaf | `#5C9E4C` | fronds | moss tones in clothing |
| spire | `#D1E0FA` | the pale luminous spire | cool rim light |
| ink | `#0E0917` (rendered `#423555`) | Line Art ink | contour colour |

Skin, hair and eye colours are natural and individual; the palette governs
clothing, accents and light.

**Avoid:**
- photorealism and photography;
- glossy 3D or video-game renders, low-poly or polygonal looks, rigid
  mannequin anatomy;
- anime, chibi proportions, emoji expressions, airbrushed glamour;
- grimdark, and "clean functional sci-fi" (no spacesuits, armour, helmets,
  guns, military insignia, sunglasses or racing costume);
- real religious symbols;
- caricature or stereotype of any ethnicity, age, body or gender;
- any background: no scenery, stars, gradients, glows, frames, captions,
  names, logos or watermarks.

## 7. Licensing and provenance (decision 4)

- Project art is **owned by Sunholo** and ships with the game under the
  Apache-2.0 licence of `stapledons-godot`. Your PR contributes it on that
  basis.
- **Never trace, copy or paste third-party work**, and never use third-party
  images (comics, film stills, other games, photos of real people) as image
  inputs or edit targets. The only allowed image inputs are this project's own
  images: the Medic set, the cast_marker_v1 portraits, the bridge captures,
  and your own outputs.
- **Don't name artists in prompts** (Moebius, Giraud, Druillet or anyone
  else). Describe the qualities instead, as the accepted prompts do: "1970s
  European science-fiction graphic-novel illustration, delicate expressive ink
  contours, textured gouache".
- No real people's likenesses. No logos or trademarks.
- Record every generation in `generation_notes.md` (§4.5). A file without a
  complete entry isn't deliverable.

## 8. Where outputs go, and how to ask for approval

1. Fork, or branch if you have push access, from `sunholo-data/stapledons-godot`
   `main`. Use one branch per PR: `art/captain-avatar-v1`, then
   `art/portraits-<entity_id>-v1` (for example `art/portraits-medic-v1`), and
   `art/portraits-departure-identities-v1` for the five younger identities.
2. Commit only under `assets/characters/<entity_id>/`: the PNGs, `manifest.json`,
   `generation_notes.md` and `review/`. No code, no `*.py`, no `.import` files,
   no edits elsewhere. Include the PNGs themselves; you can't upload to the
   bucket.
3. Open the PR against `main`. Use a draft PR while you wait on Mark. In the
   body, put:
   - what the PR contains and which step of §4 it is;
   - the contact sheet and mock-up images (drag them into the PR description so
     they render);
   - the self-check output;
   - a line: "Art owned by Sunholo, contributed under Apache-2.0;
     generation_notes.md complete."
4. Post a comment on **game-repo issue #1**
   (<https://github.com/sunholo-data/stapledons-godot/issues/1>): one line with
   the PR link and the decision you need ("pick a captain silhouette: A, B or
   C"). The issue is closed (it's a weekly bookkeeping thread), but comments
   still reach Mark.
5. Mark approves or asks for changes on the PR. **Don't merge your own PR.**
   After approval, the Claude side:
   - uploads the PNGs to `gs://stapledons-voyage-assets/ai/<sha256>.png`;
   - adds them to the game's asset index;
   - replaces the binaries in the branch with pins;
   - squash-merges.

Questions also go to issue #1 as comments. Mark decides every character's
look, name and gender; propose, don't decide.

## 9. Acceptance checks (per PR)

| Check | How |
|---|---|
| Sizes: sprites 1024×1536; portraits native square (about 1254), never upscaled | self-check output |
| Alpha: edges 0 (all four for sprites, with a 16 px margin; three for portraits), no halo or backdrop | self-check output, plus the contact sheet on a mid-grey and a black background |
| Anchor and scale: anchor at (512, 1440), 720 px/m ±2% on every captain sprite | `figure_height_px` / `height_m` in the manifest |
| Swap stability: the head doesn't move between emotions | the overlay in the contact sheet |
| Readability: the captain is identifiable at 67 px and 115 px | the downscaled strip |
| Keys: every file has a valid key in `manifest.json`, and the sha256 matches | the self-check hash against the manifest |
| Provenance: a full `generation_notes.md` entry for every file | review |

## 10. Conflicts in the older docs, resolved

| Conflict | Resolution (from Mark's 2026-10-03 rulings) | Superseded text |
|---|---|---|
| **Portrait size: 2048 vs 1254** | Native generator resolution (about 1254), never upscaled (decision 4) | `art/characters-blender-brief.md` §3.2 "2048×2048 RGBA" and its "remaining gap to the final delivery target"; §5's `# 2048x2048 RGBA`; `art/generation-cast-plan.md` "final delivery still targets 2048 px"; "request 2048x2048 if available" in the cast_marker_v1 prompts |
| **The age grid** | `age_stage` = whole years since departure, the game's key. The captain has exactly 4 stages, 0/20/40/60 (decision 2a's "4 life stages", spaced across a reachable life from the default start age 30). Crew: stage 0 in full, plus the stages their existing images show; no fixed grid | the brief's §2.2 "4 age stages", §3.3 "+0, +25, +50 and +75", §4 item 2 "8 emotions × 4 ages", and §5's `age<0\|25\|50\|75>`. Agrees with the cast plan's step 5 |
| **Filename patterns** | `portrait_<entity_id>_y<age_stage>_<emotion>.png` and `avatar_<entity_id>_y<age_stage>_<facing>.png`, `_v<n>` only on replacement; the manifest maps every file to its key | the brief's §5 `avatar_<id>_age<…>.png` and `portrait_<id>_age<…>_<emotion>.png`; the Medic's `…_age0_…_v2.png` and cast_marker_v1's `…_age0_neutral_v1.png`, where "age0" meant departure even when the face was 41 or 57 |
| **Medic v2 vs cast_marker_v1** | The Medic v2 images are the Medic's identity and the format reference for every set (decision 2b, "like the Medic's"); the game repo pins them as its accepted core set. cast_marker_v1 supplies the other ten identities. Read D-16's "Medic v2 images are superseded by the accepted illustrated cast" as closing the Medic-only review stage, not as retiring the Medic | `art/README.md` "Review and hand-off" item 4, "supersedes the Medic v2 images"; the generation-cast plan's link to a nonexistent Blender branch `art/cast-marker-v1` (the files are on Blender `main`) |
| **Delivery** | A PR to the game repo, `assets/characters/<entity_id>/` (decision 3) | the brief's §5 `exports/stapledon/characters/<id>/` and "zips on stapledons-godot issue #1"; `art/README.md` "Review and hand-off" items 2–3 |
| **In-engine previews** | You supply mock-ups (§4.5); the Claude side captures in the engine | the brief's §4 item 1 and §5 `preview_<id>.png` ("in-engine capture on the spike bridge") |
| **Avatar scale "60–80 px, validate at 70 px"** | Kept, as the 1120×630 review size; the canvas contract is 720 px/m with the anchor at (512, 1440) | the brief's §3.1 "consistent foot anchor" without a number |

## 11. The ten identity descriptions (cast_marker_v1, proposals)

These are the written art directions the accepted cast_marker_v1 portraits
were generated from. The stated age is the age **shown in the existing
image**. For the five marked ★, the departure version is younger (§4.2).
Names are not set.

- **engineer** (58): Woman, age 58, broad practical face with pale freckled skin, deep-set grey eyes, a blunt slightly bent nose, cropped silver-and-copper hair, strong weathered jaw, substantial shoulders. Work-worn charcoal olive canvas layers with cream lining, a small teal repaired seam and restrained brass harness fastening. Steady unsentimental posture, laconic competence and patience. No tools held, no grease caricature.
- **scientist** ★ (shown 41; departs 31): Man, age 41, East Asian appearance, narrow thoughtful face, warm light skin, almond-shaped dark eyes behind fine round wire spectacles, straight nose, short black hair parted unevenly with one stubborn lock, clean-shaven. Neat cream layered work coat with deep violet collar and a slim teal notebook edge in a chest pocket. Looks slightly past the listener, absorbed and curious yet human. No lab goggles or stereotypes.
- **diplomat** (62): Woman, age 62, deep brown skin, broad nose, finely lined eyes and expressive mouth, short tightly coiled silver hair, elegant sturdy neck and relaxed shoulders. Deep violet folded cloth jacket, soft cream inner layer, restrained ochre seam and one simple handmade brass clasp. Calm attentive gaze, subtle amused warmth, a person practised at listening. No crowns or luxury jewels.
- **pilot** (28): Man, age 28, olive skin, compact square face, dark close-set eyes, thick eyebrows with a small old break in one brow, short unruly black curls and faint stubble. Light coral work jacket worn open over cream with a muted teal fastening. Restless poised shoulders, a slight decisive half-smile, alert eyes. No helmet, sunglasses, guns or flashy racing costume.
- **quartermaster** ★ (shown 49; departs 29): Woman, age 49, warm medium-brown skin, round sturdy face, dark observant eyes, broad cheeks, full natural lips, tightly gathered dark braided hair with a little grey at the temples. Compact broad-shouldered build. Very orderly slate-teal cloth uniform with cream folded collar, neat brass fasteners and a small plain rectangular role badge without words. Composed firm expression, exacting but humane. No military insignia.
- **zealot** ★ (shown 36; departs 26): Androgynous person, age 36, copper-toned skin, angular long face, aquiline nose, dark intense eyes under straight brows, long black hair tied simply behind the head. Ochre and muted coral draped working clothes with one narrow hand-woven violet cord bearing an abstract geometric stitch. Upright posture and morally earnest questioning gaze, still vulnerable and human. No symbols of real religions, no priest costume, no villain styling.
- **dreamer** (24): Man, age 24, dark brown skin, softly rounded youthful face, wide thoughtful eyes, generous nose and lips, loose soft short black curls, no beard. Mismatched pale violet and moss-teal soft cloth layers, small cream patches and a loosely knotted worn scarf. Slightly inward posture, wistful absorbed gaze and sensitive mouth. Sensitive and creative, never childish or distressed caricature.
- **skeptic** ★ (shown 46; departs 26): Woman, age 46, light olive skin, a narrow long face with prominent cheekbones, keen hazel eyes, straight thin nose, short cropped black hair with grey near one temple and fine natural eye lines. High-neck muted olive work jacket with restrained cream and violet details. Shoulders slightly closed, watchful sidelong attention, one subtly questioning eyebrow, lips relaxed but unsmiling. Not hostile, not a villain.
- **fantasist** (31): Man, age 31, fair rosy skin with a few freckles, round open face, broad nose, lively blue-grey eyes, unruly reddish curls, short red moustache and uneven soft beard. Bright but balanced ochre and teal layered jacket, asymmetrical coral lapel and one small odd handmade earring. Animated eyes and a barely contained delighted idea, generous confident posture. Creative chaos in small details, not a clown or costume.
- **analyst** ★ (shown 57; departs 32): Androgynous person, age 57, medium tawny-brown skin, long oval face, dark hooded eyes, understated straight brows, broad straight nose, thin relaxed mouth, chin-length straight salt-and-pepper hair tucked behind ears. Soft plain desaturated teal tunic with a narrow cream collar and one understated violet seam, no accessories. Very still composed posture, eyes quietly attentive to patterns. Distinct from the Scientist: no glasses, no pockets of notes, no lab coat.

**The Medic** (35): a woman of about 35, with warm medium-brown skin, a
distinctive believable face, dark expressive eyes, gently arched imperfect
eyebrows, a long slightly crooked nose, soft natural lips, and thick dark wavy
hair swept loosely behind the ears with a few escaped strands. She has
compassionate attention and quiet intelligence. She wears cream layered cloth
medical workwear, a deep teal folded shawl collar, a small coral seam and an
understated worn brass fastening.

**The accepted emotion-edit pattern** (from the Medic's grieving image; adapt
the expression line for each emotion):

> Input image is the identity/style reference and edit target. Change ONLY her
> expression to quiet grief: eyes lowered slightly, a fragile tension in the
> brow and lower eyelids, lips held gently together, a person trying to remain
> present through loss. Subtle, believable, deeply felt; no theatrical crying.
> Same age. Preserve the sophisticated hand-drawn graphic-novel ink-and-gouache
> look, original palette, lighting direction, clothing, hair design,
> anatomical identity. For portraits preserve exact square bust crop, head
> scale, camera angle, pose and eye-line as closely as possible so images can
> swap in the same dialogue UI. Keep genuinely transparent alpha background, no
> scenery, caption, frame, UI or watermark. This is a static game image; no
> animation.
