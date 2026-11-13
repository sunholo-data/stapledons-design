# Life and worlds: normative baseline and check values

**Version:** baseline-2026.11 (2026-11-02). **Status:** draft normative registry for
R3-WORLDS W1.0; Mark approved the implementation recommendations in game ledger
D-144. This registry awaits review in its draft PR. Its measured, adopted and
unverified labels are part of the data, not editorial footnotes.
**Addendum (2026-11-12, draft, R3-UNIVERSE-LAB UL1a):** derived rows LW-46 to
LW-54 (§4); what the baseline's factors already include (§6); the deliberate
departures from the literature (§7). No baseline value changes.

## 1. Baseline and provenance

The dial changes only life, minds, technology and technological lifetime. It
never changes stars, planet occurrence, propagation, detection, loudness or
remnant persistence. A playthrough pins this baseline with its catalogue version.
A future measurement changes the version, never a running universe.

| Factor | Adopted baseline / prior | Evidence and precise location | Status; dial |
|---|---|---|---|
| Local density rho0 | 0.079 pc^-3 | [Gaia Collaboration, Smart et al. 2021](https://arxiv.org/abs/2012.02061), abstract and §6: 331,312 selected GCNS entries with nonzero probability of being within 100 pc (estimated 9% contamination from beyond the boundary); dividing by 4 pi 100^3/3 gives 0.0790963. §5.2/Fig. 16 luminosity-function analysis and §6 instead give a completeness-corrected main-sequence density 0.081 ± 0.003 pc^-3 | Adopted raw-count approximation, not the paper's corrected density; fixed |
| Disc scale height H | 300 pc | [Jurić et al. 2008](https://faculty.washington.edu/ivezic/Publications/tomographyI.pdf), Table 10, p. 901, bias-corrected H1; their Eq. 21–24 define the two-disc/halo model | Verified; use only the local thin-disc vertical exponential, no radial gradient; fixed |
| Type shares FGK / M / other | 0.20 / 0.75 / 0.05 | [Reylé et al. 2021](https://arxiv.org/abs/2104.14972), Table 3, p. 5, reports F=8, G=18, K=38, M=249, A=4, D=20 plus L/T/Y and unknown types. §3.5/Fig. 2 discuss spectral completeness; abstract's ~61% uses stars AND brown dwarfs | **Unverified as an empirical 0.75 M fraction for the model's denominator.** Keep the approved nominal approximation explicitly as adopted, not measured to two figures. FGK 0.20 is the existing reference assumption. Not dialled |
| Galactic mass formation rate | 1.65 ± 0.19 solar masses/yr | [Licquia & Newman 2015](https://doi.org/10.1088/0004-637X/806/1/96), abstract, Table 1 and §3.3 posterior; Kroupa IMF | Verified; mass/yr, NOT stars/yr. Archive context only, not an extra count-model multiplier |
| Planet abundance | at least order unity planets/star | [Cassan et al. 2012](https://www.nature.com/articles/nature10684), [primary preprint](https://arxiv.org/pdf/1202.0903), final results paragraph (PDF p. 4), integrating the fitted mass function over 0.5–10 AU and 5 Earth masses–10 Jupiter masses: 1.6 (+0.72, -0.89) planets/star | Verified domain-limited mean; not a probability f_p >= 1; occurrence below already includes planet abundance |
| etaEarth(FGK) | fixed adopted 0.4 planets/star | [Bryson et al. 2021](https://arxiv.org/abs/2010.14812), Table 5, model 1, conservative HZ, 0.5–1.5 Earth radii, 4800–6300 K: lower/upper extrapolation medians 0.37 (+0.48,-0.21) to 0.60 (+0.90,-0.36) | Verified; 0.37–0.60 is a bracket of model medians, NOT a uniform prior or confidence interval; 0.4 adopted; fixed |
| etaEarth(M) | fixed adopted 0.16 planets/star | [Dressing & Charbonneau 2015](https://arxiv.org/abs/1501.01623), §7.3, Table 8, p. 19: 1–1.5 Earth radii, moist/maximum greenhouse column, 15.82% (+16.60,-6.54); abstract rounds to 0.16 (+0.17,-0.07) | Verified; the cited inner edge is **moist** greenhouse (2013 prescription), not the 2014 runaway edge; adopted occurrence approximation; habitability debated; fixed |
| fLife | X=log10(lambda Vt) ~ Normal(0,50^2); f=1-exp(-10^X); median 0.632120558828558 | [Sandberg, Drexler & Ord 2018](https://arxiv.org/html/1806.02404), §3.1.1 (PDF p. 9), Table 1 life entry refers to that model | Verified rate prior; paper's evolutionarily competent life mapped here to biosphere probability as an explicit game assumption; dial life |
| fMinds | log-uniform [0.001,1], median 0.0316227766016838 | Sandberg et al., Table 1 | Verified expert-uncertainty model, not a measured frequency; dial minds |
| fComplex | fixed split, median 0.316227766016838, always >= fMinds | Game design: the split of the intelligence pathway; not a literature estimate | Design; scales with minds, clamped to [fMinds,1] |
| fTech | log-uniform [0.01,1], median 0.1 | Sandberg et al., Table 1, row f_c. §1, after Eq. 1, defines it: "f_c is the fraction of intelligent civilizations that are detectable/contactable" | Verified expert-uncertainty model; it is a fraction **detectable**, read here as "builds technology" (§6); dial tech |
| L | log-uniform [100,100000000] yr; median 100000 yr | D-144 keeps the [life feature](../features/next/life-and-intelligence-parameters.md) range. Sandberg Table 1 actually uses [100,10000000000] yr, median 1000000 yr. Sandberg §1 defines L as "the average longevity of such detectable civilizations" | Source verified; canon range is a deliberate difference (§7, row 1); dial lifetime |
| Epoch window | 5000000000 yr; onsets uniform inside it, centred on now | Life feature's analytic model. No source gives this window or a uniform onset law | Design; fixed. Estimated effect on counts: within about ±0.5 dex, sign unknown ([seti-factors §3.6](../features/next/seti-factors.md): a judgement from four abstracts, not a calculation) |
| Remnant persistence | log-uniform [1000,10000000] yr | D-144, not a measurement. [Schmidt & Frank 2019](https://doi.org/10.1017/S1473550418000095), §§2–4 discuss preservation and ambiguity, not this numerical lifetime distribution | Design; fixed |
| Ending shares collapse / ecocide / transcendence | 0.75 / 0.15 / 0.10 | D-144, superseding example rates in planet-state-transitions | Design; fixed |
| Loudness | No calibrated numeric prior yet | [Price et al. 2020](https://doi.org/10.3847/1538-3881/ab65f1), §5 and sensitivity/occurrence discussion: narrowband search of 1327 stars, 1.10–3.45 GHz | Null results constrain specified transmitters and selection functions, not universal fTech; never dialled. Instrument/loudness choices belong to S2/S3 |

All six design values explicitly marked **verify** have now been checked:
etaEarth(M), M share, fMinds, fTech, Sandberg's L range and the solar HZ
fluxes/distances (§2). Only the M share remains unverified as stated. Design
parameters do not become literature values because a related paper is cited.

## 2. Habitability physics

[Kopparapu et al. 2014](https://arxiv.org/abs/1404.5292), Table 1 (PDF p. 12),
Eqs. 4–5 (p. 9): T=Teff-5780 K; Seff=S0+aT+bT²+cT³+dT⁴;
distance in AU=sqrt(luminositySolar/Seff). These are the 1 Earth-mass limits.
Clamp Teff to 2600–7200 K and flag extrapolation outside that interval. These
are climate-model boundaries, not proof of life or measured planet climates.

| Limit | S0 | a | b | c | d |
|---|---|---|---|---|---|
| Recent Venus | 1.776 | 2.136e-4 | 2.533e-8 | -1.332e-11 | -3.097e-15 |
| Runaway greenhouse | 1.107 | 1.332e-4 | 1.580e-8 | -8.308e-12 | -1.931e-15 |
| Maximum greenhouse | 0.356 | 6.171e-5 | 1.698e-9 | -3.198e-12 | -5.575e-16 |
| Early Mars | 0.320 | 5.547e-5 | 1.526e-9 | -2.874e-12 | -5.011e-16 |

Insolation is L/a² in Earth units. Equilibrium temperature assumes full heat
redistribution: Teq=Teff sqrt(Rstar/(2a)) (1-A)^0.25. This excludes greenhouse
warming. Circular geometric transit probability (neglect planet radius) is Rstar/a.
Radial velocity K=(2 pi G/P)^(1/3) m sin(i)/(Mstar+m)^(2/3)/sqrt(1-e²), SI.
Mutual Hill radius is ((m1+m2)/(3 Mstar))^(1/3) (a1+a2)/2; separation in Hill
radii is (a2-a1)/RH ([Chambers et al. 1996](https://doi.org/10.1006/icar.1996.0019),
original §2 locator **unverified** because the full text was inaccessible).
The formula is verified against [Carrera, Davies & Johansen 2016](https://academic.oup.com/mnras/article/463/3/3226/2646619),
Eqs. 1–2. No stability guarantee is inferred from a single separation test.

Constants for check cases: AU=149597870700 m, nominal Rsun=695700000 m,
G=6.67430e-11 SI, solar mass=1.98847e30 kg, Earth mass=5.9722e24 kg,
year=365.25 days, day=86400 s. The RV cases below use these constants,
P(Earth)=365.25 d and P(Jupiter)=4332.59 d, m(Jupiter)=317.828 Earth masses,
sin(i)=1, e=0; rounding tolerances are 0.05 m/s for Jupiter and 0.0001 m/s for Earth.

## 3. Count model and historical reference cases

For r=R_ly/3.26156 in pc, local density rho exp(-|z|/H),
N(R)=2 pi rho H [r²-2H²+2H(r+H) exp(-r/H)]. At small r/H evaluate
with a convergent series to avoid cancellation. This is a locally uniform
radial disc approximation, not a Galactic-centre model.

Let q=FGK etaFGK (or add M etaM for the baseline). Then biospheres=N q fLife,
everTech=N q fLife fMinds fTech, alive=everTech L/Twin,
and overlapping a window W=everTech (L+W)/Twin, away from the epoch-window
edges. Count the epoch overlap once, not once per endpoint.

The two tables in the life feature are computed from **ref-2026-10-06**:
FGK=0.20, etaFGK=0.4, M off, fLife=0.3, fMinds*fTech=0.005, L=10000 yr,
Twin=5e9 yr. They are not tables of baseline-prior medians. Existing rounded
cells remain historical reference arithmetic. Three historical cells disagree
with strict rounding of the formula: 1000-ly everTech=803.838985686 (prints
804, not 805); 10000-ly everTech=164765.267127 (prints 1.6e5, not 1.7e5;
the old value double-rounds biospheres first); 5000-ly overlap over 24000 yr
is 0.264501263335 (prints 0.26, not 0.27). Package tests use the derived
values and report these transcription discrepancies; they do not tune the
density to reproduce inconsistent rounding.
 Optimistic uses fLife=1,
fMinds=0.1, fTech=0.1, L=1e6 yr. Pessimistic uses fLife=fMinds=0.001,
fTech=0.1 (reconstruction: omitted in the old text), L=1000 yr.

The dial multiplies each median by 10^offset. Fractions saturate at 1;
fComplex is at least fMinds. Effective luck is log10[(fLife fMinds fTech L)
/ (baseline product)], **after** saturation. Zero offsets are Lonely Universe.
The other preset offsets and the slice seed are pending calibration; this
registry contains no invented calibration results.

Histories are immutable draws from planet-keyed uniforms. Habitable worlds
have a long-lived microbial/complex background. Technological onsets share
the same draw across presets (uniform in the centred 5 Gyr window); dialled
lifetimes extend endings. PreTech precedes technology by a log-uniform
1e4–1e5 yr. A planet's measured constraints override a possible biosphere.
M-dwarf HZ worlds are labelled “habitability debated”. No synthetic planets
are added to measured systems (D-144).

### Planet occurrence away from measured systems

[NASA Exoplanet Standards Team final report (2019)](https://science.nasa.gov/wp-content/uploads/2023/04/Standards_Team_Final_Report_2019-Oct-17.pdf),
§3.a.i.1, Eq. 1 and Table 1, printed p. 5: SAG13 nominal G-dwarf occurrence
is d²N/(d ln R d ln P)=Gamma R^alpha P^beta, R in Earth radii and P in
**years**. Below 3.4 Earth radii (Gamma,alpha,beta)=(0.38,-0.19,0.26);
above it (0.73,-1.18,0.59). Integrate piecewise across the radius break.
The [SAG13 closeout](https://exoplanets.nasa.gov/system/internal_resources/details/original/680_SAG13_closeout_8.3.17.pdf),
slide 18, explicitly supplies those units. Applying this law to M dwarfs is
an adopted extrapolation; the dedicated etaEarth slot fixes HZ occurrence
separately. Cold-period extrapolation beyond ~640 days is poorly constrained
and must be labelled. This is not an observational upper limit on unseen planets.

## 4. LW check-value registry


Each row is one scalar. Values are model/check cases unless §1 labels them
measured. Printed precision defines rounding tolerance; no row claims an exact
uncertain astronomical measurement. HZ checks use Teff=5780 K, L=1.

| ID | Quantity | Value | Unit |
|---|---|---|---|
| LW-1 | Adopted local stellar density | 0.079 | pc^-3 |
| LW-2 | Adopted thin-disc scale height | 300 | pc |
| LW-3 | Adopted FGK fraction | 0.20 | — |
| LW-4 | Adopted M fraction (unverified empirical attribution) | 0.75 | — |
| LW-5 | Adopted etaEarth FGK | 0.4 | planets/star |
| LW-6 | Adopted etaEarth M | 0.16 | planets/star |
| LW-7 | Sandberg life median | 0.632120558828558 | — |
| LW-8 | Intelligence median | 0.0316227766016838 | — |
| LW-9 | Technology median | 0.1 | — |
| LW-10 | Canon lifetime median | 100000 | yr |
| LW-11 | Epoch window (design; onsets uniform inside it) | 5000000000 | yr |
| LW-12 | Complex-life split median (design) | 0.316227766016838 | — |
| LW-13 | Solar recent Venus flux | 1.776 | S⊕ |
| LW-14 | Solar runaway greenhouse flux | 1.107 | S⊕ |
| LW-15 | Solar maximum greenhouse flux | 0.356 | S⊕ |
| LW-16 | Solar early Mars flux | 0.320 | S⊕ |
| LW-17 | Solar conservative inner HZ | 0.950 | AU |
| LW-18 | Solar conservative outer HZ | 1.676 | AU |
| LW-19 | Earth equilibrium temperature at Teff 5772 K and albedo 0.3 | 255 | K |
| LW-20 | Circular Jupiter RV on solar-mass star | 12.5 | m/s |
| LW-21 | Circular Earth RV on solar-mass star | 0.0895 | m/s |
| LW-22 | Earth geometric transit probability | 0.00465 | — |
| LW-23 | Disc stars within 500 ly | 9.92 × 10⁵ | stars |
| LW-24 | Disc stars within 1000 ly | 6.70 × 10⁶ | stars |
| LW-25 | Disc stars within 3000 ly | 1.04 × 10⁸ | stars |
| LW-26 | Disc stars within 5000 ly | 3.24 × 10⁸ | stars |
| LW-27 | Disc stars within 10000 ly | 1.37 × 10⁹ | stars |
| LW-28 | Reference alive within 1000 ly | 0.00161 | civilisations |
| LW-29 | Reference overlap 1e6 yr within 10000 ly | 33.3 | civilisations |
| LW-30 | Reference overlap 24000 yr within 3000 ly | 0.085 | civilisations |
| LW-31 | Optimistic alive within 1000 ly | 1.07 | civilisations |
| LW-32 | Optimistic alive within 10000 ly | 220 | civilisations |
| LW-33 | Pessimistic alive within 1000 ly | 1.07 × 10⁻⁸ | civilisations |
| LW-34 | Collapse share (design) | 0.75 | — |
| LW-35 | Ecocide share (design) | 0.15 | — |
| LW-36 | Transcendence share (design) | 0.10 | — |
| LW-37 | Remnant persistence lower bound (design) | 1000 | yr |
| LW-38 | Remnant persistence upper bound (design) | 10000000 | yr |
| LW-39 | Canon lifetime lower bound | 100 | yr |
| LW-40 | Canon lifetime upper bound | 100000000 | yr |
| LW-41 | Sandberg lifetime upper bound (comparison only) | 10000000000 | yr |
| LW-42 | Baseline-median alive within 1000 ly (M dwarfs included) | 0.05 | civilisations |
| LW-43 | Baseline reach for LW-42 | 1000 | ly |
| LW-44 | Galactic mass formation rate (not a stars/yr rate) | 1.65 | M☉/yr |
| LW-45 | Microlensing mean abundance in the cited mass/orbit domain | 1.6 | planets/star |
| LW-46 | Mean of the canon lifetime prior, (LW-40 − LW-39)/ln(LW-40/LW-39) | 7.238 × 10⁶ | yr |
| LW-47 | Mean of the intelligence prior, (1 − 0.001)/ln(1000) | 0.1446 | — |
| LW-48 | Mean of the technology prior, (1 − 0.01)/ln(100) | 0.2150 | — |
| LW-49 | Mean of the remnant-persistence prior (design), (LW-38 − LW-37)/ln(LW-38/LW-37) | 1.086 × 10⁶ | yr |
| LW-50 | Remnant-state share of habitable-zone worlds when life, minds and technology are all 1: LW-34 × LW-49 / LW-11 | 1.63 × 10⁻⁴ | — |
| LW-51 | Baseline living share A₀ = LW-7 × LW-8 × LW-9 × LW-10 / LW-11 | 3.998 × 10⁻⁸ | — |
| LW-52 | Habitable-zone worlds per star, LW-3 × LW-5 + LW-4 × LW-6 | 0.200 | planets/star |
| LW-53 | Luck for 5 expected living technological worlds within 81.5 ly, every one found: log₁₀(5/(N(81.5 ly) × LW-52)) − log₁₀(LW-51) | 5.10 | dex |
| LW-54 | The same within 1000 ly | 1.97 | dex |

LW-42 is distinct from the historical reference parameters of LW-28. With
`sunholo/worlds` 0.1.0, `applyDial(baseline(), zeroDial())`, the adopted disc,
and occurrence `0.20*0.4 + 0.75*0.16`, `aliveWithin` gives
0.05356088273972087 civilisations at 1000 ly, rounded here to one significant
figure. This is an expectation under adopted medians, not a posterior mean or
a measurement. The game regression `worlds_test.baselineReachVm` computes it.
LW-44 and LW-45 transcribe the cited evidence and domain in §1; neither is
an additional multiplier in the local count model.

**LW-46 to LW-54 (UL1a, 2026-11-12).** Each is arithmetic on rows above. No new
source is needed; the inputs carry their own labels.

- The mean of a log-uniform value between a and b is (b − a)/ln(b/a). LW-46 to
  LW-49 are that formula. LW-49 inherits the Design label of LW-37 and LW-38.
- LW-50 is the share of habitable-zone worlds that are in a remnant state now,
  if every such world had a technological lineage. It does not depend on the
  lifetime L. It is exact only while the trace time is short beside the window.
- LW-51 is A₀, the share of habitable-zone worlds that hold a living
  technological lineage now, at the prior medians. "Dex" means orders of
  magnitude: 2 dex is a factor of 100.
- LW-52 inherits the unverified label of LW-4.
- LW-53 and LW-54 use N(R) of §3: 5005 stars within 81.5 ly and LW-24 within
  1000 ly. "Luck" is the factor by which the product fLife × fMinds × fTech × L
  must exceed its median value. Both rows assume every living world is found.
  They are reference arithmetic for the count model, not targets.
- These were proposed as LW-46p to LW-54p in
  [seti-factors §5.4](../features/next/seti-factors.md). A second reader
  recomputed each one; all nine stand as proposed.

**Still provisional: LW-55p.** The chance, under the priors of §1, that
fLife × fMinds × fTech × L is at least 10^7.5 (the first calibration's universe:
every fraction 1 and L = 3.16 × 10⁷ yr). The oracle gives **2.884 × 10⁻⁴, 1 in
3467**, by a one-dimensional integral over the life prior, and a Monte Carlo of
4 × 10⁶ draws agrees (2.85 × 10⁻⁴ ± 0.08 × 10⁻⁴). The review's figure was
"about 2.9 × 10⁻⁴". The value depends on the cube of the gap below the largest
possible product, so the threshold must be exact: at 3.2 × 10⁷ it is
2.796 × 10⁻⁴. This row is pinned from the `sunholo/worlds` package's own value
when `selection` is published (sprint R3-UNIVERSE-LAB, UL1c), not before.

The oracle for this file is a Python script (standard library only). It
recomputes LW-7 to LW-10, LW-23 to LW-33, LW-42 and LW-46 to LW-54, each by
two methods where the row is not a single product. The design repo has no
tools directory, so the script and its output are in the draft PR's
description.

## 5. Attended decisions and remaining evidence

D-144 (2026-11-02) accepts OQ1–OQ9. Binding calibration strategies are the
cautious explorer and mid-range wanderer at Standard's 16000-year Earth
clock; report the pilgrim, waiter and other clocks too. Search for the slice
seed after the default preset is calibrated. Use S2 photon budgets, never a
literal detection radius. Presets remain named only on the ordinary wire;
the Archive reveals their luck later.

**Unverified:** the nominal M=0.75 fraction's empirical attribution to Reylé; the original Chambers section locator (the formula itself has the verified alternate primary citation above).
No calibrated loudness distribution, preset offset, encounter count or slice
seed is established by W1.0. Archive prose remains a draft for Mark.

## 6. What the baseline's factors already include (no factor twice)

This section fixes what each factor means, so that the count model, the
signature ladder ([observation §7](observation.md)) and the calibration cannot
apply the same loss twice.

**The source's definitions** (Sandberg, Drexler & Ord 2018, §1, the sentence
after Eq. 1; re-opened 2026-11-12): "f_c is the fraction of intelligent
civilizations that are detectable/contactable, and L is the average longevity
of such detectable civilizations in years." Table 1 gives f_c as log-uniform
from 0.01 to 1 and L as log-uniform from 100 to 10¹⁰ years. §3 says of f_c:
"the range of estimates in the literature from 10⁻² to 1".

**How the game reads them.**

- LW-9 (fTech) is the source's f_c. A **technological world** is a world that
  emits at least one sign of technology that a good enough instrument, in the
  right place at the right time, could record.
- LW-10 (L) is the time for which that stays true.
- The game calls this "builds technology" and "technological lifetime". That
  is a reading of the source, and it is listed as a departure (§7, row 4).

**Four rules that follow.**

1. **No second "share that is detectable".** Every technological world emits
   something, by definition. Nothing may multiply the count of technological
   worlds by a further fraction of worlds that are "detectable at all".
2. **A class prevalence is a conditional share.** In the ladder, the
   prevalence of a signature class is the share of technological worlds that
   have that class at some time in their life. Classes are not exclusive: a
   world can have several. The leakage classes together must cover every
   technological world for all of L. A ladder that leaves part of a life with
   no class on has shortened L, and that must be listed as a departure with
   its reason.
3. **What turns "exists" into "recorded" is applied once, in the ladder and
   the instruments:** reach (a budget in the observation registry), geometry
   (the share of the sky a beam covers), time (how often the beam is on and
   for how long it crosses the receiver), and the ages at which a class is on.
   None of these is in LW-7 to LW-10, and the dial and the selection never
   change them.
4. **The old "one technological world in ten is radar class" is retired as a
   share of worlds.** It was applied on top of LW-9 and so counted "detectable"
   twice. In the evidence ledger it acted as the chance that a radar beam
   crosses the ship during one look. That chance now comes from the ladder's
   beam and duty values ([observation §7](observation.md), OB-139 to OB-144).

**Prevalences with no measurement behind them are Design.** No source gives
the prevalence of any signature class among technological worlds. Each is a
Design value with a swept range, listed in the observation registry.

**"Detected" is defined once,** in [observation §8](observation.md): a world
is detected when the ship's evidence ledger holds a record for it. The
encounter targets count ledger records. "Biospheres detected" is the number of
worlds with a biosignature record, false positives included.

## 7. Deliberate departures from the literature

The owner's standing rule (2026-11-12): "if we depart from physics we at least
document why for gameplay reasons". This table lists every place where the
baseline differs from its source or from the wider literature. "Size" is the
effect on the count of living technological worlds unless the row says
otherwise. Pure physics stays playable: the lab's `sandberg-as-printed` and
`pure` scenarios run rows 1 and 2 as the source prints them.

| # | The baseline | The literature | Size | Reason | Kind of reason |
|---|---|---|---|---|---|
| 1 | Lifetime prior stops at 10⁸ yr (LW-40) | Sandberg Table 1: 10¹⁰ yr (LW-41) | The prior's mean falls from 5.43 × 10⁸ to 7.24 × 10⁶ yr (LW-46): 1.88 dex | A civilisation that outlasts the 5 Gyr window (LW-11) never ends in play, and the count formula alive = everTech × L / window stops being a share once L nears the window. Ruled: D-144; kept as OQ2 of R3-UNIVERSE-LAB (D-156) | Modelling and gameplay |
| 2 | Lifetime median 10⁵ yr (LW-10) | 10⁶ yr, the median of Sandberg's range | 1.00 dex | It follows from row 1: the median of a log-uniform range is the geometric middle of its ends | Modelling |
| 3 | fLife is the chance that a habitable-zone world has a biosphere | Sandberg §3.1.1: a rate model for "evolutionarily competent life" | None on the number; it changes what the number means | The game needs a state a telescope can read (a biosphere), not a rate | Gameplay |
| 4 | fTech is "builds technology"; L is the technological lifetime | Sandberg §1: f_c is "detectable/contactable"; L is the longevity "of such detectable civilizations" | None, if §6 is obeyed. A second "detectable" share would count it twice (the old one in ten: 1 dex) | The game models detection with instruments, so "detectable" must not be a hidden extra factor | Modelling |
| 5 | Stars and planets come from measurements: density LW-1, occurrence LW-52 = 0.20 habitable-zone worlds per star | Sandberg Table 1: R★, f_p and n_e are log-uniform (1–100 per year; 0.1–1; 0.1–1). The medians give f_p × n_e = 0.10 | +0.30 dex against those medians | The map is made of real stars. The count is over space near the Sun, not a Galactic birth rate | Modelling |
| 6 | M-dwarf habitable zones count in full: 0.12 of the 0.20 (LW-4 × LW-6) | Disputed. Snyder-Beattie et al. 2021 predict M-dwarf planets are uninhabitable; Shields et al. 2016 review the open questions (both read at the abstract only, in seti-factors §3.6–§3.7) | 0.40 dex fewer worlds if they are excluded | Ruled: life feature decision 3. Each such world is labelled "habitability debated" | Gameplay, labelled |
| 7 | Onsets are uniform over 5 Gyr centred on now; planets have no ages (LW-11) | Star formation is not flat (Mor et al. 2019); most Earth-like planets are older than Earth (Lineweaver 2001). Both read at the abstract only | About ±0.5 dex, sign unknown: an estimate, not a calculation | One keyed draw per world gives the same history in every preset. A planet-age model is a named follow-up (R3-UNIVERSE-LAB UL-E) | Modelling |
| 8 | One technological lineage per planet, ever | No source measures this. Wright 2018 and Schmidt & Frank 2019 treat an earlier species as an open question | None at the prior medians (a second lineage is a 10⁻⁵ event). It caps the dial at 2.70 dex in the fractions | The exact propagation law assumes one lineage. Successive lineages are ruled in as a later option (seti-factors §5.8, ruling 2) | Modelling |
| 9 | Every civilisation in one universe has the same lifetime L | Lifetimes are a population; what is detected is length-biased (Kipping, Frank & Scharf 2020, abstract only) | None on counts at equal mean; it changes who is met | The dial sets one L. A spread is a named follow-up (UL-B) | Modelling |
| 10 | No settlement between stars | Hart 1975, Newman & Sagan 1981, Carroll-Nellenback et al. 2019: a long-lived civilisation spreads (seti-factors §3.3) | −0.6 to −1.0 dex of luck needed, if 3 to 9 daughter worlds each | Ruled out of the default (seti-factors §5.8, ruling 4) | Gameplay |
| 11 | Ending shares 0.75 / 0.15 / 0.10 (LW-34 to LW-36); remnant traces last 10³ to 10⁷ yr (LW-37, LW-38); the complex-life split (LW-12) | No source gives any of these | Remnant counts scale with LW-50 | The game needs ended states and ruins. D-144 | Gameplay (Design values) |
| 12 | Density 0.079 pc⁻³, a raw count (LW-1) | The same paper's corrected density is 0.081 ± 0.003 | −0.011 dex | Kept from the approved reference; inside the paper's error | Modelling |
| 13 | A local thin disc only: no radial gradient, no thick disc or halo (LW-2) | Jurić et al. 2008 fit two discs and a halo | Small inside 3000 ly; not valid toward the Galactic centre | The play area is local. The pilgrim to Sgr A* waits for the model galaxy | Modelling |
| 14 | Type shares 0.20 / 0.75 / 0.05 (LW-3, LW-4) | Reylé et al. 2021 count objects within 10 pc; the 0.75 is not a measured share for this model's denominator | Not sized; LW-4 stays **unverified** | Kept as the approved nominal value | Modelling |

Sizes in rows 1, 2, 5, 6 and 12 are recomputed by the oracle. Rows 7 and 10
quote the review's estimates. A departure on the instrument side (how a signal
is detected, not whether a world exists) belongs to the observation registry's
scope list and its ladder, not here.
