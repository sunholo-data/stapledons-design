# Brief: Blender assets for Stapledon's Voyage, starting with the ship interior

**Status:** DRAFT, pending Mark's decision on presentation style (D-5, see §2).
Everything here assumes option **A**. Sections marked ⚑ change under B or C.
**For:** an agent working in the Blender workspace (`~/dev/blender`) under its
`AGENTS.md` and `skills/blender-studio-workflow`.
**Game repo (consumer):** `sunholo-data/stapledons-godot`, a Godot 4
renderer with an AILANG simulation. Its mission loop imports your deliveries;
you don't edit that repo.

---

## 1. The game in one paragraph

You captain a ship inside a Higgs bubble that can travel at any fraction of
the speed of light. You have 100 years aboard. Every journey costs the
galaxy centuries. Civilizations rise and die while you travel, and the ~100
people inside the bubble become their own small civilization. The game is
**conversations and consequences**, not action: you plan journeys on a 3D
starmap, commit to them irreversibly, live with your crew during transit, and
arrive to a changed universe. The physics is real. What you see out of the
ship at speed (stars crowding forward and turning blue, darkness behind,
the black-hole lensing) is computed exactly. Tone: vast, beautiful, lonely,
bittersweet. Read `stapledons-design/vision/core-pillars.md` and
`vision/game-vision.md` first.

## 2. How the game is presented (option A; D-5 pending)

Three kinds of screen:

1. **Cosmic views.** The starmap, the relativistic sky, planets and black
   holes are rendered **live in Godot**. Not your job, except for the meshes
   listed in §6.
2. **Ship interior, which is your job.** A set of **fixed-camera tableaux**,
   one or more per deck. The player picks a deck from a menu. **There is no
   walking avatar**: the player is the unseen captain, and the camera is their
   eye. Each tableau is rendered in Blender as **layered 2.5D plates**. Godot
   adds subtle parallax, composites the **live relativistic sky** into every
   place where space is visible, and places crew figures and UI on top.
3. **People.** Conversation happens through **portrait panels** with emotion
   variants, plus **tiny figures** in the tableaux for scale and life.
   Portraits are out of scope for this first brief (§7).

Comparable feel: *Citizen Sleeper* and *80 Days* for conversation-first,
fixed-scene play. Moebius's *Airtight Garage* and *Arzach* for how the
spaces should feel.

⚑ **Under B** (live 3D shots in Godot), you deliver GLB deck models with
Godot-friendly materials instead of rendered plates; §5.3–5.5 become "export
the camera as a Godot Camera3D and the geometry as GLB".
⚑ **Under C** (walkable 3D), the scope grows by an order of magnitude; don't
start without a new brief.

## 3. The ship: canon (don't change without a design decision)

From `vision/design-decisions.md`, 2025-12-06 to 2025-12-08:

- **The bubble:** a transparent Higgs bubble about **100 m in radius**. Visible
  light passes through. Its boundary **glows faintly** where interstellar gas
  hits it at speed, most strongly on the forward side. It's a boundary, not a
  hull. There are no portholes: the levels are open to the bubble.
- **Orientation:** the ship is **vertical along its thrust axis**. Constant 1 g
  thrust is the gravity: **down = engines (aft), up = the direction of
  travel**. Standing on any level, you look **up** toward the bridge and
  observation deck, and **down** toward engineering.
- **The spire:** a monolithic, mysterious **Higgs Generator Spire** runs up the
  central axis, from engines to observation deck. It's visible from every
  level, and no crew can enter it. It may be the same object in every universe.
  It should feel *unknowable*: calm and beautiful, not a machine you
  understand. Archive terminals sit against the spire on every level.
