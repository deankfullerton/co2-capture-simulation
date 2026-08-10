# CO₂ Capture Simulation — Lab Notebook

**Project:** Post-combustion CO₂ capture, 30 wt% MEA solvent   
**Simulation platform:** ASPEN Plus   
**Author:** Dean Fullerton   
**Timeline:** June 2026 through August 2026   
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

Post-combustion CO₂ capture from coal-fired flue gas using 30 wt% monoethanolamine (MEA) solvent. Standard absorber–stripper loop with some simulated modifications as documented.

### Process targets

- **Capture rate:** ≥90% CO₂ removal (DOE benchmark)  
- **Reboiler duty:** \~3.6–4.0 GJ/tonne CO₂   
- **Lean loading:** 0.20–0.25 mol CO₂/mol MEA (target range)

### Simulation depth

- Steady-state, rate-based RadFrac (absorber \+ stripper)  
- Property method: ELECNRTL  
- Validated against: Knudsen et al. (2009) Esbjerg pilot data \+ DOE 90% benchmark  
- Post-processing: Python (pandas, matplotlib, ASPEN COM interface)  
- Techno-economics: NETL cost correlations, levelized $/tonne CO₂

### Deliverables

- [ ] ASPEN Plus backup file (.bkp)  
- [x] Validation parity plot  
- [x] Sensitivity analysis plots (reboiler duty vs. lean loading, capture rate vs. L/G)  
- [x] Python Jupyter notebooks (sensitivity \+ TEA)  
- [ ] Streamlit interactive dashboard  
- [ ] 5–7 page AIChE-style technical report  
- [ ] GitHub repository (structured, with README)

---

## Phase 1 — Flowsheet & property setup

**Target weeks:** 1–2 (early June 2026\)     **Status:** ✅Complete

### Objectives

