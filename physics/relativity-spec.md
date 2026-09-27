# Relativity visuals: accuracy spec

**Status:** Normative. Every SR/GR visual must meet this spec and ship with tests.
**Pillar:** Hard Sci-Fi Authenticity, which the design docs call "SR effects
must be accurate in all directions".

## 1. Conventions

- **n:** unit vector from the observer toward the source, in the galaxy (rest)
  frame.
- **n':** unit vector toward where the source *appears* in the ship frame.
- **Velocity:** a unit direction β̂ plus a speed β, with c = 1.
  - γ = 1/√(1−β²)
  - Rapidity φ = atanh β; the simulation integrates φ.
- **D:** Doppler factor, ν_observed/ν_emitted. D > 1 means blueshift.

## 2. Special relativity (moving observer, flat space)

| Effect | Formula | Check values (must pass) |
|---|---|---|
| Aberration | n' = (n + ((γ−1)(n·β̂) + γβ) β̂) / (γ(1 + β n·β̂)); scalar form cos θ' = (cos θ + β)/(1 + β cos θ) | 90° at 0.9c → 25.842°. 90° at 0.5c → 60°. Straight ahead and straight behind are unchanged. Result is unit length. |
| Inverse aberration (sky sampling) | n = aberrate(n', −β) | Round trip error < 1e-6 |
| Doppler | D = γ(1 + β cos θ) = 1/(γ(1 − β cos θ')) | Ahead at 0.9c: √19 = 4.3589. Astern: 0.22942. Appearing at 90°: 1/γ |
| Spectrum | A blackbody at T is seen as a blackbody at D·T (because I_ν/ν³ is invariant) | |
| Point-source brightness | F'_band/F_band = (Y(D·T)/Y(T)) / D², where Y is CIE luminance of the Planck spectrum | Bolometric total = D² |
| Extended-source brightness (sky, nebulae) | Radiance I' = D⁴ I bolometric; in a band, the radiance of a blackbody at D·T | |
| Colour | Planck × CIE 1931 2° observer → XYZ → linear sRGB (D65). Out-of-gamut colours are desaturated at constant Y | 2856 K → (0.4476, 0.4074) ±0.002. 6500 K → (0.3135, 0.3236). |
| 3D objects nearby (planets, ships) | Aberrate every vertex. For rapidly moving geometry, use retarded-time positions (Terrell–Penrose rotation) | Sphere stays circular in outline |

**Rendering requirements**

- **HDR everywhere.** Use float render targets. Tonemapping happens only at the
  end, and no 8-bit clamp may come before it. At 0.99c, D reaches 14, and the
  forward "headlight" is a real effect that has to survive.
- **float64 for γ and 1−β.** Compute them on the CPU and pass them in.
  - In a shader, 1+β cos θ must be rewritten as (1−β) + β(1+cos θ) when
    cos θ < 0.
  - Godot's `Vector3` is float32. Keep physics that needs precision in scalar
    float64 values.
- **Stars behind the camera must be culled before projection.** Otherwise
  clip-space w ≤ 0 mirrors them back into view (a bug found in the spike).
- **Temperature lookup tables must be finite and monotonic** across the whole
  range the Doppler factor can reach. In the spike, a float32 underflow at
  300 K produced NaN, which rendered as an infinitely bright blob.

## 3. General relativity (Schwarzschild; the black-hole feature)

The design docs (`gr-visual-mechanics.md`) list full ray tracing as a
non-goal. This spec replaces that with an accurate method that is still
affordable: **precomputed null geodesics**.

| Quantity | Correct value | Old implementation |
|---|---|---|
| Event horizon | r_s = 2GM/c² | The black disk was drawn at r_s ✗ |
| Photon sphere | r = 1.5 r_s | A three-sample blur at 1.5 r_s |
| Shadow (critical impact parameter) | b_c = (3√3/2) r_s ≈ 2.598 r_s. Angular radius seen from radius r: sin α = (b_c/r)·√(1 − r_s/r) | Wrong: drawn at r_s |
| Deflection | Exact: integrate d²u/dφ² = −u + (3/2) r_s u² (u = 1/r). Weak-field limit α = 2 r_s/b (radians) | Used as a UV shift, and the CPU and GPU pushed in opposite directions ✗ |
| Einstein ring | Present for any source behind the hole. Secondary and higher-order images inside it, crowding toward b_c | Missing ✗ |
| Observer gravitational shift | A static observer at r sees incoming starlight blueshifted by 1/√(1 − r_s/r). Background light is not redshifted by where it appears on screen | Computed from screen distance ✗ |
| Accretion-disk emission (later) | g = √(1 − 3r_s/2r)… plus disk Doppler; I_obs = g⁴ I_emit | n/a |

**Method**

1. On the CPU at load time, integrate the Binet equation for a grid of impact
   parameters b (dense near b_c). Store the total deflection angle Δφ(b) and a
   "captured" flag in a 1D float texture.
2. Per pixel, build the view ray. First apply the observer's own motion by
   inverse aberration, using the static-observer frame at r. Then compute b
   from the angle between the ray and the direction to the hole, look up
   Δφ(b), rotate the ray about the hole axis, and sample the sky cubemap or
   starfield. Captured rays render black.
3. Stars can't be resampled from a cubemap without losing their point
   sharpness. Transform each star instead: apply the inverse lens map to its
   direction to find image positions (primary and secondary), with
   magnification from the Jacobian.

**Check values:** shadow radius 2.598 r_s at large r; Einstein ring angle for a
chosen geometry; the weak-field limit of Δφ matching 2r_s/b to 1% at
b = 100 r_s; photon-ring images converging on b_c.

## 4. Test requirements

- A **CPU reference** (`physics/*.gd` in the Godot repo) with a unit test for
  every row of the tables above.
- **GPU vs CPU golden tests:** render synthetic single sources and require the
  sub-pixel centroid to match the reference projection within 0.75 px. The
  spike achieves under 0.1 px. Also include hidden or invisible cases: behind
  the camera, and redshifted below the visible band.
- **Reference renders** at 0, 0.5, 0.9, 0.99 and 0.999c, each looking forward,
  to the side and astern, and near the black hole at 10, 5 and 3 r_s. These
  are regenerated and diffed in CI.

## 5. Audit of the Go implementation (commit `930eca1`)

| # | File | Defect |
|---|---|---|
| 1 | `engine/relativity/transform.go:115` | Photon-direction formula applied to a source direction, so aberration had the wrong sign (stars moved backward) |
| 2 | `engine/shader/shaders/sr_warp.kage:66` | Inverse aberration used +β instead of −β. Screen radius mapped linearly to angle, which is only valid on-axis |
| 3 | `sr_warp.kage:107` | Doppler used the rest-frame angle together with an apparent-frame formula |
| 4 | `sr_warp.kage:130–136`, `space_view.go:287` | Beaming used D³, clamped to [0.08, 6] and LDR 1.0. Point sources should scale as D² bolometric |
| 5 | `relativity/color.go:124` | 7 fixed temperatures per spectral class. The 1000–40000 K clamp hides the fade to infrared and ultraviolet |
| 6 | `gr_lensing.kage:50` | Shadow drawn at r_s instead of about 2.6 r_s |
| 7 | `gr_lensing.kage:69–81`, `space_view.go:224` | Lensing was a UV displacement; CPU and GPU used opposite signs |
| 8 | `gr_redshift.kage:68` | Redshift computed from screen distance, which has no physical meaning |
| 9 | none | No tests for transform, colour or shaders; no SR/GR golden images |
