# Technical documentation

## Objective and data contract

Predict daily `units_sold` for one `region=Riyadh`, `category=Grocery` series
over the next 28 days. The raw four-column subset is embedded as compressed CSV
bytes, attributed to the course, and protected by a SHA-256 digest. Loading checks
the schema, selected series, duplicate dates, missing days, null values, numerical
validity, and nonnegative counts. The notebook stops on bad data rather than filling
gaps with information from the future. No rows need imputation or removal in this snapshot.

## Chronology and information availability

The first 760 observations alone drive exploratory diagnostics. Five subsequent
28-day windows compare three prespecified models with expanding training histories.
Six later 28-day windows produce out-of-sample calibration errors. The last 28 days
remain unseen by model selection and interval calibration. Each fold refits from
scratch using only observations preceding its forecast origin. Outcomes of earlier
folds are legitimate training data at later origins.

Expanding windows preserve two years of history for calendar learning. A rolling
window would discard potentially useful annual context. This is a design choice
for this series, not a claim that expanding windows always win under regime change.

The notebook uses a self-contained equivalent to the course backtesting harness.
It keeps the same expanding-window boundary construction and fit-on-prefix contract,
and additionally retains dates, train sizes, fit/inference times, and per-step errors.
Assertions require disjoint train/test boundaries, complete horizons, and finite forecasts.

## Model families

**Seasonal naive:** repeat the final observed seven-day cycle for all 28 forecast days.

**Holt-Winters:** additive level/trend, damped trend, and multiplicative weekly
seasonality. All counts are positive, and multiplicative seasonality allows weekly
amplitude to vary with demand level. Damping restrains 28-day trend extrapolation.
Smoothing coefficients and initial states are fitted on each training prefix;
the seasonal period and functional form are fixed before validation. A training-only
Ljung-Box screen at lags 14 and 28 examines remaining autocorrelation. With
`model_df=0`, its p-values are exploratory rather than a fully adjusted ETS test.

**LightGBM:** one recursively applied regressor using lags, strictly shifted
rolling summaries, and deterministic calendar features. Regularization, small
trees, a fixed seed, and limited threads suit this data volume and a CPU budget.
There is no randomized split and no automatic early stopping against future folds.
Each predicted value is appended to history before constructing the next step;
future actual observations cannot enter the feature-building interface.

The annual calendar terms do not encode the synthetic generator's unobserved
holiday or promotional shock realizations. Impurity/gain feature importance is
descriptive, not a causal explanation.

## Diagnostics and transformations

A robust weekly STL decomposition reports trend, seasonality, residuals, and
variance-based seasonal strength. Weekly STL does not separately identify yearly
seasonality: yearly movement may be absorbed into its trend. Weekday/month profiles
and an annual-lag autocorrelation provide complementary context.

ADF is run on the initial training prefix, with an intercept and linear trend.
Failure to reject its unit-root null triggers first differencing for diagnostic
plots; a seven-day difference is also examined. Rejecting the null does not imply
absence of seasonality or all forms of nonstationarity. Holt-Winters and LightGBM
are fitted to levels, since they explicitly model level/trend or calendar effects;
no ARIMA integration order is claimed from these diagnostics.

## Metrics and selection

MAE and RMSE measure error in units/day. WAPE is
`100 * sum(abs(actual - prediction)) / sum(abs(actual))` and is the primary ranking
metric. Unlike MAPE, it never divides by individual zero or near-zero days. A
zero-total scoring window is reported as undefined instead of inventing a percentage.
Seasonal MASE divides each fold's MAE by mean absolute seven-day differences from
that fold's training history. MASE below one refers to this in-sample scale, not
necessarily to beating the actual held-out seasonal-naive forecast.

Pooled MAE/RMSE/WAPE are calculated from the concatenated validation predictions.
Mean MASE is explicitly a mean of fold-specific scores. Actual held-out baseline
improvement is reported separately. Selection uses validation WAPE; if scores lie
within 2% relative of the minimum, the simpler eligible model wins. The complexity
order is Seasonal Naive, Holt-Winters, LightGBM. This rule is fixed before final testing.

## Prediction intervals

Six calibration origins forecast the same 28-day horizon as deployment. Absolute
out-of-sample errors are pooled within four horizon bands (days 1–7, 8–14, 15–21,
22–28); each band has 42 observations per model. For nominal coverage `1-alpha`,
the radius is the `ceil((n+1)*(1-alpha))`-th smallest score. Ranks larger than n
raise an error instead of silently using a too-low quantile. A cumulative maximum
across bands conservatively prevents decreasing radii with increasing horizon.

This is rolling-residual conformal calibration: historical fits have expanding
training sets. Time dependence, nonstationarity, and model refitting mean the usual
i.i.d. split-conformal finite-sample guarantee does not directly apply. The notebook
therefore reports observed final-test coverage and width, not a guaranteed coverage
claim. Bounds are clipped below at zero, reflecting nonnegative demand. Coverage
is pointwise; it is not 90% simultaneous coverage of an entire 28-day path.

No calibration observation is used to score its own fitted interval. The final
test alone supplies the reported held-out coverage/width table. The 28-day test
is a small, season-specific sample and cannot validate coverage for all future regimes.
Results are not retuned to make this test reach the nominal level.

## Final forecast and artifacts

After all evaluation is frozen, refit the selected family on all 1,096 known
observations. The separate next-28-day forecast has no observed targets in this
dataset. Its interval uses the already frozen pre-test radii, so no future target is
needed and no deployment coverage is claimed. Recalibration after collecting later
outcomes is a recommended operational action, not a result implemented retroactively.

Generated artifacts include fold boundaries, diagnostics, per-fold and pooled
scores, step-level forecasts, calibration ranks/radii, final-test forecasts with
bounds, future forecasts, fitted parameter summaries, run configuration, package
versions, and figures. All analysis code remains in one notebook.

## Limitations and operational use

This is a learning project using a single synthetic series. It does not estimate
real financial benefit, service levels, or performance at multiple retailers.
Forecast errors may cluster around unobserved promotions or moving holidays.
ETS's weekly component cannot independently model yearly effects; single-series
LightGBM has limited examples of annual and shock patterns. Observed execution
times compare implementations on one CPU environment, not production service latency.

A production pilot would backtest a full seasonal year, log forecast origins,
capture actuals, monitor absolute error and interval coverage, and re-evaluate
window length after structural changes. Inventory decisions also require lead
times, lost-sales information, costs, and service-level requirements absent here.
