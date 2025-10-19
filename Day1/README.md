# Day-1: NMOS Drain Current (ID) vs Drain-to-Source Voltage (VDS)

### A comprehensive guide to understanding NMOS transistor behavior, SPICE simulation, and device analysis using the **Sky130 Technology Node**.

---
# 🧾 Table of Contents

- [Introduction to SPICE Simulation](#-1-introduction-to-spice-simulation)
- [NMOS Transistor Fundamentals](#2-nmos-transistor-fundamentals)
- [NMOS Operating Regions](#-3-nmos-operating-regions)
- [Body Effect (Substrate Bias Effect)](#4-nmos-body-effect-substrate-bias-effect)
- [Drift Current Theory](#6-drift-current-theory)
- [Pinch-off Condition](#7-pinch-off-condition)
- [Delay Modeling and Analysis](#8-delay-modeling-and-analysis)
- [SPICE Setup & Simulation Labs](#-9-basic-spice-setup-and-simulation-labs)
- [Summary](#summary)

---

## 📘 1. Introduction to SPICE Simulation

### 🔹 What is SPICE?
**SPICE (Simulation Program with Integrated Circuit Emphasis)** is a powerful circuit simulator used to analyze and verify electronic circuits **before fabrication**.  
It converts a **netlist** (text-based circuit description) into mathematical models to predict voltages, currents, and power in a circuit.

Originally developed at **UC Berkeley**, SPICE has become the **industry standard** tool for analog and digital design validation.  
Popular versions include **Ngspice**, **LTspice**, **HSPICE**, and **PSPICE**.

---

### Importance of SPICE in Circuit Design
SPICE acts as a **bridge between theoretical design and real hardware** by enabling:

- Virtual testing before fabrication  
- Verification of circuit functionality (logic, analog, mixed-signal)  
- Power, delay, and noise analysis  
- Optimization of transistor dimensions (W/L ratio) for better performance  
- Early detection of design errors → saving cost and time

---

### ⚙️ Applications of SPICE
| Purpose | Description | Example |
|----------|--------------|----------|
| **Functional Verification** | Ensures correct circuit operation | Inverter logic behavior |
| **Performance Analysis** | Measures delay, rise/fall time | Timing validation |
| **Power Estimation** | Calculates static and dynamic power | Low-power design optimization |
| **Design Optimization** | Tunes sizes and loads for best trade-off | Speed vs Power vs Area |

---

### Main Types of SPICE Analysis

| **Type** | **Command** | **Purpose** |
|-----------|--------------|-------------|
| **DC Operating Point** | `.op` | Finds steady-state voltages and currents in the circuit |
| **DC Sweep** | `.dc` | Sweeps a DC source (like VDD or VIN) to generate I–V characteristics |
| **Transient Analysis** | `.tran` | Studies time-domain response (how signals vary over time) |
| **AC Analysis** | `.ac` | Examines frequency response (gain, phase, and bandwidth) |
| **Noise Analysis** | `.noise` | Measures thermal and flicker noise of devices |

---

### ⚙️ Types of SPICE Simulators

| **Type** | **Description** | **Examples** |
|-----------|----------------|--------------|
| **General-Purpose SPICE** | Original SPICE engine for basic circuit analysis | SPICE3, Ngspice |
| **Commercial SPICE** | Industry-grade versions with advanced modeling and GUI support | HSPICE, PSPICE, LTspice |
| **Fast SPICE** | Optimized for large digital circuits; uses approximations for speed | NanoSim, HSIM, FineSim |
| **Analog/Mixed-Signal SPICE** | Supports both analog and digital (mixed-signal) simulation | Spectre, Eldo, T-Spice |
| **Open-Source SPICE** | Free simulators for education and research | Ngspice, Xyce |

---

## 2. NMOS Transistor Fundamentals

### 🔸 Structure
An **NMOS transistor** has four terminals:
**Gate (G)**, **Drain (D)**, **Source (S)**, and **Body (B)**.

![alt text](image-2.png)

---

### 🔸 Operation Modes
| Gate Voltage (VGS) | Channel Condition | Transistor State |
|---------------------|------------------|------------------|
| VGS < Vt | No inversion layer | **OFF** |
| VGS > Vt | Channel formed | **ON** |

When ON, an **inversion channel** forms between Source and Drain allowing electron flow.

![alt text](image-3.png)

![alt text](image-4.png)

---

## 🧮 3. NMOS Operating Regions

| Region | Condition | Current Equation | Description |
|--------|------------|------------------|--------------|
| **Cut-off** | VGS < Vt | ID ≈ 0 | Transistor OFF |
| **Linear / Triode** | VGS > Vt and VDS small | ID = μnCox(W/L)[(VGS−Vt)VDS − (VDS²/2)] | Acts as voltage-controlled resistor |
| **Saturation** | VDS ≥ (VGS − Vt) | ID = ½ μnCox(W/L)(VGS − Vt)²(1 + λVDS) | Current saturates due to pinch-off |

---

## 4. NMOS Body Effect (Substrate Bias Effect)
- **Body Effect:** Occurs when body-source voltage (VSB) ≠ 0.  
- **Impact:** Increases threshold voltage (Vt).  
- **Reason:** Depletion region widens, reducing channel charge.  
- **Key Terms:**  
  - γ → Body effect coefficient  
  - ϕf → Fermi potential  

---

## 5. Drain Current vs Drain-to-Source Voltage (ID–VDS)

The **ID–VDS curve** shows how current varies with VDS for different gate voltages (VGS).  
- Linear rise at small VDS (resistive region).  
- Current saturates after pinch-off point.

![alt text](image-5.png)

---

## 6. Drift Current Theory
- **Drift Current:** Motion of carriers due to electric field in the channel.  
- **Equation:** ID ∝ Qᵢ(x) × Electric Field.  
- **Concept:** Electrons drift from source to drain, producing current.

---

## 7. Pinch-off Condition
- **Definition:** When VDS = VGS - Vt, channel near drain vanishes.  
- **Transition:** Linear → Saturation region.  
- **Visualization:** Channel shortens and current saturates.

---

## 8. Delay Modeling and Analysis

### Understanding Delay in Digital Circuits
In digital circuits, **delay** is the time taken for an output to respond to a change in input.  
Accurate delay modeling ensures that signals reach different parts of the chip at the correct time, avoiding setup and hold violations.

---

### Cell Delay Dependency
The delay of a logic cell depends mainly on:
1. **Input Slew:** Rate at which input signal changes (transition time).  
   - Slower slews cause longer delays.
2. **Output Load:** Capacitive load driven by output.  
   - Larger loads require more time to charge/discharge, increasing delay.

---

### Delay Lookup Tables (2D LUTs)
SPICE simulations generate delay values stored in **2D Lookup Tables (LUTs)**.  
- Rows → input slew values  
- Columns → output loads  
Each cell (e.g., **CBUF1**, **CBUF2**) uses this table to determine delay under given conditions.

![alt text](image-1.png)

---

### Capacitance and Fan-Out
When a gate drives multiple outputs (fan-out), total load capacitance is:  
**C_total = Σ (input capacitances of driven gates + wire parasitics + output pin capacitance)**  
Higher capacitance increases delay.

---

### Delay Estimation and Interpolation
If the required slew/load is not in the LUT, **linear interpolation** is used to estimate delay between two nearest values.  
If values go beyond the table range, **extrapolation** is applied — but less accurate.

---

## 🧪 9. Basic SPICE Setup and Simulation Labs

### Installation:

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

## 11. Basic SPICE Setup
- **Purpose:** Simulate and verify circuit behavior before fabrication.  
- **Simulator Types:**  
  1. Process  
  2. Circuit  
  3. Logic  
  4. Architecture  
- **SPICE Structure:** Netlist + Model files + Simulation commands.


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


## Summary

This document covers SPICE simulation and its critical role in CMOS circuit design using the **Sky130 technology**.

### Key Takeaways

- **Pre-fabrication Verification:** SPICE allows accurate analysis of circuits before fabrication.  
- **NMOS Characteristics:** Transistor behavior exhibits distinct **linear** and **saturation** regions depending on **VGS** and **VDS**.  
- **Body Effect:** Substrate bias modifies **threshold voltage (Vth)**, impacting device performance.  
- **Delay Modeling:** Reliable timing in digital circuits is achieved via **lookup table (LUT) based analysis**.  
- **Hands-on Simulation:** Ngspice demonstrations produce realistic **I–V curves** using the Sky130 PDK.



