# Engineering Project 1: One-Dimensional Heat Diffusion

## Goal

Model transient heat conduction through a one-dimensional rod using an explicit finite-difference method, investigate stability, and compare the numerical solution with a known analytical benchmark.

## Model

For a homogeneous rod, the heat equation is

```text
∂T/∂t = α ∂²T/∂x²
```

where `α` is thermal diffusivity. Use a rod of length `L = 0.10` m, `α = 1.0e-5` m²/s, fixed boundary temperatures of 100 °C and 20 °C, and an initially uniform interior temperature of 20 °C.

## Requirements

Discretize the rod into at least 41 spatial nodes. Implement the explicit update

```matlab
T(i,n+1) = T(i,n) + r*(T(i+1,n) - 2*T(i,n) + T(i-1,n));
```

with `r = alpha*dt/dx^2`. Enforce the boundary conditions at every time step. Use a stable time step with `r <= 0.5`, and deliberately run one unstable case to observe the failure mode. Plot temperature profiles at multiple times and a space-time image using `imagesc`. Save the result as `heat_diffusion.png`.

## Suggested implementation

```matlab
L = 0.10; alpha = 1e-5; nx = 41;
dx = L/(nx-1); dt = 0.4*dx^2/alpha;
nt = 500; x = linspace(0, L, nx)';
T = 20*ones(nx, 1); T(1) = 100;
history = zeros(nx, nt+1); history(:,1) = T;

for n = 1:nt
    Tnew = T;
    Tnew(2:end-1) = T(2:end-1) + alpha*dt/dx^2 * ...
        (T(3:end) - 2*T(2:end-1) + T(1:end-2));
    Tnew([1 end]) = [100; 20];
    T = Tnew;
    history(:,n+1) = T;
end
```

## Validation checklist

Check that the boundary temperatures remain fixed and that the temperature stays within the physical boundary range for the stable run. Report `dx`, `dt`, and `r`. Refine the spatial grid and compare the temperature at the rod midpoint to assess numerical convergence.

## Extensions

Add convection at one boundary, use temperature-dependent diffusivity, compare with an analytical series solution, or implement an implicit method and compare stability and runtime.
