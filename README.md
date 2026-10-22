# Two-Area Load Frequency Control (LFC) Using Automatic Generation Control (AGC)

A MATLAB/Simulink simulation of a two-area interconnected power system implementing Automatic Generation Control (AGC) to study frequency regulation, tie-line power exchange, and Area Control Error (ACE) following a load disturbance.

## Overview

Load Frequency Control (LFC) is an essential function of power system operation that maintains system frequency and scheduled power exchange between interconnected control areas. A sudden change in load creates a mismatch between generation and demand, resulting in frequency deviations and changes in tie-line power flow.

This project models a two-area interconnected power system in MATLAB/Simulink and implements primary frequency control through governor droop and secondary control through integral AGC controllers driven by Area Control Error.

The simulation investigates the dynamic response of both areas following a load disturbance and evaluates the system's ability to restore frequency and regulate tie-line power exchange.

## Objectives

- Model the dynamic behaviour of two interconnected power system areas.
- Implement governor, turbine, generator inertia, and load damping dynamics.
- Study primary frequency control through governor droop.
- Implement secondary frequency regulation using integral AGC controllers.
- Analyse frequency deviation, tie-line power deviation, and Area Control Error.
- Evaluate the transient and settling behaviour of the interconnected system following a load disturbance.

## System Model

Each control area consists of the following components:

| Component               | Function                                                                               |
| ----------------------- | -------------------------------------------------------------------------------------- |
| Governor                | Models the governor response to frequency deviations.                                  |
| Turbine                 | Represents the delay between governor action and mechanical power output.              |
| Generator and Load      | Models the frequency response of the rotating mass and load damping.                   |
| Droop Control           | Provides primary frequency response through speed regulation.                          |
| ACE Calculation         | Combines frequency deviation and tie-line power deviation.                             |
| Integral AGC Controller | Provides secondary control to reduce steady-state frequency and tie-line power errors. |
| Load Disturbance        | Introduces a step change in load to evaluate the system response.                      |
| Tie-Line Model          | Represents the coupling and power exchange between the two areas.                      |

### Control Strategy

**1. Primary frequency control**

Governor droop provides an immediate response to a frequency deviation. The governor adjusts its reference input according to the frequency error, helping arrest the initial frequency change.

**2. Secondary frequency control (AGC)**

The integral controller acts on the Area Control Error (ACE). Its purpose is to eliminate persistent frequency deviations and restore scheduled tie-line power exchange.

**3. Inter-area coupling**

The two areas are connected through a tie-line. A difference in the areas' frequency deviations produces a change in tie-line power flow, coupling their dynamic responses.

## Model Parameters

The following parameters are used in the simulation.

| Parameter                             | Area 1 | Area 2 |
| ------------------------------------- | -----: | -----: |
| Governor time constant, (T_g)         |  0.2 s |  0.3 s |
| Turbine time constant, (T_t)          |  0.5 s |  0.6 s |
| Inertia constant/model parameter, (M) |     10 |      8 |
| Load damping coefficient, (D)         |    0.6 |    0.9 |
| Speed regulation, (1/R)               |     20 |     16 |
| Frequency bias, (B)                   |   20.6 |   16.9 |
| Integral controller gain, (K)         |    0.3 |    0.3 |

### Common Parameters

| Parameter                                 |    Value |
| ----------------------------------------- | -------: |
| Base frequency                            |    60 Hz |
| System base power                         | 1000 MVA |
| Tie-line synchronizing coefficient, (P_s) |        2 |
| Simulation duration                       |     50 s |

The parameters can be modified in the corresponding Simulink Gain and Transfer Function blocks to investigate the effect of different system characteristics and controller settings.

*Note: The inertia, load damping, droop, frequency bias, and synchronizing coefficient are reported using the model's parameter values. Their precise units and normalization depend on the implementation of the corresponding blocks.*

## Control Equations

### Area Control Error (ACE)

The ACE combines the frequency deviation of a control area with its tie-line power deviation:

[
ACE_1=\Delta P\_{tie,1}+B_1\Delta f_1
]

[
ACE_2=\Delta P\_{tie,2}+B_2\Delta f_2
]

where:

- (\Delta P\_{tie,i}) is the net tie-line power deviation for area (i), with a consistent import/export sign convention.
- (B_i) is the frequency bias of area (i).
- (\Delta f_i) is the frequency deviation of area (i).

For a two-area system, the tie-line deviations are equal and opposite when expressed using the same reference direction.

### Integral AGC Controller

The secondary controller integrates ACE to generate a control adjustment:

[
\Delta P\_{AGC,i}(s)=\frac{K_i}{s}ACE_i(s)
]

