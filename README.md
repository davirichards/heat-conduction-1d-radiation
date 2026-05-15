# heat-conduction-1d-radiation

**Finite difference solution of 1D heat conduction with nonlinear radiative boundary condition: explicit FTCS scheme**

## What it does

Solves the one-dimensional heat conduction equation:

$$\frac{\partial u}{\partial t} = \alpha \frac{\partial^2 u}{\partial x^2}$$

on domain [0, 1] with:

- **Left boundary:** Dirichlet condition u(0,t) = 0
- **Right boundary:** Nonlinear radiative boundary condition (Stefan-Boltzmann law)
- **Time integration:** Explicit FTCS (Forward Time Centered Space)
- **Spatial discretization:** N = 100 uniform grid points
- **Temporal duration:** 100 million time units

The right boundary enforces radiative heat loss:

$$u_N = u_{N-1} - 2h_x\beta(u_N^4 - T_{\infty}^4)$$

Where β is the radiation parameter and T∞ is the ambient temperature.

## Why this problem matters

Heat conduction with radiation is a classical **nonlinear PDE boundary value problem** arising in:

- **Materials science:** Cooling of heated rods/plates in furnaces
- **Thermal engineering:** Radiators and heat exchangers at high temperatures
- **Aerospace:** Spacecraft thermal control and re-entry heating
- **Manufacturing:** Annealing processes with radiative cooling

The Stefan-Boltzmann law introduces nonlinearity at the boundary, making this problem qualitatively different from pure conduction. Demonstrates coupling between **parabolic PDE** (interior diffusion) and **nonlinear algebraic constraint** (radiation boundary).

## Method

| Parameter | Value | Description |
|-----------|-------|-------------|
| **Domain** | [0, 1] | Rod length |
| **Grid points** | N = 100 | Spatial discretization |
| **Thermal diffusivity** | α = 10⁻⁶ | Heat conduction coefficient |
| **Radiation parameter** | β = 5.6×10⁻⁵ | Stefan-Boltzmann scaled |
| **Ambient temperature** | T∞ = 50 K | Far-field condition |
| **Time steps** | ~2.5×10¹⁰ | Long-time evolution |
| **Stability** | ht = hx²/(8α) | CFL satisfied explicitly |

**Discrete scheme (interior):**
$$u_i^{n+1} = u_i^n + \frac{\alpha \Delta t}{h_x^2}(u_{i+1}^n - 2u_i^n + u_{i-1}^n)$$

**Right boundary (nonlinear radiative):**
$$u_N^{n+1} = u_{N-1}^{n+1} - 2h_x \beta(u_N^{n+1\,4} - T_\infty^4)$$

## Key outputs

Spatial temperature profile u(x) after long-time integration, showing:
- Temperature gradient from left boundary (cold) to right boundary
- Effect of radiative cooling on boundary temperature
- Approach toward quasi-steady state or transient dynamics
- Nonlinear profile shape determined by radiation-conduction balance

## Tech stack

- **Python** — Core implementation
- **NumPy** — Array operations and numerical computation
- **Matplotlib** — Visualization of temperature profiles

## Installation & usage

```bash
pip install numpy matplotlib
python heat_radiation_1d.py
```

Executes `solve()` with default parameters and plots final temperature distribution along rod.

## Computational performance

| Grid Size | Time Steps | Approx Runtime |
|-----------|-----------|-----------------|
| 50 | 10⁹ | 10-30 sec |
| 100 | 10¹⁰ | 100-300 sec |
| 200 | 10¹⁰ | 500+ sec |

Long time integration (Tf = 10⁸) required to resolve slow diffusion time scales dictated by α = 10⁻⁶.

## Connection to the field

This benchmark bridges nonlinear thermal boundary conditions with classical parabolic PDEs. Commonly studied in:
- Numerical PDE courses (implicit vs. explicit time stepping)
- Thermal engineering design
- Inverse problems (parameter identification from temperature profiles)
- High-temperature materials processing

The explicit scheme illustrates computational trade-offs: simplicity and clarity vs. restrictive CFL conditions on time step.
