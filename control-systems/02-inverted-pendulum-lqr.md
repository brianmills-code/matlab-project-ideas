# Control Systems Project 2: Inverted Pendulum with State Feedback

## Goal

Design and evaluate a state-feedback controller that stabilizes an inverted pendulum around its upright position, then compare pole placement with linear-quadratic regulation.

## Model

Use a linearized cart-pendulum model in the form

```text
x_dot = A*x + B*u
 y    = C*x
```

where the state contains cart position, cart velocity, pendulum angle, and angular velocity. Choose documented cart mass, pendulum mass, length, and gravity values, then derive or cite the linearized matrices. Clearly state the valid small-angle assumption.

## Requirements

Check controllability with `rank(ctrb(A,B))`. Design one controller with `place` and another with `lqr`. Simulate both from a small initial angle and from a cart-position command. Report settling time, maximum control effort, final state error, and whether the control input stays within a chosen actuator limit.

Add an observer design for the case where only cart position and pendulum angle are measured. Compare full-state feedback using true states with output-feedback using estimated states. Save `inverted_pendulum_control.png` with state trajectories, control input, and estimation error.

## Suggested MATLAB workflow

```matlab
% Replace A, B, C with matrices derived from documented parameters.
assert(rank(ctrb(A, B)) == size(A, 1), 'System is not controllable.');
K_place = place(A, B, desiredPoles);
[K_lqr, ~, ~] = lqr(A, B, Q, R);
A_closed = A - B*K_lqr;
assert(all(real(eig(A_closed)) < 0), 'Closed loop is unstable.');
```

Use a numerical ODE solver or a discrete-time simulation loop. Saturate the actuator explicitly and report how saturation affects linear-model predictions. Do not claim global stability from a linearized model.

## Validation checklist

Test multiple small initial angles, verify eigenvalues, compare controllers using identical initial conditions, and check that the estimator converges when the plant is observable. Repeat the test with a modest parameter mismatch and explain robustness limits.

## Extensions

Add reference tracking through precompensation, integral action, nonlinear pendulum dynamics, swing-up control, or a Kalman filter. Compare the linear controller with a nonlinear simulation and identify where the small-angle approximation fails.

## References

1. [MathWorks: Control System Design and Tuning](https://www.mathworks.com/help/control/)
2. [MathWorks: State-Space Control Design and Estimation](https://www.mathworks.com/help/control/state-space-control-design.html)
