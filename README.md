# Post-Combustion CO₂ Capture Simulation

Steady-state process simulation of a 90% post-combustion CO₂ capture system using 30 wt% MEA solvent, built in ASPEN Plus and validated against various literature studies. 

## Key results

| Metric | Simulated (Knudsen et al., 2009\) | Esbjerg benchmark | Error |
| :---- | :---- | :---- | :---- |
| CO₂ capture rate | 90.14% | 90% | 0.14% |
| Reboiler duty (GJ/tonne CO₂) | 4.147 | \~3.6–4.0 | 3.675% |
| Lean loading (mol CO₂/mol MEA) | 0.3 | \~0.2-0.25 | 20% |

---

## Process overview

Post-combustion capture removes CO₂ from flue gas after combustion, making it retrofittable to existing power plants. The MEA loop used in this simulation consists of four main unit operations:

1. **Absorber** — flue gas contacts lean MEA solvent; CO₂ is chemically absorbed  
2. **Stripper** — heat drives CO₂ off the rich solvent, regenerating it for reuse  
3. **Condenser** — separates the CO₂ and Water vapor by condensing the water to liquid and recycling this feed back to the stripper  
4. **Mixer** — combines the lean recycle stream with makeup streams of water and MEA to maintain material balance

The simulation uses ASPEN Plus with the ELECNRTL property method and rate-based RadFrac columns for both the absorber and stripper, which captures the ionic chemistry (carbamate, bicarbonate, protonated MEA) and mass-transfer kinetics that equilibrium-stage models miss.

---

## Repository structure

co2-capture-simulation/

├── README.md

├── lab\_notebook.md           ← running project log

├── aspen/

│   └── co2\_capture.bkp       ← ASPEN Plus backup file

├── python/

│   ├── sensitivity\_analysis.ipynb

│   ├── techno\_economics.ipynb

│   └── aspen\_com\_interface.py

├── data/

│   ├── esbjerg\_validation.csv

│   └── sensitivity\_results.csv

├── figures/

│   └── \*.png

├── report/

│   └── co2\_capture\_report.pdf

└── dashboard/

    └── app.py                ← Streamlit dashboard

---

## Methodology

### Property method

ELECNRTL (electrolyte NRTL) — required for accurate modeling of the ionic CO₂–MEA–H₂O system. Properties were imported from an AspenTech example file ‘Rate-based model of the CO2 capture process by MEA using Aspen Plus’

### Column configuration

Rate-based RadFrac for both absorber and stripper. Rate-based modeling accounts for mass-transfer resistance between vapor and liquid phases and uses reaction kinetics directly, rather than assuming equilibrium between phases at each stage. Column configuration was based on simulation described in Arachchige et al., 2012\.

### Reaction set

Equilibrium reactions (liquid phase): CO₂ hydration, MEA protonation, carbamate formation, bicarbonate equilibrium. Kinetic reactions: CO₂–MEA second-order forward reaction. All reactions were pulled from AspenPlus example files. 

### Validation approach

Simulated outputs (reboiler duty, lean/rich loading, column temperature profiles, capture rate) are compared against the published CASTOR Esbjerg pilot plant dataset (Knudsen et al., 2009). 

---

## Sensitivity analysis

Three parametric studies performed using ASPEN's built-in Sensitivity tool, post-processed in Python:

- Reboiler duty vs. lean loading   
- Capture rate vs. liquid-to-gas (L/G) ratio / carbon-to-solvent ratio  
- Reboiler duty vs. stripper pressure

Optimization: ASPEN Optimization block minimizing reboiler duty subject to ≥90% CO₂ capture. Decision variables: FLUEGAS mass flow, specified reboiler duty in STRIPPER block

---

## Techno-economic analysis

Screening-level capital and operating cost estimate (±30%) using NETL cost correlations (DOE/NETL-2019/2062). Outputs: levelized cost of CO₂ capture ($/tonne CO₂) as a function of capture rate and reboiler duty.

---

## References

Aspen Technology, Inc. (2014). *Rate-based model of the CO2 capture process by MEA using*   
    *Aspen Plus* \[Software documentation\]. Aspen Technology, Inc.   
    \[https://www.aspentech.com\](https://www.aspentech.com)  
Arachchige, U. S. P. R., Mohsin, M., Melaaen, M. C., & Tel-Tek, P. (2012). Optimization of post combustion carbon capture process-solvent selection.         *International Journal of Energy and Environment (Print)*, *3*. https://www.osti.gov/etdeweb/biblio/22106515   
Knudsen, J. N., Jensen, J. N., Vilhelmsen, P.-J., & Biede, O. (2009). Experience with CO2 capture from coal flue gas in pilot-scale: Testing of             different amine solvents. *Energy Procedia*, *1*(1), 783–790. https://doi.org/10.1016/j.egypro.2009.01.104   
---

## About

Built as a portfolio project by Dean, a rising junior in Chemical Engineering at Vanderbilt University. Summer 2026\.  
