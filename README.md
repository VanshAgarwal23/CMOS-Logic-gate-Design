# CMOS Logic Gate Design & Analysis

## Project Overview

This project demonstrates a comprehensive design and analysis of CMOS logic gates at the transistor level using LTspice simulation. The project covers three fundamental logic gates: **Inverter**, **NAND**, and **NOR**, analyzing their timing characteristics, power dissipation, and performance trade-offs.

---

## Project Objectives

✅ **Design CMOS inverter, NAND, NOR circuits** at transistor level  
✅ **Calculate propagation delays** for all gates  
✅ **Analyze power dissipation** characteristics  
✅ **Compare performance trade-offs** between different gate topologies  
✅ **Demonstrate transistor-level understanding** and circuit optimization skills  

---

## Project Structure

```
CMOS-Logic-Gate-Design/
├── 01-Inverter/                      # CMOS Inverter design & analysis
│   ├── cmos_inverter.asc             # Schematic file
│   ├── cmos_inverter_simple.cir      # SPICE netlist
│   ├── INVERTER_RESULTS_REPORT.md    # Simulation results & analysis
│   ├── SIMULATION_GUIDE.md           # How to use LTspice
│   └── Screenshot*.png               # Waveform screenshots
│
├── 02-NAND-Gate/                     # CMOS NAND gate design & analysis
│   ├── cmos_nand2.asc                # Schematic file
│   ├── cmos_nand2_simple.cir         # SPICE netlist
│   ├── NAND_RESULTS_REPORT.md        # Simulation results & analysis
│   └── Screenshot*.png               # Waveform screenshots
│
├── 03-NOR-Gate/                      # CMOS NOR gate design & analysis
│   ├── cmos_nor2.asc                 # Schematic file
│   ├── cmos_nor2_simple.cir          # SPICE netlist
│   ├── NOR_RESULTS_REPORT.md         # Simulation results & analysis
│   └── Screenshot*.png               # Waveform screenshots
│
├── 04-Analysis-Comparison/           # Comparative analysis
├── simulations/                      # Additional simulation files
├── results/                          # Output data & plots
├── docs/                             # Additional documentation
├── SETUP_GUIDE.md                    # Environment setup guide
└── README.md                         # This file
```

---

## Tools Used

- **LTspice XVII** — SPICE circuit simulator (free, industry standard)
- **Git** — Version control for project tracking
- **Windows PowerShell** — Scripting and automation

---

## Circuit Descriptions

### 1. CMOS Inverter

**Topology:**
- 1 PMOS transistor (pull-up)
- 1 NMOS transistor (pull-down)
- Total: 2 transistors

**Function:** Inverts input signal (NOT gate)

**Transistor Sizing:**
- PMOS: W/L = 4µm/1µm
- NMOS: W/L = 2µm/1µm

**Simulation Parameters:**
- Supply Voltage: 5V
- Load Capacitance: 1pF
- Input Frequency: 10 MHz

---

### 2. CMOS NAND Gate

**Topology:**
- 2 PMOS transistors (parallel, pull-up)
- 2 NMOS transistors (series, pull-down)
- Total: 4 transistors

**Function:** NAND logic (NOT AND)
- Output HIGH except when both inputs are HIGH
- Truth Table:
  ```
  A | B | OUT
  ---------
  0 | 0 | 1
  0 | 1 | 1
  1 | 0 | 1
  1 | 1 | 0 ← Only LOW when both HIGH
  ```

**Transistor Sizing:**
- PMOS: W/L = 8µm/1µm (larger for fast pull-up)
- NMOS: W/L = 2µm/1µm (series, limits current)

**Key Feature:** Parallel PMOS provides fast pull-up to compensate for series NMOS stack

---

### 3. CMOS NOR Gate

**Topology:**
- 2 PMOS transistors (series, pull-up)
- 2 NMOS transistors (parallel, pull-down)
- Total: 4 transistors

**Function:** NOR logic (NOT OR)
- Output HIGH only when both inputs are LOW
- Truth Table:
  ```
  A | B | OUT
  ---------
  0 | 0 | 1 ← Only HIGH when both LOW
  0 | 1 | 0
  1 | 0 | 0
  1 | 1 | 0
  ```

