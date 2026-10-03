# M4.2 requirements: the area-bundle contract on one page

**What this is:** the contract between the art threads and the game's iso
interior (R1 M4.2), on one page. Both handoffs link here:
[handoff-m4-2-blender.md](handoff-m4-2-blender.md) (the bridge bundle) and
[handoff-codex-art.md](handoff-codex-art.md) (the captain sprite and portraits).

**Sources (this page summarises them; where they disagree, this page and the
2026-10-03 rulings win):** [ship-interior brief](ship-interior-blender-brief.md)
§5, §6 and §9; the game repo's
[M4 design doc](https://github.com/sunholo-data/stapledons-godot/blob/main/design_docs/planned/r1/m4-first-journey.md)
§M4.0 and §M4.2 (V17 is the field-for-field schema check) and its
[sprint plan](https://github.com/sunholo-data/stapledons-godot/blob/main/design_docs/planned/r1/m4-first-journey-sprint.md);
the rulings in `vision/design-decisions.md`, 2026-10-03.

**Bridge v2 (Mark, attended 2026-10-03):** the final bridge adds an
illustrated, camera-projected **play plate**, baked **spire light**, an 8K
density and a reframed panorama. Those amendments are collected in
[§9](#9-bridge-v2-amendments-2026-10-03). Where §1–§8 and §9 differ, §9 wins
for a bundle that declares `layers.play.plate`.

## 1. The bundle: one folder per area

The game loads `assets/areas/<area>/` in `sunholo-data/stapledons-godot`.
R1 has one area, the bridge (ruling 1: bridge only in R1).

| File | Required | What |
|---|---|---|
| `manifest.json` | yes | §2 |
| `cam_<area>.json` | yes | the panorama's **perspective** camera, §3 |
| `pano_<area>.png` | yes | interior panorama, RGBA, alpha exactly 0 wherever space shows |
| `play_<area>.glb` | yes | the walkable iso play area, §5 |
| `fg_<area>.png` | yes | foreground silhouettes, RGBA, edges and bottom only |
| `isocam_<area>.json` | yes, from 2026-10-03 | the **orthographic** iso play camera, §3 (ruling 1) |
| `plate_<area>.png` | yes, when `layers.play.plate` is declared (v2) | the illustrated play plate, projected through the isocam onto the GLB, §9 |
| `plate_<area>_facing.png`, `pano_<area>_facing.png` | v2, recommended | live-glow hints: forward-facing mask, §9.3 |
| `SHA256SUMS` | yes, when binaries live in the bucket | pins for the PNGs and GLB, §8 |
| `validation.json` | recommended | the Blender-side validator's output |
| `preview_rest.png`, `preview_099c.png` | review only | in-engine captures; the loader ignores them |

The loader refuses a bundle with a missing required layer, and keeps and
ignores unknown keys. So additions (like `isocam_`, `overscan`) never break it.

## 2. `manifest.json`

Required, field for field (M4.0, V17):

```json
{
  "area": "bridge",
  "layers": {
    "panorama":   {"file": "pano_bridge.png", "parallax": 0.15},
    "play":       {"file": "play_bridge.glb", "iso_pitch_deg": -14, "iso_yaw_deg": 45, "iso_size_m": 16},
    "foreground": {"file": "fg_bridge.png", "parallax": 1.6}
  },
  "camera": "cam_bridge.json",
  "focus_m": [-8.0, 1.0, 9.0],
  "sky_visible": true
}
```

- **Optional, game side:** `placeholder` (bool, default false). True shows a
  "placeholder art" tag in the HUD.
- **Added 2026-10-03 (optional to the loader, required of the Blender
  delivery):**
  - `layers.play.camera`: `"isocam_bridge.json"`;
  - `layers.panorama.overscan_px` and `layers.foreground.overscan_px`:
    `[x, y]`, pixels of margin on **each** side (§4);
  - `pan_range_m`: `[x, y]`, the camera pan the overscan was sized for.
- **Already carried by bridge build-out v1** (kept, ignored by the loader):
  `version`, `status`, `palette_note`, `play_origin_ship_m`, `interactables`,
  `spawns`, `walk`, `source_scene`, `source_layout`, `validation`,
  `panorama_excluded_near_camera`.
- **`focus_m` is in the GLB's frame** (glTF, Y up, metres, origin at
  `play_origin_ship_m`), not in the ship frame.

## 3. Frames and cameras

**`ship_frame`** (M4.0 `interior/ship_frame.gd`; ledger D-14): metres;
**ship +Z = the direction of travel = up** for the whole journey (no flip);
origin = the bubble's centre. Blender works in this frame directly. The engine
maps ship +Z onto the live heading, so the starbow sits overhead.

| File | Frame | Contents |
|---|---|---|
| `cam_<area>.json` | ship frame | `shot`, `frame`, `position_m`, `forward`, `up`, `fov_vertical_deg`, `resolution`, `panorama`. A real perspective camera, **vertical sensor fit**, no shift, no lens distortion. With overscan it describes the **whole** plate (§4) |
| `isocam_<area>.json` | GLB frame (Y up) | `shot`, `frame`, `projection: "orthographic"`, `pitch_deg: -14`, `yaw_deg: 45`, `size_m: 16`, `size_axis: "vertical"`, `rotation_order: "YXZ"`, `focus_m`, optional `v_offset_m` (the spike used 0.38 × size). Same numbers as `layers.play` |
| `play_<area>.glb` | glTF: +Y up (= ship +Z), metres | origin at `play_origin_ship_m`; bridge: ship `[0, 0, 82]`, so the needle tip at ship z 98 is GLB y 16 |

**Projection checks:** the orthographic iso camera shows `size_m` = 16 m of
**screen height**. Pixels per metre in the play layer = viewport height / 16
(67.5 px/m at 1080 lines; 135 px/m on a 2160-line plate). A vertical 1.75 m
figure, seen from 14° above, spans 1.75 × cos 14° = 1.70 m of screen height:
about 115 px at 1080 lines, and about 67 px in the spike's 1120×630 review
captures (the "reads at 70 px" check in the character brief).

## 4. Parallax and overscan (ruling 1)

| Layer | Pan factor | Who |
|---|---|---|
| galaxy, stars, forward glow | 0 (at infinity) | engine |
| panorama | 0.15 | Blender |
| play GLB | 1 | Blender |
| foreground | 1.6 | Blender |

A plate slides `parallax × pan_m × px_per_m` pixels when the camera pans, so
each plate needs that much margin on each side:
`overscan_px = ceil(parallax × pan_range_m × plate_height_px / iso_size_m)`,
rounded up to a multiple of 16. With the bridge's `pan_range_m = [6, 3]`
(Mark, 2026-10-03) and 2160-line plates:

| Plate | `overscan_px` [x, y] | Full plate size |
|---|---|---|
| panorama | [128, 64] | 4096 × 2288 |
| foreground | [1296, 656] | 6432 × 3472 |

- **Beyond `pan_range_m` the engine clamps each plate's parallax offset**
  (Mark, 2026-10-03), so a plate edge never shows even when the camera
  follows the captain to the 22 m rim.
- The **centre 3840 × 2160** of each plate is the pan-0 view, unchanged from
  build-out v1. (v2: the panorama is reframed, §9.4; the rule still holds for
  the foreground.)
- **Panorama:** render the whole plate from the same camera position and
  orientation, widening the sensor: `fov_vertical_deg` becomes
  `2·atan(tan(39°) · H_full / 2160)` (81.2° for 2288). `cam_<area>.json` records
  the full `resolution` and the widened FOV, so the 1 px round trip still holds.
  The engine renders the sky through that same camera at the same scale.
- **Foreground:** the "centre 40% of columns is alpha 0" and "top third is
  alpha 0" rules apply to the centre view region; the margin follows the same
  edge-and-bottom rule.

## 5. The play GLB

- **Clean geometry, flat Principled base colours, nothing baked.** The engine
  applies toon shading and an ink outline (width 0.03) to every mesh. Under
  5 MB; reuse kit pieces as linked duplicates (v1: 1.34 MB, 62k triangles).
  (v2: the GLB still bakes nothing; with a declared plate the engine projects
  the plate onto it instead of toon-shading it, and the area's binaries share
  one 64 MB budget, §9.5.)
- **Collections:** `WALK_<area>`, `INTERACT_<area>` (one object each, named by
  function), and empties `SPAWN_<role>_<n>`. The game spawns its own figures
  at the `SPAWN_` empties, so no crew are in the GLB.
- **Scale and grid:** real metres; layout on a 1 m grid, pieces on 2 m
  multiples; an adult is 1.75 m. The area template carries a 1.75 m scale
  figure and a 2 m grid in a `REVIEW_` collection, for review renders only.
  It is never exported to the GLB or the plates (ruling 1).

### `WALK_` rules (ruling 1)

| Rule | Value | Check |
|---|---|---|
| Flat | every deck walk vertex within ±0.03 m of its deck plane (bridge: GLB y = 0) | validator: max abs(y) |
| Max slope | 10° from horizontal for any `WALK_` face (ramp heads only; decks are flat) | validator: face normal within 10° of +Y |
| No steps | no vertical jump over 0.05 m between adjacent `WALK_` faces | validator |
| Rim gaps | the walk edge stays ≥ 0.5 m inside the rim rail's inner face; a rail gap is walkable only where a ramp or lift `WALK_` continues through it; nothing walkable ever opens onto the void | validator: distance to rim; gap list from the layout |
| Obstacles | solid pieces (consoles, chair, benches, spire, shrine, frond bases, arch posts) are holes cut at their exact footprint, with no clearance added; floor inlays don't cut | the engine bakes with agent radius 0.35 m, height 1.8 m, max slope 10° |
| Connected | one region at agent radius 0.35 m | validator: flood fill |
| Spawns | every `SPAWN_` empty lies on `WALK_` (within 0.05 m vertically) | validator |
| Reach | every `INTERACT_` object has walkable floor within 1.5 m of its bounding box | validator |

## 6. Walking and interactables (M4.2)

- **Only the captain walks.** The captain is a static avatar sprite (no walk
  cycle) moved over `WALK_`, starting at `SPAWN_captain_0`. Its position
  **never goes to the simulation**. Crew avatars stand at `SPAWN_` points.
- **Interactables that do something in M4:**

  | `INTERACT_` object | Opens | Key |
  |---|---|---|
  | `console_navigation_0..2` | the M2 galaxy map (choose and commit a journey) | M |
  | `archive_terminal` | news from home, the legacy log, the codex | L, K |

  The others in bridge v1 (`captain_chair`, `console_decision_0..3`,
  `lift_arrival_pad`, `ramp_down`) stay in the GLB with no M4 function.
  Keyboard shortcuts keep the minimum path independent of walking.
- **The captain sprite's foot anchor** sits on the `WALK_` surface point. Its
  canvas and anchor are specified in
  [handoff-codex-art.md §4.1](handoff-codex-art.md#41-captain-avatar-sprite).

## 7. Lighting, and what art does NOT draw

**Who lights what:**
- **Plates:** rendered in Blender, banded toon shading (Shader-to-RGB, constant
  ramps) and Grease Pencil Line Art ink (Blender 5.2 has no Freestyle).
- **GLB:** lit by the engine (spike: one sun at rotation (−55°, 30°, 0°),
  violet ambient); bake nothing.
- **Sprites and portraits:** soft warm key light from the upper left and a
  delicate cool rim, as in the accepted Medic.

**Never author these.** The engine renders them live, physically exact, and
any painted version is wrong the moment the speed changes:
- the sky: stars, the galaxy, nebulae;
- planets and black holes;
- the bubble membrane;
- the forward glow and the starbow, and any Doppler tint;
- lens flares;
- figures in the GLB, baked shadows, HUD text.

Wherever space shows, alpha is exactly 0.

(v2, §9.3: the play plate and the panorama **may** carry baked static light
and shadow of ship geometry from the spire, and nothing else. Every item in
the list above stays forbidden, and characters are never baked.)

## 8. Checks, and delivery

| Check | Blender side (now) | Game side (once M4.0 lands) |
|---|---|---|
| plates 3840 × 2160 view region (+ overscan), RGBA; alpha exactly 0 where space shows | `scripts/validate_area_bundle.py` | `make validate-areas BUNDLE=assets/areas/bridge` |
| manifest and camera schema | validator | `validate-areas` |
| camera round trip: needle tip through `cam_` lands within 1 px in `pano_` | validator (v1: 0.42 px) | `validate-areas` |
| GLB: Y up, metres, `WALK_`/`SPAWN_`/`INTERACT_` present, no `REVIEW_` nodes | validator | `validate-areas` |
| `WALK_` rules (§5) | validator, extended | M4.2 bakes the navmesh |
| previews captured in the engine and actually looked at | `scripts/stapledon_area_review.py` | `make m4-smoke`, review builds |

**Delivery (ruling 3):** a PR to `sunholo-data/stapledons-godot` adding
`assets/areas/<area>/`, with the renders attached. PNGs and GLBs go to the
public bucket as `gs://stapledons-voyage-assets/areas/<sha256>.<ext>` (the
same content-addressed scheme as `sky/` and `ai/`, ledger D-18), pinned in
`assets/areas/<area>/SHA256SUMS`, one line per file: `<sha256>  <file>`.
PRs are based on `main`. Review happens on the PR. Questions go in a comment
on the art issue, game-repo [#93](https://github.com/sunholo-data/stapledons-godot/issues/93). The prefix `areas/`, the name
`isocam_<area>.json` and the path `assets/characters/<entity_id>/` are
confirmed (Mark, 2026-10-03).

## 9. Bridge v2 amendments (2026-10-03)

Mark (attended, 2026-10-03) picked **approach c** of the bridge v2 quality
study (game issue [#93](https://github.com/sunholo-data/stapledons-godot/issues/93)):
an illustrated plate, a Codex image-model refine pass, 8K, a palette toward
the captain sheet and a reframed panorama. Recorded in
`vision/design-decisions.md` (2026-10-03, "Bridge v2").

### 9.1 The play plate

- **What:** `plate_<area>.png`, RGBA. The play layer as an illustration,
  rendered in Blender through the **isocam** (same pitch, yaw, focus and
  `v_offset_m`, orthographic, no shift) and **projected by the engine onto the
  play GLB**. The camera is orthographic and only pans, so the projection is
  exact on every visible surface at every pan. The GLB keeps supplying
  coverage (alpha), walk, interactables and depth (sprite occlusion).
- **Manifest:** `layers.play.plate`:

  ```json
  "plate": {"file": "plate_bridge.png", "projection": "isocam",
            "resolution": [10920, 5940], "size_m": 22.0, "size_axis": "vertical",
            "px_per_m": 270, "covers_pan_range_m": [6, 3], "baked_light": true}
  ```

  `size_m` is the plate's vertical extent in metres; it is centred on the
  isocam's pan-0 view centre (`focus_m` + `v_offset_m` along the camera's up).
- **Density: 8K** (ruling 3): 270 px/m, twice the 4K play density. The plate
  covers the whole pan range, so for the bridge it is
  2 × (3840 + 2·6·135) × 2 × (2160 + 2·3·135) = **10920 × 5940**.
- **Alpha lock:** the plate's alpha is the render's own coverage (plus the
  solid ink lines). No paint pass, by hand, by script or by an image model, may
  change it. The Blender pass enforces it; the Codex refine is re-locked and
  verified on the Claude side (§9.6).
- **Colour bleed:** RGB may extend a few pixels past the silhouette under
  alpha 0 (the projection samples there at edges). Those RGB values are never
  shown as space, because the GLB, not the plate's alpha, decides coverage.

### 9.2 Palette

Pulled toward the captain sheet (ruling 3): cream #F2E3C7, ochre #D9A340,
violet #3B2959 and ink #423555 as on the sheet. Teal, coral, mint and leaf are
desaturated and warmed as gouache pigments, and the spire is a warm pale
white. The values live in the Blender layout (`bridge_layout_v2.json`).

### 9.3 Light: the spire is baked, everything from outside the ship is live

- **The spire is the dominant, ship-fixed light source**, so it is baked as the
  key light, with its shadows, into the plate and the panorama. That is what
  makes baking valid: it never changes with speed or heading. A low violet
  bounce fills the shadow sides. **No other light is baked**, and nothing from
  outside the ship.
- **Live, in the engine:** the forward bubble-wall glow (relativity 0.6.0
  `glowEmittanceAt`, sim `ship.ism.glow_pole_w_m2`) and the relativistic sky
  vary with speed and direction. They become a thin **live layer** on the iso
  layer: a directional tint plus a rim term, driven by the sim's glow value and
  the forward sky brightness. Emittance → radiance is **/π** for a Lambertian
  wall (M4.6a eval N3).
- **Headroom:** plates are painted with a soft shoulder to a white point of
  **0.88**, so the live term can brighten forward-facing edges without clipping.
- **Facing hints:** `plate_<area>_facing.png` and `pano_<area>_facing.png`,
  linear 8-bit grey at half resolution: `max(n · ship +Z, 0)`, the
  forward-facing mask. Ship +Z is the direction of travel, toward the glowing
  forward wall. On the play layer the engine may use the GLB's own normals
  instead (the plate is projected onto real geometry). The panorama has no
  geometry, so its mask is how the live term reaches it.
- **Still never authored:** everything in §7's list, figures, and any light from
  outside the ship. Space stays alpha 0.

### 9.4 The panorama: the forward sky first

- **The bridge panorama prioritises the forward sky** (Mark, 2026-10-03):
  - It keeps v1's **up-looking** camera, so the forward sky and the starbow stay
    in view at speed.
  - The spire's needle reads in it and stays the round-trip anchor.
  - Painted with the v2 look: spire-lit, paint pass, headroom.
- **The bridge does not depict the lower levels.** The bridge is the open top of
  the ship. Its perspective can't show the levels below it, which the v2 study
  confirmed: they lie beneath the deck.
- The round-trip anchor can be declared in `validation.needle_tip_ship_m`. Any
  point that is the topmost solid pixel of its column will do; the default is
  the needle tip.

### 9.5 Budget

- **64 MB per area** (ruling 3), for all of the area's binaries in the bucket:
  plate, panorama, foreground, GLB and facing hints. The bridge v2 build
  (Blender paint pass) measures **44 MB**:

  | File | Size |
  |---|---|
  | plate, 10920 × 5940 | 27.5 MB |
  | facing hints | 6.2 MB |
  | foreground | 6.3 MB |
  | panorama | 3.3 MB |
  | GLB, 199k triangles | 2.9 MB |

  The GLB stays small because its bevels are baked into the shared kit meshes,
  so instancing stays.
- Binaries stay out of git: content-addressed in `gs://stapledons-voyage-assets/areas/`
  and pinned in `SHA256SUMS` (§8).
- **Future areas (guidance):** lower levels are much larger than the bridge (up
  to 2.5× its radius at the equator), so the kit and textures are built for that:
  modular pieces on the 2 m grid, the gouache albedo procedural or
  texel-density based (not per-object atlases), and plates **tiled per area**
  when one 8K-density plate would exceed the budget or GPU texture limits
  (16384 px).

### 9.6 The Codex refine pass

After the Blender paint pass, a Codex image-model pass repaints the plate
toward the captain sheet's finish (ruling 1). It may change surface
rendering, ink quality and painterly finish. It **may not** change
silhouettes, geometry outlines, alpha, or add any sky, star, glow or membrane
content. The result is re-locked to the render's alpha and verified on the
Claude side (silhouette, edge alignment, region colour, tile seams, then
`validate-areas`) before it enters a bundle. Provenance notes follow the
captain's `generation_notes.md` format.

### 9.7 Checks added (game side, ruling 5)

`make validate-areas` gains plate checks for a bundle that declares
`layers.play.plate`:
- the resolution equals the declared one;
- the §8 alpha rules (no specks, no haze, exactly 0 where space shows);
- the plate's coverage matches the GLB rendered through the isocam, to within
  2 px of the silhouette;
- every GLB pixel visible within `covers_pan_range_m` has plate coverage.

### 9.8 Guidance for future level areas (not the bridge)

Later generations move down the bubble ship, so later areas are lower levels.
The canon (ship-interior brief §3) is guidance for designing those areas:
- **Each lower level is wider**, down to the equator (radius ∝ 0.55 · R · sin θ),
  and they **narrow again past it**.
- **Each lower level has a roof:** the underside of the level above is its
  ceiling. Only the bridge is open-topped.
- **Open design question** (for when those areas are designed): an enclosed
  level sees the sky only past its rim, between its floor and the ceiling
  above, and at the gap to the bubble. Its sky visibility and the
  forward-sky/starbow framing therefore differ from the bridge's. How the
  live-sky layers and panorama apply there is undecided.
- The Blender backdrop script (`stapledon_ship_backdrop.py`, branch
  `art/bridge-v2`) and its look-down camera (`backdrop_view` in
  `bridge_layout_v2.json`) are parked for that work, or for a rim or lift view.
