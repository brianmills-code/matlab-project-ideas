# Engineering Project 2: RC Circuit Response and Design

## Goal

Simulate the step response of a first-order RC circuit, estimate its time constant from sampled measurements, and select component values that satisfy a response-time requirement.

## Model

For a series resistor-capacitor circuit driven by a step voltage `Vin`, the capacitor voltage is

```text
Vc(t) = Vin * (1 - exp(-t/(R*C)))
```

The time constant is `tau = R*C`. Use `Vin = 5` V, `R = 10e3` ohms, and `C = 100e-6` F for the baseline design.

## Requirements

Generate the analytical response over at least five time constants. Estimate the time at which the capacitor reaches 63.2% of its final value and compare it with `R*C`. Compute the current `i(t) = (Vin/R)*exp(-t/(R*C))` and plot voltage and current in separate panels.

Create a sampled measurement table containing time and voltage, add a documented measurement-noise model only for testing the estimator, and estimate `tau` by minimizing the voltage residual. Convert the estimated time constant into an estimated capacitance using the known resistance. Save the figure as `rc_response.png`.

## Suggested implementation

```matlab
Vin = 5; R = 10e3; C = 100e-6;
tau = R*C;
time = linspace(0, 5*tau, 500)';
voltage = Vin*(1 - exp(-time/tau));
current = (Vin/R)*exp(-time/tau);
threshold = 0.632*Vin;
idx = find(voltage >= threshold, 1, 'first');
estimatedTauFromCurve = time(idx);

objective = @(candidateTau) sum((voltage - ...
    Vin*(1-exp(-time/candidateTau))).^2);
tauFit = fminbnd(objective, tau/10, 10*tau);
capacitanceFit = tauFit/R;
```

If `fminbnd` is unavailable, evaluate the objective over a logarithmically spaced candidate grid and select the lowest-error candidate. Validate units at every step.

## Validation checklist

The capacitor voltage should approach `Vin` asymptotically, current should decay toward zero, and the 63.2% crossing should occur near one time constant. Verify that the fitted time constant is close to the known value and that the estimated capacitance has the correct units and order of magnitude.

## Extensions

Design `R` and `C` for a specified rise-time range, compare tolerances with a Monte Carlo analysis, include a discharge phase, or model a two-stage RC filter and study its frequency response.
