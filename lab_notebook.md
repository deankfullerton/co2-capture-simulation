# CO₂ Capture Simulation — Lab Notebook
**Project:** Post-combustion CO₂ capture, 30 wt% MEA solvent
**Simulation platform:** ASPEN Plus (rate-based RadFrac)
**Author:** Dean
**Started:** June 2026
**Cert exam:** July 28, 2026
**Project deadline:** ~August 8, 2026

---

## Table of contents

- [Project scope & goals](#project-scope--goals)
- [Key references](#key-references)
- [Phase 1 — Flowsheet & property setup](#phase-1--flowsheet--property-setup)
- [Phase 2 — Convergence & pilot validation](#phase-2--convergence--pilot-validation)
- [Phase 3 — Sensitivity analysis & optimization](#phase-3--sensitivity-analysis--optimization)
- [Phase 4 — TEA & documentation](#phase-4--tea--documentation)
- [Convergence log](#convergence-log)
- [Parameter decisions](#parameter-decisions)
- [Issues & resolutions](#issues--resolutions)

---

## Project scope & goals

### System overview
Post-combustion CO₂ capture from coal-fired flue gas using 30 wt% monoethanolamine (MEA) solvent.
Standard absorber–stripper loop with cross heat exchanger and lean pump.

### Process targets
- **Capture rate:** ≥90% CO₂ removal (DOE benchmark)
- **Reboiler duty:** ~3.6–4.0 GJ/tonne CO₂ (CASTOR Esbjerg benchmark)
- **Lean loading:** 0.20–0.25 mol CO₂/mol MEA (target range)

### Feed specifications (CASTOR Esbjerg basis)
| Parameter | Value |
|---|---|
| CO₂ mol% | ~13% |
| N₂ + O₂ mol% | ~87% |
| Temperature | 50°C |
| Pressure | 1 atm |
| Flow basis | TBD (scale to pilot) |

### Simulation depth
- Steady-state, rate-based RadFrac (absorber + stripper)
- Property method: ELECNRTL
- Validated against: Knudsen et al. (2009) Esbjerg pilot data + DOE 90% benchmark
- Post-processing: Python (pandas, matplotlib, ASPEN COM interface)
- Techno-economics: NETL cost correlations, levelized $/tonne CO₂

### Deliverables
- [ ] ASPEN Plus backup file (.bkp)
- [ ] Esbjerg validation parity plot
- [ ] Sensitivity analysis plots (reboiler duty vs. lean loading, capture rate vs. L/G)
- [ ] Python Jupyter notebooks (sensitivity + TEA)
- [ ] Streamlit interactive dashboard
- [ ] 5–7 page AIChE-style technical report
- [ ] GitHub repository (structured, with README)

---

## Key references

### Primary validation dataset
- **Knudsen, J.N. et al. (2009).** "Pilot-scale demonstration of CO₂ capture from a coal-fired power plant using MEA." *Energy Procedia* 1(1), 783–790.
  - Source: CASTOR project, Esbjerg pilot plant, Denmark
  - Key data: column temperature profiles, lean/rich loading, reboiler duty, capture rate
  - Use: primary validation target for ASPEN simulation

### Process fundamentals
- **Rochelle, G.T. (2009).** "Amine scrubbing for CO₂ capture." *Science* 325(5948), 1652–1654.
  - Overview of amine capture chemistry, energy penalty, and key engineering tradeoffs

- **Kohl, A.L. & Nielsen, R.B. (1997).** *Gas Purification*, 5th ed. Gulf Publishing.
  - Chapter 2: MEA absorption fundamentals, loading, reaction chemistry
  - The standard industry reference for amine gas treating

### Thermodynamic modeling
- **Chen, C.C. & Evans, L.B. (1986).** "A local composition model for the excess Gibbs energy of aqueous electrolyte systems." *AIChE Journal* 32(3), 444–454.
  - Theoretical basis for the ELECNRTL property method used in ASPEN

- **Freguia, S. & Rochelle, G.T. (2003).** "Modeling of CO₂ capture by aqueous monoethanolamine." *AIChE Journal* 49(7), 1676–1686.
  - Rate-based modeling of MEA absorber/stripper in simulation context

### Reaction kinetics
- **Hikita, H. et al. (1977).** "The kinetics of reactions of carbon dioxide with monoethanolamine, diethanolamine and triethanolamine." *Chemical Engineering Journal* 13(1), 7–12.
  - Kinetic parameters for CO₂–MEA reaction used in rate-based ASPEN setup

### Techno-economics
- **NETL (2019).** *Cost and Performance Baseline for Fossil Energy Plants.* DOE/NETL-2019/2062.
  - Cost correlations for capital and operating costs
  - Use: TEA screening-level estimate (±30%)

---

## Phase 1 — Flowsheet & property setup

**Target weeks:** 1–2 (early June 2026)
**Status:** 🔲 Not started

### Objectives
- [ ] Configure ELECNRTL property method with MEA–CO₂–H₂O chemistry
- [ ] Define equilibrium reaction set (liquid phase)
- [ ] Define kinetic reaction set (CO₂–MEA)
- [ ] Set up feed stream (flue gas composition, T, P)
- [ ] Build flowsheet: absorber, stripper, cross-HX, lean pump, makeup
- [ ] Confirm column configurations (rate-based RadFrac, packing type, # stages)

### ELECNRTL setup notes
*[Fill in as you work — parameters used, data sources for binary interaction parameters, any deviations from defaults]*

### Reaction set — equilibrium reactions
*[Fill in: CO₂ hydration, MEA protonation, carbamate formation, bicarbonate equilibrium — with equilibrium constants and sources]*

### Reaction set — kinetic reactions
*[Fill in: forward/reverse rate expressions, Arrhenius parameters, source paper]*

### Flowsheet configuration
*[Fill in: stream names and specs, block settings for each unit op, design specs used]*

### Phase 1 issues log
*[Describe any setup errors, parameter warnings, or unexpected behavior here]*

---

## Phase 2 — Convergence & pilot validation

**Target weeks:** 3–4 (late June 2026) — with 2-week buffer if needed
**Status:** 🔲 Not started

### Objectives
- [ ] Achieve converged absorber solution
- [ ] Achieve converged stripper solution
- [ ] Full loop convergence (with recycle)
- [ ] Compare reboiler duty to Esbjerg benchmark (target: within 5%)
- [ ] Compare column temperature profiles to Knudsen et al. (2009) Figure X
- [ ] Compare lean/rich loading to published values
- [ ] Generate parity plot of simulated vs. measured outputs

### Convergence strategy notes
*[Document initialization sequence, convergence tolerances used, Design Spec targets, any tricks that worked]*

### Validation results
| Metric | Esbjerg (Knudsen 2009) | This simulation | % error |
|---|---|---|---|
| CO₂ capture rate | | | |
| Reboiler duty (GJ/tonne CO₂) | | | |
| Lean loading (mol/mol) | | | |
| Rich loading (mol/mol) | | | |
| Absorber top temp (°C) | | | |
| Absorber bottom temp (°C) | | | |

### Phase 2 issues log
*[Convergence failures, workarounds, parameter adjustments]*

---

## Phase 3 — Sensitivity analysis & optimization

**Target weeks:** 5–6 (mid July 2026)
**Status:** 🔲 Not started

### Objectives
- [ ] Sensitivity: reboiler duty vs. lean loading (hold capture rate ≥90%)
- [ ] Sensitivity: capture rate vs. L/G ratio
- [ ] Sensitivity: stripper pressure vs. reboiler duty
- [ ] ASPEN Optimization: minimize reboiler duty s.t. capture ≥90%
- [ ] Export all sensitivity results to CSV
- [ ] Python: ingest CSVs, generate matplotlib figures

### Decision variables for optimization
| Variable | Lower bound | Upper bound | Notes |
|---|---|---|---|
| Lean loading (mol/mol) | 0.15 | 0.30 | |
| L/G ratio | TBD | TBD | |
| Stripper pressure (bar) | 1.0 | 3.0 | |

### Sensitivity results
*[Paste or link CSV filenames here as they're generated]*

### Phase 3 issues log
*[COM interface issues, convergence failures during sweeps, Python errors]*

---

## Phase 4 — TEA & documentation

**Target weeks:** 7–8+ (late July – early August, primarily post-exam)
**Status:** 🔲 Not started

### Objectives
- [ ] NETL capital cost estimate for absorber, stripper, HX, reboiler
- [ ] Operating cost: steam cost (reboiler duty × steam price), solvent makeup, utilities
- [ ] Levelized cost of capture ($/tonne CO₂)
- [ ] Cost sensitivity: $/tonne vs. capture rate, $/tonne vs. reboiler duty
- [ ] Draft technical report (AIChE format)
- [ ] Finalize GitHub repo and README
- [ ] Build Streamlit dashboard
- [ ] Final review and portfolio packaging

### TEA assumptions
| Parameter | Value | Source |
|---|---|---|
| Steam price ($/GJ) | TBD | NETL or EIA |
| MEA price ($/kg) | TBD | Market |
| Plant capacity (tonne CO₂/day) | TBD | Scale from Esbjerg |
| Plant lifetime (years) | 20 | Standard TEA assumption |
| Discount rate | 10% | NETL baseline |

### TEA results
*[Fill in: capital cost, operating cost, levelized $/tonne CO₂]*

### Phase 4 issues log

---

## Convergence log

*Running record of every major convergence event — failures, fixes, and what worked.*

| Date | Phase | Issue | Fix applied | Resolved? |
|---|---|---|---|---|
| | | | | |

---

## Parameter decisions

*Every non-default parameter choice, with justification and source. This becomes your Methods section.*

| Parameter | Value used | Default | Justification | Source |
|---|---|---|---|---|
| Property method | ELECNRTL | — | Required for ionic amine-CO₂-H₂O system | Chen & Evans (1986) |
| Column type | Rate-based RadFrac | Equilibrium | Better pilot data agreement; industry standard | Freguia & Rochelle (2003) |
| | | | | |

---

## Issues & resolutions

*Catch-all for anything that doesn't fit above.*

| Date | Description | Resolution |
|---|---|---|
| | | |

