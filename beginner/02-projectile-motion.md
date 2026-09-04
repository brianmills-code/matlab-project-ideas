# Beginner Project 2: Projectile Motion Simulator

## Goal

Simulate the two-dimensional motion of a projectile launched with a chosen speed and angle, then estimate its flight time, maximum height, and horizontal range.

## Learning outcomes

This project practices function handles, vectors, parametric equations, conditional logic, plotting, units, and numerical interpretation.

## Model

Assume constant gravitational acceleration and ignore air resistance. For initial speed `v0` and launch angle `theta`, the position is

```matlab
x(t) = v0*cos(theta)*t
y(t) = v0*sin(theta)*t - 0.5*g*t.^2
```

Use `g = 9.81` m/s², `v0 = 25` m/s, and `theta = 45` degrees for the first run.

## Requirements

Convert the angle from degrees to radians. Compute the theoretical flight time and range. Generate at least 300 time samples from zero to the flight time, calculate `x` and `y`, and plot the trajectory with equal axis scaling. Add labels, units, a title, and a grid. Mark the maximum-height point on the plot and print the maximum height and range.

## Suggested implementation

```matlab
g = 9.81;
v0 = 25;
theta = 45;
thetaRad = deg2rad(theta);
flightTime = 2*v0*sin(thetaRad)/g;
time = linspace(0, flightTime, 300);
x = v0*cos(thetaRad)*time;
y = v0*sin(thetaRad)*time - 0.5*g*time.^2;

[maximumHeight, index] = max(y);
range = x(end);
plot(x, y, 'LineWidth', 1.5);
hold on;
plot(x(index), maximumHeight, 'ro', 'MarkerFaceColor', 'r');
yline(0, 'k-');
axis equal;
grid on;
xlabel('Horizontal distance (m)');
ylabel('Height (m)');
title(sprintf('Projectile at %g degrees', theta));
legend('Trajectory', 'Maximum height', 'Ground', 'Location', 'best');
```

Use `assert` to ensure `theta` is between 0 and 90 degrees and `v0` is positive. Compare the simulated final horizontal distance with the theoretical range.

## Validation checklist

The trajectory should begin and end at ground level, its maximum should occur near half the flight time, and the range should be close to `v0^2*sin(2*theta)/g`. Check several angles and explain why the range is approximately symmetric around 45 degrees under this simplified model.

## Extensions

Add sliders or a loop that compares angles from 15 to 75 degrees. Add a target at a chosen distance. Introduce a simple wind term or compare trajectories with different launch speeds.
