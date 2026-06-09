# Post-Combustion CO₂ Capture Simulation

Steady-state process simulation of a 90% post-combustion CO₂ capture system using 30 wt% MEA solvent, built in ASPEN Plus and validated against the CASTOR Esbjerg pilot plant dataset (Knudsen et al., 2009).

## Key results

> ⚠️ *Project in progress — results will be filled in as phases complete.*

| Metric | Simulated | Esbjerg benchmark | Error |
|---|---|---|---|
| CO₂ capture rate | — | 90% | — |
| Reboiler duty (GJ/tonne CO₂) | — | ~3.6–4.0 | — |
| Lean loading (mol CO₂/mol MEA) | — | ~0.22 | — |

---

## Process overview

Post-combustion capture removes CO₂ from flue gas after combustion, making it retrofittable to existing power plants. The MEA loop consists of four main unit operations:

1. **Absorber** — flue gas contacts lean MEA solvent; CO₂ is chemically absorbed
2. **Rich-lean heat exchanger** — energy recovery between rich and lean solvent streams
3. **Stripper** — heat drives CO₂ off the rich solvent, regenerating it for reuse
4. **Reboiler/condenser** — closes the thermal loop on the stripper

The simulation uses ASPEN Plus with the ELECNRTL property method and rate-based RadFrac columns for both the absorber and stripper, which captures the ionic chemistry (carbamate, bicarbonate, protonated MEA) and mass-transfer kinetics that equilibrium-stage models miss.

---

## Repository structure

```
co2-capture-simulation/
├── README.md
├── lab_notebook.md           ← running project log
├── aspen/
│   └── co2_capture.bkp       ← ASPEN Plus backup file
├── python/
│   ├── sensitivity_analysis.ipynb
│   ├── techno_economics.ipynb
│   └── aspen_com_interface.py
├── data/
│   ├── esbjerg_validation.csv
│   └── sensitivity_results.csv
├── figures/
│   └── *.png
├── report/
│   └── co2_capture_report.pdf
└── dashboard/
    └── app.py                ← Streamlit dashboard
```

---

## Methodology

### Property method
ELECNRTL (electrolyte NRTL) — required for accurate modeling of the ionic CO₂–MEA–H₂O system. Standard activity-coefficient-based methods (RK-Soave, Peng-Robinson) do not capture the ionic speciation that drives absorption.

### Column configuration
Rate-based RadFrac for both absorber and stripper. Rate-based modeling accounts for mass-transfer resistance between vapor and liquid phases and uses reaction kinetics directly, rather than assuming equilibrium between phases at each stage.

### Reaction set
Equilibrium reactions (liquid phase): CO₂ hydration, MEA protonation, carbamate formation, bicarbonate equilibrium.
Kinetic reactions: CO₂–MEA second-order forward reaction (Hikita et al., 1977 rate constants).

### Validation approach
Simulated outputs (reboiler duty, lean/rich loading, column temperature profiles, capture rate) are compared against the published CASTOR Esbjerg pilot plant dataset (Knudsen et al., 2009). Target agreement: ±5% on reboiler duty.

---

## Sensitivity analysis

Three parametric studies performed using ASPEN's built-in Sensitivity tool, post-processed in Python:

- Reboiler duty vs. lean loading (constrained: capture rate ≥90%)
- Capture rate vs. liquid-to-gas (L/G) ratio
- Reboiler duty vs. stripper pressure

Optimization: ASPEN Optimization block minimizing reboiler duty subject to ≥90% CO₂ capture. Decision variables: lean loading, L/G ratio, stripper pressure.

---

## Techno-economic analysis

Screening-level capital and operating cost estimate (±30%) using NETL cost correlations (DOE/NETL-2019/2062). Outputs: levelized cost of CO₂ capture ($/tonne CO₂) as a function of capture rate and reboiler duty.

---

## References

- Knudsen, J.N. et al. (2009). Pilot-scale demonstration of CO₂ capture from a coal-fired power plant using MEA. *Energy Procedia* 1(1), 783–790.
- Rochelle, G.T. (2009). Amine scrubbing for CO₂ capture. *Science* 325(5948), 1652–1654.
- Freguia, S. & Rochelle, G.T. (2003). Modeling of CO₂ capture by aqueous monoethanolamine. *AIChE Journal* 49(7), 1676–1686.
- Hikita, H. et al. (1977). The kinetics of reactions of carbon dioxide with monoethanolamine. *Chemical Engineering Journal* 13(1), 7–12.
- NETL (2019). Cost and Performance Baseline for Fossil Energy Plants. DOE/NETL-2019/2062.

---

## About

Built as a portfolio project by Dean, rising junior in Chemical Engineering (Materials & Sustainability focus) at Vanderbilt University. Summer 2026.
