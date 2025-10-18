# Day-1 Basics of NMOS Drain(id) Vs Drain-to-source Voltage(Vds)

A comprehensive guide to understanding CMOS transistor behavior, circuit analysis, and practical SPICE simulation using Sky130 technology.

---


## 1. Introduction to SPICE Simulation

### What is SPICE (Simulation Program with Integrated Circuit Emphasis)

SPICE is a powerful circuit simulation tool used by electronic engineers to analyze and verify circuit behavior **before physically building it**.  
It takes a circuit’s **netlist** (a text-based description of components and their connections) and performs mathematical analysis to predict voltages, currents, and power at different points in the circuit.

SPICE was originally developed at the **University of California, Berkeley**, and has since become the **industry standard** for circuit-level simulation.  
Popular variants include **Ngspice**, **LTspice**, **HSPICE**, and **PSPICE**.

---

### Importance of SPICE in Circuit Design

In modern **VLSI (Very-Large-Scale Integration)** and **analog design**, even small design errors can lead to huge manufacturing costs.  
Fabricating an IC is expensive, so SPICE helps designers:

- **Test circuits virtually** before fabrication.  
- **Verify functionality** of components (e.g., logic gates, amplifiers).  
- **Study performance parameters** such as delay, power, and gain.  
- **Identify design flaws** early in the design cycle, saving time and cost.  
- **Optimize device dimensions (W/L ratio)** for better performance and lower power consumption.

Thus, SPICE acts as a **bridge between theoretical design and real hardware**.

---

### Applications of SPICE Simulation

1. **Verification of Circuit Functionality**
   - Ensures that the designed circuit performs as expected.
   - Example: Checking if an inverter outputs logic ‘1’ when input is logic ‘0’.

2. **Performance Analysis**
   - Measures timing characteristics such as **propagation delay**, **rise time**, and **fall time**.
   - Helps verify that the circuit meets **speed requirements**.

3. **Power Analysis**
   - Calculates **static and dynamic power consumption**.
   - Aids in optimizing circuits for **low-power VLSI design**.

4. **Design Optimization**
   - Allows experimentation with different transistor sizes, resistances, and capacitances.
   - Helps achieve the best **trade-off between speed, area, and power**.

---

## 3. Delay Modeling and Analysis

### Understanding Delay in Digital Circuits
In digital circuits, **delay** is the time taken for an output to respond to a change in input.  
Accurate delay modeling ensures that signals reach different parts of the chip at the correct time, avoiding setup and hold violations.

---

### Cell Delay Dependency
The delay of a logic cell depends mainly on two factors:
1. **Input Slew:** The rate at which the input signal changes (transition time).  
   - Slower slews cause longer delays.
2. **Output Load:** The capacitive load driven by the output.  
   - Larger loads require more time to charge/discharge, increasing delay.

---

### Delay Lookup Tables (2D LUTs)
SPICE simulations generate delay values stored in **2D Lookup Tables (LUTs)**.  
- Rows → represent input slew values.  
- Columns → represent output loads.  
Each cell (e.g., **CBUF1**, **CBUF2**) uses this table to determine delay under given conditions.

---

### Capacitance and Fan-Out
When a gate drives multiple outputs (fan-out), total load capacitance is:
**C_total = Σ (input capacitances of driven gates + wire parasitics + output pin capacitance)**  
Higher capacitance increases delay.

---

### Delay Estimation and Interpolation
If the required slew/load is not in the LUT, **linear interpolation** is used to estimate delay between two nearest values.  
If values go beyond the table range, **extrapolation** is applied—but it’s less accurate.

---

## 4. NMOS Transistor Fundamentals
- **NMOS Structure:** Has 4 terminals — Gate (G), Drain (D), Source (S), and Body (B).  
- **Operation:**
  - **VGS = 0:** No channel → transistor OFF.  
  - **VGS > Vt:** Channel forms → transistor ON.  
- **Channel Formation:** Inversion layer forms between source and drain allowing current flow.

---

## 5. NMOS Body Effect (Substrate Bias Effect)
- **Body Effect:** Occurs when body-source voltage (VSB) ≠ 0.  
- **Impact:** Increases threshold voltage (Vt).  
- **Reason:** Depletion region widens, reducing channel charge.  
- **Key Terms:**  
  - γ → Body effect coefficient  
  - ϕf → Fermi potential  

---

## 6. NMOS Resistive (Linear) Region
- **Condition:** VGS > Vt and small VDS.  
- **Behavior:** Acts as a voltage-controlled resistor.  
- **Current Flow:** Uniform channel charge allows linear current variation with VDS.

---

