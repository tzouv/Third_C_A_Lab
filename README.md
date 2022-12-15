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

Taking the results from gem5 simualtor and feed them on the McPAT, the simulator can give us information about the total leakage and the dynamic power. Also, the gem5 produces the simulated seconds of each benchmark. By comparing these data on the following formula we can calculate the **total energy** consumption of each benchmark:

> Total energy = ( Total_Leakage + Runtime_Dynamic) * Sim_seconds (J)

The final results of each cache parameter of each benchmark are presented on the following graphs:

![image](https://user-images.githubusercontent.com/118462296/207976545-39a76fd4-f4dc-4ff3-893f-145db87f7665.png)

![image](https://user-images.githubusercontent.com/118462296/207976644-18a1bec5-749b-49d2-bbb1-0b38858a4c30.png)

![image](https://user-images.githubusercontent.com/118462296/207976709-a51337de-1fad-4077-939e-5346a7134906.png)

![image](https://user-images.githubusercontent.com/118462296/207976730-4d32486c-cfbe-4bf0-8f95-da50c427154c.png)

![image](https://user-images.githubusercontent.com/118462296/207976746-1094f4b8-0caf-4dde-b188-63868f4f38d4.png)

**Comments**
Typically, the more complexity of the cache design, the more energy consumes the CPU to execute the benchmarks. We can observe that some parameters have larger affect on the energy than others and the L2 cache design does not have big impact on the total energy.

_Question 2_

The McPAT simulator gives us the **peak power** data for each simulation. We run the program for each cache parameters of the benchmarks, and the following results are presented below:

![image](https://user-images.githubusercontent.com/118462296/207977481-25e5fdc6-af03-48aa-bf3e-e416303e34b9.png)

![image](https://user-images.githubusercontent.com/118462296/207977528-956f420b-3e63-44d6-9d73-33445a913311.png)

![image](https://user-images.githubusercontent.com/118462296/207977588-d5a6aeeb-c5c0-42d2-b7e9-48a29a97e2d9.png)

![image](https://user-images.githubusercontent.com/118462296/207977609-f11fe2c1-c5ba-49b4-949a-25ead1016c95.png)

![image](https://user-images.githubusercontent.com/118462296/207977665-10a881c1-aa5c-4e6c-9514-a372a448f853.png)

