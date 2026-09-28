# CMOS 2-Input NOR Gate — New Final Technical Characterization

## 1. Circuit Configuration

| Parameter | New Final Value |
|---|---:|
| Supply voltage, VDD | 5 V |
| Output load, CL | 50 fF |
| PMOS M1/M2 | W = 8 µm, L = 1 µm |
| NMOS M3/M4 | W = 2 µm, L = 1 µm |
| Input A | PULSE(0 5 10 ns 1 ns 1 ns 40 ns 1 µs) |
| Input B | 0 V DC |
| Transient window | 0–100 ns |
| Maximum timestep | 10 ps |
| Simulator | LTspice 26.0.2 |

## 2. CMOS Architecture

The 2-input NOR gate uses two PMOS transistors in series in the pull-up network and two NMOS transistors in parallel in the pull-down network.

The circuit implements:

**Y = ¬(A + B)**

The series PMOS network conducts only when both A and B are low. The parallel NMOS network conducts whenever either A or B is high.

| A | B | Y |
|---:|---:|---:|
| 0 | 0 | 1 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 0 |

## 3. Latest Input Stimulus

The new NOR uses:

`Vin_A = PULSE(0 5 10ns 1ns 1ns 40ns 1us)`

Input B is fixed at 0 V.

This means the first rising transition of A occurs at 10 ns and the first falling transition occurs at approximately 51 ns, which matches the propagation-delay measurement intervals in the supplied LTspice log.

## 4. Transient Simulation

The final simulation uses:

- Start time: 0 ns
- Stop time: 100 ns
- Maximum timestep: 10 ps
- Supply: 5 V
- Load: 50 fF

LTspice successfully converged to the operating point and completed the transient simulation.

## 5. Device Models

The simulation uses Level-1 MOS models:

- NMOS: VTO = 0.7 V, KP = 20 µA/V², LAMBDA = 0.04
- PMOS: VTO = −0.7 V, KP = 10 µA/V², LAMBDA = 0.05

LTspice reported that the W/L dimensions of M1–M4 are shorter/narrower than recommended for the selected Level-1 model. These warnings did not prevent successful simulation.

## 6. Propagation Delay

The propagation-delay measurements use the 50% VDD threshold:

**VTH = VDD/2 = 2.5 V**

### 6.1 High-to-Low Delay

**tpHL = 0.535308 ns = 535.308 ps**

Measured from:

**10.500000 ns → 11.035308 ns**

### 6.2 Low-to-High Delay

**tpLH = 0.424656 ns = 424.656 ps**

Measured from:

**51.500000 ns → 51.924656 ns**

### 6.3 Average Propagation Delay

**tp(avg) = (tpHL + tpLH)/2**

**tp(avg) = 0.479982 ns = 479.982 ps**

The ratio:

**tpHL/tpLH ≈ 1.261**

Therefore, the falling transition takes approximately 26.1% longer than the rising transition under this exact simulation configuration.

## 7. Power Characterization

The latest LTspice log reports:

`avg_power = -1.32487182873e-05 W FROM 0 TO 1e-07`

The negative sign results from the current reference direction of the voltage source. Using the magnitude as consumed supply power:

**Pavg = 13.248718 µW**

The power measurement uses exactly the same 0–100 ns interval as the transient simulation.

## 8. Energy Characterization

For the actual 100 ns measurement window:

**E = Pavg × T**

**E = 13.248718 µW × 100 ns**

**E = 1.324872 pJ**

This is the integrated supply energy over the complete 0–100 ns simulation window. It should not be described as the energy of one isolated switching transition.

## 9. Peak Current

The latest LTspice run reports:

`MAX(I(Vdd)) = -2.63874620939e-12 A`

Therefore:

**|Ipeak| = 2.638746 pA**

The peak-current measurement is also evaluated over 0–100 ns.

## 10. Technical Interpretation

The NOR2 topology has a series PMOS pull-up path. Two PMOS devices must conduct simultaneously to charge the output, increasing effective pull-up resistance. The PMOS width of 8 µm is therefore larger than the 2 µm NMOS width and compensates for the series structure and lower PMOS mobility.

The NMOS devices are parallel. When input A becomes high, M3 can discharge the output directly while M4 remains off because B is fixed at 0 V.

This produces the measured asymmetric delays:

- tpHL = 535.308 ps
- tpLH = 424.656 ps

## 11. Final Verified Results

| Metric | New NOR2 Result |
|---|---:|
| VDD | 5 V |
| CL | 50 fF |
| PMOS W/L | 8 µm / 1 µm |
| NMOS W/L | 2 µm / 1 µm |
| Transient window | 100 ns |
| Maximum timestep | 10 ps |
| tpHL | 0.535308 ns |
| tpLH | 0.424656 ns |
| Average delay | 0.479982 ns |
| Average power | 13.248718 µW |
| Integrated energy, 0–100 ns | 1.324872 pJ |
| Peak-current magnitude | 2.638746 pA |

## 12. Figure Placement

**Figure 1 — New NOR2 CMOS Schematic:** place immediately after the architecture subsection.

**Figure 2 — New NOR2 Transient Waveform:** place immediately after the transient-simulation subsection and before the propagation-delay calculations.

The waveform should be connected directly to the 2.5 V crossing measurements used to obtain tpHL and tpLH.

## 13. Important Comparison Note

The new NOR2 timing simulation is now internally consistent: the transient simulation, average-power measurement, and peak-current measurement all use the 0–100 ns interval.

For comparison with the inverter and NAND2, the common timing framework is:

- VDD = 5 V
- CL = 50 fF
- transient window = 100 ns
- maximum timestep = 10 ps
- propagation-delay threshold = 2.5 V

However, the input pulse configuration and inactive-input conditions should still be reported explicitly because switching activity is not necessarily identical across the three logic gates.

## 14. Conclusion

The new LTspice characterization successfully demonstrates the 2-input CMOS NOR gate using the final 5 V, 50 fF configuration. The measured high-to-low delay is 0.535308 ns, the low-to-high delay is 0.424656 ns, and the average propagation delay is 0.479982 ns.

The latest run reports an average supply-power magnitude of 13.248718 µW over the 0–100 ns simulation interval. The corresponding integrated supply energy is 1.324872 pJ. The peak-current magnitude is 2.638746 pA.

These results are specific to the selected Level-1 MOS models, transistor dimensions, load capacitance, input stimulus, and simulation settings. LTspice also reports geometry-related warnings for the selected Level-1 devices, which should be retained in the formal project documentation.
