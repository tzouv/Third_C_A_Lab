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
Moreover from McPAT results we can see that:
Xeon peak power = 134.938W
ARM  peak power =1.74189W

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


_Question 3_

It is difficult to determine the "ideal" architecture of processor for each benchmark based for all data of the labs. It looks like a multiple function variable which has the EPD data (from this lab), the CPI thas is calculated from the gem5 and the cost function that we presented on the second lab, as variables. So, we have to calculate the output of this function for each cache variable of each benchmark. It is difficult to find the accurate math function because each variable has different weigh on the final result. So we decided to choose the "ideal" architecture by observing the CPI graphs from 2nd result, the EDP graphs for this lab and the cost function.

**401.bzip benchmark**

The best solution for this benchmark is to choose a cache line equals to 128, because it offers a reduction of the CPI and the energy increases not so much, while cost is constant. The best L1 association is 4 because CPI is the small and also the energy and the cost are as small as posiible. We chose also the L1 data cache to be 64KB to reduce the CPI as much as we can with the EDP to be not so increased. The L1 data cache and also the parameters of the L2 cache will be the smallest because they have not affect on the CPI and keep the energy and cost low.
The optimized CPU parameters for the 401 benchmark are the following:
* Cache line = 128
* L1 association = 4
* L1 data cache size = 64KB
* L1 instruction cache size = 16KB
* L2 cache size = 4MB
* L2 association = 4

**429.mfc benchmark**

The better option for the cache line is 32 because all the variables are the minimum possible. The best compromization between the CPI, the energy and the cost is when we select for L1 data size 32KB, for L1 instruction size 64KB and for L2 size 2MB. We can achieve small CPI and the increases on the energy and cost will be the smallest possible. The final parameters for this benchmark will be:
* Cache line = 32
* L1 association = 4
* L1 data cache size = 32KB
* L1 instruction cache size = 64KB
* L2 cache size = 2MB
* L2 association = 4

**456.hmmer benchmark**

To take the minimum possible CPI with the smallest increase on the EDP parameter and on the cost, we decided to choose the architecture:
* Cache line = 128
* L1 association = 4
* L1 data cache size = 64KB
* L1 instruction cache size = 32KB
* L2 cache size = 1MB
* L2 association = 4

On this benchmark the gratest affect on the CPI has the cache line. But also increases a lot the total energy. So the best selection is on 128. Also, the L1 data size on 64KB has great affect on the CPI by keeping the energy on logic levels. The L2 parameters do not decrease the CPI, so we select the cheaper and less energy cost solution, which is the simpliest design.

**458.sjeng benchmark**

On this benchmark, the cache line size has an extremelly impact to decrease the CPI for 40%. However, the cost and the energy will be the maximum possible for this parameter, we choose this solution. We try to keep the EDP and the cost low by choosing the simpliest architecture on the other cache characteristics. The final results will be the following:
* Cache line = 256
* L1 association = 2
* L1 data cache size = 16KB
* L1 instruction cache size = 16KB
* L2 cache size = 1MB
* L2 association = 4

**470.lbm benchmark**

The minimum CPI to have faster program, can be achieved by using cache line 256. This will increase the EDP and the cost but in normal levels. Another good affect will have the L1 data cache on 32KB because we have the minimum possible energy and a small increament on the cost. As for the L1 instruction cache and for L2 we chose to use the simpliest architecture because does not have an effect on the CPI and the simpliest is the cheaper. The final design will be the following: 
* Cache line = 256
* L1 association = 4
* L1 data cache size = 32KB
* L1 instruction cache size = 16KB
* L2 cache size = 1MB
* L2 association = 4

**Results Reliability**

  In order to be an accurate power and area modeling tool McPAT need to have both relative and absolute accuracy. Relative accuracy means that changes in modeled power 
as a result of architecture modifications should reflect on a relative scale the changes one would see in a real design. The relative accuracy of McPAT ensures that 
the relative power weights of different components of a chip have been correctly modeled. While the absolute magnitude accuracy of McPAT means that the power numbers 
for individual components and total power are evaluated correctly.Some of the reasons that may result in a difference between the modeled power numbers and actual 
data, are that McPAT only models on-chip memory controllers/channels as I/Os , while cpus are complicated SOC with different types of I/Os including memory, PCI-e, and 
Ethernet which are unknown parts McPAT doesn't model in detail. Although, these errors on both area and power considered acceptable. The use of different modeling 
programms for the evaluation of two different sides of the same system can import more errors , since one programm is based in the evaluations of the other.  And if 
the evaluations has some errors these errors are imported directly to the model of the other. 




### **_Bibliography_**
1. https://www.hpl.hp.com/research/mcpat/micro09.pdf
2. Computer Architecture Hennessy John L. , Patterson David A. (6th Edition).

### **_Review_**

In this lab we got familiar with McPAT ,a modeling tool for power of different cpu architectures. The final lab was easier than the second because does not need so much time to wait for the simulation executions. Also we were more familiar with the scripts to take the data more quickly and the graphics are more easier by using the Microsoft excel program. The questions of the first step of this lab demanded more searching in the bibliography and had more critical thinking than the other labs. Another difficult thing was the final question on the second part, because it is difficult to select the "ideal" architecture. It is a mltiple function problem with different weighs for each variable and also there is not so much bibliography on this part. So the results emerged for our experience on computer architecture. In general the lab was very educational and with the other two labs combined gave us a valid example as how modern computer architects operate and the things they take into consideration when designing a cpu.

