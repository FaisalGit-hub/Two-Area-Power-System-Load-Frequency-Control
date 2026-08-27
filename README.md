# Two-Area Load Frequency Control (AGC) — Simulink Model

A MATLAB/Simulink model of a **two-area interconnected power system** with **Automatic Generation Control (AGC)**. The model simulates governor–turbine–generator dynamics, tie-line power exchange between the two areas, and secondary (integral) frequency control driven by the **Area Control Error (ACE)**.

## Overview

In an interconnected power system, load changes in one area cause frequency deviations and unscheduled power flow on the tie-line connecting the areas. This model demonstrates how **primary control** (governor droop) and **secondary control** (AGC / integral control on ACE) work together to:

- Restore system frequency to its nominal value in each area
- Restore the tie-line power flow to its scheduled value
- Share the load change appropriately between the two generating areas

Each area is modeled with:

- **Governor** — first-order lag representing speed-governor dynamics
- **Turbine** — first-order lag representing turbine response
- **Inertia & Load** — first-order model of the rotating mass and load damping
- **Droop control (1/R)** — primary frequency response feedback
- **ACE calculation** — combines frequency deviation (weighted by frequency bias `B`) and tie-line power deviation
- **Integral controller (K)** — secondary control acting on the ACE to drive steady-state error to zero
- **Load disturbance input** — step change in load used to test the control response

The two areas are coupled through a **tie-line synchronizing power coefficient (Ps)**, which converts the frequency difference between areas into a tie-line power exchange.

## Model Parameters

| Area | Governor time constant | Turbine time constant | Inertia (M) | Load damping (D) | Droop (1/R) | Frequency bias (B) | Integral gain (K) |
|------|------------------------|------------------------|-------------|-------------------|-------------|----------------------|--------------------|
| Area 1 | 0.2 s | 0.5 s | 10 | 0.6 | 20 | 20.6 | 0.3 |
| Area 2 | 0.3 s | 0.6 s | 8 | 0.9 | 16 | 16.9 | 0.3 |

**Common parameters:**
- Base frequency: 60 Hz
- Base MVA: 1000 MVA
- Tie-line synchronizing coefficient (Ps): 2

> Values can be changed directly in the corresponding Gain / Transfer Fcn blocks in the model to study different operating conditions or area sizes.

## Signals / Scopes

The model outputs and plots the following signals for each area:

- `DF in Per Unit` / `DF in Hz` — frequency deviation
- `Pm in Per Unit` / `DPm in MW` — governor/turbine mechanical power output
- `DP12 in Per Unit` / `DP12 in MW` — tie-line power deviation
- `F in Hz` — absolute area frequency
- `ACE1`, `ACE2` — Area Control Error for each area


## Requirements

- MATLAB (R2021a or later recommended)
- Simulink
- No additional toolboxes required

## How to Run

1. Clone this repository:
```bash
   git clone https://github.com/<your-username>/two-area-agc.git
```
2. Open MATLAB and navigate to the cloned folder.
3. Open the model:
```matlab
   open('two_area_agc.slx')
```
4. Run the simulation (Ctrl+T or the **Run** button in Simulink).
5. Apply a step load disturbance using the **Load Disturbance** blocks and observe the frequency deviation, tie-line power, and ACE responses on the scopes.

## Key Concepts Demonstrated

- Primary frequency control via governor droop
- Secondary frequency control via Automatic Generation Control (AGC)
- Area Control Error (ACE) formulation: `ACE = ΔP_tie + B·Δf`
- Tie-line power flow modeling between interconnected areas
- Steady-state frequency and tie-line power restoration after load disturbances

## Possible Extensions

- Add a third interconnected area
- Introduce generation rate constraints (GRC) and governor dead-band nonlinearities
- Replace integral control with PI/PID or fuzzy/optimal (LQR) AGC controllers
- Compare classical AGC with modern control strategies (e.g., robust control, reinforcement learning)
