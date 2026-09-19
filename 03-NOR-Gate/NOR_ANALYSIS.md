# CMOS NOR2 Gate - Analysis Results
## Date: 2026-09-19
## Status: Pending Simulation

---

## Circuit Specifications

### Logic Function
- **NOR Gate**: Output = NOT(A OR B)
- **Truth Table**:
  | A | B | OUT |
  |---|---|-----|
  | 0 | 0 | 1   |
  | 0 | 1 | 0   |
  | 1 | 0 | 0   |
  | 1 | 1 | 0   |

### Transistor Sizing
- **PMOS (M1, M2 - Series)**: W/L = 8µm / 1µm = 8:1 each
- **NMOS (M3, M4 - Parallel)**: W/L = 2µm / 1µm = 2:1 each
- **Total NMOS width**: 4µm (parallel devices)

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

- [ ] Output = 1 only when A=0 and B=0
- [ ] Output = 0 when A=1 or B=1
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

### Comparison to NAND Gate
- [ ] NOR delay vs. NAND delay (typically NOR is slower)
- [ ] NOR power vs. NAND power
- [ ] Why is NOR delay different from NAND?

---

## Design Notes

### Why NMOS is larger in parallel than PMOS in series?
- **Series PMOS stack**: Reduces pull-up current, needs larger transistors
- **Parallel NMOS**: Can be smaller because they discharge in parallel
- **Asymmetry**: NOR gates are typically slower than NAND gates

### Series PMOS vs Parallel NMOS
- **PMOS (Pull-up)**: Series = slower, need large W/L to compensate
- **NMOS (Pull-down)**: Parallel = faster pull-down

---

## Comparison: NAND vs NOR Delays

NOR gates are typically **slower** than NAND gates because:
1. Series PMOS stack has lower current delivery
2. PMOS has lower mobility than NMOS
3. Requires larger transistors to maintain speed

---

## Next Steps

1. [ ] Run simulation: `cmos_nor2.cir`
2. [ ] Extract and record measurements
3. [ ] Document observations
4. [ ] Compare with NAND results
5. [ ] Commit to Git
6. [ ] Proceed to comparative analysis

---

*Document prepared for CMOS Logic Gate Design & Analysis Project*
*Author: Vansh Agrawal (VanshAgrawal23)*
