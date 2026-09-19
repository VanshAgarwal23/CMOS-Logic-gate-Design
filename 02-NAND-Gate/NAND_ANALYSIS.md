# CMOS NAND2 Gate - Analysis Results
## Date: 2026-09-19
## Status: Pending Simulation

---

## Circuit Specifications

### Logic Function
- **NAND Gate**: Output = NOT(A AND B)
- **Truth Table**:
  | A | B | OUT |
  |---|---|-----|
  | 0 | 0 | 1   |
  | 0 | 1 | 1   |
  | 1 | 0 | 1   |
  | 1 | 1 | 0   |

### Transistor Sizing
- **PMOS (M1, M2 - Parallel)**: W/L = 8µm / 1µm = 8:1 each
- **NMOS (M3, M4 - Series)**: W/L = 2µm / 1µm = 2:1 each
- **Total PMOS width**: 16µm (to match series NMOS stack)

### Operating Conditions
- **Supply Voltage (VDD)**: 5V
- **Temperature**: 25°C
- **Load Capacitance**: 1pF
- **Input A Frequency**: 5 MHz
- **Input B Frequency**: 10 MHz
- **Technology Node**: 1µm CMOS

---

## Simulation Results

### Timing Analysis

| Parameter | Value | Unit | Notes |
|-----------|-------|------|-------|
| tpHL (High→Low) | — | ns | When inputs go HIGH |
| tpLH (Low→High) | — | ns | When inputs go LOW |
| Average Delay (tp) | — | ns | Mean delay |
| Rise Time (trise) | — | ns | Output rise time |
| Fall Time (tfall) | — | ns | Output fall time |

### Power Analysis

| Parameter | Value | Unit | Notes |
|-----------|-------|------|-------|
| Average Power (Pavg) | — | µW | Dynamic + Static power |
| Peak Current (Ipeak) | — | mA | Maximum supply current |
| Energy per transition | — | fJ | Power × delay |

### Logic Verification

- [ ] Output = 1 when A=0 or B=0
- [ ] Output = 0 only when A=1 and B=1
- [ ] Proper voltage levels (0V and 5V)
- [ ] No spurious transitions observed

---

## Performance Observations

### Timing Characteristics
- [ ] Delays measured for all input transitions
- [ ] Rise/fall times recorded
- [ ] Worst-case delay identified

### Power Characteristics
- [ ] Power consumption reasonable?
- [ ] Peak current within expected range?
- [ ] Static vs. dynamic power analyzed?

### Comparison to Inverter
- [ ] NAND delay vs. Inverter delay
- [ ] NAND power vs. Inverter power
- [ ] Why is NAND delay higher/lower?

---

## Design Notes

### Why PMOS is larger (8:1 vs 2:1 NMOS)?
- PMOS has lower mobility than NMOS (approximately 2-3x)
- Series NMOS stack reduces pull-down current
- Larger PMOS ensures balanced rise/fall times

### Series NMOS vs Parallel PMOS
- **NMOS (Pull-down)**: Series = slower pull-down, but simpler
- **PMOS (Pull-up)**: Parallel = faster pull-up to compensate

---

## Next Steps

1. [ ] Run simulation: `cmos_nand2.cir`
2. [ ] Extract and record measurements
3. [ ] Document observations
4. [ ] Commit to Git
5. [ ] Move to NOR gate analysis

---

*Document prepared for CMOS Logic Gate Design & Analysis Project*
*Author: Vansh Agrawal (VanshAgrawal23)*