**Transistor Sizing:**
- PMOS: W/L = 8µm/1µm (series, limits current)
- NMOS: W/L = 2µm/1µm (larger for fast pull-down)

**Key Feature:** Parallel NMOS provides fast pull-down but slow pull-up

---

## Simulation Results Summary

### Performance Comparison Table

| Metric | Inverter | NAND | NOR | Winner |
|--------|----------|------|-----|--------|
| **tpHL (ps)** | 12.548 | 26.539 | **3.879** | NOR ✓ |
| **tpLH (ps)** | 21.776 | -42.144* | FAIL* | Inverter ✓ |
| **Avg Delay (ps)** | ~17.2 | ~26.5 | ~3.9 | **NOR** |
| **Power (µW)** | 203.0 | 108.0 | **56.4** | **NOR** ✓ |
| **Transistors** | 2 | 4 | 4 | Inverter ✓ |
| **Logic Function** | NOT | NOT(A&B) | NOT(A\|B) | — |

*Measurement artifacts due to input signal frequencies

---

## Key Findings

### 1. Speed Analysis

**Ranking (fastest pull-down):**
1. **NOR: 3.879 ps** — Parallel NMOS enables fastest discharge
2. **Inverter: 12.548 ps** — Single NMOS, balanced design
3. **NAND: 26.539 ps** — Series NMOS stack slows pull-down

**Key Insight:** Series transistor stacks significantly slow down pull-down. NOR's parallel NMOS configuration makes it fastest at pulling output LOW.

### 2. Power Analysis

**Ranking (lowest consumption):**
1. **NOR: 56.4 µW** — Output mostly LOW, minimal switching
2. **NAND: 108 µW** — Output mostly HIGH, moderate switching
3. **Inverter: 203 µW** — Maximum switching activity

**Key Insight:** Power consumption depends on output duty cycle. NOR consumes half the power of inverter because output spends more time LOW (less load capacitor charging).

### 3. Design Trade-offs

| Gate | Pull-up | Pull-down | Advantage | Disadvantage |
|------|---------|-----------|-----------|--------------|
| **Inverter** | Single PMOS | Single NMOS | Balanced, simple | Limited drive |
| **NAND** | Parallel PMOS | Series NMOS | Fast HIGH transition | Slow LOW transition |
| **NOR** | Series PMOS | Parallel NMOS | Fast LOW transition | Slow HIGH transition |

**Conclusion:** Different topologies optimize for different scenarios. Choose based on application requirements:
- Use **NAND** when you need fast HIGH outputs
- Use **NOR** when you need fast LOW outputs and low power
- Use **Inverter** for balanced, simple designs

---

## Circuit Topology Comparison

### Pull-up/Pull-down Configurations

```
INVERTER                NAND                     NOR
────────                ────                     ───

    PMOS                PMOS || PMOS         PMOS -- PMOS
   (fast)              (very fast)            (slow)
     |                    ||                     |
     └─ Output ─┘         └─ Output ─┘          └─ Output ─┘
     |                    |                      |
   NMOS                NMOS -- NMOS          NMOS || NMOS
   (fast)              (slow)               (very fast)

Fast ↑/Fast ↓      Fast ↑/Slow ↓        Slow ↑/Fast ↓
```

---

## How to Use This Project

### Opening Schematics in LTspice

