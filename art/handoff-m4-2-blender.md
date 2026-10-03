# Handoff: finish bridge v1 for M4.2 (Blender thread)

**For:** a Claude Opus agent in the Blender workspace, `~/dev/blender`, working
under its `AGENTS.md` and `skills/blender-studio-workflow/SKILL.md`.
**Date:** 2026-10-03. **Status:** ready to start.
**Contract:** [m4-2-requirements.md](m4-2-requirements.md), the one-page
bundle contract. Read it first; this page says what to do, and that page says
what "done" looks like.
**Background:** [art/README.md](README.md) (the art bible) and
[ship-interior-blender-brief.md](ship-interior-blender-brief.md) (§5 physics
contract, §6 GLB kit, §9 bundle format). This handoff supersedes the brief
where they differ (§7 below).

## 1. Goal

Finish **bridge build-out v1** so that the game's iso interior (R1 M4.2) can
load it as `assets/areas/bridge/`, with parallax overscan, an exported iso play
camera and a navmesh that follows written rules. Deliver it as a PR to the
game repo, reviewed by Mark there. **The bridge is the only area in R1.**

## 2. Mark's decisions (attended, 2026-10-03)

Recorded in `vision/design-decisions.md` (2026-10-03).

- **Decision 1. Blender: finish bridge v1 for M4.2; bridge only in R1.**
  - Merge `art/bridge-buildout-v1` to the Blender repo's `main`, as a PR.
  - Add parallax overscan to the panorama and foreground plates.
  - Export the iso play camera: orthographic, pitch −14°, yaw 45°, iso size
    16 m, as in the M4.2 manifest.
  - Define `WALK_` navmesh rules: flat, a maximum slope, rim gaps and an
    obstacle convention.
  - Add a 1.75 m scale figure and a 2 m grid to the area template, for review
    only.
  - Pass the Blender-side validator now, and M4.0's
    `make validate-areas BUNDLE=...` once M4.0 lands in the game repo.
- **Decision 3. Delivery:** a PR to `sunholo-data/stapledons-godot` into
   `assets/areas/<area>/`. Large binaries go to the public bucket
   `gs://stapledons-voyage-assets/` (content-addressed `<sha256>.<ext>`),
   referenced by pins. Review happens on the PR, with renders attached.

(Decision 2 concerns the Codex art thread; decision 4 concerns portraits.)

**Follow-up rulings (Mark, attended 2026-10-03):**
- Confirmed: the bucket prefix `areas/`, `isocam_<area>.json`, and bridge
  bundle PRs based on `main`.
- Overscan is sized for `pan_range_m [6, 3]`; beyond that range parallax is
  clamped.
