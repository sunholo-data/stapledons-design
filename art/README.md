# Art bible and brief index

This is how every authored visual asset for Stapledon's Voyage is made, and
where each brief lives. **Read this first, then the brief for your task.**

## The briefs

| Brief | What | Needed by | Status |
|---|---|---|---|
| [ship-interior-blender-brief.md](ship-interior-blender-brief.md) | Isometric play areas, interior panoramas, foreground plates | R1 M4 | **Approved.** Start with the bridge style frame |
| [characters-blender-brief.md](characters-blender-brief.md) | Crew, captain, Archive robots, background people: illustrated portraits and matching static mini avatars | R1 M4 | Illustrated direction accepted. [Generation cast plan](generation-cast-plan.md) in development |
| [ship-exterior-blender-brief.md](ship-exterior-blender-brief.md) | Exterior bubble ship, departure/arrival camera moves, galaxy-map marker | R1 M2 (marker), M4 (views) | Revised spherical silhouette and boundary review pending |
| [../features/ai-showcase.md](../features/ai-showcase.md) | **Runtime AI** (D-7): emotion markers selecting portraits, generated voices, the growing asset library, recorded outputs for replay | R1 M4 | Planned. Voice and marker style frame on the accepted Medic |
| [worlds-civilizations-blender-brief.md](worlds-civilizations-blender-brief.md) | Alien species, civilization and ruin kits, artifacts, alien ships | R2 (later) | Scoping. Don't start before R1 ships |

**Order:**
1. the interior style frame (it sets the look for everything);
2. then characters (one character style frame) and the exterior marker, in parallel;
3. then the full interior and character builds, after their style approvals;
4. worlds and civilizations after R1.

## Who renders what

| The engine (Godot) renders, live and physically exact: | You author: |
|---|---|
| the relativistic sky: catalogue stars and the full-sky galaxy, with aberration, Doppler colour and brightness | ship interiors: isometric play areas, panoramas, foreground plates |
| black holes: shadow, lensing, Einstein rings (M3) | characters: generated illustrated portraits and matching static mini avatars |
| planets (NASA textures, procedural), rings and moons | ship exterior model and camera moves |
| the bubble boundary's speed-dependent glow | alien species, civilization and ruin kits (R2) |
| toon shading and ink outlines on play-area GLBs | |

**Never author anything in the left column.** A painted star, glow or lensing
effect would be physically wrong the moment the ship's speed changes. If a shot
needs one, leave the space transparent and export the camera.

## Style

**French 70s science-fiction comics:** Moebius (Jean Giraud), Philippe
Druillet, *Métal Hurlant* (design decision 2025-12-08):

1. organic and mechanical blended: technology that looks grown;
2. saturated colour against vast emptiness;
3. tiny humans in massive structures;
4. clean, flowing curves;
5. cathedral-like spaces.

Tone: vast, beautiful, lonely, bittersweet. Not photorealistic, not grimdark,
not "clean functional sci-fi".

**Techniques:**
- **Blender environment plates** (panoramas, foregrounds): banded toon shading
  (Shader-to-RGB with constant colour ramps) and ink lines through **Grease
  Pencil Line Art**. Blender 5.2 has **no Freestyle**; probe the Line Art API
  before relying on it.
- **Character images:** rich, expressive generated illustrations, with discrete
  images for emotion and age changes. Keep facial identity, clothing and palette
  consistent across a character's portraits and small static avatar. Character
  portraits do not require Blender models, rigs or animation.
- **Real-time GLBs** (play areas, exterior): clean geometry and flat
  Principled base colours. The engine applies toon shading and the ink outline.
  Bake nothing.

## Shared conventions

- **Ship frame:**
  - units are metres;
  - **Blender +Z = the thrust axis = up = the direction of travel**;
  - the origin is the bubble's centre.

  The bubble is a **perfect sphere**, about 100 m in radius and transparent,
  with the spire on the axis. The forest level and all trees stay inside it.
  Exterior reviews must show a readable representation of the boundary; the
  engine supplies its appearance and speed-dependent glow.
- **Workspace:** `~/dev/blender` under its `AGENTS.md` and
  `skills/blender-studio-workflow`:
  - Blender generator scripts go in `scripts/`, and scenes use distinct names in `scenes/`;
  - always prefer a reproducible generator script over hand edits;
  - `exports/` and `renders/` are gitignored, so hand deliveries over as zips.
- **Cameras:** every rendered plate whose sky the engine fills gets a
  `cam_<name>.json` (see the interior brief, §5.2). Use a perspective camera with
  a vertical sensor fit, no shift, and no distortion.
- **Naming:** use `snake_case`, prefix by asset type (`play_`, `pano_`, `fg_`,
  `char_`, `portrait_`, `ext_`, `civ_`), and version with `_v<n>` when replacing.
- **Reference implementation:** stapledons-godot branch `spike/iso-bridge`
  (`spike/interior3.gd` is the five-layer composite).

## Review and hand-off

1. **Style frames first.** Every brief opens with one style frame. **Stop for
   Mark's approval** before any build-out.
2. **Mark reviews on GitHub from his laptop.** He can't see local files, so push
   previews as images and send **GitHub links**. For in-engine previews, use a
   worktree of the game repo's `spike/iso-bridge` branch, and commit captures
   there only.
3. **Never edit `main` of stapledons-godot.** The Stapledon mission loop owns
   it. Deliver bundles as zips on stapledons-godot issue #1; the loop imports
   them.
4. **Current review state (2026-10-01, ledger D-16):** bridge style frame v1
   is approved for build-out; interior build-out proceeds (interior brief §7
   step 2). The illustrated cast (`cast_marker_v1`) is accepted and supersedes
   the Medic v2 images (`style_revision_v2`). The exterior style frames are
   still to be reviewed. Art is iterated as data: the game swaps in each
   delivered bundle without code changes, and Mark tweaks as it goes. See
   `vision/design-decisions.md` (2026-09-28 reviews; 2026-10-01 D-16).
5. **Decisions are Mark's:** style frames, any change to ship canon, character
   designs, and anything that would paint over the sky. If Mark rules in your
   session, record it (see stapledons-godot `CLAUDE.md`, "Recording Mark's
   decisions").

## Canon sources

- `vision/core-pillars.md`, `vision/game-vision.md`: the pillars and the game.
- `vision/design-decisions.md`: ship canon (2025-12-06 to 12-08), the interior
  model (2026-09-28, D-6).
- `physics/relativity-spec.md`: why the sky is the engine's job.