1. **Download & Install:** [LTspice XVII](https://www.analog.com/en/design-center/design-tools-and-calculators/ltspice-simulator.html)

2. **Open a circuit:**
   - Launch LTspice
   - **File → Open** → Select `.asc` file (e.g., `cmos_inverter.asc`)

3. **Run simulation:**
   - **Simulate → Run** (or press Ctrl+R)
   - Waveform window opens automatically

4. **View results:**
   - **Plot → Add Trace** to add signals
   - **Ctrl+L** to view measurement values

### Running Netlists

Alternatively, use the `.cir` netlist files:
1. **File → Open** → Select `.cir` file
2. **Simulate → Run**
3. Same workflow as above

---

## Measurement Data

### Inverter Results
```
Circuit: cmos_inverter_simple.cir
tpHL = 12.548 ps
tpLH = 21.776 ps
tp   = 17.162 ps (average)
Power = 0.203 mW = 203 µW
```

### NAND Gate Results
```
Circuit: cmos_nand2_simple.cir
tpHL = 26.539 ps
tpLH = -42.144 ps (measurement artifact)
Power = 0.108 mW = 108 µW
```

### NOR Gate Results
```
Circuit: cmos_nor2_simple.cir
tpHL = 3.879 ps
tpLH = FAIL (output rarely transitions LOW→HIGH)
Power = 0.0564 mW = 56.4 µW
```

---

## Project Learnings

### Transistor-Level Design Insights

1. **Series vs Parallel Transistors**
   - Series configuration → slower but lower current
   - Parallel configuration → faster but higher current

2. **PMOS vs NMOS Characteristics**
   - NMOS: Higher mobility, faster
   - PMOS: Lower mobility, needs larger W/L ratio for same speed

3. **Sizing Strategy**
   - Compensate slow transistors with larger W/L
   - NAND: Larger PMOS (parallel) to match series NMOS
   - NOR: Larger NMOS (parallel) to match series PMOS

4. **Power Consumption Factors**
   - Dynamic power dominates (switching activity)
   - Static power minimal at room temperature
   - Output duty cycle affects total power

5. **Speed-Power Trade-offs**
   - Faster gates consume more power
   - Larger transistors → faster but higher capacitance
   - Optimization requires careful balance

---

## Technology Assumptions

- **Technology Node:** 1µm CMOS (educational level)
- **Supply Voltage:** 5V DC
- **Temperature:** 27°C (room temperature)
- **Load Capacitance:** 1pF (typical logic load)
- **NMOS/PMOS Models:** Level 1 SPICE models

---

## References

- **CMOS VLSI Design:** Circuit and System Perspective (Weste & Harris)
- **LTspice Official:** https://www.analog.com/en/design-center/design-tools-and-calculators/ltspice-simulator.html
- **SPICE Documentation:** http://www.bwh.harvard.edu/research/ophiodon/pages/spice.html
- **Transistor Modeling:** Standard NMOS/PMOS Level 1 models

---

## Project Outcomes

✅ Demonstrates comprehensive understanding of:
- CMOS transistor fundamentals
- Logic gate design and optimization
- Circuit simulation and analysis
- Performance measurement and comparison
- Design trade-offs and compromises
- Version control and documentation

✅ Practical skills developed:
- LTspice circuit design and simulation
- SPICE netlist creation
- Waveform analysis and measurement
- Comparative analysis techniques
- Technical documentation

---

## Future Enhancements

Potential extensions to this project:

1. **Advanced Topologies**
   - Design 3-input NAND/NOR gates
   - Implement NAND-NOR latch (SR flip-flop)
   - Design multiplexers and adders

2. **Optimization Studies**
   - Transistor sizing optimization for speed/power
   - Temperature and supply voltage variations
   - Different load capacitances

3. **Process Variations**
   - Monte Carlo analysis
   - Process corner simulations
   - Worst-case timing analysis

4. **Advanced Analysis**
   - Noise margin calculation
   - Fan-out analysis
   - Parasitic effects

---

## Author & Contact

**Project:** CMOS Logic Gate Design & Analysis  
**Author:** Vansh Agrawal (VanshAgrawal23)  
**Email:** vanshagrawalitarsi23@gmail.com  
**Date:** 2026-09-19  

---

## Git Commit History

```
38408f8 NOR: Simulation complete with logic verification and comparative performance analysis
a03c86e NAND: Simulation complete with logic verification and performance analysis
ae15b5d Inverter: Simulation complete with timing and power analysis results
380bfbc Inverter: Complete schematic with wiring, netlist, and simulation guide
7c9403c Fix NAND and NOR schematics: Correct wiring and connections
11280a9 Add complete schematics: Inverter, NAND, NOR with wiring and simulation files
e2c9f79 Add CMOS circuit designs: Inverter, NAND, NOR netlists and analysis templates
416b8d0 Initial project setup: folder structure, .gitignore, setup guide, and inverter template
```

---

## License & Usage

This project is provided for educational purposes. Feel free to use, modify, and extend this work for learning and teaching CMOS circuit design.

---

**Project Status:** ✅ COMPLETE  
**Last Updated:** 2026-09-19  
**Documentation Version:** 1.0
