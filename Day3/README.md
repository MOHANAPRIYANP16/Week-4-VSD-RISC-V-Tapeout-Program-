
# CMOS Switching Threshold and Dynamic Simulation

## Introduction

The **Voltage-Transfer Characteristic (VTC)** is a plot of output voltage (Vout) versus input voltage (Vin) when Vin is slowly swept from 0 to VDD. It captures the **static switching behavior** of a CMOS inverter:

- `Vout = f(Vin)`

As Vin increases from 0 to VDD, the inverter passes through three regions:

1. **Low input region (Vin ≈ 0):**  
   - PMOS fully ON, NMOS OFF → Vout ≈ VDD.  
   - Strong connection from VDD to output maintains high output.

2. **Transition region:**  
   - Both NMOS and PMOS conduct partially.  
   - Output drops from VDD toward 0 V.  
   - **Switching threshold (Vm)** occurs here, where Vin = Vout.

3. **High input region (Vin ≈ VDD):**  
   - NMOS fully ON, PMOS OFF → Vout ≈ 0.  
   - Output pulled down to ground.

The resulting **S-shaped VTC** reflects how NMOS and PMOS alternately control the output.

---

## SPICE Simulation of CMOS Inverter

To simulate the CMOS inverter:

1. **Connect components**: PMOS (M1) to VDD, NMOS (M2) to VSS, input (Vin) to gates, output (Vout) from common drain.  
2. **Set component values**: Transistor sizes (W/L), supply voltage (VDD), load capacitance (CL).  
3. **Identify and assign node names**: in, out, VDD, VSS for clarity.  

![alt text](<Screenshot from 2025-10-19 23-43-07.png>)

### Labs
###  day3_inv_tran_Wp084_Wn036.spice

<details> <summary><strong>day2_nfet_idvgs_L015_W039.spice </strong></summary>

```

*Model Description
.param temp=27


*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt


*Netlist Description


XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w=0.84 l=0.15
XM2 out in 0 0 sky130_fd_pr__nfet_01v8 w=0.36 l=0.15


Cload out 0 50fF

Vdd vdd 0 1.8V
Vin in 0 PULSE(0V 1.8V 0 0.1ns 0.1ns 2ns 4ns)

*simulation commands

.tran 1n 10n

.control
run
.endc

.end
```
</details>

run this command inside the design of sky130CircuitDesignWorkshop to Simulate transistor I–V characteristics  :

```bash
   ngspice day3_inv_tran_Wp084_Wn036.spice

```
Then plot the waveforms in ngspice by running :

```bash
  plot out vs time in
```

![alt text](https://github.com/MOHANAPRIYANP16/Week-4-VSD-RISC-V-Tapeout-Program-/blob/main/Day3/Images/day3_tarn_workflow1.png)

![alt text](https://github.com/MOHANAPRIYANP16/Week-4-VSD-RISC-V-Tapeout-Program-/blob/main/Day3/Images/day3_tran_model1.png)

---

# CMOS Inverter – Propagation Delay

Propagation delays are determined using transient analysis:

| Delay Type       | Formula                                                                 |
|-----------------|-------------------------------------------------------------------------|
| Rise (tPLH)      | t(Vout = 0.5 * VDD rising) - t(Vin = 0.5 * VDD falling)                |
| Fall (tPHL)      | t(Vout = 0.5 * VDD falling) - t(Vin = 0.5 * VDD rising)                |

- `0.5 * VDD` represents the 50% voltage level.  
- Rise and fall delays are critical for evaluating inverter speed.

---


### day3_inv_vtc_Wp084_Wn036.spice

<details> <summary><strong> day3_inv_vtc_Wp084_Wn036.spice </strong></summary>

```
*Model Description
.param temp=27


*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt


*Netlist Description


XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w=0.84 l=0.15
XM2 out in 0 0 sky130_fd_pr__nfet_01v8 w=0.36 l=0.15


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
   ngspice day3_inv_vtc_Wp084_Wn036.spice

```
Then plot the waveforms in ngspice by running :

```bash
 plot out vs in
```

![alt text](https://github.com/MOHANAPRIYANP16/Week-4-VSD-RISC-V-Tapeout-Program-/blob/main/Day3/Images/day3_inv_workflow1.png)

![alt text](https://github.com/MOHANAPRIYANP16/Week-4-VSD-RISC-V-Tapeout-Program-/blob/main/Day3/Images/day3_inv_model1.png)

---

# Switching Threshold (Vm)

- **Vm** is the input voltage where `Vin = Vout`.  
- At Vm:
  - Both NMOS and PMOS are ON and operating in **saturation**.  
  - The inverter exhibits **high gain** with a steep VTC slope.  
  - Direct path from VDD to VSS leads to leakage current.

**Examples:**
- Vm ≈ 0.98 V (graph 1)  
- Vm ≈ 1.2 V (graph 2)

**Operating regions along the VTC:**
1. PMOS Linear / NMOS OFF  
2. PMOS Linear / NMOS Saturation  
3. PMOS Saturation / NMOS Saturation → Vm region  
4. PMOS Saturation / NMOS Linear  
5. PMOS OFF / NMOS Linear  

- Drift current relation: `Idsp + Idsn = 0` → used to calculate Vm and determine transistor sizing (W/L).

---

# PMOS & NMOS Sizing Insights

- **Optimal Clock Inverter:** `(Wp/Lp) = 2 × (Wn/Ln)`  
  - Balanced rise/fall delays (~80 ps)  
  - Ideal for clock buffers and clock tree synthesis.

- **Data Path Inverters:**  
  - Other Wp/Lp ratios used for standard inverters or buffers.

- **Switching Threshold Behavior:**  
  - `(Wp/Lp) = 2~5 × (Wn/Ln)` → lower Vm, inverter switches earlier.

- **Impact of PMOS Width:**  
  - Increasing Wp reduces rise delay due to higher charging current.

- **On-Resistance:**  
  - R_on(PMOS) ≈ 2.5 × R_on(NMOS)  
  - PMOS is made wider to balance rise/fall times.

---

# CMOS Inverter Applications

### Static Timing Analysis (STA)
- Serves as reference delay cells for timing arcs.  
- Helps measure rise/fall delays, setup, and hold times.

### Clock Tree Synthesis (CTS)
- Used as clock buffers for fanout management.  
- Regenerates clock signals and introduces intentional skew for balanced timing.  
- Matched rise/fall delays reduce the need for duty cycle correction; otherwise, correction circuits are used.