- **Levels:** **10–20+ levels radiating outward from the spire**, **mostly open
  at the sides**, looking out through the bubble to space. They're connected
  by **lifts and ramps of different sizes arranged somewhat chaotically**.
  Level types:
  - **Command:** bridge and **observation deck at the very top (forward)**. The
    major-decision hub, with the cosmos overhead.
  - **Residential:** small houses and dwellings for ~100 people.
  - **Garden cathedral** (outer shell): where the crew remember planets.
    Cultural rituals. **"Sad but happy."**
  - **Commons:** markets, gathering spaces.
  - **Industrial:** engineering and fabrication. Mostly background.
  - **Archive shrine:** the AI's core room at the spire.
  - **Enclosed rooms inside levels:** medical bay, private quarters. These may
    be windowless and claustrophobic, which gives the contrast the design wants.
- **Archive robots:** small mobile robots of the ship AI, which can go anywhere
  the crew can.
- **Scale:** **tiny humans against cathedral-scale structures.** The spire and
  the levels dwarf people.

## 4. Visual style

**French 70s science-fiction comics:** Moebius (Jean Giraud), Philippe
Druillet, *Métal Hurlant*. From the 2025-12-08 decision:

1. **Organic and mechanical blended:** technology that looks grown.
2. **Saturated colour against vast emptiness.**
3. **Tiny humans in massive structures.**
4. **Clean, flowing curves** rather than sharp angles.
5. **Cathedral-like spaces** that evoke awe.

**In Blender, this is non-photorealistic rendering:**
- Flat or banded toon shading (Shader-to-RGB with colour ramps), a
  **restricted palette per deck**, and soft gradients in large surfaces.
- **Ink lines** from Line Art or Freestyle: slightly varying weight, darker on
  silhouettes, lighter on creases.
- **No photorealism,** no film grain, no lens effects that fight the style.
- Each deck has its own colour identity, while the ship stays one coherent
  object.
- **Style test first (§8, step 1).** Mark approves a style frame before any
  deck is built out.

## 5. The physics contract (the part that must be exact)

The sky seen from inside the ship is rendered live by Godot, with correct
aberration, Doppler colour and brightness for the ship's speed and direction.
Your renders must let it do that **exactly**.

### 5.1 Never paint space
**No stars, nebulae, planets or galaxy glow** in any plate. Every pixel where
space would be seen through the bubble is **transparent in the plates and
white in the sky mask**. The engine fills it with the physically correct sky.

### 5.2 Separate the bubble from the sky
The bubble boundary is its own layer: a thin, near-invisible membrane with
faint fresnel edges. Deliver:
- a **sky mask** (space visible through the bubble), and
- a **bubble-glow mask** (the boundary surface, so the engine can light the
  speed-dependent glow where it belongs).

### 5.3 Export every camera exactly
The engine renders the sky *from your camera*. A wrong direction or field of
view puts the starbow in the wrong place, which breaks the physics. For each
tableau, write `camera.json`:

```json
{
  "deck": "observation",
  "shot": "main",
  "resolution": [3840, 2160],
  "fov_vertical_deg": 55.0,
  "position_m": [0.0, 0.0, 92.0],
  "rotation_quat_wxyz": [0.707, 0.707, 0.0, 0.0],
  "axes": "ship frame: +Z = direction of travel (up, toward the bridge); origin = spire centre at the engine end; metres",
  "clip_m": [0.05, 2000.0],
  "blender_file": "scenes/stapledon_ship_v1.blend",
  "blender_camera": "CAM_observation_main"
}
```

- Keep the ship frame fixed: **Blender +Z = the thrust axis = up = the
  direction of travel.** The engine converts this to Godot's Y-up.
- Units are metres. Use a real perspective camera: no shift, and no lens
  distortion unless it's in the JSON.

### 5.4 Depth for parallax
Render a linear **depth pass** (32-bit EXR, metres from the camera) and split
each tableau into **3–4 depth layers**: background structure, midground, and
foreground framing. Each layer is a PNG with alpha. The engine uses layer
depths for subtle parallax, and the sky sits behind everything at infinity.

### 5.5 Where people stand
Mark **crew spawn points** in each tableau as empties named
`SPAWN_<role>_<n>`, and export them to the manifest in both pixel coordinates
and ship metres. Include standing height, so figures scale correctly with
depth.

## 6. First deliverables, in order

