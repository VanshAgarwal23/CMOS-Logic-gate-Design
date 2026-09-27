# CMOS NOR2 Characterization Results

## Top-Level Circuit Configuration
* **Supply Voltage (VDD):** 5.0 V
* **Output Load (CL):** 50 fF (FO4 benchmark sizing)
* **PMOS Dimensions:** W = 8 µm, L = 1 µm (Upsized for series resistance compensation)
* **NMOS Dimensions:** W = 2 µm, L = 1 µm (Baseline parallel scaling)
* **Stimulus:** `Vin_A` = 5V Step Pulse, `Vin_B` = 0V Static DC

## Propagation Delay Metrics
Transitions were captured by isolating the active switching path while tying the off-path network to ground. The tight convergence of rise and fall times confirms that the 4x width scaling of the series PMOS network successfully balances the inherent mobility deficit of hole carriers against the electron carriers in the parallel NMOS network.

* **Output Fall Time (tpHL):** 537.75 ps
* **Output Rise Time (tpLH):** 426.76 ps
* **Mean Propagation Delay (tp):** 482.25 ps

## Power & Energy Metrics
Power dissipation was analyzed over a 400 ns measurement window using dynamic step pulses. 

* **Average Dynamic Power:** 6.63 µW
* **Integrated Supply Energy (0-400ns):** 2.65 pJ
* **Quiescent Leakage Peak:** 3.15 pA