### 7. day5_inv_devicevariation_wp7_wn042.spice

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


### 8. day5_inv_supplyvariation_Wp1_Wn036.spice

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