## 7. Drift Current Theory
- **Drift Current:** Motion of carriers due to electric field in the channel.  
- **Equation:** ID ∝ Qᵢ(x) × Electric Field.  
- **Concept:** Electrons drift from source to drain, producing current.

---

## 8. Drain Current Model (Linear Region)
- **Equation:** ID = μnCox(W/L)[(VGS - Vt)VDS - (VDS²/2)].  
- **Graph:** Linear rise of ID with VDS.  
- **SPICE Check:** ID–VDS curve confirms linear behavior at small VDS.

---

## 9. Pinch-off Condition
- **Definition:** When VDS = VGS - Vt, channel near drain vanishes.  
- **Transition:** Linear → Saturation region.  
- **Visualization:** Channel shortens and current saturates.

---

## 10. Drain Current Model (Saturation Region)
- **Equation:** ID = (½)μnCox(W/L)(VGS - Vt)²(1 + λVDS).  
- **Effect:** Channel length modulation increases ID slightly with VDS.  
- **Comparison:** Saturation ID is nearly constant vs. linear increase earlier.

---

## 11. Basic SPICE Setup
- **Purpose:** Simulate and verify circuit behavior before fabrication.  
- **Simulator Types:**  
  1. Process  
  2. Circuit  
  3. Logic  
  4. Architecture  
- **SPICE Structure:** Netlist + Model files + Simulation commands.

---

##  12. SPICE Analysis Types
| Type | Description |
|------|--------------|
| DC Analysis | Finds DC operating point |
| AC Analysis | Frequency response |
| Transient | Time-domain behavior |
| Pole-Zero | System poles/zeros |
| Distortion | Harmonic levels |
| Sensitivity | Parameter impact |
| Noise | Device noise levels |

---

## 13. SPICE Netlist Syntax
- **Example:** NMOS test circuit using voltage source and resistor.  
- **Key Lines:**  
  - `.lib` or `.include` → Add model files.  
  - Transistor → `M1 drain gate source body model W= L=`  
  - Commands → `.op`, `.dc`, `.tran`.

---

## 14. SPICE Lab with sky130 Models
- **PDK:** SkyWater 130nm used in Ngspice.  
- **Files:** `sky130.lib.spice`, `nfet_01v8`, `pfet_01v8`.  
- **Example:** `day1_nfet_idvds_L2_W5.spice`  
- **Analysis:** `.op`, `.dc` used to plot ID–VDS curves.

---

## 15. SPICE Simulation Results and Observations
- **Output:** ID–VDS characteristics confirm theory.  
- **Observation:** Clear transition from linear to saturation region.  
- **Insight:** Validates NMOS operation and SPICE accuracy.

---

Installation :

install the ngspice tool through this github :

(https://github.com/spatha0011/spatha_vsd-hdp/tree/main/Day0)[https://github.com/spatha0011/spatha_vsd-hdp/tree/main/Day0]


then gitclone the this respository for sky130circuit design workshop :
```bash
git clone https://github.com/kunalg123/sky130CircuitDesignWorkshop.git
```

![alt text](https://github.com/MOHANAPRIYANP16/Week-4-VSD-RISC-V-Tapeout-Program-/blob/main/Day1/Images/git_clone_sky130_ngspice.png)

Then get into that folder what get cloned :

```bash
cd sky130CircuitDesignWorkshop
```
then list the files and folders inside it :

```bash
 ls
```
then get into the design folder where all required sky130 circuit designs are available :

```bash
 cd design
```
Then simulate the circuit using ngspice tool

## Labs 

### 1. day1_nfet_idvds_L2_W5.spice

<details> <summary><strong>day1_nfet_idvds_L2_W5.spice </strong></summary>

```
*Model Description
.param temp=27


*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt


*Netlist Description



XM1 Vdd n1 0 0 sky130_fd_pr__nfet_01v8 w=5 l=2

R1 n1 in 55

Vdd vdd 0 1.8V
Vin in 0 1.8V

*simulation commands

.op
.dc Vdd 0 1.8 0.1 Vin 0 1.8 0.2

.control

run
display
setplot dc1
.endc

.end
```

</details>

run this command inside the design of sky130CircuitDesignWorkshop to Simulate transistor I–V characteristics  :

```bash
   ngspice day1_nfet_idvds_L2_W5.spice
```
Then plot the waveforms in ngspice by running :

```bash
   plot -vdd#branch
```

![alt text](https://github.com/MOHANAPRIYANP16/Week-4-VSD-RISC-V-Tapeout-Program-/blob/main/Day1/Images/ngspice_day1.png)

![alt text](https://github.com/MOHANAPRIYANP16/Week-4-VSD-RISC-V-Tapeout-Program-/blob/main/Day1/Images/model_description_day1.png)
