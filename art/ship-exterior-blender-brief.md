# Brief: the ship from outside, and journey views

**Status:** approved scope; revised style frame pending approval (2026-09-28).
Mark requested a perfect spherical bubble, a readable boundary, and a forest
level inside the ship after reviewing the elongated first-pass exterior.
**Read first:** [art/README.md](README.md), then `vision/design-decisions.md`
(the ship canon, 2025-12-06 to 12-08), `features/phase4-polish/arrival-sequence.md`,
`features/future/opening-sequence.md` and `features/phase2-core-views/galaxy-map.md`.
**For:** an agent in `~/dev/blender`. **Needed by:** R1 M2 (galaxy-map marker)
and M4 (departure and arrival views).
**Reference:** spike v1 (stapledons-godot branch `spike/iso-bridge`,
`spike/out/iso_bridge_*.png`) shows the ship from outside. Its blockout
proportions are superseded by the interior canon: 100 m bubble, spire on the
axis, open levels.

---

## 1. What this covers, and what it doesn't

**You author:**
- the **exterior model** of the bubble ship;
- **camera choreography** for departure, arrival, emergence and orbit shots;
- the **galaxy-map ship marker**.

**The engine renders, live:**
- everything in the sky, including the SR and GR effects during these shots;
- planets;
- the black hole (the opening's "structure", and the end-game crossing);
- **the bubble's glow**.

The arrival and opening sequences are mostly physics spectacle: tumbling at
0.99c, the stars compressing forward, decelerating past the planets. The ship
model and the camera moves are what make them cinematic. **Never author the
effects themselves.**

## 2. The exterior model (`ext_ship.glb`)

- **Canon:** a **transparent, perfectly spherical Higgs bubble, ~100 m radius**, containing the whole
  inhabited ship.
  - **Spire:** runs along the vertical thrust axis. **Up is forward**; the
    engines are at the bottom (aft).
  - **Levels:** 10–20+ open levels radiate from the spire.
  - **Bridge:** a disc at the top of the spire, under the bubble's forward pole.
  - **Garden cathedral / forest level:** inside the spherical boundary. Keep
    trunks, branches, canopy and their supporting deck within the bubble; no
    detached tree or outboard garden annex.
  - **Silhouette:** equal bubble radii on all three axes. The inhabited levels
    should fill the sphere readably, rather than forming a long egg-shaped stack.
  - **Scale cues:** lifts, ramps, lights, tiny figures.
- **Seen from outside,** the inhabited interior shows through the transparent
  bubble: warm lights on the levels and the pale luminous spire. It should read
  as **a lantern-city drifting in the dark**. That's the image to aim for.
- **The bubble surface:** deliver it as a **separate mesh** (`bubble`) with a
  simple transparent glTF material. The engine replaces it with the physical
  boundary glow, which is strongest forward and scales with speed and
  interstellar density. Show a **readable boundary representation** in review
  captures, including at rest, so the containing sphere can be judged. A
  transparent rim or Fresnel treatment may serve as an engine preview; label
  it as a visual aid, not a validated physical glow. Never bake it into the
  ship model or paint it into a starfield.
- **LODs:**
  - `LOD0` for close departure and arrival shots (≤50k tris; reuse the interior
    kit at low detail);
  - `LOD1` for mid shots (≤10k);
  - `LOD2` as a marker-scale silhouette (≤1k).
- **Engines (aft):** the Higgs drive isn't a rocket and has **no exhaust
  plume**; 1 g thrust comes from the bubble physics. Give the aft pole a
  distinct, quiet form (the spire's root), and nothing that looks like fire.

## 3. Camera choreography (`cam_path_<shot>.json`)

Author each sequence as a **Blender camera animation**, and export it as
**ship-frame keyframes**: time, position, forward, up, vertical FOV, in the
same frame and convention as the interior `cam_*.json`, sampled at 30 fps. The
engine plays the path while the physics renders the sky, so everything stays
exact.

| Shot | Sequence | Notes |
|---|---|---|
| `departure_orbit` | M4: leaving a star system | a slow pull-back from the bridge to a wide shot of the lantern-city against the planet, then the ship turns bow (up) toward the target |
| `departure_burn` | M4 | a side-on wide shot as it accelerates: the physics does the rest (the sky crowds forward) |
| `cruise_idle` | M4: transit | slow orbits around the ship at 3 distances, for idle and loading moments |
| `arrival_decel` | M4 | the reverse of the burn: a wide shot as the sky relaxes to normal |
| `arrival_approach` | M4: α Cen, and later others | a push-in from wide to the bridge dome, ending on the interior bridge camera (hand-off to the play view) |
| `emergence_tumble` | opening | a tumbling camera around the ship (the arrival design's Phase 1). The *emergence* itself is the engine's black hole |

**Rules:**
- Paths are in the ship frame, and the camera never passes inside the bubble
  except in `arrival_approach`'s final hand-off.
- Keep them smooth (no more than 2 rad/s² angular acceleration, except the tumble).
- Each shot is 8–30 s.

## 4. Galaxy-map marker (M2)

- **A tiny readable glyph** for "you are here" on a 3D starmap of real stars,
  seen from far away: the `LOD2` silhouette (bubble outline and spire line),
  delivered as a GLB and as a 256×256 RGBA icon.
- **Heading indicator:** a subtle forward (up) mark along the spire axis.
- Don't author trajectory lines or time-dilation labels. They're UI, drawn by
  the engine.

## 5. First deliverables, in order

1. **Exterior style frame:** a `LOD0` lantern-city shot composited on a
   placeholder starfield in the spike (`spike/interior3.gd` can host an
   exterior camera, or a simple scene), at rest and at 0.99c. **Stop for
   Mark's approval.** Include a clear spherical boundary and show the forest
   contained inside it. The revised style frame is not yet approved.
2. **The galaxy-map marker** (GLB and icon). It's needed for M2 and is small,
   so it can go first after the style frame.
3. **The exterior model with its LODs.**
4. **Camera paths:** `departure_orbit`, `arrival_approach` and `cruise_idle`
   first (M4); then the rest.

## 6. Delivery

```
exports/stapledon/exterior/
  ext_ship.glb                 # LOD0-2 as separate nodes; bubble as its own mesh
  marker_ship.glb, marker_ship_256.png
  cam_path_<shot>.json         # ship-frame keyframes, 30 fps
  preview_<shot>_<rest|099c>.png   # in-engine captures (review only)
```

**Checks:**
- The bubble mesh is separate, centred at the ship origin, with equal radii
  on X, Y and Z (about 100 m).
- All authored ship geometry, including forest canopy, fits inside the sphere.
- Exterior review captures make the spherical boundary readable. Clearly label
  any preview-only boundary shader; production glow remains engine physics.
- There's no emissive exhaust.
- Camera paths replay in the engine without clipping through the bubble
  (except the approach hand-off).
- The marker reads at 24 px.

## 7. Mark decides
The exterior look (style frame), the marker glyph, and the shot list.