- [x] Configure ELECNRTL property method with MEA–CO₂–H₂O chemistry  
- [x] Define equilibrium reaction set (liquid phase)  
- [x] Define kinetic reaction set (CO₂–MEA)  
- [x] Set up feed stream (flue gas composition, T, P)  
- [x] Build flowsheet: absorber, stripper, lean pump, makeup  
- [x] Confirm column configurations (rate-based RadFrac, packing type, \# stages)

### ELECNRTL setup notes

ELECNRTL setup was preloaded from an ASPEN Plus Template for ELECNRTL Rate-Based MEA Carbon Capture (AspenTech, 2014).

### Components 

**Table 1\.**

| Component ID | Type | Component name | Alias | CAS number |
| :---- | :---- | :---- | :---- | :---- |
| MEA | Conventional | MONOETHANOLAMINE | C2H7NO | 141-43-5 |
| H2O | Conventional | WATER | H2O | 7732-18-5 |
| CO2 | Conventional | CARBON-DIOXIDE | CO2 | 124-38-9 |
| H3O+ | Conventional | H3O+ | H3O+ |  |
| OH- | Conventional | OH- | OH- |  |
| HCO3- | Conventional | HCO3- | HCO3- |  |
| CO3-2 | Conventional | CO3-- | CO3-2 |  |
| MEAH+ | Conventional | MEA+ | C2H8NO+ |  |
| MEACOO- | Conventional | MEACOO- | C3H6NO3- |  |
| N2 | Conventional | NITROGEN | N2 | 7727-37-9 |
| O2 | Conventional | OXYGEN | O2 | 7782-44-7 |
| CO | Conventional | CARBON-MONOXIDE | CO | 630-08-0 |
| H2 | Conventional | HYDROGEN | H2 | 1333-74-0 |
| H2S | Conventional | HYDROGEN-SULFIDE | H2S | 6/4/7783 |
| HS- | Conventional | HS- | HS- |  |
| S-2 | Conventional | S-- | S-2 |  |

### Reaction Set 

**Table 2\.** Equilibrium Reactions

| Reaction | Stoichiometry |
| :---- | ----- |
| 1 | 2 H2O  ←→   H3O\+ \+  OH\- |
| 2 | CO2  \+  2 H2O  ←→ HCO3\-  \+  H3O\+ |
| 3 | HCO3\-  \+  H2O  ←→ CO3\-2  \+  H3O\+ |
| 4 | MEAH\+  \+  H2O  ←→  MEA  \+  H3O\+ |
| 5 | MEACOO\-  \+  H2O ←→  MEA  \+  HCO3\- |
| 6 | H2S  \+  H2O  ←→  HS\-  \+  H3O\+ |
| 7 | HS\-  \+  H2O  ←→  S\-2  \+  H3O\+ |

**Table 3\.** Kinetic Reactions

| Reaction | Stoichiometry |
| :---- | ----- |
| 1 | CO2  \+  OH\-  →  HCO3\- |
| 2 | HCO3\- →   CO2  \+  OH\-  |
| 3 | MEA  \+  CO2  \+  H2O  →   MEACOO\-  \+  H3O\+ |
| 4 | MEACOO\-  \+  H3O\+→ MEA  \+  CO2  \+  H2O  |

### Flowsheet configuration

**Figure 1\.**   
![Alt text](./figures/FLOWSHEET%20LAYOUT.jpg)

### Initial Feed Stream Values

**Table 4\.**

| Stream | Flow Rate (kg/sec) | Temperature (K) | Pressure (bar) | Composition (Mole Frac) |
| :---- | :---- | :---- | :---- | :---- |
| FLUEGAS | 500  | 313 | 1.1 |  H2O 0.0818 CO2 0.1358 N2 0.7286 O2 0.0354  |
| W-MAKEUP | 10  | 313.15 | 1.1 |  H2O 1  |
| MEAIN | 10  | 313.15 | 1.1 |  MEA 1  |
| LEANIN (TEAR STREAM) | 600  | 313.15 | 1.1 |  MEA 0.175 H2O 0.7 CO2 0.025  |

	Initial Stream values were derived from the literature and adjusted for convergence errors. The initial specifications for the flue gas stream were adapted from the paper “Optimization of… Solvent Selection” (Arachchige et al., 2012). A design spec was used to determine the appropriate makeup streamflow rates based on the amount of MEA and Water entering and leaving the simulation. The values shown are just placeholders. The flow rates of FLUEGAS and LEANIN were used to stabilize and converge the RadFrac columns as well as the recycle loop. Once the recycle loop converged, a third design spec to achieve the optimal gas-to-solvent ratio that would result in a 90% CO2 capture rate.

### Rad-Frac Column Set Up

**Table 5\.** 

| Column | Absorber  | Stripper |
| :---- | :---- | :---- |
| Stages | 15 | 15 |
| Operating Pressure (bar) | 1  | 2 |
| Re-Boiler  | None | Kettle  |
| Condenser | None | None |
| Packing Type  | FLEXIPAC; KOCH; METAL; 250Y | FLEXIPAC; KOCH; METAL; 250Y |
| Packing Height (meter) | 20  | 18 |
| Packing Diameter (meter) | 11 | 7.5 |
| Reaction Condition Factor | 0.9 | 0.9 |
| Film Discretization Ratio | 5 | 5 |
| Flow Model | Mixed | Mixed |
| Interfacial area factor | 1.5 | 2 |
| Liquid \- Film resistance; Number of discretization points | Discretize film; 5 | Discretize film; 5 |
| Vapor \- Film resistance | Consider Film | Consider Film |
| Mass transfer coefficient method | Bravo et al. (1985) | Bravo et al. (1985) |
| Heat transfer coefficient method | Chilton and Colburn | Chilton and Colburn |
| Interfacial area method | Bravo et al. (1985) | Bravo et al. (1985) |
| Holdup Method Correlation | Billet and Schultes (1993) | Bravo et al. (1992) |

	These specifications for the rate-based Rad-Frac columns were developed from the specifications outlined in “Optimization of… Solvent Selection” (Arachchige et al., 2012), and information from the AspenTech software guide for MEA Carbon Capture modeling (AspenTech, 2014). Specifications were updated and changed to improve convergence throughout the process, and the table above reflects the final user-selected values for each column. 

### Other Block Specifications

**Table 6\.**

| Block | Description | Specifications |
| :---- | :---- | :---- |
| PUMP | Increases the pressure of the rich solvent exiting the absorber to be greater than the operating pressure of the Stripper | Pressure Increase: 1 bar |
| HEATER | Heats the rich solvent from the absorber before it enters the Stripper | Temperature: 80 C Pressure: 3 bar |
| FLASHSEP | Separates the vapor product of the Stripper column by condensing the water in the stream back to liquid and returning it to the Stripper and producing a stream of pure CO2 vapor to be captured. | Temperature: 40 C Pressure: 1.8 atm |
| PUMP2 | Increased the pressure of the exiting water reflux from the flash separator to above the operating pressure of the Stripper so that it can be recycled.  | Pressure Increase: 1 atm |
| MIXER | Mixes the LEANOUT, water, and MEA makeup streams to be recycled back into the Absorber.  | N/A |
| COOLER | Cools the recycle stream back to the temperature of the FLUEGAS stream.  | Temperature: 40 C Pressure: 1.1 bar |

### Phase 1 issues log

* ABSORBER would not converge; repeatedly showed error ‘FAILED IN SULZER PROPRIETARY CORRELATION’; changed packing type to FLEXIPAC to solve this issue  
* STRIPPER block was not converging; disabled condenser and created a separate condenser FLASHSEP block to solve issue  
* Water Reflux from FLASHSEP was changed to enter at stage 2 so the recycle loop can properly converge and zero flow issues are not seen 

---

## Phase 2 — Convergence & pilot validation

**Target weeks:** 3–4 (late June 2026\) — with 2-week buffer if needed. **Status:** ✅Complete

### Objectives

- [x] Achieve converged absorber solution  
- [x] Achieve converged stripper solution  
- [x] Full loop convergence (with recycle)  
- [x] Compare reboiler duty to Esbjerg benchmark (target: within 5%)  
- [x] Compare column temperature profiles to Knudsen et al. (2009) Figure X  
- [x] Compare lean/rich loading to published values  
- [x] Generate parity plot of simulated vs. measured outputs

### Convergence strategy notes

The ABSORBER and STRIPPER blocks converged independently before being connected and converging as a process. Then the recycle loop was added to connect the LEANOUT stream from the STRIPPER block to the inlet of the ABSORBER block. MIXER block was added to the recycle loop to account for mass balance issues by feeding in makeup streams of MEA and WATER whose mass flow was determined by two separate design specs. Once the recycle loop converged, a third design spec was added that varied the flow rate of FLUEGAS to find the optimal liquid-to-gas ratio that would achieve a 90% capture rate. 

### Validation results

**Table 1\.**

| Metric | Source | Validation Target | This simulation | % error |
| :---- | :---- | :---- | :---- | :---- |
| CO₂ capture rate |  (Knudsen et al., 2009\) | 90% | 90.14% | 0.14% |
| Reboiler duty (GJ/tonne CO₂) | (Knudsen et al., 2009\) | 3.7 | 4.147 | 12.08% |
| Lean loading (mol/mol) | (Freguia & Rochelle, 2003\) | 0.2 \- 0.27 | 0.3 | 11% |
| Rich loading (mol/mol) | (Freguia & Rochelle, 2003\) | 0.4 \- 0.45  | 0.53 | 17.77% |

### Phase 2 issues log

* Recycle loop was throwing many convergence errors; iterations were not narrowing to a solution and were varying widely. Solved by changing Wegstein acceleration parameters from \-5-0 to \-1-0 and consecutive direct substitution steps from 0 to 2\.  
* Error log kept saying the system is not in mass balance. Design Specs added to regulate two makeup streams of MEA and Water so that the entire system was kept in mass balance.   
* Converged after these two fixes but small errors remained in the MIXER block where the electrolyte chemistry was off the convergence target by a small amount  
* All blocks and streams converged once a design spec was added to regulate the efficiency of the simulation through varying the L/G ratio entering the ABSORBER block. 

---

## Phase 3 — Sensitivity analysis & optimization

**Target weeks:** 5–6 (mid July 2026\)  **Status:** ✅Complete  
Objectives

- [x] Sensitivity: reboiler duty vs. lean loading   
- [x] Sensitivity: capture rate vs. L/G ratio  
- [x] Sensitivity: stripper pressure vs. reboiler duty  
- [x] ASPEN Optimization: minimize reboiler duty s.t. capture ≥90%  
- [x] Export all sensitivity results to CSV  
- [x] Python: ingest CSVs, generate matplotlib figures

### Decision Variables for Sensitivity Analysis 

**Table 1\.**

| Name  | Variable | Lower bound | Upper bound | Notes |
| :---- | :---- | :---- | :---- | :---- |
| S-1 | Reboiler Duty (MW) | 150 | 200 | Ran with some minor errors but did not affect sensitivity results |
| S-2 | Solvent to Carbon Ratio | 0.50 | 2.73 | Ran with some minor errors but results were reasonable  |
| S-3 | Stripper pressure (atm) | 1.5 | 4 | Ran with some minor errors but the results were reasonable |

### Sensitivity results

**Figure 1\.**
*S-1: L/G Ratio vs Capture Rate\.*
![Alt text](./figures/SOLVENT%20to%20CARBON%20RATIO%20VS%20CAPTURE.png)

**Figure 2\.**  
*S-2: Reboiler Duty v.s. Lean Loading\.*
![Alt text](./figures/REBOILER%20DUTY%20vs%20LEAN%20LOADING.png)

**Figure 3\.**  
*S-3: Stripper Pressure v.s. Reboiler Duty\.* 
![Alt text](./figures/PRESSURE%20VS%20DUTY.png)

### Variables for Optimization Block

**Table 2\.** 

| Type | Variable | Specifications  |
| :---- | :---- | :---- |
| Objective | Reboiler Duty (MW) | Minimize |
| Constraint | Capture Rate | EFF \= (“FLUEGAS, CO2” \- “CLEANGAS, CO2” ) / “FLUEGAS, CO2’ EFF \>= 0.9 |
| Descion | FLUEGAS (kg/sec) | Vary from 200 \- 1000 (kg/sec) |
| Descion | Reboiler Duty (MW) | Vary from 150  \- 200 (MW) |

### Optimization Results

Minimized Reboiler Duty: 178.101 MW  
Capture Rate: 91.4 %

### Phase 3 issues log

* S-1 was able to run without errors and produced reasonable results. Ran S-1 to account for liquid-to-gas as well as carbon-to-solvent ratio.  
* S-2 ran without issues   
* To run S-3, the specification in the STRIPPER block had to be changed from a specified reboiler duty of 180 MW to a specified bottoms rate of 599.2342834 kg/sec which was taken from the results of LEANOUT.   
* In order for each sensitivity analysis to run, the simulation had to be run and converged, and then the capture design spec had to be turned off before each sensitivity analysis was run   
* No Python or CSV export Issues

---

## Phase 4 — TEA & documentation

**Target weeks:** 7–8+ (late July – early August, primarily post-exam)  **Status:** ✅Complete

### Objectives

- [x] NETL capital cost estimate for absorber, stripper, HX, reboiler  
- [x] Operating cost: steam cost (reboiler duty × steam price), solvent makeup, utilities  
- [x] Levelized cost of capture ($/tonne CO₂)  
- [ ] Draft technical report (AIChE format)  
- [ ] Finalize GitHub repo and README  
- [ ] Build Streamlit dashboard  
- [ ] Final review and portfolio packaging

### TEA assumptions

**Table 1\.**

| Parameter | Value | Source |
| :---- | :---- | :---- |
| Steam price ($/GJ) | 6.53 | ASPEN  |
| MEA price ($/kg) | 1.04 | Businessanalytiq, 2020 |
| Plant capacity (tonne CO₂/day) | 3348.14 | ASPEN |
| Plant lifetime (years) | 20 | Standard TEA assumption |
| Discount rate | 10% | NETL baseline |

### TEA results

**Table 2\.** Installed Costs

| Equipment | Value ($USD) |
| :---- | :---- |
| STRIPPER | 3,925,000 |
| HEATER | 335,800 |
| ABSORBER | 6,508,000 |
| COOLER | 1,906,200 |
| MIXER | 0 |
| PUMP2 | 46,700 |
| PUMP | 368,400 |
| FLASHSEP | 431,300 |

**Table 3**. Operating Costs 

| Parameter  | Value ($USD/hr) |
| :---- | :---- |
| Steam Cost  | 7427.15 |
| Electricity  | 9.92 |
| Cooling Water  | 371.52 |
| Solvent Makeup Cost  | 689.09  |

**Table 4\.** Annual Costs   
*Assumed 8000 operating hours per year*

| Parameter | Value |
| :---- | :---- |
| Operating ($USD/year) | 67981463 |
| Capital Costs ($USD/year) | 1588219 |
| CO2 Captured (tonne/year) | 1222079 |
| Levelized Cost of Capture ($USD/tonne) | 56.93 |

### Phase 4 issues log

* Intially attempted to use literature cost correlations to estimate costs of the ASPEN models  
* ASPEN model information was not detailed enough to compute these cost estimates by hand  
* Estimates were complied with the results from the optimization block   
* Switched methods to use ASPENs in house economic analysis tool to estimate costs   
* ASPEN provided estimates for all major equipment and utilities   
* Error in estimating stripper cost as reboiler duty is outside cost estimate limits; should assume actual costs would be higher than estimates. 

---

## Design decisions

*Every non-default design/paramter choice, with justification and source.*

| Parameter | Value used | Default | Justification | Source |
| :---- | :---- | :---- | :---- | :---- |
| Property method | ELECNRTL | — | Imported directly from Aspen Template. Required for ionic amine-CO₂-H₂O system | AspenTech, 2014 |
| Column type | Rate-based RadFrac | Equilibrium | Better pilot data agreement; industry standard | Arachchige et al., 2012 |
| Heat Exchanger  | Removed from Design; instead, a heater was added to RICHOUT and a cooler was added to the recycle loop | Cools LEANOUT stream using heat exchange with RICHOUT stream | Did not reduce duty enough to justify convergence errors the heat exchanger presented |  |
| Stripper Condenser  | Removed the condenser from STRIPPER configuration instead, placed a FLASHSEP block to condense the stream exiting from the top stage of the STRIPPER.  | STRIPPER is configured with a condenser in the block. A vapor and liquid stream exit the top stage of the condenser.  | Was causing convergence issues that would not resolve; H2O and CO2  were not separating in the top stage.  |  |

---

## References 

Arachchige, U. S. P. R., Mohsin, M., Melaaen, M. C., & Tel-Tek, P. (2012). Optimization of post-combustion carbon capture process-solvent selection. *International Journal of Energy and Environment (Print)*, *3*. https://www.osti.gov/etdeweb/biblio/22106515   
Aspen Technology, Inc. (2014). *Rate-based model of the CO2 capture process by MEA using Aspen Plus* \[Software documentation\]. Aspen Technology, Inc. https://www.aspentech.com  
Freguia, S., & Rochelle, G. T. (2003). Modeling of CO2 capture by aqueous monoethanolamine. *AIChE Journal*, *49*(7), 1676–1686. https://doi.org/10.1002/aic.690490708   
Knudsen, J. N., Jensen, J. N., Vilhelmsen, P.-J., & Biede, O. (2009). Experience with CO2 capture from coal flue gas in pilot-scale: Testing of different amine solvents. *Energy Procedia*, *1*(1), 783–790. https://doi.org/10.1016/j.egypro.2009.01.104   
(2020, August 25). Monoethanolamine price index. *Businessanalytiq*. https://businessanalytiq.com/procurementanalytics/index/monoethanolamine-price-index


