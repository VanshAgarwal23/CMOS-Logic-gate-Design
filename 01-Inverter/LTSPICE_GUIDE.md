# How to Run CMOS Inverter Simulation in LTspice

## Quick Start Guide

### Step 1: Open the Netlist File in LTspice
1. Open **LTspice XVII** (search "LTspice" in Windows)
2. Go to **File → Open**
3. Navigate to: `C:\Users\Test\Vivado_projects\CMOS-Logic-Gate-Design\01-Inverter\`
4. Select **cmos_inverter.cir** and click **Open**

### Step 2: Run the Simulation
1. The netlist will open in the text editor
2. Click **Simulate → Run** (or press **Ctrl+R**)
3. Wait for the simulation to complete (should take a few seconds)
4. A waveform viewer window will appear automatically

### Step 3: View the Results

#### In the Waveform Window:
- **Blue line (V(in))**: Input signal (rises and falls)
- **Red line (V(out))**: Output signal (inverted)
- **Green line (V(vdd))**: Power supply (constant at 5V)

#### You should see:
- When input goes HIGH (0→5V), output goes LOW (5→0V)
- When input goes LOW (5→0V), output goes HIGH (0→5V)
- There will be a small time delay between input change and output change (propagation delay)

### Step 4: Extract Measurements

The simulation automatically calculates:
- **tpHL**: Propagation delay (High to Low) in nanoseconds
- **tpLH**: Propagation delay (Low to High) in nanoseconds
- **tp**: Average propagation delay
- **trise**: Output rise time
- **tfall**: Output fall time
- **avg_power**: Average power dissipation in Watts
- **peak_current**: Peak current from VDD in Amperes

#### To see measurements:
1. In the waveform window, go to **Simulate → Edit Simulation Cmd**
2. Look for the measurement results in the SPICE error log window
3. Or press **Ctrl+L** to open the error/log window

### Step 5: Analyze the Data

Record these values:
```
Propagation Delay (tpHL): _____ ns
Propagation Delay (tpLH): _____ ns
Average Delay (tp):       _____ ns
Rise Time (trise):        _____ ns
Fall Time (tfall):        _____ ns
Average Power:            _____ µW
Peak Current:             _____ mA
```

### Step 6: Save the Results

1. **Export waveform data**:
   - Right-click on a waveform → **Export Data** → Save as CSV

2. **Save a screenshot**:
   - Press **Print Screen** and paste into a document

3. **Copy measurement values**:
   - From the error log, copy all .meas values to a text file

## Important Notes

### Simulation Parameters:
- **Input frequency**: 10 MHz (100ns period)
- **Temperature**: 25°C (room temperature)
- **Supply voltage**: 5V DC
- **Load capacitance**: 1pF (typical for logic gate)

### If simulation doesn't work:
- Check that the file path is correct
- Ensure all text is properly formatted (no stray characters)
- Try: **Simulate → SPICE Error Log** to see error messages

### To modify the circuit:
- Edit the netlist file and change:
  - **Vdd value** for different supply voltages
  - **W/L ratios** of transistors (M1 and M2) for sizing optimization
  - **CL value** for different load capacitances
  - **Pulse parameters** for different input signals

## What Each Line Does:

| Line | Purpose |
|------|---------|
| `Vdd vdd 0 DC 5` | 5V power supply |
| `Vin in 0 PULSE(...)` | Input signal generator |
| `CL out 0 1pF` | Load capacitance |
| `M1, M2` | NMOS and PMOS transistors |
| `.model NMOD, PMOD` | Transistor parameters |
| `.tran 0 300ns` | Run for 300 nanoseconds |
| `.meas` commands | Calculate timing and power |

## Next Steps After Simulation:

1. Document all measurement values
2. Create a summary text file with results
3. Commit changes to Git
4. Move to NAND gate design

---

**Questions?** Let me know if you get stuck at any step!
