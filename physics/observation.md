# Observation: photon budgets, link budgets and check values

**Version:** baseline-2026.11 (2026-11-12). **Status:** draft normative registry for
R3-REASONS milestone RT0. It awaits review in its draft PR. The labels in §3
(Verified, Adopted, Design, Ruling, unverified) are part of the data, not footnotes.
Four rulings (Mark, attended 2026-11-12) are recorded in §6.
**Addendum (2026-11-13, draft):** transits, observing from the moving ship and
host magnitudes: §2.8–§2.11 and rows OB-53 to OB-118. Four design values in
§6 await a ruling.

## 1. What this registry pins

The ship is an observatory. This registry fixes the numbers that decide what
its instruments can see: how many photons arrive from a star or a planet, how
faint a planet is beside its star, how long a spectrum takes, how far a radio
transmitter can be heard, and how much extra infrared a civilisation's waste
heat adds to a star.

**The rule: an instrument's reach follows from a budget, never from an authored
range.** An instrument is a set of physical parameters (aperture, band,
throughput, noise). A target is a source of photons or radio power. The game
counts what arrives, compares it with the noise, and gets a signal-to-noise
ratio (SNR). No table anywhere says "this telescope sees 60 light-years". If a
distance appears in this file, it is the output of a formula in §2 for stated
inputs.

The package `sunholo/observe` 0.2.0 must reproduce every `OB-n` row in §4.
The functions it needs are named beside each formula. This file extends
[signals-and-instruments §4](../features/next/signals-and-instruments.md) and
obeys [life-and-worlds §5](life-and-worlds.md): "Use S2 photon budgets, never a
literal detection radius." The reach in LW-42 and LW-43 is the radius of a
count model. It is not an instrument range, and nothing here changes it.

This registry lives in its own file, not in `relativity-spec.md` (ruling 4,
§6). The game's `sim/tools/lore_values.ail` must be taught to read this file in
RT1, or `make lore-values` will not list the OB rows.

## 2. Formulas

SI units throughout unless a row states another unit. Angles are in radians
inside formulas.

### 2.1 Constants

| Symbol | Value | Meaning |
|---|---|---|
| h | 6.62607015 × 10⁻³⁴ J s | Planck constant (exact) |
| c | 299792458 m/s | speed of light (exact) |
| k | 1.380649 × 10⁻²³ J/K | Boltzmann constant (exact) |
| AU | 149597870700 m | astronomical unit (exact) |
| pc | 648000/π AU = 3.0856775814913673 × 10¹⁶ m | parsec |
| ly | c × 365.25 × 86400 s = 9.4607304725808 × 10¹⁵ m | Julian light-year |
| Jy | 10⁻²⁶ W m⁻² Hz⁻¹ | jansky, the radio flux-density unit |
| mas | π/(180 × 3600 × 1000) rad | milliarcsecond |
| R☉, T☉ | 6.957 × 10⁸ m, 5772 K | nominal solar radius and effective temperature |
| R⊕, R_J | 6.3781 × 10⁶ m, 7.1492 × 10⁷ m | nominal equatorial radii of Earth and Jupiter |

### 2.2 Photon rate from a star (`photonRate`)

The star is a **blackbody**: a sphere of radius R and temperature T that emits
the Planck spectrum. A **band** is a top hat: every wavelength from λ₁ to λ₂
counts fully, and nothing outside counts.

The photon flux at distance d, in photons s⁻¹ m⁻², is

```
F = π (R/d)² ∫[λ₁..λ₂] 2c / λ⁴ / (exp(hc/(λ k T)) − 1) dλ
```

The integral has an exact series. With x = hc/(λ k T), x_hi = x(λ₁), x_lo = x(λ₂):

```
∫ = 2c (kT/(hc))³ [G(x_lo) − G(x_hi)]
G(x) = Σ[n=1..∞] exp(−n x) (x²/n + 2x/n² + 2/n³)
```

Any method is allowed if it is accurate to a relative 10⁻⁶. The oracle uses
the series and checks it against Simpson's rule.

The **aperture** is an unobscured circle of diameter D, with area A = π D²/4.
The **throughput** τ is the fraction of photons entering the aperture that end
up counted. It includes the optics, the coronagraph and the detector. The
counted rate, in photons s⁻¹, is

```
N = F × (π D²/4) × τ
```

A **spectral resolution element** at wavelength λ₀ and resolving power R is the
band from λ₀ − Δλ/2 to λ₀ + Δλ/2, with Δλ = λ₀/R.

### 2.3 Planet contrast in reflected light (`planetContrast`)

**Contrast** is the planet's brightness divided by its star's, at the same
wavelength. For a planet of radius R_p at orbital distance a:

```
C = A_g × Φ(α) × (R_p/a)²
Φ(α) = (sin α + (π − α) cos α) / π
```

A_g is the **geometric albedo**: the planet's brightness at full phase,
relative to a flat, perfectly diffusing disc of the same size. α is the
**phase angle**: the angle star–planet–observer, 0 at full phase and π/2 at
**quadrature** (half lit, widest apart on the sky). Φ is the phase function of
a **Lambert sphere**, a ball that scatters light equally in all directions.
The planet's photon rate is C times the star's rate from §2.2.

### 2.4 Signal-to-noise ratio and integration time (`snr`)

All rates are counted photons per second in one resolution element.

- S: the planet's rate.
- B: the background rate. B = ζ × N★ + Z. ζ is the coronagraph's **raw
  contrast**: the starlight left at the planet's position, as a fraction of
  the star's rate N★ through the same throughput. Z is **zodiacal light**,
  the glow of sunlit dust (below).
- D: the **dark rate**, counts the detector makes with no light.
  D = n_pix × ξ, for n_pix pixels and a dark current ξ per pixel.

After t seconds:

```
SNR(t) = S t / sqrt((S + B + D) t)
t(SNR) = SNR² (S + B + D) / S²
```

**Two thresholds are pinned** (ruling 2, §6). SNR 5 is **a hint**. SNR 10 is
**strong**; it is the LUVOIR study's requirement.

**Zodiacal light.** Dust fills the plane of a planetary system and scatters
its star's light. The telescope looks through two layers: the dust of the
system it sits in (**local zodi**) and the dust around the target star
(**exozodi**). Both are spread over the sky, so their brightness is given per
square arcsecond, in magnitudes. A source 5 magnitudes fainter is 100 times
dimmer; the factor for m magnitudes is 10^(−0.4 m).

```
Z  = N₀ × Ω × (ℓ × 10^(−0.4 z) + n × 10^(−0.4 z₁))
N₀ = N★(10 pc) × 10^(0.4 M_V)
Ω  = π (X × λ₀/D)²           (converted to arcsec²)
```

- N₀ is the counted rate of a star of apparent magnitude 0 with the host
  star's spectrum, through the same instrument and resolution element. M_V is
  the star's **absolute magnitude**: its magnitude as seen from 10 pc.
- Ω is the **photometric aperture**: the patch of sky whose light is added up
  with the planet's. Its radius is X diffraction scales.
- z is the surface brightness of local zodi and ℓ its level. ℓ = 1 is the
  view from 1 AU inside a system like the Sun's.
