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
| Light cone and Earth's link (signals-and-instruments §2–§3.1) | A static source at distance d is seen as it was at t − d/c. The Earth rate (Earth-seconds of history per ship-second) is the Doppler factor for Sol's direction; straight astern D = e^(−φ) = 1/(γ(1+β)), straight ahead e^(φ). A signal sent Δ after departure catches a ship on a straight outbound cruise at Δ/(1 − β) after departure. A plan announced from port A to port B is first heard at B a wait max(0, d_A + d_B − T) after arrival (T the leg's galaxy time; the triangle excess d_A + d_B − \|AB\| at high γ). A port on a heard beam's ray from Sol needs no wait: the beam already covers it. Earth aims ahead along a heard course, at where the ship will be when its light arrives; the aim swings at v⊥ / (r (1 − β_r)), never faster than βγ / r | Earth rate astern 0.99c: 0.070888; at β = 0.999999: 7.0711 × 10⁻⁴; ahead at β = 0.999999: 1414.2. Catch-up of a signal sent one day after leaving, 0.9c: 10 days. Excess from a port 40 ly out, a 30 ly sideways turn: 60 ly. Aim swing for a 0.99c crossing 40 ly out: 1.418° a year; the fastest at the top speed, 40 ly out: 2.773° a day (110.9° a day at 1 ly) |
| Spectrum | A blackbody at T is seen as a blackbody at D·T (because I_ν/ν³ is invariant) | |
| Point-source brightness | F'_band/F_band = (Y(D·T)/Y(T)) / D², where Y is CIE luminance of the Planck spectrum | Bolometric total = D² |
| Extended-source brightness (sky, nebulae) | Radiance I' = D⁴ I bolometric; in a band, the radiance of a blackbody at D·T | |
| Resolved emitter with surface structure (a star's disc; proposed, star-surfaces) | A map of local temperature T(x): each surface element is a blackbody at T(x), seen at D·T(x); at rest the map is normalised so the disc-integrated band luminance equals the uniform disc's | RS-24. The surface's apparent evolution rate is D × its proper rate, which follows from the emission time t − (light travel time); D is not applied twice (RS-25) |
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

**Frames**

- **Directions are galactic Cartesian (IAU):** x toward the galactic centre
  (l 0°, b 0°), y toward l 90°, z toward the north galactic pole (NGP). Galactic
  longitude increases to the *left* for an observer facing the centre with the
  NGP up.
- **Any map into an engine frame must be a proper rotation (det +1), defined in
  one place.** A reflection keeps every angle, so aberration, Doppler and
  brightness checks still pass, and a GPU-vs-CPU golden that shares the map
  cannot catch it. The Godot build fell into exactly this: its first map,
  (x, y, z) → (y, z, −x), has det −1 and rendered the whole sky as a mirror
  image (stapledons-godot D-28, 2026-10-03). The fix was (x, y, z) → (−y, z, −x).
- **Check (must pass, map-independent):** face the galactic centre with the NGP
  up. Antares (l 352°) and α Cen (l 316°) appear to the right of the centre, and
  Vega (l 67°) and the Scutum star cloud (l 27°) to the left. A ship's starboard
  side then faces l 270°, and its port side l 90°.

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
4. **Observer at finite r.** For an observer at radius r, tabulate δ(ψ, r): the
   bending of a ray seen at angle ψ from the direction to the hole. This is a
   2D table. It may be generated by the integrator or by the exact elliptic-
   integral form, provided the other cross-checks it.

**Check values** (IDs in §7):
- The shadow's critical impact parameter is 2.598 r_s (RS-9). Its angular
  radius is 14.27°, 27.69° and 45.00° at 10, 5 and 3 r_s (RS-10 to RS-12).
- **Weak field, a pair** (D-13, 2026-10-01): the deflection is within 1% of
  2r_s/b at b = 1000 r_s (measured 0.148%; RS-14), and within a relative 3e-4
  of 2r_s/b + (15π/16)(r_s/b)² at b = 100 r_s (measured 2.68e-4; RS-15). The
  first-order formula alone is 1.48% off at b = 100 r_s, which is why the old
  "1% at 100 r_s" check was replaced.
- The Einstein ring appears at the angle predicted for a chosen geometry
  (RS-Q4).
- Photon-ring images converge on b_c (RS-Q3).
- A static observer's clock-rate and blueshift factor is 1/√(1 − r_s/r) (RS-16
  to RS-20).

## 3b. General relativity (Kerr; the spinning hole)

**Release-1 domain:** uncharged Kerr; equatorial observers at R ∈ [2, 10⁶]
in units of r_s = 2GM/c²; one exact manifest spin per table set. Spin is
s = Jc/(GM²), 0 ≤ s < 1; the reference-accuracy and rendered-table domain is
s ≤ 0.998. The scalar API accepts the represented binary64 s, without an
independent spin complement. No accuracy claim concerns an unrepresented
decimal or the extremal limit. Ergosphere interiors, polar rendered observers,
free-fall ship dynamics, accretion disks, jets, charge, superradiance and Kerr
catalogue-star inverse maps are outside this release.

Here **u = 2R** is radius in GM/c², metric signature is (−,+,+,+), the spin
axis is +z, and θ is Boyer–Lindquist colatitude. Define
Σ = u²+s²cos²θ, Δ = (u−u₊)(u−u₋), A = (u²+s²)²−Δs²sin²θ,
u₊/₋ = 1 ± √((1−s)(1+s)), N = √(ΔΣ/A), ω = 2su/A and
h = √(A/Σ)sinθ. Frequencies below are c³/GM; physical angular rates multiply
by c³/(GM). Package radius-returning functions return R = u/2.
Reject nonfinite inputs before comparisons/square roots; enforce the relevant
exterior/static/timelike domain, including |b|<1 and a positive circular-clock
radicand. Being above the photon orbit permits a circular timelike orbit;
stability separately requires R≥R_ISCO of the chosen sense. At R=3, s=0.9,
the prograde orbit is stable and retrograde unstable; the prograde ISCO is a
readout below the unchanged R=2 visit floor, not a permitted visit radius.

| Quantity | Required formula / behavior | Check family |
|---|---|---|
| Horizons | R₊/₋ = (1 ± √((1−s)(1+s)))/2 | RK-1/2 |
| Static limit | R_SL = (1+√(1−s²cos²θ))/2; equatorial R_SL = 1 | RK-3 |
| Photon orbits | u_ph = 2[1+cos((2/3)acos(∓s))], minus argument prograde | RK-4/5 |
| ISCO | Z₁=1+(1−s²)^(1/3)[(1+s)^(1/3)+(1−s)^(1/3)], Z₂=√(3s²+Z₁²); u_ISCO=3+Z₂∓√((3−Z₁)(3+Z₁+2Z₂)) | RK-6/7 |
| Circular orbit, k=+1 pro / −1 retro | Ω=k/(u^(3/2)+ks); dτ/dt=u^(3/4)√(u^(3/2)−3√u+2ks)/(u^(3/2)+ks); signed ZAMO speed v_Z=(Ω−ω)h/N | RK-8/8b/12 |
| Horizon rotation | Ω_H=s/(2u₊) | RK-9 |
| Frame dragging; ZAMO clock | ω=−g_tφ/g_φφ=2su/A; lapse N=√(ΔΣ/A) | RK-10/11 |
| Static clock | √(1−2u/Σ), outside the static limit; zero is a boundary limit, not a static timelike observer | RK-13 |
| Shadow at infinity | λ=−(u³−3u²+s²u+s²)/(s(u−1)); η=4u²Δ/(u−1)²−(λ−s)²; α=−λ/sinθ_o, β=±√(η+s²cos²θ_o−λ²cot²θ_o) | RK-14–17 |

The closed forms and locally nonrotating frames follow
[Bardeen, Press & Teukolsky 1972, §§II–III](https://adsabs.harvard.edu/pdf/1972ApJ...178..347B)
(BPT72); the shadow curve follows Bardeen 1973, *Timelike and null geodesics in
the Kerr metric*, in *Black Holes (Les Astres Occlus)*. The separated geodesics
follow [Carter 1968](https://doi.org/10.1103/PhysRev.174.1559).
These citations supply the physical model; implementation checks below are
independent numeric evidence, not a claim that visual gates have passed.

**Frame and frequency contract.** Tables use the ZAMO tetrad
e₀=(∂t+ω∂φ)/N, e_r=√(Δ/Σ)∂u, e_θ=∂θ/√Σ, e_φ=∂φ/h.
Source direction n_Z points toward the source (§1); future incoming momentum
is p=E_Z(e₀−n_Zⁱeᵢ). Compute E∞=−p_t=NE_Z+ωL, λ=L/E∞ and
η=(p_θ/E∞)²+cos²θ(λ²/sin²θ−s²) from that projection.
Static hover moves at v_S=−ωh/N along e_φ relative to ZAMO. A circular
ship uses (Ω−ω)h/N; radial speed b relative to the static observer gives
v_Z=(b/γ_S,0,v_S), γ_Z=γ_Sγ_b. The ship tetrad is the **single pure boost**
from ZAMO along this combined vector; a sequential boost with Wigner rotation
is a different frame. For every mode dτ/dt=N/γ_Z. Pixel directions use
n_Z=aberrate(n_ship,−v_Z) (§2). The frequency ratio is ray-dependent:
g_total=γ_Z(1+v_Z·n_Z)(1−ωλ)/N. Static hover reduces to
1/√(−g_tt) for every ray. Multiplying static blueshift by a ZAMO orbit
Doppler factor is invalid. Keep precise quantities in float64 scalars and
use §2's stable Doppler form before float32 shader upload.

**Carter integration and capture.** Set E∞=1, P=u²+s²−sλ,
R=P²−Δ[η+(λ−s)²], Θ=η+s²cos²θ−λ²cot²θ. With Mino parameter ζ,
dλ_affine=Σdζ, evolve all four coordinates:
du/dζ=σ_r√R; dθ/dζ=σ_θ√Θ;
dφ/dζ=sP/Δ+λcsc²θ−s; dt/dζ=(u²+s²)P/Δ+s(λ−s sin²θ).
Trace toward the source with negative ζ, signs from incoming momentum;
retain unwrapped φ and recover the asymptotic direction using its accumulated
value and outgoing momentum. Locate simple radial/polar turning roots,
split the step at the event and flip only the corresponding sign once.
Constant-equator solutions stay on the equator; double radial roots are
critical rays. Axis rays require a regular angular chart. Never hide an
invalid potential by unconditional sqrt(max(0,potential)); any roundoff clamp
has a fixed scale-aware tolerance validated independently.
Longitude is undefined at a pole: keep unwrapped φ within each regular chart
and retain the chart-transition phase, so no winding or physical twist is
reset or reflected. A regular Cartesian angular direction plus accumulated
dragging is an equivalent geometric representation. Test exact λ=0 crossings
and arbitrary small nonzero λ on both sides of the axis at the same angular
accuracy budget. Exact-axis examples alone do not establish near-axis accuracy;
a coordinate singularity cannot be counted as capture or used to omit a probe.
Classify capture separately from integration by the real roots and allowed
intervals of R, initial past-directed radial branch and horizon access.
Returning rays have an accessible turning root; step exhaustion is an error,
never evidence of capture. Check null norm, constants, turns and convergence.

One regular lens-angle representation removes the radial dragging rotation
from a unit angular direction m. In past Mino time its equation is
m''=s²q e_z−(K+2s²q²)m, q=m_z, K=λ²+η. Let B be the positive root of
B²−(K−s²)B−s²η=0, A=−sign(n_θ)√(η/B), C=−λ/√(B+s²). Then
m=Rot_z(node)(cosν,C sinν,A sinν), ν'=√(B+s²q²), and the full node rate is
drag−λs²/[√(B+s²)(√(B+s²)+ν')]. The rationalized node rate has no polar
denominator. For η=0 retain the constant equator, ν'=|λ|, A=0,
C=−sign(λ); radial λ=η=0 has ν=0. The two stored bending/twist angles may
be δ_∥=ν+ψ−π and δ_⊥=node. Remove the flat-space arc before float32
storage, and reconstruct the actual Cartesian direction with A,C above.
These are unwrapped asymptotic phases; a finite-far BL φ is a different
coordinate and must not be substituted for the sky azimuth.

**Tables and limits.** Use n_Z=−e_r cosψ+sinψ(e_φ cosχ+e_θ sinχ),
ψ,χ in radians, χ periodic. Store the capture boundary angle ψ_b(χ,R),
and both unwrapped lens channels. If using logarithmic regularisation,
L=ln(tanh((ψ−ψ_b)/ψ_b)), fit each a_i independently and reconstruct
δ_i=stored_i−a_iL (i=∥,⊥). Twist finiteness must be demonstrated, never
assumed by setting a_⊥=0. Require cutoff/resolution convergence and a finite
whole-range audit, including boundary coefficients and the χ seam. Manifest
spin, inclination, tetrad, schema, package content and bin hashes must match
exactly; missing assets show an explicit unavailable view, without substituting
the nearest spin. The infinite-observer α,β curve is a far-row anchor;
finite-R silhouette comparisons use the same finite observer and frame.

**Tides and zero spin.** Canonical maths belongs in sunholo/relativity before
CPU/GPU mirrors. Tides require the full Weyl tensor, including its magnetic
part, projected into ZAMO then the same ship boost: E_ij=C_i a j b UᵃUᵇ,
relative acceleration −E·separation. A principal-frame scalar alone is
insufficient. Before claiming numeric tides, an independent metric-derivative
curvature oracle must verify trace, sign, units and both orbit senses/radial
motion at all accepted spins/domain edges. The reproducible metric-derived
curvature fixture below supplies numeric acceptance probes, in M⁻²; multiply
by 4 for package units c²/r_s² before the SI acceleration conversion.
Stretch is minus the minimum electric-tensor eigenvalue, squeeze the maximum;
retain the full tensor and its frame, not only those extrema. Finite fixture
coverage does not establish a continuum-wide conditioning bound. Failure to
verify blocks release, never authorizes a null or Schwarzschild substitute.
At s=0 call existing Schwarzschild scalar clock,
orbit-speed and tide exports directly; use a nonsingular circle for shadow
maths. At nonzero s on the equator evaluate the static clock as
sqrt(1−1/(u/2)), preserving the legacy operation order. The production s=0
renderer and legacy protocol streams keep their existing paths and bytes.

**Check tolerances and provenance.** RK closed forms and root/extremum values
use 1e-12 relative error; RK-16 uses 1e-9 absolute error. At photon endpoints,
α_min=−λ(u_pro), α_max=−λ(u_ret), β²=η, with |η|≤1e-10 M²
for float64 endpoint evaluation (not exact η=0). Rounded display text uses
half a unit in its last printed digit, separately from physics assertions.
RK-16 is precisely (1/N)Σ√(λ(u_j)²+η(u_j)), N=200000,
u_j=u_pro+j(u_ret−u_pro)/(N−1), j=0…N−1, exact inclusive endpoints,
equal weights. It is a uniform-u diagnostic, not an angular, area or perimeter
mean and not a player-facing shadow radius. RK-17 solves the visible-arc
β²=0 endpoints and maximizes β²; the 45° case is a maths test only.
RK-18 uses IAU 2015 nominal GM_sun=1.3271244e20 m³/s² and exact
c=299792458 m/s. The reproducible Decimal60 scalar oracle and fixture are
[`tools/kerr_ref.py`](https://github.com/sunholo-data/stapledons-godot/blob/2b47456b4ef43033607694e6c528911b8ab267db/tools/kerr_ref.py)
and [`tests/fixtures/kerr/rk.json`](https://github.com/sunholo-data/stapledons-godot/blob/2b47456b4ef43033607694e6c528911b8ab267db/tests/fixtures/kerr/rk.json):
`python3 -I tools/kerr_ref.py --check tests/fixtures/kerr/rk.json`.
The scalar fixture covers RK-1–13, RK-18 and RK-Q1. The maintained Decimal60
[`shadow` oracle](https://github.com/sunholo-data/stapledons-godot/blob/878236b05f1ff0d5e2e724eddfff717ada354be2/tools/kerr_ref.py)
now promotes the approved design's retained V32 calculation into
[`shadow.json`](https://github.com/sunholo-data/stapledons-godot/blob/878236b05f1ff0d5e2e724eddfff717ada354be2/tests/fixtures/kerr/shadow.json),
including RK-14–17 roots/extrema and the complete N=200000 sums.

Additional independent acceptance data at immutable game commit
[`878236b0`](https://github.com/sunholo-data/stapledons-godot/commit/878236b05f1ff0d5e2e724eddfff717ada354be2):

| Oracle source | Fixture and exact scope | Reproduction from game root |
|---|---|---|
| [`kerr_ref.py`](https://github.com/sunholo-data/stapledons-godot/blob/878236b05f1ff0d5e2e724eddfff717ada354be2/tools/kerr_ref.py), Decimal60 polynomial roots | [`shadow.json`](https://github.com/sunholo-data/stapledons-godot/blob/878236b05f1ff0d5e2e724eddfff717ada354be2/tests/fixtures/kerr/shadow.json), RK-14–17 at infinity | `python3 -I tools/kerr_ref.py --shadow` |
| [`kerr_curvature_ref.py`](https://github.com/sunholo-data/stapledons-godot/blob/878236b05f1ff0d5e2e724eddfff717ada354be2/tools/kerr_curvature_ref.py), Decimal60 analytic metric derivatives and full Riemann projection | [`curvature.json`](https://github.com/sunholo-data/stapledons-godot/blob/878236b05f1ff0d5e2e724eddfff717ada354be2/tests/fixtures/kerr/curvature.json), 80 tensors: s=0/.5/.9/.998 × u=4/6/20/2000000 × ZAMO/hover/pro/retro/radial b=−.5 | `python3 -I tools/kerr_curvature_ref.py` |
| [`kerr_null_ref.py`](https://github.com/sunholo-data/stapledons-godot/blob/878236b05f1ff0d5e2e724eddfff717ada354be2/tools/kerr_null_ref.py), compactified second-order Hamiltonian DOP853 | [`null.json`](https://github.com/sunholo-data/stapledons-godot/blob/878236b05f1ff0d5e2e724eddfff717ada354be2/tests/fixtures/kerr/null.json), 72 rays, 49 escaping: s=0/.9/.998 × u=4/6/20 × eight directions | `python3 -I tools/kerr_null_ref.py` |
| Same independent null oracle | [`nearcritical.json`](https://github.com/sunholo-data/stapledons-godot/blob/878236b05f1ff0d5e2e724eddfff717ada354be2/tests/fixtures/kerr/nearcritical.json), 12 escaping rays: s=.9/.998 × prograde endpoint/u=3/retrograde endpoint critical orbit × tangent perturbation 1e-5/1e-8, observer u=6 | `python3 -I tools/kerr_null_ref.py --nearcritical` |

These Python tools are test oracles, never production table generators. The
curvature oracle uses metric derivatives rather than the package's principal
Weyl construction. The null oracle uses independent radial polynomial topology,
a regular angular sphere chart and adaptive second-order integration rather
than the package's first-integral RK4 solver. Its reference environment is
NumPy 2.5.4 / SciPy 1.18.1 (scalar/curvature tools need only the standard library).
Null sky values include a checked asymptotic tail; stored BL t and chart φ are
at the recorded finite far sphere. A finite BL arrival time at infinity is not
claimed. Tolerance refinement, null/conservation and domain tests accompany
fixture matching; none substitutes for the near-axis or whole-range gates.
At the 1e-8 near-critical cutoff, the independent null oracle's worst recorded
refinement difference is 9.94e-6 rad, within the 1.2e-4 rad budget. The much
smaller ordinary-ray differences do not extend that claim to critical rays.
Package publication/evaluation, table coefficient fitting and cutoff/resolution
convergence, CPU/GPU goldens and visual review remain separate acceptance gates;
these reproducible headless data alone do not satisfy them.

The attended continuation's independent near-axis fixture at game commit
[`53d9bc65`](https://github.com/sunholo-data/stapledons-godot/commit/53d9bc65)
adds 336 rays (280 escaping), seven spins0/.1/.25/.5/.75/.9/.998,
u=4/6/2e6, both hemispheres and azimuth senses, with maximum oracle
refinement3.51e-15 and null residual1.38e-13. Reproduce with
`python3 -I tools/kerr_null_ref.py --nearaxis`; the source still requires
the pinned NumPy/SciPy environment. Discrete coverage is not a continuum
conditioning proof. The same commit records a400-ray sweep and a5,880-ray
near-axis sweep with zero solver failures on the regular/compact package
solver. Solver-failure coverage is separate from accuracy coverage.

**Implementation preflight disposition (2026-11-02):** the planned
single-F32-boundary/1536-column consumer format has not passed. At u=6,
s=.9,χ=.7 its1,535 refined midpoint probes give9.01e-4rad maximum even
with the exact boundary; single-F32 boundary rounding causes59 false
captures. Doubling to3072 columns reduces the former to2.26e-4rad, still
above1.2e-4; it does not repair boundary precision. This is failed
implementation evidence, not a changed physical accuracy budget. Review a
precision-preserving boundary and antipode interpolation/chart before any
production pins or visuals; do not narrow the cutoff or omit those probes.

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

**Kerr additions (GR13–GR17).** Assert every RK check family through the CPU
reference and `tests/test_physics.gd`. GPU-vs-CPU goldens in `make golden`
must cover: GR13 static-hover mask boundaries at s=0.9, R=10/5/3, with
the ZAMO boundary boosted to the same static observer (≤0.75 px); GR14
finite-R asymmetry and prograde pinch versus that CPU boundary (≤0.005
absolute, retaining GR13), plus a separate far-row convergence to the
infinity asymmetry 0.41211167869793890; GR15 fixed near-edge markers and
the frame-dragging twist (≤1 px); GR16 forced Kerr-path s=0 at those radii
versus Schwarzschild (≤0.5 px); GR17 deliberate boundary/seam mutations
failing GR13, twist/reconstruction mutations failing GR15 or the named
outgoing-direction probe. Each acting mutant must fail, then restore green.
Independent outgoing-direction and s=0 Carter/Binet probes use 1.2e-4 rad
at R≤100 and 1e-3 relative above. Run whole-range finite/seam/reflection
audits and strict-VM/interpreter byte parity for generated probes/rows.
Reference renders show the flattened off-centre shadow, frame dragging,
hover/prograde/retrograde views, and s=0 Schwarzschild agreement at 10/5/3.
Kerr catalogue-star splats, both image orders and rings are suppressed until
their inverse map is verified; a visible diagnostic must state unavailability.
Open and inspect the renders before review; no visual landing precedes Mark's
review. True settings preserve physical geometry/frequency transport; any
Processed exposure or frequency correction is explicitly an instrument mode.

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

## 6. Bubble-derived physics

The Higgs bubble is the game's one admitted hand-wave. Its consequences (the
photon drive, ISM drag and glow, the forward CMB, tides across the bubble,
hover power) are derived exactly in [higgs-bubble.md](higgs-bubble.md), with
check values HB-n. Two of them meet this spec:
- the forward CMB is a blackbody at D·T_CMB (§2 "Spectrum"), giving HB-62 to
  HB-70;
- tides near a hole are not shielded by the wall, which is why the M3 demo
  hole is Sgr A* (HB-71 to HB-87).

## 7. Check-value index

Stable IDs for tests and for the in-game Archive (`lore/archive/`), which cites
them in its front matter. As in `higgs-bubble.md`, each registry row holds one
number in the columns `ID | Quantity | Value | Unit`, and `—` means
dimensionless.

| ID | Quantity | Value | Unit |
|---|---|---|---|
| RS-1 | Aberration: a source at 90° (rest frame) seen from 0.9c appears at | 25.842 | deg |
| RS-2 | Aberration: a source at 90° (rest frame) seen from 0.5c appears at | 60 | deg |
| RS-3 | Doppler factor straight ahead at 0.9c (√19) | 4.3589 | — |
| RS-4 | Doppler factor straight astern at 0.9c | 0.22942 | — |
| RS-5 | Colour: chromaticity x of a 2856 K blackbody (± 0.002) | 0.4476 | — |
| RS-6 | Colour: chromaticity y of a 2856 K blackbody (± 0.002) | 0.4074 | — |
| RS-7 | Colour: chromaticity x of a 6500 K blackbody | 0.3135 | — |
| RS-8 | Colour: chromaticity y of a 6500 K blackbody | 0.3236 | — |
| RS-9 | Critical impact parameter b_c = (3√3/2) r_s | 2.598 | r_s |
| RS-10 | Shadow angular radius, sin α = (b_c/r)√(1 − r_s/r), at 10 r_s | 14.27 | deg |
| RS-11 | Shadow angular radius at 5 r_s | 27.69 | deg |
| RS-12 | Shadow angular radius at 3 r_s | 45.00 | deg |
| RS-13 | Shadow angular radius at 1.5 r_s (the shadow fills half the sky) | 90 | deg |
| RS-14 | Weak-field tolerance vs 2r_s/b at b = 1000 r_s (measured 0.148%) | 1 | % |
| RS-15 | Weak-field relative tolerance vs 2r_s/b + (15π/16)(r_s/b)² at b = 100 r_s (measured 2.68e-4) | 3 × 10⁻⁴ | — |
| RS-16 | Static observer factor 1/√(1 − r_s/r) at 10 r_s | 1.054 | — |
| RS-17 | Static observer factor at 5 r_s | 1.118 | — |
| RS-18 | Static observer factor at 3 r_s | 1.225 | — |
| RS-19 | Static observer factor at 2 r_s | 1.414 | — |
| RS-20 | Static observer factor at 1.5 r_s (√3) | 1.732 | — |
| RS-21 | Photon sphere radius | 1.5 | r_s |
| RS-22 | Reference speed for RS-1, RS-3 and RS-4 | 0.9 | c |
| RS-23 | Reference speed for RS-2 | 0.5 | c |
| RS-24 | Proposed (star-surfaces): disc-integrated V luminance of a temperature-mapped stellar disc ÷ the uniform disc's, at β = 0 (tolerance 1e-4 by quadrature, 1e-6 for a closed-form single spot, 0.5% GPU) | 1 | — |
| RS-25 | Proposed (star-surfaces): apparent ÷ proper rate of a static source's surface evolution, straight ahead at 0.9c (√19; receding 0.22942) | 4.3589 | — |
| RS-26 | Earth rate (Doppler factor for Sol straight astern) at 0.99c | 0.070888 | — |
| RS-27 | Earth rate straight astern at 0.999c | 0.022366 | — |
| RS-28 | Earth rate straight astern at the top speed, β = 0.999999 (γ 707.1, D-15) | 7.0711 × 10⁻⁴ | — |
| RS-29 | Earth rate straight ahead (heading home) at 0.99c | 14.107 | — |
| RS-30 | Earth rate straight ahead at β = 0.999999 | 1414.2 | — |
| RS-31 | Catch-up: a signal sent one day after departure reaches a 0.9c ship, days after departure, 1/(1 − β) | 10 | day |
| RS-32 | Catch-up of the same signal at β = 0.999999: 1/(1 − β) = 10⁶ days | 2,737.9 | yr |
| RS-33 | Triangle excess d_A + d_B − \|AB\|: from a port 40 ly out, a 30 ly turn at right angles | 60 | ly |
| RS-34 | Triangle excess: from a port 40 ly out to a star 10 ly from Sol, off to the side at right angles to the Sol–port line | 8.769 | ly |
| RS-35 | Triangle excess \|A\| + \|B\| − \|AB\| for B on the far side of Sol from A (A 40 ly out, B 40 ly the other way; the formula's only zero) | 0 | ly |
| RS-36 | Triangle excess for B beyond A on Sol's ray through A (A 40 ly, B 60 ly): the formula is not the wait here; coverage makes the wait 0 (RS-37) | 80 | ly |
| RS-37 | Wait at a new port on a heard beam's ray (beyond the port, or between Sol and the port): covered by the beam already aimed along it | 0 | yr |
| RS-38 | Swing of Earth's aim (degrees per year) following a ship that crosses its sky at right angles at 0.99c, 40 ly out: β/r | 1.418 | deg |
| RS-39 | The fastest swing Earth's aim ever needs (degrees per day) for a ship at the top speed, 40 ly out: βγ/r, when the ship recedes at cos α = β off the line of sight | 2.773 | deg |
| RS-40 | The same fastest swing (degrees per day) for a ship at the top speed 1 ly from Sol | 110.9 | deg |

Kerr numeric families (a suffixed row is a distinct member of the same RK
family; each row is one number, and unsuffixed spin-dependent rows use s=0.9
unless stated). These are infinity screen coordinates where α or β is named;
they are not angular sizes at a finite observer radius.

| ID | Quantity | Value | Unit |
|---|---|---|---|
| RK-1 | Outer horizon u₊, s=0.9 | 1.4358898943540674 | GM/c² |
| RK-1-s05 | Outer horizon u₊, s=0.5 | 1.8660254037844386 | GM/c² |
| RK-1-s0998 | Outer horizon u₊, s=0.998 | 1.0632139225171164 | GM/c² |
| RK-2 | Inner horizon u₋, s=0.9 | 0.56411010564593264 | GM/c² |
| RK-2-s05 | Inner horizon u₋, s=0.5 | 0.13397459621556135 | GM/c² |
| RK-2-s0998 | Inner horizon u₋, s=0.998 | 0.93678607748288357 | GM/c² |
| RK-3 | Equatorial static limit u_SL, any accepted spin | 2 | GM/c² |
| RK-4 | Prograde photon orbit, s=0.9 | 1.5578546274233828 | GM/c² |
| RK-4-retro | Retrograde photon orbit, s=0.9 | 3.9102679391030367 | GM/c² |
| RK-5 | Prograde photon orbit, s=0.998 | 1.0739092576799517 | GM/c² |
| RK-5-retro | Retrograde photon orbit, s=0.998 | 3.9982218928479460 | GM/c² |
| RK-6 | Prograde ISCO, s=0.5 | 4.2330025295308257 | GM/c² |
| RK-6-retro | Retrograde ISCO, s=0.5 | 7.5545847145123584 | GM/c² |
| RK-7 | Prograde ISCO, s=0.9 | 2.3208830417618872 | GM/c² |
| RK-7-retro | Retrograde ISCO, s=0.9 | 8.7173522796064893 | GM/c² |
| RK-8 | Prograde ISCO clock dτ/dt, s=0.9 | 0.37086794439864592 | — |
| RK-8-retro | Retrograde ISCO clock dτ/dt, s=0.9 | 0.79319251811829484 | — |
| RK-8b | Prograde ISCO clock dτ/dt, s=0.998 | 0.092670308365368573 | — |
| RK-9 | Horizon angular velocity Ω_H, s=0.9 | 0.31339450313662925 | c³/GM |
| RK-10 | Frame dragging ω at the equatorial static limit, s=0.9 | 0.16014234875444840 | c³/GM |
| RK-11 | ZAMO lapse N at the equatorial static limit, s=0.9 | 0.37964208654863802 | — |
| RK-12 | Prograde ISCO signed ZAMO speed, s=0.9 | 0.62454869546086524 | c |
| RK-12-s05 | Prograde ISCO signed ZAMO speed, s=0.5 | 0.56164924228192780 | c |
| RK-14 | Equatorial shadow α_min at infinity, s=0.9; prograde side | −2.8444214034761693 | GM/c² |
| RK-14-max | Equatorial shadow α_max at infinity, s=0.9; retrograde side | 6.8323192304466666 | GM/c² |
| RK-15 | Equatorial shadow top β_max=3√3 at infinity | 5.1961524227066319 | GM/c² |
| RK-16 | Inclusive equal-weight uniform-u diagnostic, s=0.9 (§3b) | 5.0437368945142949 | GM/c² |
| RK-16-s0998 | Same diagnostic, s=0.998 | 4.8770013845209779 | GM/c² |
| RK-17 | Visible-arc α_min at θ_o=45°, s=0.9, infinity | −3.4170599872799702 | GM/c² |
| RK-17-max | Visible-arc α_max at θ_o=45°, s=0.9, infinity | 6.2964531467076725 | GM/c² |
| RK-17-top | Visible-arc β_max at θ_o=45°, s=0.9, infinity | 5.0696345295581239 | GM/c² |
| RK-18 | r_s for 10 M☉ | 29.532500761002495 | km |
| RK-18-static | Equatorial static-limit radius, 10 M☉, s=0.9 | 29.532500761002495 | km |
| RK-18-horizon | Outer horizon radius, 10 M☉, s=0.9 | 21.202709698863643 | km |
| RK-18-isco | Prograde ISCO radius, 10 M☉, s=0.9 | 34.270740098515360 | km |
| RK-18-period | Prograde-ISCO coordinate period, 10 M☉, s=0.9 | 1.3727616115380832 | ms |
| RK-18-omega | Horizon angular velocity, 10 M☉, s=0.9 | 6362.7059001439946 | rad/s |

Qualitative checks (not numbers, so not in the registry):
- **RS-Q1:** straight ahead and straight behind are unchanged by aberration,
  and the aberrated direction is unit length.
- **RS-Q2:** a source appearing at 90° has D = 1/γ; point-source bolometric
  brightness scales as D²; aberration round-trip error < 1e-6.
- **RS-Q3:** photon-ring images converge on b_c.
- **RS-Q4:** an Einstein ring appears for any source behind the hole, at the
  predicted angle.
- **RK-13:** the static-clock rate tends to zero at the static limit; a
  static timelike observer cannot exist at or inside it. At s=0 use
  `staticClockRate(u/2)` directly, byte-identical to the existing export.
- **RK-Q1:** at s=0 the horizon is 1 r_s, ISCO 3 r_s, photon sphere 1.5 r_s,
  infinity shadow a circle of radius 3√3/2 r_s (α²+β²=27 M² within
  1e-12 absolute), frame dragging zero and no exterior ergosphere. Existing
  scalar clock/speed/tide calls dispatch exactly; independent Carter tables
  and forced Kerr-path GPU goldens reduce to §3 within their stated budgets.
