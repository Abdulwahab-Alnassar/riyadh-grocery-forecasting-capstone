# Capstone rubric mapping

The course marks the weights below as a **draft**, not an approved final grade.
This is an evidence checklist, not a self-awarded score.

| Published section | Draft weight | Concrete evidence in the single notebook |
|---|---:|---|
| Time Series Structure & Diagnostics | 15 | Section 1: raw-data audit, robust weekly STL, interpreted weekday/month profiles, ACF/PACF, ADF and conditional diagnostic differencing |
| Classical Forecasting Models | 15 | Section 2: damped-trend multiplicative-weekly Holt-Winters, stated rationale, fitted coefficients, Ljung-Box table, residual ACF and prose |
| ML/GBM Forecasting & Feature Engineering | 15 | Section 3: LightGBM, 28 lag/rolling/calendar features, strictly shifted training summaries, recursive 28-day forecasts, two executable leakage checks |
| Backtesting Framework & Time-Based Validation | 20 | Section 4: five expanding-window folds, printed boundaries and train sizes, shared origins for all three models, independently refitted models |
| Evaluation Metrics & Reporting | 10 | Sections 4–5: MAE, RMSE, WAPE, training-scaled seasonal MASE; fold and pooled tables; actual held-out baseline comparison |
| Probabilistic Forecasting & Prediction Intervals | 15 | Section 6: six later calibration origins, corrected quantile ranks, horizon-band radii, nominal 90% bounds, final-test coverage and width |
| Model Comparison & Documentation | 10 | Section 7: written evidence-based recommendation addressing history length, interpretability, interval support, compute, scale, and limitations |

The notebook also issues a 28-day forecast beyond the dataset, exports its artifacts,
and includes captured outputs from actual execution. It does not claim live forecasts
or a production service.

## GitHub and personal submission requirements

| Requirement | Status of this deliverable |
|---|---|
| Trainee name in README and notebook | Included: Abdulwahab Mohammed Ali Alnassar |
| Programme name | Included: Time Series Forecasting for AI Systems, SDAIA Academy |
| Actual cohort dates | Not provided by the trainee; must be filled before submission |
| Professional README and run instructions | Included |
| Technical documentation beyond notebook markdown | Included in `docs/TECHNICAL_DOCUMENTATION.md` |
| Dataset provenance and version | Included in `data/PROVENANCE.md`; checksum checked during execution |
| `.gitignore` for secrets and generated files | Included |
| Meaningful development history | Actual local build commits supplied in a Git bundle; subsequent trainee commits should remain meaningful |
| Active trainee GitHub account | Requires the trainee's own account; not asserted |
| Notebook published to a documented repository | Files are prepared; actual GitHub publication is not asserted |
| Colab badge | Opens the Colab picker now; replace with the repository-specific link after publication |
| SDAIA Academy GitHub link | Included |

Source: [Capstone requirements](https://mohammadyusif.github.io/time-series-forecasting-ai-systems/capstone.html).
