# CMOS Inverter - Next Steps to Get Waveforms

## Files Created for You

### 1. **cmos_inverter.asc** (Schematic - Already Wired by You)
- ✅ All components placed and wired correctly
- ✅ Ready to simulate
- Location: `C:\Users\Test\Vivado_projects\CMOS-Logic-Gate-Design\01-Inverter\`

### 2. **cmos_inverter_final.cir** (Netlist - Just Created)
- ✅ Complete SPICE netlist with all models defined
- ✅ All measurements configured
- Location: `C:\Users\Test\Vivado_projects\CMOS-Logic-Gate-Design\01-Inverter\`

---

## How to Generate Waveforms

### **Method 1: Using the Schematic (.asc file) - RECOMMENDED**

1. **Open LTspice**
2. **File → Open** → Select: `cmos_inverter.asc`
3. **Simulate → Run** (or press **Ctrl+R**)
4. Wait 5-10 seconds
5. **Waveform window pops up automatically** with three signals:
   - V(Vin) — Blue line (input)
   - V(out) — Red line (output, inverted)
   - V(Vdd) — Green line (power supply)

### **Method 2: Using the Netlist (.cir file) - Alternative**

1. **Open LTspice**
2. **File → Open** → Select: `cmos_inverter_final.cir`
3. **Simulate → Run** (or press **Ctrl+R**)
4. Waveforms appear automatically

---

## What the Waveforms Should Show

### **Expected Output:**

```
V(Vin)  ╱‾‾‾╲    ╱‾‾‾╲    ╱‾‾‾╲
       ╱     ╲  ╱     ╲  ╱     ╲  (Input: 0-5V pulse)
      ╱       ╲╱       ╲╱       ╲

V(out) ‾╲___╱‾ ‾╲___╱‾ ‾╲___╱‾  (Output: Inverted, 5-0V pulse)
        ↑     ↑ ↑     ↑ ↑     ↑
        └─────┘ └─────┘ └─────┘ (Propagation delay)

V(Vdd) ‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾  (Constant 5V)
```

### **Key Verification:**
- ✅ When Vin goes HIGH (0→5V), out goes LOW (5→0V)
- ✅ When Vin goes LOW (5→0V), out goes HIGH (0→5V)
- ✅ Small delay between input and output (typical: 0.1-1ns)
- ✅ Output swings fully 0-5V

---

## Extract Measurement Data

### **After Waveforms Appear:**

1. **Press Ctrl+L** to open "SPICE Error Log"
2. **Scroll down** to find measurements like:
   ```
   tpHL = 0.xxx ns
   tpLH = 0.xxx ns
   tp = 0.xxx ns
   trise = 0.xxx ns
   tfall = 0.xxx ns
   avg_power = xxx.xxx uW
   peak_current = xxx.xxx mA
   ```

3. **Record these values** (you'll need them later for comparison)

---

## If No Waveforms Appear

### **Troubleshooting:**

**Error: "Cannot find model"**
- Solution: Use `cmos_inverter_final.cir` instead (has models defined)

**Error: "Unconnected node"**
- Solution: Make sure GND is connected (should see "0" flag in schematic)

**Blank waveform window**
- Solution: Try zooming in (scroll wheel in waveform window)
- Or close window and re-run simulation

---

## Your Next Action

**Try ONE of these:**

1. **Open `cmos_inverter.asc` → Simulate → Run**
2. **OR Open `cmos_inverter_final.cir` → Simulate → Run**

**Then tell me:**
- ✅ Do you see waveforms?
- ✅ What do the measurements show?
- ✅ Any errors?

Once inverter works, we'll do **NAND and NOR** the exact same way! 🚀

---

*Last Updated: 2026-09-19*