1. **Style frame.** One observation-deck view with a placeholder starfield
   composited, only to preview the style (not delivered as a plate). **Stop for
   Mark's approval.**
2. **The ship master model**, as one reproducible generator script:
   `scripts/stapledon_ship.py` → `scenes/stapledon_ship_v1.blend`. It contains
   the bubble, the spire and the level stack at blocking detail, with lifts and
   ramps. Every deck's tableaux are cameras in this one model, which is what
   keeps the geometry consistent across decks.
3. **Two outward decks, fully:**
   - **Observation deck** (top, looking up and forward: this is where the
     relativistic "tunnel" of stars shows);
   - **Bridge.**

   Each gets 1–2 shots.
4. **Two human-scale decks:**
   - **garden cathedral** (open, looking outward);
   - **a residential level** (small houses, open to the bubble).
5. **One enclosed room:** medical or quarters, with no sky visible, to prove
   the claustrophobic contrast.
6. **Ship exterior GLB:** the bubble and spire seen from outside, for
   departure and arrival shots and as the starmap marker. Keep the glTF
   materials simple; the engine adds the glow.

## 7. Out of scope for this brief

- **Crew portraits** (emotion variants). Moebius-style faces are an art-direction
  question in their own right, so they get a separate brief once the style
  frame is approved. Tiny *figures* for scale (silhouettes, simple posed
  meshes) are in scope as set dressing.
- Animation beyond an optional slow idle (spire shimmer, lift movement).
- Planets, black holes and the galaxy. The engine renders those.

## 8. Delivery format

In the Blender repo:

```
exports/stapledon/decks/<deck>/<shot>/
  layer_0_background.png    # RGBA, 3840x2160
  layer_1_midground.png
  layer_2_foreground.png
  sky_mask.png              # white = space visible (8-bit, exact edges, no AA fringe into walls)
  bubble_mask.png           # white = bubble boundary surface
  depth.exr                 # linear metres, 32-bit float
  camera.json               # §5.3
  manifest.json             # layers with depths, spawn points, deck type, palette note
  preview.png               # composite with a PLACEHOLDER sky, for review only; never used in-game
exports/stapledon/ship/stapledon_ship_exterior.glb
```

`manifest.json` extends the format in
`features/scene-based-interior-navigation.md`:

```json
{
  "deck": "observation", "shot": "main", "deckType": "outward",
  "layers": [
    {"id": "background", "file": "layer_0_background.png", "depth_m": 60.0},
    {"id": "midground",  "file": "layer_1_midground.png",  "depth_m": 18.0},
    {"id": "foreground", "file": "layer_2_foreground.png", "depth_m": 4.0}
  ],
  "sky": {"mask": "sky_mask.png", "bubble": "bubble_mask.png", "camera": "camera.json"},
  "spawns": [{"id": "SPAWN_captain_1", "px": [1720, 1510], "ship_m": [3.2, -1.0, 90.0], "height_m": 1.75}]
}
```

**Checks before hand-off** (on top of the Blender workflow's own
verification):
- **No painted space:** zero pixels in the plates where `sky_mask` is white
  have alpha > 0.
- **Masks agree with geometry:** the mask edges line up with the plate alpha,
  with no fringe gaps. Check it with a script, not by eye.
- **Camera round trip:** reprojecting a known ship-frame point (the spire tip)
  through `camera.json` lands within 1 px of where it appears in the render.
  This is what the engine relies on.
- **Previews looked at:** render `preview.png` for every shot and actually look
  at it. Post contact sheets for Mark's review.

## 9. Hand-off

- Commit the generator scripts, scenes and exports in the Blender repo.
- Tell the Stapledon mission through the ticket or message channel, or by
  commenting on stapledons-godot issue #1, with the export paths and a contact
  sheet.
- The game's mission loop imports the plates into `assets/decks/` as part of
  its interior milestone (R1 M4). **You don't edit the game repo.**

## 10. Decisions you must not make alone (ask Mark)

- The presentation style (D-5: A, B or C) and the style frame.
- Changing any canon in §3: the ship layout, orientation or spire.
- Anything that would put visible content where the sky belongs.
