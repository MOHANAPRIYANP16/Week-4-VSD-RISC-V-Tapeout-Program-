

# Velocity Saturation in MOSFETs & CMOS Inverter Voltage Transfer Characteristics (VTC)

> Understanding short-channel effects, NMOS/PMOS behavior, and CMOS inverter operation using SPICE simulations.

---

## Table of Contents (Short Version)

1. [Introduction](#introduction) – Overview of NMOS/PMOS behavior, VTC, and SPICE simulations.  
2. [NMOS Operating Regions](#nmos-operating-regions) – Cutoff, Linear, and Saturation regions explained.  
3. [Velocity Saturation Effect](#velocity-saturation-effect) – Effect of short-channel scaling on Id–Vgs/Id–Vds.  
4. [Long Channel vs Short Channel NMOS](#long-channel-vs-short-channel-nmos) – Comparison of performance and peak current.  
5. [SPICE Simulation](#spice-simulation) – Step-by-step transistor I–V simulation using Sky130 PDK.  
6. [Summary](#summary)
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

## Summary

This repository demonstrates **NMOS and PMOS transistor behavior** with a focus on short-channel effects and CMOS inverter characteristics.  

Key Points:

- NMOS operates in **Cutoff, Linear, and Saturation regions**, with transition at \( V_{DS} = V_{GS} - V_T \).  
- **Velocity saturation** in short-channel devices limits drain current and modifies Id–Vgs behavior from quadratic to linear at high gate voltages.  
- Long-channel devices follow ideal quadratic behavior, while short-channel devices show reduced peak current and early saturation.  
- CMOS inverters’ **voltage-transfer characteristics (VTC)** explain switching thresholds and delays.  
- **SPICE simulations** using the Sky130 PDK visualize transistor I–V curves, highlighting the effects of channel length scaling.  
- Critical electric field (Ec) defines the onset of velocity saturation.  

This study helps understand **channel length scaling, device performance, and inverter delay characteristics** for modern CMOS design.