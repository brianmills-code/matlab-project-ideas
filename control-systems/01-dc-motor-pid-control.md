# Control Systems Project 1: DC-Motor Speed Control with PID

## Goal

Model a DC motor, design a PID speed controller, and evaluate reference tracking, disturbance rejection, actuator saturation, and robustness to parameter changes.

## Plant model

Use the standard armature-controlled DC-motor model with armature resistance `R`, inductance `L`, torque constant `Kt`, back-EMF constant `Ke`, rotor inertia `J`, and viscous friction `b`. The transfer function from voltage to angular speed is

```text
G(s) = Kt / ((L*s + R)*(J*s + b) + Kt*Ke)
```

Choose and document physically plausible parameters, or use a motor datasheet with a public reference. The controller must regulate speed in rad/s while respecting a voltage limit.

## Requirements

Create the plant with `tf`, design a baseline proportional controller, and then tune PI and PID controllers. Use `pidtune` when Control System Toolbox is available; otherwise implement a documented grid search over gains. Compare closed-loop step responses and compute rise time, settling time, overshoot, steady-state error, and control effort.

Add a step load disturbance or an equivalent torque disturbance. Include actuator saturation in the simulation and explain how saturation changes the response. Save a three-panel figure as `dc_motor_pid_comparison.png` showing speed, control voltage, and disturbance response.

## Suggested MATLAB workflow

```matlab
s = tf('s');
R = 2; L = 0.5; Kt = 0.1; Ke = 0.1;
J = 0.02; b = 0.01;
G = Kt/((L*s + R)*(J*s + b) + Kt*Ke);

Kp = pidtune(G, 'PI');
T = feedback(Kp*G, 1);
step(T); grid on;
info = stepinfo(T);
```

Document the tuning target and whether the design favors fast reference tracking or disturbance rejection. Compare nominal performance with at least two parameter variations, such as doubled inertia or increased friction.

## Validation checklist

Confirm closed-loop stability, zero or acceptably small steady-state error for a step reference, and bounded control voltage. Verify that all reported metrics use the same reference and simulation time. Check that controller performance degrades gracefully when motor parameters change.

## Extensions

Add anti-windup, two-degree-of-freedom PID, a state-space observer, speed-command profiles, or a hardware-in-the-loop version. A portfolio-quality report should explain why the final gains were selected rather than presenting only the best numerical result.

## References

1. [MathWorks: PID Controller Tuning](https://www.mathworks.com/help/control/pid-controller-design.html)
2. [MathWorks: PID Control](https://www.mathworks.com/discovery/pid-control.html)
