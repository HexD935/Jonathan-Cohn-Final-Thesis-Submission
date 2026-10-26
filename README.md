# Direct Wavelet-Coefficient Impedance Estimation in Unbalanced Three-Phase Four-Wire Networks

MATLAB/Simulink code for my EEE4022S final-year project at the University of Cape Town (2026).

**Author:** Jonathan Paul Cohn  
**Supervisor:** Prof. Michel Malengret

## Overview

The code estimates the load impedance (Z<sub>Load</sub>), the Thevenin source impedance (Z<sub>th</sub>) and the neutral impedance (Z<sub>N</sub>) of a simulated three-phase four-wire network, using only the measured load voltages and currents. All calculations use the coefficients of a six-level sym4 discrete wavelet transform, without reconstructing the signals in the time domain. The source frequency ramps from 48 to 52 Hz over the 10 s simulation, sampled at 8192 Hz.

The method was built up in steps, and each folder matches one step in Chapter 3 of the thesis.

## Repository structure

| Folder | Thesis section | What it does |
|---|---|---|
| `Step05_Wavelet_Decomposition` | 3.5 | Six-level sym4 decomposition with the 7-sample circular shift, energy in each band, and reconstruction check |
| `Step06_Wavelet_Load_Impedance` | 3.6 | Z<sub>Load</sub> from the A6 coefficients in 200 ms windows with a fixed 50 Hz reference |
| `Step07_Frequency_Tracking` | 3.7 | Frequency estimate for each window (48–52 Hz search in 0.1 Hz steps), used as the reference |
| `Step08_Multi_Coefficient` | 3.8 | Adaptive 10-cycle windows using A6 + D6 + D5, updated every 20 ms |
| `Step09_Thevenin_Estimation` | 3.9 | Switch detection and Z<sub>th</sub> from P = I<sub>1</sub>/I<sub>2</sub>, single phase |
| `Step10_Three_Phase_Neutral` | 3.10 | Part 1: three phases with an ideal neutral. Part 2: non-ideal neutral, with Z<sub>N</sub> estimated from the three phases |
| `FINAL` | – | Final version of the complete estimator |

Steps 1–4 (first-principles Fourier analysis in Excel) are described in Sections 3.1–3.4 of the thesis and are not included here.

Each folder contains a Simulink model (`*_model.slx`) and the MATLAB script that analyses its output (`*_main.m`). Step 10 has a model and a script for each part.

## How to run

1. Set the MATLAB current folder to the step's folder.
2. Open the model and run it (10 s). This stores the measured voltages and currents in the workspace as `out.simout`.
3. Run the matching `*_main.m` script. It prints the error summary and plots the results.

## Requirements

MATLAB and Simulink with Simscape.

## Notes

- Step 10 Part 2 uses Simulink's Backward Euler local solver (step 1/8192 s), which adds a small, predictable series resistance to every inductor. The script reports errors against both the nominal component values and the values the solver actually simulates (see Section 3.10.4).
- All results come from noise-free simulations; no measured grid data is used.