The actual sign of the control action depends on the summing-junction convention in the Simulink model. The controller acts to reduce ACE and restore the system toward its scheduled operating condition.

### Tie-Line Power Dynamics

For small deviations around an operating point, the incremental tie-line power exchange can be represented by:

[
\Delta P\_{12}(s)=\frac{P_s}{s}
\left[\Delta f_1(s)-\Delta f_2(s)\right]
]

Here, (P_s) is the tie-line synchronizing coefficient. The sign depends on the direction chosen for positive tie-line power flow.

## Simulation Results

The model was simulated for 50 seconds, and the following responses were observed in per unit (pu).

### 1. Tie-Line Power Deviation



The tie-line power deviation initially moves away from its nominal value following the disturbance. It subsequently recovers toward zero over the simulation interval.

This response illustrates the interaction between the two control areas and the effect of secondary control on regulating power exchange.

### 2. Area Control Error (ACE)



Both ACE signals exhibit transient oscillations following the disturbance. The oscillations progressively decay, and both signals approach zero during the simulation.

This indicates that the integral controllers are acting to reduce the area control errors and restore the system toward its scheduled operating condition.

### 3. Frequency Deviation in Area 1



The frequency deviation in Area 1 exhibits an initial transient followed by damped oscillations. The magnitude of these oscillations decreases over time, and the response approaches zero.

This demonstrates the combined effect of primary governor response and secondary AGC action in regulating frequency following a disturbance.

### 4. Frequency Deviation in Area 2



Area 2 also exhibits a transient frequency response, with oscillations that progressively diminish during the simulation.

Although the disturbance is applied to one area, the tie-line coupling causes the interconnected area to respond as well. The observed recovery illustrates the interaction between the two areas and their frequency-control mechanisms.

### Results Summary

| Output                       | Observed response                    |
| ---------------------------- | ------------------------------------ |
| Frequency deviation — Area 1 | Damped oscillations approaching zero |
| Frequency deviation — Area 2 | Damped oscillations approaching zero |
| Tie-line power deviation     | Recovers toward zero                 |
| ACE — Area 1                 | Oscillations decay toward zero       |
| ACE — Area 2                 | Oscillations decay toward zero       |

The simulated responses are consistent with a system approaching its scheduled operating condition following a load disturbance. Quantitative performance measures such as settling time, peak deviation, and overshoot can be evaluated separately from the simulation data.

## Technologies Used

- **MATLAB** — modelling and simulation environment.
- **Simulink** — block-based power system modelling.
- **Transfer Function Blocks** — representation of governor, turbine, and generator-load dynamics.
- **Gain and Sum Blocks** — implementation of droop control, frequency bias, and ACE calculations.
- **Scope Blocks** — visualisation of system responses.

## Requirements

- MATLAB R2021a or later recommended.
- Simulink.
- No additional toolboxes required for the basic model, subject to the blocks used in the implementation.

## How to Run

1. Clone or download the repository:

   ```bash
   git clone https://github.com/<your-username>/two-area-agc.git
   ```

2. Open MATLAB and navigate to the project directory.

3. Open the Simulink model:

   ```matlab
   open('two_area_agc.slx')
   ```

4. Run the simulation using the **Run** button or `Ctrl+T`.

5. Observe the frequency deviation, tie-line power deviation, and ACE signals using the Scope blocks.

6. Modify the load disturbance and controller parameters to investigate their effects on system dynamics.

## Repository Structure

```text
two-area-agc/
├── two_area_agc.slx
├── README.md
└── results/
    ├── tie_line_power_deviation.png
    ├── area_control_error.png
    ├── frequency_deviation_area1.png
    └── frequency_deviation_area2.png
```

The `results` directory contains the plots embedded in this README. The filenames and directory structure should match the image links used above.

## Possible Extensions

- Add an Automatic Voltage Regulator (AVR) to investigate combined LFC and voltage-control dynamics.
- Compare frequency responses with AGC enabled and disabled.
- Investigate the effect of different integral controller gains on system stability and transient response.
- Introduce generation rate constraints and governor dead-band nonlinearities.
- Extend the model to three or more interconnected control areas.
- Compare conventional integral AGC with PI, PID, optimal, or robust control strategies.

## Conclusion

This project demonstrates the modelling and simulation of a two-area interconnected power system with primary frequency control and secondary AGC. The simulated frequency deviations, tie-line power deviation, and ACE responses exhibit damped transients and approach zero over the 50-second observation interval.

The project provides a foundation for studying interconnected power system dynamics, frequency regulation, tie-line power control, and the influence of controller parameters on system performance.

---

**Project:** Two-Area Load Frequency Control Using AGC
**Environment:** MATLAB/Simulink
**Simulation duration:** 50 seconds
**Signal units:** Per unit (pu)
