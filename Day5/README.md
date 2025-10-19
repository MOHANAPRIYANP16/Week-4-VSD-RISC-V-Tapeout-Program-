
# Day 5: Supply Voltage and Device Variation Analysis

## Table of Contents

1. [Overview](#overview)  
2. [Importance](#importance)  
3. [Device Variation Simulation](#day5_inv_devicevariation_wp7_wn042spice)  
4. [Supply Voltage Variation Simulation](#day5_inv_supplyvariation_wp1_wn036spice)  
5. [Conclusion](#conclusion)


## Overview

Study the effect of **supply voltage (VDD)** changes and **transistor sizing (W/L ratios)** on CMOS inverter performance.  
Focus on how VTC, switching threshold, noise margins, and delays are influenced by variations.

---

## Importance

- Ensures **reliable inverter operation** across voltage and device variations.  
- Predicts changes in **switching threshold (Vm), noise margins, and propagation delays**.  
- Improves circuit **robustness** and helps **maximize yield**.  
- Guides **safe operating margins** for voltage, timing, and current.  
- Critical for **high-speed and low-power designs** where small variations affect performance.

---

###  day5_inv_devicevariation_wp7_wn042.spice

<details> <summary><strong> day5_inv_devicevariation_wp7_wn042.spice </strong></summary>

```
*Model Description
.param temp=27


*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt


*Netlist Description


XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w=7 l=0.15
XM2 out in 0 0 sky130_fd_pr__nfet_01v8 w=0.42 l=0.15


Cload out 0 50fF

Vdd vdd 0 1.8V
Vin in 0 1.8V

*simulation commands

.op

.dc Vin 0 1.8 0.01

.control
run
setplot dc1
display
.endc

.end
```
</details>

run this command inside the design of sky130CircuitDesignWorkshop to Simulate transistor I–V characteristics  :

```bash
   ngspice day5_inv_devicevariation_wp7_wn042.spice

```
Then plot the waveforms in ngspice by running :

```bash
   plot out vs in
```

![alt text](https://github.com/MOHANAPRIYANP16/Week-4-VSD-RISC-V-Tapeout-Program-/blob/main/Day5/Images/day5_deviartion_workflow.png)

![alt text](https://github.com/MOHANAPRIYANP16/Week-4-VSD-RISC-V-Tapeout-Program-/blob/main/Day5/Images/day5_devariatent_model.png)


###  day5_inv_supplyvariation_Wp1_Wn036.spice

<details> <summary><strong> day5_inv_supplyvariation_Wp1_Wn036.spice </strong></summary>

```
*Model Description
.param temp=27


*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt


*Netlist Description


XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w=1 l=0.15
XM2 out in 0 0 sky130_fd_pr__nfet_01v8 w=0.36 l=0.15


Cload out 0 50fF

Vdd vdd 0 1.8V
Vin in 0 1.8V

.control

let powersupply = 1.8
alter Vdd = powersupply
    let voltagesupplyvariation = 0
    dowhile voltagesupplyvariation < 6
    dc Vin 0 1.8 0.01
    let powersupply = powersupply - 0.2
    alter Vdd = powersupply
    let voltagesupplyvariation = voltagesupplyvariation + 1
      end
 
plot dc1.out vs in dc2.out vs in dc3.out vs in dc4.out vs in dc5.out vs in dc6.out vs in xlabel "input voltage(V)" ylabel "output voltage(V)" title "Inveter dc characteristics as a function of supply voltage"

.endc

.end
```
</details>

run this command inside the design of sky130CircuitDesignWorkshop to Simulate transistor I–V characteristics  :

```bash
   ngspice day5_inv_supplyvariation_Wp1_Wn036.spice

```
![alt text](https://github.com/MOHANAPRIYANP16/Week-4-VSD-RISC-V-Tapeout-Program-/blob/main/Day5/Images/Day5_supplyvariation_model.png)


### Conclusion

- CMOS inverter behavior is highly sensitive to supply voltage and device variations.

- Proper sizing and voltage selection allow designers to tune switching threshold, speed, and noise immunity.

- Variation studies are critical for robust digital circuit design under PVT corners, ensuring reliable, high-performance operation.