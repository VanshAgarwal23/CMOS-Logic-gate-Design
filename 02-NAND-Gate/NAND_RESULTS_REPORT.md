# CMOS 2-Input NAND Gate — LTspice Simulation Results Report

## 1. Overview

This report documents the design, simulation, and performance evaluation of a **CMOS 2-input NAND gate** implemented and simulated using **LTspice 26.0.2**.

The NAND gate uses complementary MOS transistor networks consisting of a **parallel PMOS pull-up network** and a **series NMOS pull-down network**. The design was evaluated through transient simulation to verify its logic operation and determine its propagation delay, average power consumption, and total energy delivered by the supply during the simulation interval.

The final measurements reported here are taken directly from the successful LTspice simulation of the current design.

---

## 2. CMOS NAND Gate Architecture

A 2-input CMOS NAND gate consists of two complementary transistor networks:

- **Pull-Up Network (PUN):** Two PMOS transistors connected in parallel.
- **Pull-Down Network (PDN):** Two NMOS transistors connected in series.
- **Output:** Connected between the pull-up and pull-down networks.
- **Load:** A 50 fF capacitive load connected from the output to ground.

The NAND gate implements:

\[
Y = \overline{A \cdot B}
\]

The output becomes LOW only when both inputs are HIGH. For all other input combinations, the output remains HIGH.

### Transistor Configuration

| Transistor | Type | Network | W | L |
|---|---|---|---:|---:|
| M1 | PMOS | Pull-up | 4 µm | 1 µm |
| M2 | PMOS | Pull-up | 4 µm | 1 µm |
| M3 | NMOS | Pull-down | 2 µm | 1 µm |
| M4 | NMOS | Pull-down | 2 µm | 1 µm |

The PMOS devices were sized wider than the NMOS devices to provide stronger pull-up drive capability and compensate for the lower carrier mobility of holes compared with electrons.

---

## 3. Circuit Configuration

### 3.1 Supply Voltage

```text
VDD = 5 V
```

### 3.2 Output Load

```text
CL = 50 fF
```

The capacitive load is used to evaluate the dynamic behavior of the CMOS NAND gate under a defined capacitive loading condition.

### 3.3 Input Configuration

Input A is driven by:

```text
PULSE(0 5 10ns 1ns 1ns 40ns 100ns)
```

Therefore:

- Initial voltage = 0 V
- Final voltage = 5 V
- Delay = 10 ns
- Rise time = 1 ns
- Fall time = 1 ns
- Pulse width = 40 ns
- Period = 100 ns

Input B is held permanently HIGH:

```text
V(in_b) = 5 V
```

Holding input B HIGH allows transitions at input A to directly produce the required NAND output transitions for propagation-delay measurement.

---

## 4. MOSFET Models

The simulation uses Level-1 MOSFET models.

### NMOS Model

```text
.model nmos_model NMOS (
+ LEVEL=1
+ VTO=0.7
+ KP=20u
+ LAMBDA=0.04
+ TOX=20n
)
```

### PMOS Model

```text
.model pmos_model PMOS (
+ LEVEL=1
+ VTO=-0.7
+ KP=10u
+ LAMBDA=0.05
+ TOX=20n
)
```

---

## 5. LTspice Simulation Setup

The transient simulation was performed using:

```text
.tran 0 100n 0 10p
```

This corresponds to:

- Simulation duration = **100 ns**
- Maximum timestep = **10 ps**

The small maximum timestep provides sufficient temporal resolution for extracting the sub-nanosecond propagation delays.

### Simulation Status

```text
Simulator: LTspice 26.0.2
Analysis: Transient
Duration: 100 ns
Maximum timestep: 10 ps
Status: SUCCESSFUL
```

---

## 6. NAND Logic Verification

The CMOS transistor arrangement implements the following NAND truth table:

| Input A | Input B | Output Y |
|:---:|:---:|:---:|
| 0 | 0 | 1 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

The output is LOW only when both PMOS pull-up paths are OFF and the complete NMOS pull-down path is conducting.

During the transient simulation, input B is fixed at logic HIGH while input A transitions between logic LOW and HIGH. Consequently:

- When `A = 0`, the output is driven HIGH.
- When `A = 1`, both NMOS devices conduct and the output is driven LOW.

---

## 7. Transient Waveform Analysis

The simulated waveform contains:

- `V(in_a)` — Input A
- `V(in_b)` — Input B
- `V(out)` — NAND output
- `I(Vdd)` — Supply current

When input A rises through the switching region, the series NMOS pull-down network becomes active and the output transitions from HIGH to LOW.

When input A falls, the NMOS pull-down path is disabled and the PMOS pull-up network restores the output to HIGH.

The supply-current waveform exhibits transient current activity during switching, corresponding to dynamic charging and discharging of circuit capacitances.

The waveform is provided in:

```text
NAND_WAVEFORM.png
```

---

## 8. Propagation Delay Analysis

Propagation delay was extracted using a 2.5 V threshold, corresponding to 50% of the 5 V supply.

### 8.1 High-to-Low Propagation Delay

```text
tpHL = 0.903516373149 ns
```

Measurement interval:

```text
Start = 10.500000 ns
End   = 11.403516 ns
```

