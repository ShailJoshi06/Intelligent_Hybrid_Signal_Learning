# Intelligent Hybrid Signal Learning using Convex Combination of NLMS and TFLANN Filters

## Overview

This project implements a real-time, phone-to-phone audio system identification setup that adaptively models a channel which may be linear or non-linear, without knowing in advance which one it is.

Two adaptive filtering approaches are combined, namely NLMS (Normalized Least Mean Square) and TFLANN (Trigonometric Functional Link Artificial Neural Network). 

A convex combination of the two filters' outputs is learned adaptively via a sigmoid-based mixing parameter λ(n), which shifts weight toward whichever filter (NLMS or TFLANN) is currently modeling the system better, without any manual switching.

The system was:
1. First designed and simulated in MATLAB Simulink.
2. Then deployed for real-time operation across two Android phones communicating over Wi-Fi (UDP), using the Simulink Support Package for Android and a companion Android Studio app.

## Repository Contents

| File | Description |
|---|---|
| Phone_A.slx | Simulink model for Phone A: The Transmitter |
| Phone_B.slx | Simulink model for Phone B: The Reciever |
| 23110277_23110296_EE609_A3.pdf | Explanation, Implementation, and Results |


## Requirements

- MATLAB with Simulink
- Simulink Support Package for Android Devices (for deployment to phones)
- Two Android phones on the same Wi-Fi network (UDP communication on port 25000)
- An accompanying Android Studio app generated from MATLAB Simulink itself (used for the on-phone GUI: slider control, scopes for λ(n), e(n), y1(n) vs y2(n), d(n) vs y(n), etc.)

## Running the Simulation (Desktop)

1. Open Phone_A.slx and Phone_B.slx in Simulink.
2. Run both models simultaneously (they communicate over UDP, so they can be run as two Simulink sessions on the same or networked machines).
3. Toggle the linear/non-linear switch in Phone_B.slx manually during the run to observe how λ(n) adapts.
4. View the scopes for:
   - d(n) vs y(n): overall model tracking
   - y1(n) vs y2(n): NLMS vs TFLANN outputs
   - e1(n) vs e2(n): individual model errors
   - e(n): overall combined error
   - λ(n): combination parameter behavior

## Deploying to Phones

1. Install the Simulink Support Package for Android Devices.
2. Deploy Phone_A.slx to one phone and Phone_B.slx to the other (or use the companion Android Studio app for the GUI/scopes on Phone B).
3. Ensure both phones are connected to the same Wi-Fi network.
4. On Phone B's app, use the slider to switch the desired signal between linear (i(n)) and non-linear (i_nl(n)).

## Key Results

- When the channel is non-linear, λ(n) → 0: the combination relies on TFLANN, whose error (e2) stays low while NLMS's error (e1) is high.
- When the channel is linear, λ(n) → 1: NLMS converges quickly and nearly perfectly (e1 ≈ 0), while TFLANN still has finite error but tracks reasonably well.
- The overall combined error e(n) stays low in both regimes, confirming that the convex combination successfully and automatically identifies which model is best suited to the current channel condition, both in simulation and in real-time on-phone testing.

See the full report (23110277_23110296_EE609_A3.pdf) for detailed plots and discussion.

See the [video](https://youtu.be/3n_FwyIYHvE?si=o1sjBDCUbZPgc1v9) for the live implementation demo. 
