# Third Computer Architecture Lab: McPAT and GEM5 simulators

Authors: Konstantinidis Paschalis, Tzouvaras Evangelos

### **_Intro_**
The objactive of this lab is to be familiar with the McPAT simulator that produces results about the consuming power of a program. Also, this lab has the goal to compine results from gem5 simulator alongside with the McPAT to choose the "ideal" processor architecture for each benchmark. 

### **_First Part: Familiarzation with McPAT simulator_**

_Question 1_

**Dynamic power**

Circuits dissipate dynamic power when they charge and discharge the capacitive loads to switch states (when they change their value from 0 to 1 and vice versa). Dynamic power is proportional to the total load capacitance, the supply voltage, the voltage swing during switching, the clock frequency, and the activity factor. The McPAT calculates the load capacitance of a module by decomposing it into basic circuit blocks, and using analytic models for each block with appropriately sized devices. Also it calculates the activity factor using access statistics from architectural simulation together with circuit properties. 
Switching circuits also dissipate short-circuit power due to a momentary short through the pull-up and pull-down devices.McPAT computeS the shortcircuit power using the equations derived in the work by Nose et al.

**Leakage**

Transistors in circuits leak, dissipating static power. Leakage current depends on the width of the transistors and the local state of the devices. There are two leakage mechanisms. 
Subthreshold leakage occurs because a small current passes between the source and drain of off-state transistors.Gate leakage is the current leaking through the gate terminal, and varies greatly with the state of the device. McPAT determines the unit leakage current using Intel’s data .

**Different programs on the same processor**

If we run different programs on the same processor, this will have on affect on the dynamic power, since every program uses the same hardware to be executed but some programs may be using multiple times the same hardware or much more frequent.
Leakage current is a static metric and it will not affect.

_Question 2_

The watt ratings refer to the maximum power that a cpu can handle under full load.The efficiency of the system , thus the battery is also determined from the program that will run on the cpu and how well can a cpu handle that load. Also, the battery has a specific energy capacitance. So the metric that can give us the bigger length of the battery is the Wh and not the Watt. So a 5W CPU can give larger battery life cycle.
Standalone mcpat can't give us the answer on this problem. We probably want data about how each if the cpus respond under specific load . For example from gem5 we can take the total execution time of the program and from mcpat we can get watts and caclulate the total energy that is required.

_Question 3_

To compare the McPAT results from Xeon and ARM A9 processors, a good metric is the EDAP (Energy-Delay-Area Product). 
The EDAP can be calculated from the following formula: 

>EDAP = (Runtime Dynamic + Subthreshold Leakage + Gate Leakage) * x * Area
, where x is the runtime.

By using the formula above, the results are the following:
* Xeon EDAP = 45053.5127063x
* ARM EDAP = 850.1267084x

We can see from EDAP parameter that although ARM has 50 times more delay it is still much more energy efficient.

### **_Second Part: Gem5 and McPAT: EDP improvement_**

_Question 1_



