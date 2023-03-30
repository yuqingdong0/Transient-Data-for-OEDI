# High Impedance Fault Case
## Contents
High-impedance faults (HIFs) in general occur in electric distribution systems. HIFs occur when a conductor contacts a tree with a high-impedance or when a broken conductor touches the ground. 
In ATP, HIF is modeled using two paralleled DC batteries connected with diodes in opposite directions. For typical cases, HIF exists in one single phase, so the simulated cases
are all in phase A.

The data file label is named after the following rules:<br>
* Loading Condition: 0.4/1.0 ➡️ L1/L2<br>
* PV Capacity: 0.4/0.6/0.8/1.0 ➡️ C1/C2/C3/C4<br>
* Fault Location: ADJ1/ADJ2/ADJ3/* ➡️ B1/B2/B3/...<br>
* All the csv files end with **HIF**

*_ADJ1, ADJ2, ADJ3 are fault locations outside the feeder, approximately 250 feet, 1 mile and 1 mile away.The other fault locations are selected with all the three-phase
buses as shown below._<br>

![IEEE123](https://user-images.githubusercontent.com/113486786/228717564-b676c9ff-ce75-4367-86f6-a9953742c344.png)


## Results
There are 7 measured variables under each corresponding scenario in one data file, shown as below:

| Variable Name | Description | 
| :---: | :---: |
| Voltage SBUS A | *Feederhead bus voltage in phase A* |
| Voltage SBUS B | *Feederhead bus voltage in phase B* | 
| Voltage SBUS C | *Feederhead bus voltage in phase C* |
| Current HXX004 HXX010 | *Fault current in phase A* | 
| Current SBUS A 25 A | *Feederhead current in phase A* | 
| Current SBUS B 25 B | *Feederhead current in phase B* | 
| Current SBUS C 25 C | *Feederhead current in phase C* |
