# CMOS Inverter — Simulation Results Report

## 1. Overview

This report documents the final LTspice 26.0.2 characterization of the CMOS inverter in the `01-Inverter` stage of the CMOS Logic Gate Design project. The final characterization uses explicit Level-1 NMOS/PMOS models, a 5 V supply, a 50 fF output load, controlled switching activity within a 100 ns simulation window, and LTspice `.meas` extraction for propagation delay, average power, and integrated supply energy.

## 2. Final Circuit Configuration

| Parameter | Final value |
|---|---:|
| Supply voltage | 5 V |
| PMOS M1 | W = 4 µm, L = 1 µm |
| NMOS M2 | W = 2 µm, L = 1 µm |
| Output load | 50 fF |
| Input waveform | `PULSE(0 5 10ns 1ns 1ns 40ns 1us)` |
| Simulation interval | 0–100 ns |
| Maximum timestep | 10 ps |
| Input node | `IN_NODE` |
| Output node | `OUT_NODE` |

The PMOS source and bulk are connected to VDD, the NMOS source and bulk are connected to ground, and both gates are driven by `IN_NODE`.

## 3. Device Models

```spice
.model nmos_model NMOS (LEVEL=1 VTO=0.7 KP=20u LAMBDA=0.04 TOX=20n)
.model pmos_model PMOS (LEVEL=1 VTO=-0.7 KP=10u LAMBDA=0.05 TOX=20n)
```

The final characterization does not depend on the earlier BSS84/BSS170 discrete-device models.

## 4. Simulation Method

The input pulse has a 1 µs period while the transient analysis covers only 100 ns. Consequently, the characterization window contains the first rising transition near 10 ns and the first falling transition near 51 ns without a second periodic cycle entering the measurement window.

```spice
.tran 0 100n 0 10p
```

## 5. Logic Verification

The simulated inverter exhibits the expected complementary behavior:

- Input LOW drives the output HIGH.
- Input HIGH drives the output LOW.
- The output transitions in the opposite direction to the input.
- The output reaches approximately the 0 V and 5 V rails under the 50 fF load.

## 6. Propagation Delay

LTspice extracted:

- `tpHL = 5.57339588367e-10 s = 0.557339588367 ns`
- `tpLH = 5.38142708830e-10 s = 0.538142708830 ns`

Measurement intervals:

- `tpHL`: 10.500000 ns → 11.057340 ns
- `tpLH`: 51.500000 ns → 52.038143 ns

The mean propagation delay is:

```text
tp = (tpHL + tpLH) / 2
tp = 0.547741148598 ns
```

**Average propagation delay = 0.547741 ns**

Both constituent delays were successfully extracted, so the mean uses verified data only.

## 7. Power and Energy

Average supply power over 0–100 ns:

```text
Pavg = 1.37587907619e-05 W
     = 13.7587907619 µW
```

Integrated supply energy over 0–100 ns:

```text
Esw = 1.37587907619e-12 J
    = 1.37587907619 pJ
```

The consistency check is:

```text
Esw / 100 ns = 13.7587907619 µW
```

**Measurement note:** `Esw` is total supply energy delivered during the complete 0–100 ns window. It is not the energy of one isolated switching transition.

## 8. Waveform Analysis

The final waveform contains input voltage, output voltage, supply voltage, and supply-current activity. The input changes from 0 V to 5 V near 10 ns, causing the output to transition from HIGH to LOW. The input returns from 5 V to 0 V near 51 ns, causing the output to transition from LOW to HIGH.

Switching-current activity is concentrated around the input/output transitions while the supply voltage remains at 5 V.

## 9. LTspice Warnings

The successful run reported Level-1 model/device warnings concerning:

- oxide thickness being thinner than recommended;
- M1/M2 channel length being shorter than recommended;
- M1/M2 channel width being narrower than recommended.

These are model/device warnings and did not prevent operating-point convergence, transient simulation, or measurement extraction.

## 10. Final Results

| Metric | Verified result |
|---|---:|
| `tpHL` | **0.557340 ns** |
| `tpLH` | **0.538143 ns** |
| Average propagation delay | **0.547741 ns** |
| Average power | **13.758791 µW** |
| Energy, 0–100 ns | **1.375879 pJ** |
| VDD | **5 V** |
| CL | **50 fF** |

## 11. Evaluation-Point Verification

1. **Standardized power benchmarking:** The inverter uses 5 V, a 50 fF load, a 100 ns characterization window, and a 10 ps maximum timestep. Direct power comparison across gates still requires identical switching activity.
2. **Isolated transitions:** The 1 µs input period places only the first rising and falling transitions inside the 100 ns measurement window.
3. **Device sizing:** PMOS W/L = 4/1 µm and NMOS W/L = 2/1 µm.
4. **Realistic load:** Output load is 50 fF rather than the earlier 1 pF load.
5. **Verified mean delay:** Both `tpHL` and `tpLH` were successfully measured and averaged directly.

## 12. Conclusion

The final CMOS inverter simulation completed successfully and produced verified propagation-delay, average-power, and integrated-energy measurements. The corrected topology, explicit MOS models, 50 fF load, controlled input stimulus, and verified measurement directives provide the final characterization basis for the inverter.

### Final verified values

- **tpHL = 0.557340 ns**
- **tpLH = 0.538143 ns**
- **Average propagation delay = 0.547741 ns**
- **Average power = 13.758791 µW**
- **Energy over 0–100 ns = 1.375879 pJ**
