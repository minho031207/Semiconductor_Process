# Automotive NMOS Optimization (TCAD)

### Overview
- Objective: Minimize high-temperature (423K) degradation in NMOS for automotive applications.
- Period: 2026.04 - 2026.06
- Key Focus: Process parameter optimization (Halo, LDD, P-well).

### Performance Summary
| Parameter | Baseline (423K) | Optimized | Improvement |
| :--- | :--- | :--- | :--- |
| **SS** | 1015.2 mV/dec | 198.6 mV/dec | 80.4% ↓ |
| **Ioff** | 2.81E-05 A/µm | 1.58E-12 A/µm | Suppressed |
| **Ion/Ioff** | 23 | 1.39E+08 | Stability Restored |

### Optimization Strategy
1. Halo Doping: Prevent punch-through and depletion layer expansion.
2. LDD Engineering: Mitigate peak electric field near S/D regions.
3. P-well Refinement: Suppress off-state leakage currents.

### Documentation
- [Final Report (PDF)](Link_to_file)

### Key Visuals
- High-temperature degradation trends (SS & Ioff vs. Temperature)
 ![Degradation Trends](온도상승에따른성능변화.png)
- Optimization design map (Ion/Ioff vs. SS)
 ![Degradation Trends](베이스라인1.png)
 ![Degradation Trends](베이스라인2.png)
- Process sequence and final doping profile comparison
 ![Degradation Trends](최종1.png)
 ![Degradation Trends](최종2.png)
 ![Degradation Trends](단면.png)