- z₁ is the surface brightness of one "zodi" of exozodi, and n is how many
  zodis the target system has.

N₀ grows as D² and Ω shrinks as D⁻², so Z does not depend on the aperture. It
does not depend on distance either. A larger telescope collects more planet
light against the same zodiacal rate.
### 2.5 What the coronagraph can reach

Three limits apply before any photon is counted.

- **Diffraction scale:** θ = λ/D radians. Smaller is sharper.
- **Inner working angle (IWA):** the coronagraph cannot see a planet closer to
  its star than θ_IWA = n_IWA × λ/D. A planet at projected separation a and
  distance d sits at angle a/d. It is observable only if a/d ≥ θ_IWA. The
  largest distance is d_max = a / θ_IWA.
- **Noise floor:** a planet whose contrast is below C_floor = 10^(−0.4 Δmag_floor)
  cannot be detected in any integration time.

### 2.6 Radio link budget (`linkSnr`)

A transmitter's **EIRP** (equivalent isotropic radiated power, W) is the power
an all-directions transmitter would need to look as bright as the real,
beamed one does from inside its beam. At distance d the flux is EIRP/(4π d²),
in W m⁻².

A receiver has an **effective area** A_e (m²) and a **system temperature**
T_sys (K), which measures its noise. Its **system equivalent flux density** is

```
SEFD = 2 k T_sys / A_e          (W m⁻² Hz⁻¹; divide by 10⁻²⁶ for Jy)
```

For a signal narrower than one channel of **bandwidth** B (Hz):

```
linkSnr = EIRP / (4π d² × SEFD × B)
SNR(t)  = linkSnr × sqrt(n_pol × B × t)
d(SNR)  = sqrt( EIRP / (4π × SNR × SEFD) × sqrt(n_pol × t / B) )
```

`linkSnr` is the SNR in one channel after one coherence time, t = 1/B. The
second line is the **radiometer equation**: averaging for t seconds gains
sqrt(B t). n_pol is the number of polarisations summed; every row uses
n_pol = 1. The formulas need B t ≥ 1.

### 2.7 Waste heat in the mid-infrared

A civilisation absorbs a fraction γ of its star's luminosity and re-radiates
it as a blackbody at temperature T_w. The star's own light drops by the same
fraction. At wavelength λ, the waste-heat flux divided by the remaining
starlight is

```
E(λ) = γ/(1 − γ) × K(λ)
K(λ) = (T★/T_w)⁴ × (exp(hc/(λ k T★)) − 1) / (exp(hc/(λ k T_w)) − 1)
```

E is the **excess**: E = 0.1 means the star looks 10 % too bright at λ. E
does not depend on distance. Distance matters only through the photon rate
(§2.2) needed to measure it. γ = 1 is a Kardashev type II civilisation, one
that uses its star's whole output.

### 2.8 Transit photometry (the census)

A planet **transits** when it crosses its star's disc as seen by the
observer. The star dims a little for a few hours. Orbits here are circular,
with radius a and period P. The **inclination** i is the angle between the
orbit's axis and the line of sight; i = 90° is edge-on. The **impact
parameter** b = a |cos i| / R★ is how far from the disc's centre the planet
crosses, in stellar radii.

```
transits if    b = a |cos i| / R★ < 1
probability    p = R★ / a                       (random orientation)
depth          δ = (R_p / R★)²
period         P = 2π sqrt(a³ / GM★)            (when P is not measured)
duration       T = (P/π) asin( sqrt((R★ + R_p)² − (b R★)²) / (a sin i) )
SNR            = δ × sqrt(N★ × T × N_tr)
```

T runs from first to last contact. N★ is the star's counted rate in the band
(§2.2), with the throughput of an instrument that has no coronagraph, and
N_tr is the number of transits added together. The noise is photon noise on
the in-transit light only: the brightness out of transit is taken as known.
p agrees with LW-22.

In a stay of t_stop, the transits that are certain to fall inside it number
floor(t_stop / P). A planet with P > t_stop shows a transit only by luck, with
chance t_stop / P.

### 2.9 Transit spectroscopy (reading air in transit)

During a transit a thin ring of the planet's atmosphere is backlit. At a
wavelength where a gas absorbs, the planet looks slightly larger, so the dip
is slightly deeper. The extra depth is the **signal**.

```
scale height   H = k T_atm / (μ m_u g),   g = G M_p / R_p²
signal         Δ = 2 n R_p H / R★²
SNR per transit, per resolution element = Δ × sqrt(N★ × T)
transits to a target SNR:  N_tr = (SNR / SNR_per_transit)²
calendar time:             N_tr × P       (every transit observed)
```

The **scale height** H is the height over which air pressure falls by a
factor e. μ is the mean molecular mass in atomic mass units m_u. n is how many
scale heights the absorbing band adds to the planet's apparent radius. N★ is
the star's counted rate in one resolution element (§2.2), with no coronagraph.

Transit is the only way to read a planet that sits inside the inner working
angle (§2.5). Most habitable-zone planets of M dwarfs do.

### 2.10 Observing from the moving ship

The ship moves at speed β (in units of c) with γ = 1/sqrt(1 − β²). For a
source whose direction makes angle θ with the heading in the galaxy's frame
(θ = 0 is dead ahead), the **Doppler factor** is

```
D = γ (1 + β cos θ) = 1 / (γ (1 − β cos θ′))
```

θ′ is the angle at which the source appears from the ship. Both forms, the
aberration formula and the rules below are in
[relativity-spec §1–§2](relativity-spec.md) (rows RS-1 to RS-4); nothing here
re-derives them. D > 1 ahead (bluer, brighter), D < 1 astern.

Every observed quantity changes as follows. Primed values are what the ship
measures, per second of ship time.

