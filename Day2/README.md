

# Velocity Saturation in MOSFETs & CMOS Inverter Voltage Transfer Characteristics (VTC)

> Understanding short-channel effects, NMOS/PMOS behavior, and CMOS inverter operation using SPICE simulations.

---

## Table of Contents (Short Version)

1. [Introduction](#introduction) – Overview of NMOS/PMOS behavior, VTC, and SPICE simulations.  
2. [NMOS Operating Regions](#nmos-operating-regions) – Cutoff, Linear, and Saturation regions explained.  
3. [Velocity Saturation Effect](#velocity-saturation-effect) – Effect of short-channel scaling on Id–Vgs/Id–Vds.  
4. [Long Channel vs Short Channel NMOS](#long-channel-vs-short-channel-nmos) – Comparison of performance and peak current.  
5. [SPICE Simulation](#spice-simulation) – Step-by-step transistor I–V simulation using Sky130 PDK.  
6. [CMOS Voltage-Transfer Characteristics (VTC)](#cmos-voltage-transfer-characteristics-vtc)
7. [Summary](#summary)
---

## Introduction

This repository explores the **behavior of NMOS and PMOS transistors**, focusing on:

1. **Drain current characteristics** (Id–Vds, Id–Vgs) for long- and short-channel devices.  
2. **Velocity saturation** effects in short-channel MOSFETs.  
3. **CMOS inverter voltage-transfer characteristics (VTC)** and delay estimation.  
4. **SPICE simulations** using the Sky130 PDK to visualize transistor behavior.  

**Objective:**  
To understand how channel length scaling impacts transistor operation and inverter performance.

---

## NMOS Operating Regions

The NMOS transistor operates in three primary regions:

| Region | Condition | Behavior |
|--------|-----------|----------|
| Cutoff | Vgs < Vth | No conduction, Id ≈ 0 |
| Linear (Resistive) | Vgs > Vth, Vds < (Vgs − Vth) | Id increases linearly with Vds |
| Saturation | Vgs > Vth, Vds ≥ (Vgs − Vth) | Id reaches a constant value, affected by channel length modulation |

> **Note:** The transition between linear and saturation regions occurs at \( Vds = Vgs - Vth \).

---

## Velocity Saturation Effect

From the graph, two distinct regions can be identified based on the condition \( V_{DS} = V_{GS} - V_T \).  

### Linear Region
- The drain current (Id) increases **linearly** with the drain-to-source voltage (Vds) when Vds is below the threshold.  
- Defined for: \( V_{DS} < V_{GS} - V_T \).  

### Saturation Region
- The drain current (Id) becomes largely **independent of Vds** and is influenced by **channel length modulation**.  
- Defined for: \( V_{DS} \ge V_{GS} - V_T \).  

![alt text](https://github.com/MOHANAPRIYANP16/Week-4-VSD-RISC-V-Tapeout-Program-/blob/main/Day2/Images/velocity_saturation_effect.png)



In **short-channel MOSFETs**, at high electric fields:

- Electrons reach **maximum drift velocity**, \(v_{sat}\).  
- Further increase in Vgs does **not increase current quadratically**.  
- The drain current **Id becomes linear** with Vgs at high gate voltages.

**Mechanism:**

1. Low electric field → electron velocity ∝ electric field (linear).  
2. High electric field → electron velocity saturates due to **scattering effects**.  

![alt text](https://github.com/MOHANAPRIYANP16/Week-4-VSD-RISC-V-Tapeout-Program-/blob/main/Day2/Images/short_channel.png)

**Critical Electric Field (Ec):**  
Used to determine when velocity saturation starts.

---

## Long Channel vs Short Channel NMOS

| Parameter | Long Channel | Short Channel |
|-----------|-------------|---------------|
| W/L | 1.8 μm / 1.2 μm | 0.375 μm / 0.25 μm |
| Drain Current (Id) | Quadratic w.r.t Vgs | Linear at high Vgs |
| Peak Current | Higher | Lower |
| Switching Speed | Moderate | Faster |
| Short-Channel Effects | Minimal | Significant (Voltage Saturation, DIBL) |

**Observation:**  
- Short-channel devices saturate earlier, limiting Id.  
- Long-channel devices follow ideal MOSFET quadratic Id–Vgs behavior.

![alt text](https://github.com/MOHANAPRIYANP16/Week-4-VSD-RISC-V-Tapeout-Program-/blob/main/Day2/Images/long_channel.png)

---

## SPICE Simulation 


### 1. day2_nfet_idvds_L015_W039.spice

<details> <summary><strong>day2_nfet_idvds_L015_W039.spice </strong></summary>

```
*Model Description
.param temp=27


*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt


*Netlist Description



XM1 Vdd n1 0 0 sky130_fd_pr__nfet_01v8 w=0.39 l=0.15

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
   ngspice day2_nfet_idvds_L015_W039.spice
```
Then plot the waveforms in ngspice by running :

```bash
   plot -vdd#branch
```


![alt text](https://github.com/MOHANAPRIYANP16/Week-4-VSD-RISC-V-Tapeout-Program-/blob/main/Day2/Images/day2_vds.png)

![alt text](https://github.com/MOHANAPRIYANP16/Week-4-VSD-RISC-V-Tapeout-Program-/blob/main/Day2/Images/day2_vds_model_description.png)

---

### 2. day2_nfet_idvgs_L015_W039.spice

<details> <summary><strong>day2_nfet_idvgs_L015_W039.spice </strong></summary>

```
*Model Description
.param temp=27


*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt


*Netlist Description

XM1 Vdd n1 0 0 sky130_fd_pr__nfet_01v8 w=0.39 l=0.15

R1 n1 in 55

Vdd vdd 0 1.8V
Vin in 0 1.8V

*simulation commands

.op
.dc Vin 0 1.8 0.1 

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
   ngspice day2_nfet_idvgs_L015_W039.spice
```
Then plot the waveforms in ngspice by running :

```bash
   plot -vdd#branch
```

![alt text](https://github.com/MOHANAPRIYANP16/Week-4-VSD-RISC-V-Tapeout-Program-/blob/main/Day2/Images/day2_vgs.png)

![alt text](https://github.com/MOHANAPRIYANP16/Week-4-VSD-RISC-V-Tapeout-Program-/blob/main/Day2/Images/day2_vgs_model.png)

---


## CMOS Voltage-Transfer Characteristics (VTC)

The **Voltage-Transfer Characteristic (VTC)** of a CMOS inverter describes how the output voltage (Vout) changes with respect to the input voltage (Vin).  
It is essential for understanding **cell delays** and designing reliable digital circuits.

### MOSFET as a Switch

**NMOS:** ON when \( V_{GS} > V_T \), OFF when \( V_{GS} < V_T \)  
**PMOS:** ON when \( V_{GS} < V_T \), OFF when \( V_{GS} > V_T \)  

- **OFF State:** The MOSFET behaves like an **open switch** (infinite resistance).  
  - Condition: \( |V_{GS}| < |V_T| \)  
- **ON State:** The MOSFET behaves like a **closed switch** (finite resistance).  
  - Condition: \( |V_{GS}| > |V_T| \)  

![alt text](https://github.com/MOHANAPRIYANP16/Week-4-VSD-RISC-V-Tapeout-Program-/blob/main/Day2/Images/cmos_device_characteristic.png)

---

### Standard MOS Voltage-Current Parameters

![alt text](https://github.com/MOHANAPRIYANP16/Week-4-VSD-RISC-V-Tapeout-Program-/blob/main/Day2/Images/Standard%20MOS%20Voltage-Current%20Parameters.png)

- **Transistor-Level CMOS Inverter:**  
  - PMOS connected to Vdd, NMOS connected to Vss, Vin applied to both gates.  
  - Output (Vout) taken from the common drain node; CL represents load capacitance.  

- **Vin = Vdd (Logic HIGH Input):**  
  - NMOS ON (acts as Rn), PMOS OFF (open switch) → Vout = 0  
  - The capacitor discharges through NMOS to Vss.  

- **Vin = 0 (Logic LOW Input):**  
  - PMOS ON (acts as Rp), NMOS OFF (open switch) → Vout = Vdd  
  - The capacitor charges through PMOS to Vdd.  

---

### Load Line Analysis for NMOS and PMOS

To derive **cell delays**, plot the **load curves** for NMOS and PMOS transistors:  
- Convert PMOS gate-source voltage (VgsP) to equivalent Vin.  
- Replace all internal node voltages with Vin, Vdd, Vss, and Vout.  

![alt text](https://github.com/MOHANAPRIYANP16/Week-4-VSD-RISC-V-Tapeout-Program-/blob/main/Day2/Images/Load%20Line%20Analysis%20for%20NMOS%20and%20PMOS.png)

- **PMOS Load Curve:**  
  Shows capacitor charging behavior and Vout transition.  

![alt text](https://github.com/MOHANAPRIYANP16/Week-4-VSD-RISC-V-Tapeout-Program-/blob/main/Day2/Images/PMOS%20Load%20Curve.png)

- **NMOS Load Curve:**  
  Shows capacitor discharging behavior.  

![alt text](https://github.com/MOHANAPRIYANP16/Week-4-VSD-RISC-V-Tapeout-Program-/blob/main/Day2/Images/NMOS%20Load%20Curve.png)

### Merged PMOS–NMOS Load Curves → VTC

- Combining PMOS and NMOS curves gives the **complete inverter VTC**.  
- The VTC is used to determine **switching threshold, noise margins, and delay characteristics**.  

![alt text](https://github.com/MOHANAPRIYANP16/Week-4-VSD-RISC-V-Tapeout-Program-/blob/main/Day2/Images/Merged%20PMOS%E2%80%93NMOS%20Load%20Curves%20%E2%86%92%20VTC.png)

## Summary

This repository demonstrates the behavior and characteristics of CMOS inverters and MOSFETs through SPICE simulations and theoretical analysis. Key highlights include:

1. **NMOS Operating Regions:**
   - MOSFETs operate in **Cutoff**, **Linear (Resistive)**, and **Saturation** regions.
   - Linear region: Drain current \( I_D \) increases linearly with \( V_{DS} \).  
   - Saturation region: \( I_D \) is largely independent of \( V_{DS} \) and affected by channel length modulation.

2. **Velocity Saturation in Short-Channel MOSFETs:**
   - At high electric fields, electron velocity saturates (\( v_{sat} \)), causing \( I_D \) to grow linearly with \( V_{GS} \) instead of quadratically.
   - Low electric fields: \( v \propto E \) (linear).  
   - High electric fields: velocity saturates due to scattering, limiting drain current.

3. **Long vs Short Channel NMOS:**
   - Long-channel devices: Quadratic \( I_D–V_{GS} \) behavior, higher peak current.  
   - Short-channel devices: Early saturation, lower peak current, faster switching, significant short-channel effects (Voltage Saturation, DIBL).

4. **CMOS Voltage-Transfer Characteristics (VTC):**
   - VTC shows how output voltage (Vout) responds to input voltage (Vin).  
   - Critical for analyzing **cell delays**, **switching thresholds**, and **noise margins**.

5. **MOSFET as a Switch:**
   - **NMOS:** ON if \( V_{GS} > V_T \), OFF if \( V_{GS} < V_T \).  
   - **PMOS:** ON if \( V_{GS} < V_T \), OFF if \( V_{GS} > V_T \).  
   - OFF state: behaves like an open switch (infinite resistance).  
   - ON state: behaves like a closed switch (finite resistance).  

6. **Transistor-Level CMOS Inverter Behavior:**
   - **Vin = Vdd:** NMOS ON, PMOS OFF → Vout = 0 (capacitor discharges through NMOS).  
   - **Vin = 0:** PMOS ON, NMOS OFF → Vout = Vdd (capacitor charges through PMOS).

7. **Load Line Analysis & VTC Plotting:**
   - Load curves for NMOS and PMOS determine the inverter’s VTC.
   - Merging PMOS and NMOS curves produces the **complete VTC**, which is used to evaluate **switching thresholds, noise margins, and cell delays**.

This summary consolidates **theoretical concepts, SPICE simulations, and graphical observations** to provide a comprehensive understanding of CMOS inverter operation and MOSFET behavior.