- Art questions and PR links go to game-repo issue
  [#93](https://github.com/sunholo-data/stapledons-godot/issues/93), which
  replaces issue #1.

## 3. Starting state (verified 2026-10-03)

| What | Where |
|---|---|
| Build-out v1 commit | Blender `ba19b98` on branch `art/bridge-buildout-v1` (pushed, **not merged**; `main` is at `32d4956`, the branch's parent, so the merge is a fast-forward) |
| Review README | `docs/stapledon/bridge_buildout_v1/README.md` (branch only) |
| Bundle zip (tracked) | `docs/stapledon/bridge_buildout_v1/bridge_buildout_v1.zip`, 5.0 MB |
| Bundle export (gitignored, local) | `exports/stapledon/areas/bridge/`: `play_bridge.glb`, `pano_bridge.png`, `cam_bridge.json`, `fg_bridge.png`, `manifest.json`, `validation.json`, `preview_rest.png`, `preview_099c.png` |
| Layout data | `assets/stapledon/bridge_layout_v1.json` (pieces, palette, rail gaps `[[204, 212]]`, spawns, panorama eye) |
| Kit | `scripts/stapledon_kit.py` (13 shared meshes, 75 objects) |
| Generator | `scripts/stapledon_bridge_buildout_v1.py` → `scenes/stapledon_bridge_buildout_v1.blend` and the bundle |
| Validator | `scripts/validate_area_bundle.py` (passes: 11 `INTERACT_`, 10 `SPAWN_`, 16 flat `WALK_bridge` sectors with max z 0.024 m, round trip 0.42 px) |
| In-engine review | `scripts/stapledon_area_review.py`, driving a disposable worktree of game branch `spike/iso-bridge` (`spike/interior3.gd`) |
| Captures already published | game repo `spike/iso-bridge`: `spike/out/bridge_buildout_v1/{preview_rest,preview_099c,bridge_parallax_sheet}.png` |

**Brief errors to ignore** (corrected here; the brief now points to this page):
- The brief says to start from `scripts/stapledon_interior_spike.py`. The
  current generator is `stapledon_kit.py` plus
  `stapledon_bridge_buildout_v1.py`.
- `scenes/stapledon_ship_v1.blend` and `scripts/stapledon_ship.py` (brief §7
  step 2, the ship master model) do not exist. They are **not** needed for R1.

## 4. Revision list (in order)

1. **Merge v1.** Open a PR `art/bridge-buildout-v1` → `main` in
   `sunholo-data/blender`. Merge it (decision 1 authorises it), then branch
   `art/bridge-v1-m4-2` from the new `main` for the steps below. No force
   pushes to `main`.
2. **Area template: review aids.** Add a `REVIEW_` collection to the template
   the generator builds: a 1.75 m scale figure and a 2 m grid (lines every
   2 m, a 1 m subgrid is optional). Show it in the Blender iso preview only.
   It must not appear in the GLB, the panorama or the foreground. Extend the
   validator to fail if any `REVIEW_` node is in the GLB.
3. **`WALK_` rules.** Apply the [§5 rules](m4-2-requirements.md#walk_-rules-ruling-1):
   flat within ±0.03 m, max slope 10°, no steps over 0.05 m, the walk edge
   ≥ 0.5 m inside the rim rail, rail gaps walkable only where `ramp_down`'s
   walk surface continues, solid pieces cut out at their exact footprint, one
   connected region at agent radius 0.35 m, every `SPAWN_` on the walk,
   every `INTERACT_` reachable within 1.5 m. The current 16 deck sectors are
   whole sectors with nothing cut out, so obstacles need cutting. Generate
   the cuts from the layout's piece footprints, not by hand. Add each rule as
   a validator check with a positive control (a deliberately broken copy must
   fail).
4. **Iso play camera.** Write `isocam_bridge.json` (schema in
   [§3](m4-2-requirements.md#3-frames-and-cameras)): orthographic,
   `pitch_deg −14`, `yaw_deg 45`, `size_m 16`, `size_axis "vertical"`,
   `rotation_order "YXZ"`, `focus_m [-8.0, 1.0, 9.0]` in the GLB frame,
   `v_offset_m 6.08`. Reference it from `layers.play.camera`. Make the Blender
   iso preview render through exactly these numbers (converted from glTF Y-up
   back to Blender Z-up), so the preview and the game agree.
5. **Parallax overscan.** Render both plates with margins sized by the
   [§4 formula](m4-2-requirements.md#4-parallax-and-overscan-ruling-1):
   `pan_range_m [6, 3]` (Mark, 2026-10-03) gives a panorama of 4096 × 2288 (overscan
   `[128, 64]`) and a foreground of 6432 × 3472 (`[1296, 656]`). Keep the
   centre 3840 × 2160 identical to v1. For the panorama, widen the sensor
   from the same eye (`fov_vertical_deg` ≈ 81.2°) and write the full
   resolution and FOV into `cam_bridge.json`, so the round trip stays under
   1 px. Record `overscan_px` per layer and `pan_range_m` in the manifest.
   Beyond that range the engine **clamps** plate parallax (Mark,
   2026-10-03), so no larger overscan is needed even when the captain walks
   to the 22 m rim.
6. **Validate and review.** Re-run the generator, the validator and the
   in-engine review (§5), and open the captures and look at them: at rest, at
   0.99c, and the parallax sheet at the pan extremes, confirming no plate edge
   shows.
7. **Deliver** (§6).
8. **Once M4.0 lands in the game repo** (`make validate-areas` exists on
   `main`), run it on the delivered bundle and fix whatever it finds. That
   check is part of done.

## 5. Run commands

```sh
cd ~/dev/blender
./blender.sh --background --python-exit-code 1 --python scripts/stapledon_bridge_buildout_v1.py
AREA_BUNDLE=exports/stapledon/areas/bridge ./blender.sh --background --factory-startup \
  --python-exit-code 1 --python scripts/validate_area_bundle.py
python3 scripts/stapledon_area_review.py <disposable spike/iso-bridge clone> bridge bridge_buildout_v1
```

For the review script, make a **fresh clone** of `stapledons-godot` on branch
`spike/iso-bridge` under your scratch directory. Don't use a worktree of, or
switch branches in, the game repo's main checkout.

After M4.0 lands, in your delivery clone of the game repo (§6):

```sh
make validate-areas BUNDLE=assets/areas/bridge
make validate-areas BUNDLE=tests/fixtures/areas/bridge_blockout   # the fixture must still pass
```

## 6. Acceptance checks and delivery

**Done when every row holds:**

| # | Check | Command / evidence |
|---|---|---|
| A1 | v1 merged to Blender `main` via PR | `git -C ~/dev/blender merge-base --is-ancestor ba19b98 origin/main` |
| A2 | Blender validator passes, including the new `WALK_`, `REVIEW_`, overscan and isocam checks | the validator command above exits 0; `validation.json` committed in the bundle |
| A3 | Each new validator check fails on a broken copy | positive-control output recorded in the PR |
| A4 | Round trip ≤ 1 px on the overscanned panorama | `validation.json` → `camera_roundtrip_error_px`, needle `difference_px` |
| A5 | Manifest has every V17 field plus `layers.play.camera`, `overscan_px`, `pan_range_m` | `python3 -c` or `jq` over `manifest.json` in the PR |
| A6 | GLB under 5 MB, Y up, metres, no `REVIEW_` | validator |
| A7 | Captures opened and looked at: rest, 0.99c, parallax sheet at the pan extremes | images attached to the PR |
| A8 | `make validate-areas` passes on the bundle (after M4.0 lands) | command output in the PR |

**Delivery route (decision 3):**
1. Fresh clone of the game repo under your scratch directory; branch
   `art/bridge-v1-bundle` from `origin/main`. **Never** commit in, or switch
   branches in, the game repo's main checkout
   (`~/dev/sunholo-data/stapledons-godot`). The mission loop works there.
2. Add `assets/areas/bridge/` with the JSON files (`manifest.json`,
   `cam_bridge.json`, `isocam_bridge.json`, `validation.json`) and
   `SHA256SUMS` (one line per binary: `<sha256>  <file>`).
3. Upload the PNGs and the GLB to the public bucket, named by content:
   `gcloud storage cp --no-clobber <file> gs://stapledons-voyage-assets/areas/<sha256>.<ext>`.
   That needs gcloud auth on the `stapledons-voyage` project. Without it, say
   so in the PR and the game side uploads them.
4. Until the game repo has a fetch target for `areas/` (a Claude-side task, §9),
   also commit the binaries on the PR branch, so reviewers and `validate-areas`
   can see them. Squash-merge so they don't enter `main`'s history if the fetch
   target lands first.
5. Open the PR into **`main`** (Mark, 2026-10-03). Put the captures in the
   body. Post a one-line comment on the art issue, game-repo
   [#93](https://github.com/sunholo-data/stapledons-godot/issues/93), linking the PR.
6. In the Blender repo, commit the scripts, the layout and the README update
   on `art/bridge-v1-m4-2` and open a PR there too.

## 7. What this supersedes

- Brief §7 step 2 (ship master model) and steps 4–6 (lower areas, enclosed
  room, exterior): **deferred past R1.** Bridge only (decision 1).
- Brief §10 and art README "Review and hand-off" item 3 (zips on issue #1;
  the mission loop imports them): **replaced by a PR to the game repo**
  (decision 3). The art issue #93 is for questions and the PR link.
- Brief §9 bundle list: adds `isocam_<area>.json`, `SHA256SUMS`, overscan
  sizes and keys (see [m4-2-requirements.md](m4-2-requirements.md)).
- Bridge v1 README "Plates: they still have no parallax overscan": now
  required.

## 8. Do not

- Don't edit, commit in, or switch branches in the game repo's main checkout.
  Work only in your own fresh clone, and merge only through the PR.
- Don't paint the sky, stars, galaxy, planets, the bubble membrane, the
  forward glow or the starbow. Space stays alpha 0.
- Don't change ship canon (brief §3: the bubble, orientation, the spire, the
  levels, the bridge on the forward pole, scale) or the approved bridge look
  (palette, floor rings, consoles). Those are Mark's decisions.
- Don't add areas beyond the bridge.
- Don't put figures in the GLB. The game spawns them at `SPAWN_`.
- Don't add Python to the game repo. Its Python policy allows only listed
  oracles and harnesses; the game-side validator is Godot (`tools/validate_area.gd`).

## 9. Questions

Ask in a comment on the **art issue, game-repo #93**
(<https://github.com/sunholo-data/stapledons-godot/issues/93>). It replaces issue #1 for art. If
Mark rules in your session, record it per the game repo's `CLAUDE.md`
("Recording Mark's decisions").

**Settled by Mark (2026-10-03):** bucket prefix `areas/`; file name
`isocam_<area>.json`; bridge bundle PRs based on `main`; overscan for
`pan_range_m [6, 3]` with clamped parallax beyond it.

**Claude-side tasks in the game repo** (yours if nobody else has picked them
up; say so on #93 before starting):
- a `make area-assets` target that fetches `areas/<sha256>.<ext>` by the pins in
  `assets/areas/<area>/SHA256SUMS` (like `make sky-assets`);
- clamped plate parallax beyond `pan_range_m` in the M4.2 composite;
- an importer from `assets/characters/<entity_id>/` (the Codex deliveries)
  into `data/ai/core`, and the bucket upload of approved character PNGs.

## 10. Bridge v2: final art (Mark, attended 2026-10-03)

Bridge v1 stays the playable demo's bridge (game PR #97). For the final art,
Mark picked approach c of the quality study (game issue
[#93](https://github.com/sunholo-data/stapledons-godot/issues/93)). The
contract is [m4-2-requirements.md §9](m4-2-requirements.md#9-bridge-v2-amendments-2026-10-03).

**Rulings:**
1. Approach c, the illustrated plate, plus a Codex image-model refine pass
   toward the captain sprites' hand-illustrated finish.
2. Baked static light and shadow of ship geometry are allowed in the plate.
   The light is the **spire's** (the ship-fixed key light). Nothing from outside
   the ship is baked: the forward glow and the sky are a live layer (§9.3).
   Characters are never baked; space stays alpha 0.
3. 8K plate density (270 px/m), a 64 MB per-area bucket budget, and the
   palette pulled toward the captain sheet.
4. More ship structure in the panorama: the spire running down and the
   lower levels below the bridge (larger toward the equator, smaller past it),
   with space still mostly visible.
5. Claude-side game work: the `layers.play.plate` key, the projection shader in
   the M4.2 composite, a linear or compensated tonemap on the iso layer, the
   live glow term, and plate checks in `validate-areas`.

**Pipeline** (Blender repo, branch `art/bridge-v2`):
1. The build-out generator with `bridge_layout_v2.json` writes the walk, spawns,
   interactables, isocam and the reframed `cam_bridge.json` into
   `exports/stapledon/areas_v2/bridge/`. The walk geometry is v1's, unchanged.
2. `scripts/stapledon_bridge_v2.py` adds the study's richer kit (bevels baked
   into the shared kit meshes), exports the GLB, and renders spire-lit colour,
   Line Art and facing passes for the 8K plate and the panorama.
3. `scripts/stapledon_bridge_v2_paint.py` runs the deterministic paint pass
   under the alpha lock, with headroom (white point 0.88).
4. The Codex refine pass (brief and inputs in the public bucket under
   `refs/bridge_v2/`). Its result is re-locked and verified on the Claude side.
5. The in-engine integration PR to the game repo, with the ruling-5 work.
   It isn't opened until the refine is back.

