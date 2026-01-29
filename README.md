# Health PMPM Modeling Using a Two-Part Framework

## Business Context
This project replicates a health actuarial pricing workflow for estimating
per-member-per-month (PMPM) costs in the presence of highly sparse healthcare
utilization.

In individual-level healthcare data, a large share of members incur zero
medical cost over a year, while positive costs are highly skewed among a
smaller subset.  
This project applies a two-part modeling framework to separately model
utilization behavior and conditional cost severity, and then aggregates
predictions to the PMPM level.

The overall structure mirrors workflows commonly used in health insurance
pricing, Medicare Advantage analytics, and population risk modeling.

---

## Data (High-Level Overview)
- Source type: public, de-identified survey-style healthcare data  
- Observation unit: individual-year  
- Key variable groups:
  - Demographics: age, sex, age bands
  - Utilization indicator (any use vs. no use)
  - Annual healthcare cost
  - Derived PMPM cost
- Optional survey weights are supported and evaluated diagnostically

Raw data files, detailed variable mappings, and preprocessing logic are
intentionally excluded from this repository.

---

## Modeling Framework

A two-part model is used to address zero-inflated healthcare cost data:

### Part 1 — Utilization
- Models the probability of any healthcare use
- Binary outcome with logistic regression
- Predictors based on demographic characteristics

### Part 2 — Positive Cost Severity
- Models annual cost conditional on positive utilization
- Gamma regression with log link
- Fitted only on observations with positive cost

### PMPM Aggregation
- Expected annual cost = P(use) × E(cost | use)
- PMPM estimate = Expected annual cost / 12

This separation allows utilization behavior and cost intensity to be modeled
independently, improving interpretability and stability in a pricing context.

---

## Model Development and Comparison
Multiple model specifications are evaluated to assess robustness and stability,
including:
- Baseline demographic models
- Interaction effects
- Nonlinear age effects using spline-based specifications

Models are compared using PMPM-level metrics (MAE, RMSE), utilization calibration,
and subgroup stability by age band and sex.  
Model selection emphasizes interpretability and pricing usability rather than
pure predictive optimization.

---

## Diagnostics and Validation
The workflow includes structured diagnostics aligned with actuarial practice:
- Distribution checks for costs, utilization, and predictions
- Aggregate and subgroup comparisons of actual vs. predicted PMPM
- Calibration analysis for predicted utilization probabilities
- Sensitivity checks for weighted vs. unweighted estimates

Diagnostics are designed to support model reasonableness and downstream pricing
use, rather than academic model selection.

---

## Outputs
Selected summary tables are included to illustrate model behavior and PMPM-level
results:
- Aggregate PMPM actual vs. predicted summaries
- PMPM comparisons by age band and sex
- Model comparison summaries across specifications
- Utilization calibration by prediction deciles

Only high-level outputs used for interpretation are included.

---

## Future Work
This project adopts a two-part modeling framework primarily due to data
availability constraints and interpretability considerations in a pricing
context. Potential extensions include:

- Constructing visit-based utilization measures using event-level data to
  explore hurdle-style or count-based specifications
- Evaluating zero-inflated or mixture models if a defensible structural-zero
  subgroup can be identified (e.g., eligibility or access constraints)
- Incorporating richer clinical covariates to assess whether additional model
  complexity is supported by the data
- Comparing more flexible models against simpler specifications under
  business-oriented criteria such as stability and transparency

These extensions are intentionally deferred to preserve a clear, pricing-focused
baseline framework.

---

## Notes on Reproducibility
This repository focuses on modeling logic, workflow structure, and actuarial
reasoning.

Implementation details, full preprocessing steps, and model code are
intentionally omitted to prevent direct replication.