Therefore:

\[
t_{pHL}=0.903516\text{ ns}
\]

### 8.2 Low-to-High Propagation Delay

```text
tpLH = 0.540486687822 ns
```

Measurement interval:

```text
Start = 51.500000 ns
End   = 52.040487 ns
```

Therefore:

\[
t_{pLH}=0.540487\text{ ns}
\]

### 8.3 Average Propagation Delay

\[
t_p=\frac{t_{pHL}+t_{pLH}}{2}
\]

\[
t_p=
\frac{0.903516373149+0.540486687822}{2}
\]

\[
\boxed{t_p=0.722002\text{ ns}}
\]

---

## 9. Power Analysis

Dynamic supply power was measured over the complete 100 ns transient simulation window.

The LTspice measurement used:

```text
.meas tran Pavg AVG (-V(N001)*I(Vdd)) FROM 0 TO 100n
```

### Average Power

```text
Pavg = 1.37397155119e-05 W
```

Therefore:

\[
\boxed{P_{avg}=13.739716\ \mu W}
\]

This represents the average power delivered by the supply during the specified 0–100 ns measurement interval.

---

## 10. Energy Analysis

The integrated supply energy was measured using:

```text
.meas tran Esw INTEG (-V(N001)*I(Vdd)) FROM 0 TO 100n
```

Measured value:

```text
Esw = 1.37397155119e-12 J
```

Therefore:

\[
\boxed{E_{0-100ns}=1.373972\text{ pJ}}
\]

The result is consistent with the measured average power:

\[
E=P_{avg}\times T
\]

\[
E=(13.7397155\ \mu W)(100\ ns)
\]

\[
E=1.373972\ pJ
\]

### Important Measurement Note

The reported **1.373972 pJ** is the **total energy delivered by the supply over the complete 0–100 ns measurement window**.

It should **not** be interpreted as the energy consumed by one isolated switching transition.

For comparison of switching energy between different CMOS gates, the same supply voltage, load capacitance, input frequency, switching activity, and measurement interval should be maintained.

---

## 11. Verified Simulation Results

| Parameter | Measured Value |
|---|---:|
| Supply voltage | 5 V |
| Output load | 50 fF |
| Simulation duration | 100 ns |
| Maximum timestep | 10 ps |
| `tpHL` | 0.903516 ns |
| `tpLH` | 0.540487 ns |
| **Average propagation delay** | **0.722002 ns** |
| **Average supply power** | **13.739716 µW** |
| **Total supply energy, 0–100 ns** | **1.373972 pJ** |

---

## 12. LTspice Model Warnings

LTspice reported model/device warnings during simulation concerning the selected Level-1 MOSFET models.

The warnings include:

- Oxide thickness being thinner than the recommended range.
- MOSFET channel dimensions being shorter/narrower than recommended for the selected model.
- Similar warnings for the PMOS and NMOS devices.

These warnings did **not** prevent the transient simulation from completing successfully.

The reported numerical results should therefore be understood as results of the specified **Level-1 compact-model simulation**, rather than as extracted values from a modern foundry-qualified CMOS process model.

---

## 13. Design and Simulation Assessment

The final NAND implementation satisfies the required CMOS topology:

- PMOS devices are connected in parallel in the pull-up network.
- NMOS devices are connected in series in the pull-down network.
- PMOS devices use `W = 4 µm`.
- NMOS devices use `W = 2 µm`.
- A 50 fF output load is included.
- Input A provides controlled transitions.
- Input B is maintained at 5 V for delay extraction.
- Both propagation delays are directly measured by LTspice.
- Average propagation delay is calculated only from the measured `tpHL` and `tpLH` values.
- Supply power and integrated supply energy are directly extracted from the transient simulation.

---

## 14. Project Artifacts

The following files accompany this report:

| File | Description |
|---|---|
| `cmos_nand2.asc` | Original LTspice schematic |
| `cmos_nand2_simple.cir` | Final SPICE netlist and measurement directives |
| `cmos_nand2_simple.db` | LTspice measurement database |
| `NAND_SCHEMATIC.png` | Rendered CMOS NAND schematic |
| `NAND_WAVEFORM.png` | Transient simulation waveform |
| `NAND_MEASUREMENTS.txt` | Raw and converted LTspice measurements |
| `NAND_RESULTS_REPORT.md` | This technical report |

---

## 15. Conclusion

A **2-input CMOS NAND gate** was successfully designed and simulated in LTspice 26.0.2 using complementary PMOS and NMOS transistor networks. The transient waveform confirms the expected NAND switching behavior, while the LTspice measurement directives successfully extracted both propagation-delay components.

The measured high-to-low propagation delay is **0.903516 ns**, and the low-to-high propagation delay is **0.540487 ns**, giving an average propagation delay of **0.722002 ns**.

For the specified 5 V supply, 50 fF load, and 100 ns simulation interval, the measured average supply power is **13.739716 µW**, while the integrated supply energy over the complete measurement interval is **1.373972 pJ**.

Overall, the simulation provides a complete characterization of the implemented CMOS NAND gate in terms of **logic functionality, transient response, propagation delay, power consumption, and supply energy** under the specified simulation conditions.
