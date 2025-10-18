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
Installation :

install the ngspice tool through this github :

(https://github.com/spatha0011/spatha_vsd-hdp/tree/main/Day0)[https://github.com/spatha0011/spatha_vsd-hdp/tree/main/Day0]


then gitclone the this respository for sky130circuit design workshop :
```bash
git clone https://github.com/kunalg123/sky130CircuitDesignWorkshop.git
```

![alt text](git_clone_sky130_ngspice.png)

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

![alt text](ngspice_day1.png)

![alt text](model_description_day1.png)
