# Baroreflex-Blood-Pressure-Model

# Baroreflex-Mediated Blood Pressure Regulation: ODE Simulation

Code for the Biomathematics 374 research project (Stellenbosch University, 2026).
A three-variable ODE model of mean arterial pressure (P), sympathetic activity (S) and
baroreceptor firing (B). It is used to study how reduced baroreceptor sensitivity (k) and a
simple pharmacological term (d) change the equilibrium and stability of blood pressure regulation.

**Run it in your browser (no installation):**
[Open in Google Colab] (https://colab.research.google.com/github/mellsachimone-lang/baroreflex-bp-model/blob/main/Lisa Chimone (25352016) BioMaths code.ipynb),
then choose Runtime → Run all.

## The model

```
dP/dt = -λ(P − P_set) + α·S − d·(P − P_set)
dS/dt = β(S_set − B) − γ(S − S_rest)
dB/dt = k(P − P_set) − δ(B − B_rest)
```

P is in mmHg; S and B are dimensionless activities relative to rest (not bounded in this linear model); time is in minutes.

| Parameter | Value | Units | Meaning |
|---|---|---|---|
| λ | 0.08 | min⁻¹ | Pressure relaxation rate |
| α | 0.6 | mmHg min⁻¹ | Effect of sympathetic activity on pressure |
| β | 1.0 | min⁻¹ | Baroreceptor inhibition of sympathetic activity |
| γ | 0.8 | min⁻¹ | Sympathetic decay rate |
| δ | 0.5 | min⁻¹ | Baroreceptor decay rate |
| P_set | 100 | mmHg | Pressure set-point |
| S_set = S_rest = B_rest | 0.5 | dimensionless | Reference levels |
| k | 0.08 (healthy), 0.02 (reduced) | mmHg⁻¹ min⁻¹ | Baroreceptor sensitivity |
| d | 0, 0.02, 0.05 | min⁻¹ | Drug term |

Initial conditions: P(0) = 80, S(0) = 0.6, B(0) = 0.5. Parameter values are illustrative and are not fitted to patient data.

## Scenarios

| Scenario | k | d |
|---|---|---|
| Healthy | 0.08 | 0 |
| Hypertension (reduced sensitivity) | 0.02 | 0 |
| Hypertension + low dose | 0.02 | 0.02 |
| Hypertension + standard dose | 0.02 | 0.05 |

## How to run locally (Windows)

1. Install Python 3.9 or newer from https://www.python.org/downloads/ (tick "Add Python to PATH").
2. Download this repository (green **Code** button → **Download ZIP**) and unzip it.
3. Open a terminal in that folder and run:
```
   python -m venv .venv
   .venv\Scripts\activate
   pip install -r requirements.txt
   jupyter notebook
```
4. Open `cardiovascular_sim.ipynb` and choose **Kernel → Restart & Run All**.

All parameters are in the first code cell. To add a scenario, add one line to the `SCENARIOS` list.

## What the notebook produces

| File | Contents |
|---|---|
| plot_P_timeseries.png | Blood pressure over time, all scenarios |
| plot_S_timeseries.png | Sympathetic activity over time |
| plot_B_timeseries.png | Baroreceptor firing rate over time |
| plot_SB_panels.png | S(t) and B(t) side by side |
| plot_stability_table.png | Equilibria, eigenvalues, stability |
| plot_phase_portrait.png | P vs S phase portrait |
| plot_sensitivity_k.png | Equilibrium pressure vs k |
| plot_extended_sensitivity.png | Effect of peripheral gain α and drug dose d |

It also prints the stability analysis, four verification checks, and every number quoted in the report.

## Expected key results

| Scenario | P* (mmHg) | S* | B* | Stable? |
|---|---|---|---|---|
| Healthy | 101.50 | 0.2000 | 0.7400 | Yes |
| Hypertension | 102.73 | 0.3636 | 0.6091 | Yes |
| Hypertension + low dose | 102.31 | 0.3846 | 0.5923 | Yes |
| Hypertension + standard dose | 101.88 | 0.4063 | 0.5750 | Yes |

## Verification
The notebook checks that (1) numerical and closed-form equilibrium pressures agree, (2) simulations end at their equilibria, (3) tighter solver tolerances give the same curves, and (4) stability is lost at the analytically predicted sensitivity (k ≈ 1.1).

## Authorship and acknowledgements
- Initial code skeleton: Regalo Dube, who implemented an earlier formulation of the model.
- Author (Lisa Chimone): replaced the model equations, Jacobian and parameters; chose the scenarios; edited and ran the analysis; verified results against theory.
- Generative AI (Claude, Anthropic) was used to suggest the equilibrium-solver fix, and to debug the verification, extended-sensitivity and report-numbers cells.

## References
- Ursino, M. (1998). *Am J Physiol Heart Circ Physiol*, 275(5), H1733-H1747.
- Olufsen, M. S. et al. (2006). *Am J Physiol Regul Integr Comp Physiol*, 291(5), R1355-R1368.
