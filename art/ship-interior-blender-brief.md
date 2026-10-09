# Brief: Blender assets for Stapledon's Voyage, starting with the ship interior

> **Current scope and presentation — 2026-10-09:** Historical M4.2 handoff and five-layer presentation below are superseded for current playable work by the unified painted 3D ship (D-52=A), approved seven major tiers (D-34), lower architecture envelope R=95 m inside the R=100 m bubble (D-35), and shared geometry/sky observer. Existing bridge remains unchanged. Mark limits the first human-gameplay design to bridge and Level 1 Commons. Commons architecture and zoning are already approved; use the references in [human-life-bridge-commons.md](../features/next/human-life-bridge-commons.md), not the rejected barrel pavilion. Do not restart asset work from this old handoff.

> **Current task (2026-10-03):** [handoff-m4-2-blender.md](handoff-m4-2-blender.md)
> (finish bridge v1 for M4.2; bridge only in R1), with the contract in
> [m4-2-requirements.md](m4-2-requirements.md). Where they differ from this
> brief, they win. That includes the starting scripts (`stapledon_kit.py` and
> `stapledon_bridge_buildout_v1.py`, not `stapledon_interior_spike.py`) and
> delivery (a PR, not a zip on issue #1).

**Status:** APPROVED direction (Mark, 2026-09-28, ledger D-6). Bridge style
frame v1 (§7 step 1) is APPROVED for build-out (ledger D-16, 2026-10-01):
start §7 step 2. Deliver early and often: the game runs on whatever bundle is
current, and Mark reviews and tweaks each delivery.
**For:** an agent in the Blender workspace (`~/dev/blender`), working under its
`AGENTS.md` and `skills/blender-studio-workflow`.
**Read first:** [art/README.md](README.md), the art bible: shared style,
conventions, review process and the other briefs.
**Game repo (consumer):** `sunholo-data/stapledons-godot` (Godot 4 renderer plus
an AILANG simulation). Its mission loop imports your deliveries; you never edit
that repo.
**Reference implementation:** the working spike on branch `spike/iso-bridge`:
- `spike/interior3.gd`: the five-layer composite with parallax;
- `spike/v2/cam_*.json`: the camera contract;
- `spike/out/v3/*_parallax_sheet.png`: what it looks like.

The Blender generator for that blockout is in the Blender repo at
`scripts/stapledon_interior_spike.py`. Start from it; don't start from scratch.

---

## 1. The game in one paragraph

You captain a ship inside a Higgs bubble that can travel at any fraction of
the speed of light. You have 100 years aboard. Every journey costs the galaxy
centuries. Civilizations rise and die while you travel, and the ~100 people
inside the bubble become their own small civilization. The game is
**conversations and consequences**. The physics is real: what you see out of
the ship at speed is computed exactly. Stars crowd forward and turn blue, and
the sky behind and to the sides goes dark. Tone: vast, beautiful, lonely,
bittersweet. Read `vision/core-pillars.md` and `vision/game-vision.md` first.

## 2. How the interior is presented (decided)

The player is **inside the bubble**, walking the ship. **Up is forward**, the
direction of travel. The screen is built from five layers, far to near:

| # | Layer | Who makes it | When the camera pans sideways |
|---|---|---|---|
| 1 | Full-sky galaxy | engine (live, relativistic) | **fixed**: at infinity |
| 2 | Catalogue stars | engine (live, relativistic) | **fixed** |
| 3 | **Interior panorama:** the ship's own structure beyond the play area | **you** (rendered plate) | slow (about ×0.15) |
| 4 | **Isometric play area:** the room-sized space the player walks | **you** (GLB models) | 1:1 |
| 5 | **Foreground silhouettes:** fronds, struts, rails near the viewer | **you** (rendered plate) | fast (about ×1.6) |

Conversations use **large portraits** (a separate brief; §8).

## 3. The ship: canon (don't change it without a design decision)

From `vision/design-decisions.md`, 2025-12-06 to 2025-12-08 and 2026-09-28:

- **The bubble:** transparent, about **100 m in radius**, centred on the ship
  origin. It's a boundary, not a hull: the levels are open to it. Its faint glow
  (interstellar gas hitting it at speed, strongest forward) is rendered by the
  engine. **Never paint it.**
- **Orientation:** the ship is vertical along the thrust axis.
  **Blender +Z = up = the direction of travel**, and 1 g "down" points to the
  engines.
- **The spire:** a monolithic, calm, faintly luminous **Higgs Generator Spire**
  runs up the central axis and tapers to a needle at the top. It's visible from
  every level, and nobody can enter it. Archive terminals sit against it on
  every level.
- **The levels:** **10–20+ open levels radiating from the spire**, open at the
  sides with **wide gaps to the bubble**. The spike used 55% of the bubble's
  cross-section. They're linked by **irregular lifts and ramps**. **Level sizes
  follow the sphere** (Mark, 2026-10-03; guidance for future areas, as later
  generations move down the ship): the bridge, at the top, is the smallest
  platform and the only open-topped one. Each lower level is wider down to the
  equator (radius ∝ the bubble's cross-section, 0.55 · R · sin θ) and **roofed
  by the underside of the level above**; past the equator they narrow again.
  The bridge's views don't depict them. Level types:
  - **residential:** small dwellings;
  - **garden cathedral:** in the outer shell, "sad but happy";
  - **commons:** markets and gathering spaces;
  - **industrial / engineering:** mostly background;
  - **Archive shrine:** at the spire;
  - **enclosed rooms** (medical, private quarters): claustrophobic, no sky.
- **The bridge:** a disc at **the very top of the spire**, directly beneath the
  bubble's **forward pole**, which **is** its dome. It's the decision hub, and
  where the starbow gathers overhead at speed.
- **Scale:** **tiny humans in cathedral-scale spaces.** Levels are about 25 m
  apart, and the spire is about 14 m across at its base.

## 4. Visual style

**French 70s science-fiction comics:** Moebius (Jean Giraud), Philippe
Druillet, *Métal Hurlant*. From the 2025-12-08 decision:

1. organic and mechanical blended: technology that looks grown;
2. saturated colour against vast emptiness;
3. tiny humans in massive structures;
4. clean, flowing curves;
5. cathedral-like spaces.

**Techniques:**
- **Panoramas and foreground plates** (rendered in Blender): banded toon shading
  (Shader-to-RGB with a constant colour ramp) and **ink lines**. Blender 5.2 has
  **no Freestyle**, so use **Grease Pencil Line Art** (verify the Blender 5 API
  with a probe first; see the workflow skill).
- **The play area** (rendered in Godot): deliver **clean geometry and flat
  Principled base colours**. The engine applies its own toon shading and ink
  outline to the GLBs. Don't bake lighting into play-area textures.
- **Palette:** restricted per level, with the ship coherent overall. The spike
  palette (cream, coral, teal, ochre, violet, mint, leaf, pale spire) is a
  starting point, not canon.

## 5. The physics contract (the part that must be exact)

The engine renders the sky live, with correct aberration, Doppler colour and
brightness for the ship's speed. Your deliveries must let it do that exactly.

1. **Never paint space.** No stars, nebulae, galaxy or planets in any plate.
   Wherever space is visible, the panorama is **transparent** (alpha 0). The
   bubble membrane isn't rendered either (engine layer).
2. **Export every panorama camera** as `cam_<area>.json`, in the format the
   spike's `cam_*.json` uses:

   ```json
   {
     "shot": "bridge",
     "frame": "ship: +Z = direction of travel (up), origin = bubble centre, metres",
     "position_m": [16.0, -8.0, 83.7],
     "forward": [-0.271, 0.135, 0.953],
     "up": [0.852, -0.426, 0.303],
     "fov_vertical_deg": 78.0,
     "resolution": [3840, 2160],
     "panorama": "stapledon_pano_bridge.png"
   }
   ```

   Use a real perspective camera with a vertical sensor fit: no shift and no
   lens distortion. The engine renders the sky through exactly this camera, so
   the starbow lands where the architecture says "up".
3. **Aim panorama cameras upward and outward** wherever you can. The physics
   rewards it:
   - **Forward (up) is where the sky blazes at speed.** Sideways goes dark.
   - **The bridge panorama is mostly sky,** by design.
   - **Lower levels** should frame the ship's interior across the void (the
     spire, the level above as a ceiling, far decks) with **bands of open space
     past the level rims**.
4. **The foreground plates are the only layer nearer than the play area.** Keep
   them at the edges and the bottom: never across the centre, never over a
   likely spot for the player.

## 6. The isometric play areas (GLB kit)

- **Build a modular kit** in real metres: floor tiles or pads, walls and
  railings, the rim edge (open to the void), doors or arches, lift and ramp
  heads, Archive terminals, consoles, dwellings, garden beds, furniture, props.
  Lay areas out on a **1 m grid**, with pieces snapping on **2 m** multiples.
- **Areas are room-sized:** the camera shows about **16 m of height**, tilted
  back about 14°. Design every area to read well from that one angle, rotated
  45° (the spike's `iso_cam.rotation_degrees = (-14, 45, 0)`).
- **Walkable surfaces are flat and marked:** include a `WALK_` collection (a
  navmesh source) and `SPAWN_<role>_<n>` empties for crew positions. Put
  interactables (consoles, terminals, doors) in an `INTERACT_` collection, one
  object each, named by function.
- **Export:** one GLB per area, +Y up in glTF (Blender's +Z maps to it), with
  modifiers applied and flat materials. Keep them light (under 5 MB per area)
  and reuse kit pieces with instancing.

## 7. First deliverables, in order

1. **Style frame: the bridge.** One composed frame, made by rendering the
   panorama and the foreground plate, and dropping them plus your bridge GLB
   into the spike (`spike/interior3.gd`, bridge shot) with the live sky. Deliver
   the in-engine capture at rest and at 0.99c. **Stop for Mark's approval.**
2. **The ship master model** (evolve the spike generator into
   `scripts/stapledon_ship.py` → `scenes/stapledon_ship_v1.blend`): the bubble
   (not rendered), the spire, 10+ levels with their character, lifts and ramps.
   Every panorama camera lives in this one model, which keeps the geometry
   consistent everywhere.
3. **The bridge area in full:** the GLB play area plus its panorama, camera and
   foreground plate.
4. **Two lower areas:** a **residential level** and the **garden cathedral**,
   each with its play area, panorama, camera and foreground plate.
5. **One enclosed room** (medical or quarters): a play area with no sky and no
   panorama (or an interior-only one), proving the contrast.
6. **An exterior GLB** of the bubble and spire from outside, for departure and
   arrival shots and the starmap marker. The spike v1 view from outside is the
   reference.

## 8. Out of scope here

- **Large portraits** get their own brief once the style frame is approved.
  Moebius-style faces are an art-direction question in their own right.
- **Character models:** simple posed low-poly figures are fine for scale now.
  Walk cycles come later, in a separate pass.
- **Planets, black holes and the galaxy** are all rendered by the engine.

## 9. Delivery format

In the Blender repo, with scripts and scenes committed. `exports/` and
`renders/` are gitignored there, so hand over through the zip described below.

```
exports/stapledon/areas/<area>/
  play_<area>.glb           # isometric play area (flat materials; WALK_, SPAWN_, INTERACT_ collections)
  pano_<area>.png           # 3840x2160 RGBA, transparent where space is visible
  cam_<area>.json           # §5.2
  fg_<area>.png             # 3840x2160 RGBA foreground silhouettes (edges/bottom only)
  manifest.json             # layers, parallax factors, focus point, palette note
  preview_rest.png          # in-engine capture via the spike, at rest   (review only)
  preview_099c.png          # in-engine capture via the spike, at 0.99c  (review only)
exports/stapledon/ship/stapledon_exterior.glb
```

```json
{
  "area": "bridge",
  "layers": {"panorama": {"file": "pano_bridge.png", "parallax": 0.15},
             "play": {"file": "play_bridge.glb", "iso_pitch_deg": -14, "iso_yaw_deg": 45, "iso_size_m": 16},
             "foreground": {"file": "fg_bridge.png", "parallax": 1.6}},
  "camera": "cam_bridge.json",
  "focus_m": [-8.0, 1.0, 9.0],
  "sky_visible": true
}
```

**Checks before hand-off** (on top of the workflow skill's verification):
- **No painted space:** the alpha where space should be is exactly 0. Check it
  with a script.
- **Camera round trip:** project a known point (the spire needle tip) through
  `cam_<area>.json`; it must land within 1 px of where it appears in
  `pano_<area>.png`.
- **The GLB re-imports cleanly** (`make validate-glb`): the collections are
  present, and it's in metres with Y up.
- **Previews captured in the spike and actually looked at.**

## 10. Hand-off

- **In the Blender repo:** commit the generator scripts and scenes.
- **To the Stapledon mission:** attach the `exports/stapledon/areas/<area>`
  bundle as a zip to a comment on stapledons-godot issue #1, with the preview
  captures inline. The mission loop imports it into `assets/areas/` for M4.
  **Don't edit the game repo.**

## 11. Decisions you must not make alone (ask Mark)

- the style frame;
- any change to the canon in §3;
- anything that paints content where the sky belongs.