| Quantity | From the ship | Note |
|---|---|---|
| Wavelength of any feature | λ′ = λ / D | The O₂ A band sits at 0.76/D µm |
| Spectrum of a blackbody | a blackbody at T × D | relativity-spec §2, "Spectrum" |
| Solid angle of a small source | Ω′ = Ω / D² | So the star's angular radius is (R/d)/D |
| Angle between two nearby directions | separation / D | Planet–star separation shrinks ahead and grows astern |
| Counted rate in a fixed instrument band [λ₁, λ₂] | N′ = D × N_rest([D λ₁, D λ₂]) | Any spectrum. For a blackbody this equals T → T D with Ω → Ω/D² |
| All photons, every wavelength | × D | Energy flux is × D² (relativity-spec §2, "Bolometric total = D²") |
| Planet-to-star contrast | unchanged | Both transform the same way; compare at the same emitted wavelength |
| Surface brightness (exozodi) in the same band, relative to the star's rate | × D² per ship-frame arcsec² | The star's light is squeezed into a smaller solid angle, the photometric aperture is not |
| Rates of change at the source (a transit's duration) | duration / D | relativity-spec §2, "Resolved emitter" (RS-24) |
| Instrument: aperture, band, IWA in radians, dark rate | unchanged | The instrument is at rest in the ship |

The O₂ A band is inside a design band [λ_lo, λ_hi] when
0.76/λ_hi ≤ D ≤ 0.76/λ_lo. The heading angles follow from
cos θ = (D/γ − 1)/β and cos θ′ = (cos θ + β)/(1 + β cos θ).

### 2.11 Host absolute magnitude from a blackbody

§2.4 needs each host star's absolute V magnitude M_V. For a blackbody of
temperature T and radius R it is defined relative to the Sun:

```
M_V(T, R) = 4.81 − 2.5 log₁₀( R² P_V(T) / (R☉² P_V(T☉)) )
```

P_V(T) is the integral of §2.2 over the V-like band. The Sun's value is 4.81
by construction. This is the magnitude the blackbody model implies; §5 says
how far real cool stars are from it.

## 3. Provenance

"Verified" means the cited location was opened for this registry and the
number read there. "Adopted" means a verified number is used in a simplified
way. "Design" means a game choice with no literature claim. "Ruling" means a
choice Mark made (attended 2026-11-12, §6). "unverified" means the source was
not opened or does not state the number.

| Quantity | Adopted value | Evidence and precise location | Status |
|---|---|---|---|
| h, c, k, AU, pc | §2.1 | SI 2019 definitions; IAU 2012 Resolution B2 (AU); IAU 2015 Resolution B2 (pc) | Adopted, exact by definition; the resolutions were not re-opened for this pass |
| R☉, T☉, R⊕, R_J | 6.957 × 10⁸ m, 5772 K, 6.3781 × 10⁶ m, 7.1492 × 10⁷ m | [IAU 2015 Resolution B3](https://arxiv.org/abs/1510.07674), recommendation 3 (the list of nominal values) | Verified |
| Star spectrum | Blackbody at T☉ and R☉ | Game model. Its luminosity 4π R☉² σ T☉⁴ equals the nominal 3.828 × 10²⁶ W of the same resolution | Design; a game approximation (§5) |
| Earth-twin geometric albedo; phase function | A_g = 0.2; Lambert | [Stark et al. 2019](https://arxiv.org/abs/1904.11988), Table 1 (rows A_G and Φ) and the text above it | Verified |
| Raw contrast ζ | 10⁻¹⁰ | [LUVOIR Final Report 2019](https://arxiv.org/abs/1912.06219), Table 1-8 and Table 8-7 (1 × 10⁻¹⁰); Stark et al. 2019, Table 2 (ζ_floor = 10⁻¹⁰); [HabEx Final Report 2019](https://arxiv.org/abs/2001.06683), fact sheet (coronagraph 2.5 × 10⁻¹⁰ at the IWA, starshade 1 × 10⁻¹⁰) | Verified |
| Inner working angle | n_IWA = 3.5 | LUVOIR Final Report, Table 1-8 and Table 8-7: visible channel 3.5 λ/D. HabEx fact sheet: 2.4 λ/D = 62 mas at 0.5 µm for a 4 m. Stark et al. 2019, §4.1: useful throughput exists inside the classical IWA | Verified; adopted as a hard edge, a game approximation |
| Resolving power; O₂ A band | R = 140 at λ₀ = 0.76 µm | LUVOIR Final Report, §1 instrument overview beside Table 1-8: the visible spectrograph has "R=140, chosen for optimal study of the narrow O2 absorption band at 760 nm" | Verified |
| Detection thresholds | SNR 5 per resolution element is "a hint"; SNR 10 is "strong" | LUVOIR Final Report, §3.4.2 step 7, asks SNR ~ 10 at R = 140. Stark et al. 2019, Table 2, uses S/N 5 per spectral bin in the continuum, at R = 70 | Sources verified; both thresholds pinned by Ruling 2 (Mark, attended 2026-11-12) |
| Dark current ξ | 3 × 10⁻⁵ counts pixel⁻¹ s⁻¹ | Stark et al. 2019, Table 2 | Verified |
| Pixels per resolution element n_pix | 192 | Stark et al. 2019, Table 2 and the text after Eq. 14: 192 pixels per spectral bin at R = 70 and 1 µm | Verified there; used here at R = 140 and 0.76 µm, Adopted |
| Throughput τ | 0.04, the same at all wavelengths | Stark et al. 2019, Eq. 16 and Table 2: 0.95 (contamination) × 0.21 (optics, low case at 1 µm) × 0.9 (detector) × 0.75 (read-out) = 0.135. A coronagraph core throughput of 0.30 gives 0.0404, rounded to 0.04 | Components verified; the core throughput 0.30 is **unverified** (Stark gives it only as curves in figures); the product is Design |
| Noise floor | Δmag_floor = 26.5 | Stark et al. 2019, Table 2; HabEx fact sheet (Δmag limit 26.5) | Verified |
| Zodiacal light | z = 23 mag arcsec⁻² (local, V band), z₁ = 22 mag arcsec⁻² (one zodi of exozodi, V band, solar twin), n = 4.5 zodis | Stark et al. 2019, Table 1 (rows z, z′ and n; n is the median of the LBTI HOSTS best-fit distribution) and Eq. 8–9 for the count rates | Verified; included by Ruling 1 (Mark, attended 2026-11-12). Used at 0.76 µm and for every star, Adopted (§5) |
| Local zodi level | ℓ = 1 in every row | Game choice: the view from 1 AU in a Sun-like system, as the studies assume. A ship in interstellar space would have ℓ = 0 | Design |
| Photometric aperture | radius X = 0.7 λ/D | Stark et al. 2019, Table 2 (rows X and Ω). Stark measures it in units of the Lyot-stop diameter; this registry uses the full aperture D | Verified; Adopted |
| Sun's absolute V magnitude | M_V = 4.81 | [Willmer 2018](https://arxiv.org/abs/1804.07788), abstract and Table 3, Johnson V, Vega system | Verified |
| Integration-time formula | t = SNR² (S + B + D)/S² | Stark et al. 2019, Eq. 5, uses (S + 2B')/S², where B' is all background and the 2 pays for background subtraction | Verified; this registry keeps plain Poisson noise and drops the factor 2, a game approximation (confirmed, Mark, attended 2026-11-12) |
| Reference aperture | 10 m; also 4 m and 100 m | Game ruling (sprint R3-REASONS, OQ1). The studies span 4 m (HabEx), 8 m and 15 m (LUVOIR Final Report, fact sheet) | Design |
| V-like band | top hat, centre 0.551 µm, width 0.088 µm | Game band. Willmer 2018, Table 4, gives Johnson V a pivot wavelength of 0.5511 µm and a bandwidth of 0.0871 µm | Design; the comparison values are Verified |
| Jupiter-twin albedo; hot-Jupiter albedo | 0.5; 0.1 | Game values for two comparison rows | Design; **unverified** against measurements |
| Radiometer equation | §2.6 | [Sheikh et al. 2025](https://arxiv.org/abs/2502.02614), Eq. 1–3, which cite Enriquez et al. 2017 | Verified in Sheikh; Enriquez not opened |
| Radar EIRP | 2 × 10¹³ W | Sheikh et al. 2025, §2 case 1: Arecibo's S-band EIRP "is 20 TW (Ekers et al. 2002)" | Verified in the secondary source; Ekers not opened |
| Large-array SEFD | 1.5 Jy | Sheikh et al. 2025, §2: SKA1-Mid with MeerKAT, from Braun et al. 2017 Table 4 | Verified in Sheikh; Braun not opened. Used only in OB-33 |
| Radar reach, published | 12000 ly | Sheikh et al. 2025, §2 result list, case 1: SNR 5, 0.01 Hz, 1 hour | Verified; OB-33 reproduces it (11930 ly). The same formula also reproduces their case 4 (0.97 ly). It gives 83 ly for case 2 (they print 65 ly) and 0.80 ly for case 3 (they print 4.0 ly, in both §2 and Table 2). A second reading of §2 found one SEFD, one SNR and one integration time stated for all four cases, so no stated input explains the two. Case 3's printed value is 5 times the formula's, the size of the SNR factor; that is an observation, not a finding about the paper. No row uses cases 2 or 3 |
| TV station power and carrier | ERP 1000 kW; 0.4 of it in the video carrier; EIRP = 1.64 × ERP; carrier width 0.1 Hz | [Sullivan & Knowles 1985](https://doi.org/10.1017/S0074180900146662), IAU Symposium 112, pp. 327–334, §2: carriers "of width 0.1–1.0 Hz", effective radiated power "typically 100–1000 kw", the factors 1.64 and 0.4 | Verified; the top of the power range and the narrowest width are Adopted |
| Leakage reach, published | about 30 ly for an Arecibo-like antenna, about 500 ly for a Cyclops-like array (radars) | Sullivan & Knowles 1985, §1, summarising [Sullivan, Brown & Wetherill 1978](https://doi.org/10.1126/science.199.4327.377) | Verified in the 1985 paper. The 1978 paper's own tables are **unverified**: only its abstract was opened. Context only; no row uses it |
| Reference dish | 100 m, aperture efficiency 0.70, T_sys 25 K | The ship's starting radio receiver | Ruling 3 (Mark, attended 2026-11-12) |
| Square-kilometre receiver | A_e = 10⁶ m², T_sys 25 K | The later, large radio receiver | Ruling 3 (Mark, attended 2026-11-12) |
| Waste-heat model | §2.7 | [Wright et al. 2014](https://arxiv.org/abs/1408.1134) (the Ĝ search, paper II), §2.2 Eq. 2 and Eq. 5–6 with α = γ and ε = ν = 0; Table 1 defines γ and T_waste | Verified |
| Waste-heat temperature | 300 K | Wright et al. 2014, Table 1, gives about 285 K for humanity and §2.2 gives about 250 K for a classical Dyson sphere | Design, inside the verified range |
| Infrared excess threshold | E = 0.1 | Game threshold for OB-40 | Design |
| NO₂ as a technosignature | no number adopted | [Kopparapu et al. 2021](https://arxiv.org/abs/2102.05027), abstract: present Earth-level NO₂ at 10 pc, SNR ~ 5 in ~ 400 hours with a 15 m telescope, cloud-free | Verified; context for the likelihood work in RT1b |
| CFCs as a technosignature | no number adopted | [Lin, Gonzalez Abad & Loeb 2014](https://arxiv.org/abs/1406.3025), abstract: JWST, a planet transiting a white dwarf, about 1.2 days (CCl₃F) and 1.7 days (CF₄) for 10 times the terrestrial level | Verified; the paper covers white-dwarf hosts only and states no reach in light-years |
| Planet and solar mass parameters | GM☉ = 1.3271244 × 10²⁰, GM⊕ = 3.986004 × 10¹⁴ m³ s⁻² | IAU 2015 Resolution B3, recommendation 3 | Verified |
| Atomic mass constant | m_u = 1.66053906660 × 10⁻²⁷ kg | CODATA 2018 | Adopted; not re-opened |
| Transit geometry | §2.8 | Standard results for a circular orbit. Check: the formula gives 55.84 min for TRAPPIST-1 e against 55.76 ± 0.26 min measured ([Agol et al. 2021](https://arxiv.org/abs/2010.01074), Table 5), and 13.10 h for Earth against 13.1 h in [Rodler & López-Morales 2014](https://arxiv.org/abs/1312.1585), Table 1 | Adopted; no derivation source opened, the two checks are Verified |
| TRAPPIST-1 | R★ = 0.1192 R☉, T = 2566 K | Agol et al. 2021, Table 7 | Verified |
| TRAPPIST-1 e | R = 0.920 R⊕, M = 0.692 M⊕ (Table 6); P = 6.101013 d (Table 2); a = 0.02925 AU (Table 6); b = 0.191 (Table 5) | Agol et al. 2021. Table 6 also gives g = 8.01 m s⁻², which the formula reproduces | Verified |
| TRAPPIST-1 distance | 12.42988881 pc | NASA Exoplanet Archive, `pscomppars`, `sy_dist` for TRAPPIST-1 e, reference Gaia DR2; queried 2026-11-13. The same record gives V = 17.02 and Ic = 14.0 | Verified |
| Throughput with no coronagraph | τ_T = 0.135 | Stark et al. 2019, Eq. 16 and Table 2: 0.95 × 0.21 × 0.9 × 0.75 = 0.1347, the spectrograph path without the coronagraph's core loss | Components verified; use for transit work is Design |
| Scale heights in a band | n = 2 | [Kreidberg 2018](https://arxiv.org/abs/1709.05941), Eq. 2–3 and the sentence after: n is "typically around two for cloud-free atmospheres at low spectral resolution" | Verified; used for the O₂ A band, Adopted |
| Earth-like atmosphere | μ = 28.97, T_atm = 255 K | Game values; 255 K is LW-19 | Design; μ **unverified** (standard value, no source opened) |
| Hot Jupiter | R_J, a = 0.05 AU, Sun-like star, 10 pc, b = 0 | Game case, the planet of OB-5 | Design |
| Warm sub-Neptune and host | 2.5 R⊕, 8 M⊕, 300 K, μ = 2.3; a = 0.15 AU; star 3430 K, 0.361 R☉, 0.40 M☉, 10 pc | Game case. The star's T and R are the M3V row of [Mamajek's dwarf sequence](https://www.pas.rochester.edu/~emamajek/EEM_dwarf_UBVIJHK_colors_Teff.txt) (version 2022.04.16; Pecaut & Mamajek 2013 updated) | Design; star T and R Verified; the 0.40 M☉ is **unverified** |
| Oxygen in transit, published | no number adopted | Rodler & López-Morales 2014, Table 2: the 39 m E-ELT at high resolution needs 31–48 transits (about 3–5 years) for a 3σ O₂ detection on an Earth twin of an M7V–M8V star at 5 pc; abstract: feasible only within 8 pc and later than M3. [Snellen et al. 2013](https://arxiv.org/abs/1302.3251), abstract: "a few dozen transits" with the extremely large telescopes. [Lustig-Yaeger, Meadows & Lincowski 2019](https://arxiv.org/abs/1905.07070), abstract: with JWST, biogenic O₂ and O₃ "will be extremely challenging to detect" on the TRAPPIST-1 planets | Verified; compared with the rows after §4 |
| Dwarf sequence (T, R, M_V) | K5V: 4440 K, 0.701 R☉, M_V 7.28. M4V: 3210 K, 0.274 R☉, M_V 12.61, V−Ic 2.831. M8V: 2570 K, 0.114 R☉, M_V 18.60, V−Ic 4.62 | Mamajek's dwarf sequence, version 2022.04.16 | Verified |
| Sun in the I band | M_I = 4.10; Cousins I pivot 0.8034 µm, bandwidth 0.1482 µm | Willmer 2018, Table 3 and Table 4 | Verified; used only for the comparison in §5 |
| Doppler and aberration | §2.10 | relativity-spec §1–§2 and rows RS-1 to RS-4. The photon-count forms (N′, × D, the surface-brightness rule) follow from its invariant I_ν/ν³; they are not printed there | Adopted from the spec; the photon-count forms are this registry's statement and are checked two ways by the oracle |
| Vegetation red edge | no number adopted | [Kiang et al. 2007](https://arxiv.org/abs/astro-ph/0701391) (paper II), introduction: the edge is a steep rise in reflectance over about 700–850 nm, and may sit in the near-infrared for M stars | Verified; context only |

## 4. OB check-value registry

Each row is one scalar. A row passes when the computed value, rounded to the
row's printed significant figures, equals the printed value. Trailing zeros
count as significant figures. Unless a row says otherwise:

- the star is the Sun of §2.1 as a blackbody;
- the "Earth twin" has R⊕, A_g = 0.2, a = 1 AU, and is at quadrature;
- the "reference instrument" has D = 10 m, τ = 0.04, ζ = 10⁻¹⁰, n_IWA = 3.5,
  n_pix = 192, ξ = 3 × 10⁻⁵ s⁻¹;
- zodiacal light is included: ℓ = 1, z = 23, n = 4.5, z₁ = 22, X = 0.7,
  M_V = 4.81;
- "O₂ A element" is one resolution element at λ₀ = 0.76 µm, R = 140;
- transit rows (OB-53 on) use D = 10 m, τ_T = 0.135, no coronagraph, no
  zodiacal light and no dark counts; "30-day stay" is a design value (§6);
- "reference dish" is the ship's starting receiver: 100 m across, aperture
  efficiency 0.70, T_sys = 25 K. "Large array" is A_e = 10⁶ m² at
  T_sys = 25 K. Radio rows use n_pol = 1.

| ID | Quantity | Value | Unit |
|---|---|---|---|
| OB-1 | Lambert phase function at quadrature, Φ(π/2) = 1/π | 0.3183 | — |
| OB-2 | Lambert phase function at phase angle 60° | 0.6090 | — |
| OB-3 | Contrast of the Earth twin | 1.157 × 10⁻¹⁰ | — |
| OB-4 | Contrast of a Jupiter twin: R_J, A_g = 0.5, a = 5.2 AU, quadrature | 1.344 × 10⁻⁹ | — |
| OB-5 | Contrast of a hot Jupiter in reflected light: R_J, A_g = 0.1, a = 0.05 AU, quadrature | 2.908 × 10⁻⁶ | — |
| OB-6 | Noise-floor contrast for Δmag_floor = 26.5 | 2.512 × 10⁻¹¹ | — |
| OB-7 | Photon flux from the Sun at 10 pc in the V-like band | 9.975 × 10⁷ | photons s⁻¹ m⁻² |
| OB-8 | Photon rate from the Sun at 10 pc in the V-like band into a 10 m aperture, τ = 1 | 7.834 × 10⁹ | photons s⁻¹ |
| OB-9 | Counted rate from the Sun at 10 pc, O₂ A element, reference instrument | 1.914 × 10⁷ | photons s⁻¹ |
| OB-10 | Counted rate S from the Earth twin at 10 pc, O₂ A element, reference instrument | 2.215 × 10⁻³ | photons s⁻¹ |
| OB-11 | The same at 10 ly | 2.356 × 10⁻² | photons s⁻¹ |
| OB-12 | Residual starlight ζ × N★ for OB-10 (ζ × OB-9) | 1.914 × 10⁻³ | photons s⁻¹ |
| OB-13 | Dark rate D of the reference instrument (192 × 3 × 10⁻⁵) | 5.760 × 10⁻³ | counts s⁻¹ |
| OB-14 | SNR on the Earth twin at 10 pc after 10 hours (36000 s), O₂ A element | 3.477 | — |
| OB-15 | Time to a hint (SNR 5) on the Earth twin at 10 pc, O₂ A element, reference instrument | 20.68 | hours |
| OB-16 | The same to strong (SNR 10) | 82.70 | hours |
| OB-17 | Time to a hint (SNR 5) at 10 ly, reference instrument (strong: OB-49) | 0.6805 | hours |
| OB-18 | Time to SNR 5 at 10 ly with D = 4 m | 8.554 | hours |
| OB-19 | Time to SNR 5 at 10 pc with D = 4 m. Photon budget only: OB-25 puts this planet inside the inner working angle, so it is not observable | 615.9 | hours |
| OB-20 | Time to SNR 5 at 10 pc with D = 100 m | 0.05993 | hours |
| OB-21 | Diffraction scale λ/D at 0.76 µm for D = 10 m | 15.68 | mas |
| OB-22 | Inner working angle 3.5 λ/D at 0.76 µm for D = 10 m | 54.87 | mas |
| OB-23 | Angular separation of 1 AU at 10 pc | 100.0 | mas |
| OB-24 | Largest distance at which 1 AU clears the inner working angle at 0.76 µm, D = 10 m | 59.45 | ly |
| OB-25 | The same for D = 4 m | 23.78 | ly |
| OB-26 | The same for D = 100 m | 594.5 | ly |
| OB-27 | Time to a hint (SNR 5) on the Earth twin at the distance of OB-24, reference instrument (strong: OB-50) | 183.1 | hours |
| OB-28 | Effective area of the reference dish, 0.70 × π × 100²/4 | 5498 | m² |
| OB-29 | SEFD of the reference dish | 12.56 | Jy |
| OB-30 | `linkSnr` for EIRP 2 × 10¹³ W at 100 ly, reference dish, B = 1 Hz | 14.16 | — |
| OB-31 | Distance at which EIRP 2 × 10¹³ W gives SNR 1: reference dish, B = 1 Hz, t = 1 s | 376.3 | ly |
| OB-32 | The same at SNR 10 | 119.0 | ly |
| OB-33 | Distance at which EIRP 2 × 10¹³ W gives SNR 5: SEFD 1.5 Jy, B = 0.01 Hz, t = 3600 s (Sheikh et al. 2025 print 12000 ly) | 1.193 × 10⁴ | ly |
| OB-34 | EIRP of a TV video carrier: 1.64 × 0.4 × 1000 kW | 6.560 × 10⁵ | W |
| OB-35 | Distance at which OB-34 gives SNR 1: reference dish, B = 0.1 Hz, t = 3600 s | 0.9388 | ly |
| OB-36 | The same with the large array | 12.66 | ly |
| OB-37 | The same as OB-36 at SNR 10 | 4.004 | ly |
| OB-38 | Waste-heat spectral factor K at 10 µm, T_w = 300 K, Sun-like star | 323.2 | — |
| OB-39 | Excess E at 10 µm for γ = 0.01 (1 % of a Kardashev type II), T_w = 300 K | 3.265 | — |
| OB-40 | Smallest γ whose excess at 10 µm reaches E = 0.1, T_w = 300 K | 3.093 × 10⁻⁴ | — |
| OB-41 | Threshold SNR for "a hint" | 5 | — |
| OB-42 | Threshold SNR for "strong" | 10 | — |
| OB-43 | Photometric aperture Ω at 0.76 µm for D = 10 m, X = 0.7 | 3.783 × 10⁻⁴ | arcsec² |
| OB-44 | Zero-magnitude rate N₀: Sun's spectrum, O₂ A element, reference instrument | 1.607 × 10⁹ | photons s⁻¹ |
| OB-45 | Local zodi rate (ℓ = 1, z = 23) for OB-10; the same for any aperture and distance | 3.835 × 10⁻⁴ | photons s⁻¹ |
| OB-46 | Exozodi rate (n = 4.5, z₁ = 22) for OB-10; the same for any aperture and distance | 4.335 × 10⁻³ | photons s⁻¹ |
| OB-47 | Total background B for OB-10 (OB-12 + OB-45 + OB-46) | 6.633 × 10⁻³ | photons s⁻¹ |
| OB-48 | OB-15 with no zodiacal light (Z = 0), for comparison | 14.00 | hours |
| OB-49 | Time to strong (SNR 10) at 10 ly, reference instrument | 2.722 | hours |
| OB-50 | Time to strong (SNR 10) at the distance of OB-24, reference instrument | 732.3 | hours |
| OB-51 | Distance at which EIRP 2 × 10¹³ W gives a hint (SNR 5): reference dish, B = 1 Hz, t = 1 s | 168.3 | ly |
| OB-52 | Distance at which OB-34 gives a hint (SNR 5): large array, B = 0.1 Hz, t = 3600 s | 5.662 | ly |
| OB-53 | Transit probability of an Earth twin, R☉/1 AU (LW-22) | 0.004650 | — |
| OB-54 | Transit depth of the Earth twin | 8.405 × 10⁻⁵ | — |
| OB-55 | Period at 1 AU from GM☉ | 365.3 | days |
| OB-56 | Transit duration of the Earth twin, b = 0 | 13.10 | hours |
| OB-57 | Counted rate from the Sun at 10 pc, V-like band, 10 m, τ_T = 0.135 | 1.058 × 10⁹ | photons s⁻¹ |
| OB-58 | Photon-limited SNR of one Earth-twin transit at 10 pc, V-like band | 593.5 | — |
| OB-59 | Chance that an Earth-twin transit falls inside a 30-day stay (30 d / P) | 0.08213 | — |
| OB-60 | Earth-twin transits certain to fall inside a 30-day stay: none | 0 | transits |
| OB-61 | Transit probability of TRAPPIST-1 e | 0.01895 | — |
| OB-62 | Transit depth of TRAPPIST-1 e (measured: 0.5012 %) | 5.007 × 10⁻³ | — |
| OB-63 | Transit duration of TRAPPIST-1 e, b = 0.191 (measured: 55.76 min) | 55.84 | minutes |
| OB-64 | Photon-limited SNR of one TRAPPIST-1 e transit, V-like band, blackbody star | 54.00 | — |
| OB-65 | TRAPPIST-1 e transits certain to fall inside a 30-day stay | 4 | transits |
| OB-66 | SNR of OB-64 over those 4 transits | 108.0 | — |
| OB-67 | Transit probability of the hot Jupiter (a = 0.05 AU) | 0.09301 | — |
| OB-68 | Transit depth of the hot Jupiter | 0.01056 | — |
| OB-69 | Period of the hot Jupiter | 4.084 | days |
| OB-70 | Transit duration of the hot Jupiter, b = 0 | 3.205 | hours |
| OB-71 | Photon-limited SNR of one hot-Jupiter transit at 10 pc, V-like band | 3.689 × 10⁴ | — |
| OB-72 | The same over the 7 transits of a 30-day stay | 9.761 × 10⁴ | — |
| OB-73 | Scale height on TRAPPIST-1 e with Earth-like air (μ = 28.97, 255 K) | 9.136 | km |
| OB-74 | Transit-spectroscopy signal of TRAPPIST-1 e, n = 2 | 31.18 | ppm |
| OB-75 | Counted rate from TRAPPIST-1 (blackbody) in the O₂ A element, 10 m, τ_T | 9501 | photons s⁻¹ |
| OB-76 | SNR per transit in the O₂ A element, TRAPPIST-1 e, 10 m | 0.1759 | — |
| OB-77 | Transits to a hint (SNR 5), TRAPPIST-1 e, 10 m | 807.8 | transits |
| OB-78 | Calendar time for OB-77, every transit observed | 13.49 | years |
| OB-79 | Transits to strong (SNR 10), TRAPPIST-1 e, 10 m | 3231 | transits |
| OB-80 | Calendar time for OB-79: not within a working lifetime | 53.97 | years |
| OB-81 | SNR per transit in the O₂ A element, TRAPPIST-1 e, D = 100 m | 1.759 | — |
| OB-82 | Transits to a hint, TRAPPIST-1 e, 100 m | 8.078 | transits |
| OB-83 | Calendar time for OB-82 | 0.1349 | years |
| OB-84 | Transits to strong, TRAPPIST-1 e, 100 m | 32.31 | transits |
| OB-85 | Calendar time for OB-84 | 0.5397 | years |
| OB-86 | Scale height on the Earth twin (μ = 28.97, 255 K) | 7.469 | km |
| OB-87 | Transit-spectroscopy signal of the Earth twin across the Sun, n = 2 | 0.3937 | ppm |
| OB-88 | SNR per transit in the O₂ A element, Earth twin at 10 pc, 10 m | 0.6871 | — |
| OB-89 | Calendar time to a hint, Earth twin, 10 m (one transit a year) | 52.96 | years |
| OB-90 | Calendar time to strong, Earth twin, 10 m: hopeless | 211.8 | years |
| OB-91 | SNR per transit, Earth twin at 10 pc, D = 100 m (photon noise only; the signal is OB-87) | 6.871 | — |
| OB-92 | Scale height on the warm sub-Neptune (μ = 2.3, 300 K) | 86.47 | km |
| OB-93 | Transit-spectroscopy signal of the sub-Neptune across its M3V star, n = 2 | 87.44 | ppm |
| OB-94 | Period of the sub-Neptune (0.15 AU, 0.40 M☉) | 33.55 | days |
| OB-95 | SNR per transit in one R = 140 element at 0.76 µm, sub-Neptune at 10 pc, 10 m | 8.530 | — |
| OB-96 | Transits to strong (SNR 10), sub-Neptune, 10 m | 1.374 | transits |
| OB-97 | Transit duration of the sub-Neptune, b = 0 | 3.051 | hours |
| OB-98 | Sun dead ahead at 0.9c: counted rate in the fixed V-like band, relative to rest | 2.667 | — |
| OB-99 | Sun dead astern at 0.9c: the same | 5.851 × 10⁻⁶ | — |
| OB-100 | 0.9c: smallest angle θ from the heading (galaxy frame) at which the O₂ A band is inside 0.5–1.0 µm | 112.0 | deg |
| OB-101 | 0.9c: largest such angle θ | 138.0 | deg |
| OB-102 | 0.9c: the same inner edge as an apparent angle θ′ on the ship's sky | 37.58 | deg |
| OB-103 | 0.9c: the outer edge as an apparent angle θ′ | 61.72 | deg |
| OB-104 | Sun dead ahead at 0.99c: counted rate in the fixed V-like band, relative to rest | 1.232 | — |
| OB-105 | Sun dead astern at 0.99c: the same | 3.423 × 10⁻²³ | — |
| OB-106 | 0.99c: smallest angle θ (galaxy frame) with the O₂ A band inside 0.5–1.0 µm | 142.5 | deg |
| OB-107 | 0.99c: largest such angle θ | 154.4 | deg |
| OB-108 | 0.99c: the inner edge as an apparent angle θ′ | 23.60 | deg |
| OB-109 | 0.99c: the outer edge as an apparent angle θ′ | 34.65 | deg |
| OB-110 | Largest β at which the O₂ A band stays inside 0.5–1.0 µm over the whole sky | 0.2677 | — |
| OB-111 | Separation of 1 AU at 10 pc seen dead ahead at 0.9c (OB-23 / RS-3) | 22.94 | mas |
| OB-112 | The same dead astern at 0.9c (OB-23 / RS-4) | 435.9 | mas |
| OB-113 | Blackbody M_V of the Sun (the anchor) | 4.810 | mag |
| OB-114 | Blackbody M_V of a K5V star: 4440 K, 0.701 R☉ (sequence value 7.28) | 7.063 | mag |
| OB-115 | Blackbody M_V of an M4V star: 3210 K, 0.274 R☉ (sequence value 12.61) | 11.54 | mag |
| OB-116 | Blackbody M_V of TRAPPIST-1: 2566 K, 0.1192 R☉ (M8V sequence value 18.60) | 15.55 | mag |
| OB-117 | SNR in the O₂ A element on TRAPPIST-1 e from the 4 transits of a 30-day stay, 10 m | 0.3519 | — |
| OB-118 | The same with D = 100 m | 3.519 | — |

### What the rows say about reach

These are outputs, not rules. They are stated so a reviewer can compare them
with the feature document.

- **Life is a tens-of-light-years question at 10 m, and the angle sets it.**
  The reference instrument takes 21 hours for a hint and 83 hours for a
  strong reading of an Earth twin at 10 pc (OB-15, OB-16). Beyond 59 ly
  (OB-24) the planet is too close to its star to separate at 0.76 µm, however
  long the exposure. At that edge a hint takes 183 hours and a strong reading
  732 hours, a month (OB-27, OB-50). A 4 m aperture stops at 24 ly (OB-25); a
  100 m aperture reaches 594 ly (OB-26). This agrees with
  signals-and-instruments §4.2.
- **Zodiacal light costs about half again at 10 pc.** Without it OB-15 would
  be 14 hours (OB-48), not 21. Most of it is the target system's own dust
  (OB-46 against OB-45). It matters most where the planet is faint: at 10 ly
  the cost is a tenth (OB-17), at the 59 ly edge it is two thirds.
- **Radar-like leakage reaches further than the feature document says.**
  §4.3 there says "up to hundreds of light-years ... with a large receiver".
  The ship's 100 m dish hears a 2 × 10¹³ W radar at 376 ly in one second at
  1 Hz (OB-31; 168 ly for a hint, OB-51), and a large array hears it at about
  12000 ly in an hour at 0.01 Hz (OB-33). This PR corrects that cell. The budget is not what hides a radar. Its beam covers a tiny part
  of the sky for a short time. That chance belongs to the likelihood model
  (RT1b), not to this file.
- **TV-like leakage is a light-years signal** (OB-35 to OB-37), as §4.3 says.
- **Waste heat is a question of fraction, not distance.** One percent of a
  star's output at 300 K makes the star more than four times brighter at 10 µm
  (OB-39: the excess alone is 3.3 times the starlight). A fraction below
  3 × 10⁻⁴ hides under a 10 % threshold (OB-40). Humanity's fraction of the
  Sun's output is far below that.

- **The 10 m telescope cannot read oxygen on a transiting M-dwarf planet.**
  On TRAPPIST-1 e the signal is 31 parts per million (OB-74) and one transit
  gives SNR 0.18 in the O₂ A element (OB-76). A hint needs 808 transits,
  13.5 years of catching every one (OB-77, OB-78). A strong reading needs 54
  years (OB-80). A 30-day stay gives SNR 0.35 (OB-117). These rows are
  optimistic in two known ways. The blackbody star is 1.7 to 2.5 times too
  bright near 0.8 µm (§5, item 11). And the published simulations are harsher:
  Rodler & López-Morales 2014, scaled by aperture, distance and threshold to
  this case, give about 8,000 to 12,500 transits, ten to fifteen times the
  row. Their method (high resolution, from the ground, with real noise) is not
  the registry's, so the scaling is rough. Both say the same thing: not at
  10 m.
- **A 100 m telescope can.** It reaches a hint in 8 transits and a strong
  reading in 32, about half a year (OB-82 to OB-85). A 30-day stay gives
  SNR 3.5 (OB-118), short of a hint. Scaled from Rodler & López-Morales it is
  80 to 125 transits, one to two years. So with 100 m, oxygen on a nearby
  transiting M-dwarf planet is a stay of months to years, not days.
- **An Earth twin across a Sun-like star is hopeless in transit.** One planet
  in 215 is aligned (OB-53). It transits once a year, so a 30-day stay
  usually sees none (OB-59, OB-60). The signal is 0.39 parts per million
  (OB-87). The 10 m needs 53 years for a hint (OB-89). Photon noise alone
  would let a 100 m see it in one transit (OB-91), but that needs the star
  and the instrument steady to a fraction of a part per million, which these
  rows do not model. That planet is read with the coronagraph (OB-15), not in
  transit.
- **Finding transits is easy; photons are never the limit.** Even the
  Earth twin's dip has SNR 594 in one transit (OB-58). What limits a real
  census is the star's own flicker and the wait for a transit. Neither is in
  these rows.
- **From the moving ship the sky is not one sky.** At 0.9c a Sun-like star
  dead ahead is 2.7 times brighter in the V-like band and one dead astern
  is 170,000 times fainter (OB-98, OB-99). The O₂ A band falls inside a
  0.5–1.0 µm instrument only for stars in a ring: 38° to 62° from the heading
  on the ship's sky at 0.9c, 24° to 35° at 0.99c (OB-102, OB-103, OB-108,
  OB-109). Below 0.27c it is in band everywhere (OB-110). Ahead, planets
  crowd toward their stars: 1 AU at 10 pc shrinks from 100 mas to 23 mas at
  0.9c, inside the inner working angle (OB-111 against OB-22).

## 5. Scope: what is simplified on purpose

Each item is a **game approximation**. None is a claim about a real mission.

1. **The star is a blackbody.** Real stars have absorption lines and are not
   Planck curves. For the Sun in V the blackbody gives 0.955 of the photon
   flux that Willmer 2018's magnitude and zero point give (4.81; Table 4:
   3.62701 × 10⁻⁹ erg s⁻¹ cm⁻² Å⁻¹, bandwidth 0.0871 µm). Other bands are not
   checked.
2. **Bands are top hats, and throughput is grey.** One τ applies at every
   wavelength. Real optics and detectors vary with wavelength.
3. **Zodiacal light is smooth and has the star's colour.** The V-band
   surface brightnesses are used at 0.76 µm. Every target system has the
   median 4.5 zodis at 22 mag arcsec⁻², whatever its star and wherever the
   planet sits. Real systems differ by orders of magnitude. The local level
   is fixed at ℓ = 1, as if the ship observed from 1 AU in a system like the
   Sun's. The photometric aperture uses the full aperture D.
4. **No factor 2 for background subtraction**, no read noise, and no
   clock-induced charge. The dark rate uses a pixel count taken from a
   different resolving power and wavelength.
5. **SNR is on the continuum in one resolution element.** It is the SNR of the
   planet's light at 0.76 µm, not of the O₂ absorption itself. Detecting the
   band needs the neighbouring elements too. The chance that a given spectrum
   means oxygen, and that oxygen means life, belongs to RT1b's likelihoods.
6. **The inner working angle is a hard edge and one number.** Real coronagraphs
   lose throughput gradually, and the angle depends on the design.
7. **The albedo is the same at all wavelengths, and the planet is a Lambert
   sphere.** No clouds, glint, rings or thermal emission. A real hot Jupiter
   glows in the infrared far more brightly than OB-5's reflected light.
8. **Circular orbit seen at quadrature**, so the projected separation equals
   the orbital distance.
9. **Radio:** one polarisation, a signal narrower than one channel, no Doppler
   drift, no interstellar scattering, and a transmitter that points at the
   receiver for the whole integration.
10. **Waste heat is one blackbody at one temperature**, and the star keeps its
    spectrum's shape while it dims. OB-40's threshold is a game number, not a
    survey's calibration limit.

11. **A blackbody is a poor M dwarf in visible light.** Cool stars are
    darkened by molecular bands that a Planck curve does not have. In V the
    blackbody is too bright by 0.22 mag for a K5V star, 1.07 mag for an M4V
    and 3.05 mag for TRAPPIST-1 against the M8V sequence value (OB-114 to
    OB-116; 1.0 mag against that star's own catalogued V = 17.02). One
    magnitude is a factor 2.5 in photons; three are a factor 16. Near 0.8 µm
    the error is smaller: the blackbody TRAPPIST-1 is 0.55 mag too bright
    against its catalogued Ic = 14.0 and 1.0 mag against the sequence, a
    factor 1.7 to 2.5; for an M4V it is 0.13 mag. Every M-dwarf rate in this
    file carries that error: OB-64, OB-66 and OB-75 to OB-85 are too
    favourable. If a catalogue M_V is mixed with a blackbody rate in §2.4's
    N₀, the zodiacal rate is wrong by the V error. Use the blackbody M_V
    with the blackbody rate, or real values for both.
12. **Transit noise is photon noise only.** No stellar flicker, no spots, no
    instrument drift, no noise floor, no limb darkening. The out-of-transit
    brightness is taken as known, which is the same choice as the plain
    Poisson formula of §2.4. A signal of a fraction of a part per million
    (OB-87) passes these formulas; nothing here says an instrument could
    hold steady enough to see it.
13. **The O₂ signal is n = 2 scale heights in one R = 140 element.** No
    clouds, no refraction, no blending with the star's own lines. The
    published high-resolution method adds many narrow lines together and is a
    different measurement.
14. **Every transit is observed.** Calendar times assume the ship stays and
    never misses one.
15. **At cruise the star is still a blackbody and the instrument band is a
    top hat.** The rule N′ = D × N_rest holds for any spectrum, but the rows
    use the Planck curve far from its peak, where real stars differ most.

## 6. Rulings and remaining evidence

**Unverified:** the coronagraph core throughput of 0.30 inside τ; the two
giant-planet albedos; the
tables of Sullivan, Brown & Wetherill 1978 (abstract only); Sheikh et al.
2025's cases 2 and 3, which the formula does not reproduce (§3). The primary
sources behind three secondary citations were not opened: Ekers et al. 2002
(radar EIRP), Enriquez et al. 2017 (radiometer equation) and Braun et al.
2017 (array SEFD). No Habitable Worlds Observatory document was opened; the
6 m class figure in signals-and-instruments §4.2 is not checked here.

**Rulings (Mark, attended 2026-11-12).**

1. Zodiacal light is included: local zodi and exozodi enter the background
   (§2.4). OB-48 keeps the Z = 0 value of OB-15 for comparison.
2. Two detection thresholds are pinned: SNR 5 is "a hint" and SNR 10 is
   "strong" (OB-41, OB-42).
3. The 100 m dish (efficiency 0.70, 25 K) is the ship's starting radio
   receiver. The 10⁶ m² array is the later large case.
4. The registry stays in this file. `sim/tools/lore_values.ail` must be
   taught to read it in RT1.

The same session confirmed the plain Poisson SNR formula, without the factor
2 for background subtraction, as a game approximation.

**Open for review:** the local zodi level ℓ for a ship that is not inside a
planetary system; the core throughput inside τ.

**Design values awaiting a ruling (addendum).** The sim's evidence ledger
uses these. This registry records them and does not rule.

| Value | Used by the builder | What the rows show |
|---|---|---|
| Integration time per target at a stop | 30 days | Enough for coronagraph spectra to 59 ly except a strong reading at the very edge (OB-50: 732 hours is 30.5 days). Catches 4 transits of TRAPPIST-1 e and usually none of a one-year planet (OB-60, OB-65) |
| Integration time per target at cruise | 1 hour | Ship time. At rest, an hour gives a hint on an Earth twin only out to about 10 ly (OB-17: 0.68 hours at 10 ly) |
| Design band of the spectrograph | 0.5–1.0 µm | Sets the ring of headings in OB-100 to OB-110 |
| Geometric albedo | 0.2 for every planet | Verified for an Earth twin only (§3). OB-4 and OB-5 use other design values |

Also open, from this addendum: a noise floor for transit work (none is set,
scope item 12); the throughput 0.135 with no coronagraph; n = 2 for the
O₂ A band.

**Unverified in the addendum:** μ = 28.97; the 0.40 M☉ of the M3V host; the
CODATA value of m_u (not re-opened). The transit formulas are standard and no
derivation source was opened; two published values check them (§3).

**Oracle.** An independent Python script (standard library only) recomputes
every row from §2 and compares it with the table above at the printed
significant figures. The design repo has no tools directory, so the script
and its output are in the draft PR's description.
