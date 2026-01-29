# Health PMPM Modeling with Two-Part Framework

## Business context
This project replicates a common health actuarial pricing workflow for estimating
per-member-per-month (PMPM) costs in the presence of highly sparse utilization.

In healthcare data, a large proportion of members incur zero cost within a year,
while a smaller subset generates positive expenditures.  
This project applies a two-part modeling framework to separately model utilization
and conditional cost severity, and then aggregates predictions to the PMPM level.

The workflow mirrors pricing-style analysis used in health insurance,
Medicare Advantage, and population risk modeling.

---

## Data (high level)
- Source type: public survey-style healthcare data (de-identified)
- Observation unit: individual-year
- Key variables:
  - Demographics: age, sex, age band
  - Utilization indicator (any use vs. no use)
  - Annual healthcare cost
  - Derived PMPM cost
- Optional survey weights are supported and evaluated diagnostically

Raw data, variable mappings, and preprocessing logic are intentionally omitted.

---

## Modeling framework
A two-part model is used to handle zero-inflated healthcare costs:

**Part 1 — Utilization**
- Models the probability of any healthcare use
- Binary outcome (use vs. no use)
- Logistic regression with demographic predictors

**Part 2 — Positive cost severity**
- Models annual cost conditional on having positive utilization
- Gamma regression with log link
- Fitted only on members with positive cost

**Combination**
- Expected annual cost = P(use) × E(cost | use)
- PMPM estimate = Expected annual cost / 12

This structure separates utilization behavior from cost intensity, which is
standard in health actuarial modeling.

---

## Model development and comparison
Multiple model specifications are evaluated to assess stability and fit, including:
- Baseline demographic models
- Interaction terms
- Nonlinear age effects using spline-based specifications

Models are compared using:
- PMPM-level MAE and RMSE
- R²-style diagnostics for utilization probability
- Group-level comparisons by age band and sex
- Calibration checks for predicted utilization probabilities

The focus is on relative model behavior and interpretability rather than
maximizing predictive performance.

---

## Diagnostics and validation
The workflow includes structured diagnostics to ensure model reasonableness:
- Distribution checks for costs, utilization, and predictions
- Actual vs. predicted PMPM at aggregate and subgroup levels
- Utilization calibration by prediction deciles
- Sensitivity analysis for weighted vs. unweighted estimates

These diagnostics are designed to replicate how an analyst would validate a
pricing or forecasting model before downstream use.

---

## Outputs
Example outputs include:
- Aggregate PMPM actual vs. predicted summaries
- PMPM comparisons by age band and sex
- Utilization calibration tables
- Model comparison metrics across specifications

Only summarized outputs and visualizations are included in this repository.

---

## Notes on reproducibility
This repository focuses on modeling logic, workflow structure, and actuarial
reasoning.

Implementation details, full data preprocessing steps, and model tuning code
are intentionally omitted to prevent direct replication.
