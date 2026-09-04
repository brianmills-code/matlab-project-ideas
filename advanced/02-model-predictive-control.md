# Advanced Project 2: Constrained Model Predictive Control

## Goal

Implement a small model predictive controller (MPC) for a discrete-time system, enforce actuator limits, compare MPC with a baseline feedback controller, and evaluate tracking quality and control effort.

## System model

Use the discrete state-space model

```text
x(k+1) = A*x(k) + B*u(k)
y(k)   = C*x(k)
```

with

```matlab
A = [1 0.1; 0 0.95];
B = [0; 0.1];
C = [1 0];
D = 0;
```

The first state is the measured output. Simulate at least 100 time steps with a reference that changes from 0 to 1 and then to 0.5. Constrain the actuator to `-0.5 <= u(k) <= 0.5`.

## Requirements

At each time step, predict the next `Np` outputs over a finite horizon, choose a sequence of future control inputs, minimize tracking error plus control effort, apply only the first input, and repeat. Use a quadratic cost such as

```text
J = sum((y_pred - r_pred).^2) + lambda*sum(delta_u.^2)
```

where `lambda` penalizes aggressive control changes. Enforce input bounds in the optimization. Compare the MPC response with a proportional controller using the same actuator limit.

Report rise time, steady-state error, overshoot, total control effort, and the number of saturated control actions. Plot reference/output, control input with limit lines, and tracking error in `mpc_comparison.png`.

## Suggested implementation route

For a toolbox-free version, enumerate a modest set of candidate control moves around the previous input, simulate each candidate sequence with a helper function, evaluate the cost, and retain the best sequence. For a scalable version, derive the prediction matrices and solve the constrained quadratic program with `quadprog` from Optimization Toolbox.

A simulation skeleton is:

```matlab
N = 120;
Np = 10;
lambda = 0.05;
x = zeros(2, 1);
uPrevious = 0;
y = zeros(N, 1);
u = zeros(N, 1);
reference = [ones(40, 1); 0.5*ones(N-40, 1)];

for k = 1:N
    % Build candidate sequences or a constrained QP here.
    % Select the sequence with minimum predicted cost.
    u(k) = min(0.5, max(-0.5, selectedSequence(1)));
    x = A*x + B*u(k);
    y(k) = C*x + D*u(k);
    uPrevious = u(k);
end
```

The project must document the candidate generation or QP formulation, prediction horizon, weighting parameters, and why only the first optimized action is applied.

## Validation checklist

Confirm that every simulated input respects the actuator limits. Test at least three values of `Np` and `lambda`, and explain the trade-off between fast tracking and smooth control. Compare the MPC and proportional-controller metrics using the same initial state and reference. Check that the controller remains numerically stable for a longer simulation.

## Extensions

Add a state observer when only the output is measured, include a rate limit on `u`, use a nonzero target state, introduce disturbances, or implement integral action to remove steady-state error. A strong portfolio version includes unit tests for prediction, constraint handling, and metric calculations.
