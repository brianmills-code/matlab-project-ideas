# Advanced Project 1: Constrained Dynamic Model Calibration

## Goal

Build a reusable MATLAB workflow that estimates unknown physical parameters of a first-order dynamic system from measured data, evaluates uncertainty through repeated resampling, and compares the calibrated model against observations.

## Problem definition

Use the cooling/heating model

```text
T(t) = T_ambient + (T_initial - T_ambient) * exp(-k*t)
```

where `k` is an unknown positive rate constant. The project should accept a CSV file named `temperature_response.csv` with columns `TimeMin` and `MeasuredC`. Keep the data-import layer separate from the estimation layer so another experiment can be substituted without changing the optimizer.

## Requirements

Clean and validate the imported data, sort by time, remove or explicitly report missing observations, and reject duplicate or non-finite time values. Estimate `k` by minimizing the sum of squared residuals between measured and modeled temperatures. Report the fitted parameter, residual sum of squares, root-mean-square error, and coefficient of determination.

Plot measured values, the fitted curve, and residuals in a two-panel figure saved as `model_calibration.png`. Include a residual-vs-time diagnostic and explain whether the residuals suggest systematic model error.

Use bounded optimization so `k` remains positive. If Optimization Toolbox is unavailable, implement a one-dimensional grid search followed by a local refinement around the best grid value.

## Suggested implementation

```matlab
T = readtable('temperature_response.csv');
T = sortrows(T, 'TimeMin');
assert(all(isfinite(T.TimeMin)) && all(isfinite(T.MeasuredC)), ...
    'Input data must be finite.');
assert(all(diff(T.TimeMin) > 0), 'Time values must be strictly increasing.');

ambient = 22;
initial = T.MeasuredC(1);
model = @(k, time) ambient + (initial - ambient).*exp(-k.*time);
objective = @(k) sum((T.MeasuredC - model(k, T.TimeMin)).^2);

% Optimization Toolbox route:
kFit = fminbnd(objective, 0, 2);
fitC = model(kFit, T.TimeMin);
residual = T.MeasuredC - fitC;
rmse = sqrt(mean(residual.^2));
```

For a more rigorous implementation, estimate `ambient` and `initial` together with `k`, scale parameters where needed, and compare the one-parameter and three-parameter models using cross-validation or an information criterion.

## Validation checklist

Test the estimator with a known synthetic dataset before using real observations. Confirm that the fitted curve has the same number of samples as the input, `kFit > 0`, and the RMSE is lower than the baseline model that predicts the mean measurement. Inspect residuals for trends, changing variance, and outliers.

## Extensions

Bootstrap the observations to create confidence intervals for `k`. Add a second exponential term, compare models with and without ambient-temperature estimation, or implement analytical gradients and benchmark convergence against finite-difference optimization.
