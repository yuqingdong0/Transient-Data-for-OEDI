# IEEE 13 Test Model
## Contents
The data is simulated through IEEE 13 bus system with 1 PV in ATP-EMTP software. The ATP model is converted from an open-source model [IEEE13_PV.xml](https://github.com/GRIDAPPSD/CIMHub/blob/feature/SETO/OEDI/xml/IEEE13_PV.xml) via [CIMHub](https://github.com/GRIDAPPSD/CIMHub/tree/feature/SETO). The data format is in COMTRADE file.<br>
The data file label is named after the following rules:<br>
* Loading Condition: 0.4/0.7/1.0 --> l1/l2/l3<br>
* PV Capacity: 0.2/1.0 --> c1/c2<br>
* Fault Location: ADJ1/634_bat1/xf1 --> b1/b2/b3<br>
* Fault Type: Three-phase/Single-phase/Line-to-line phase fault --> f1/f2/f3<br>

*ADJ1 is 250 feet awy from the feeder.<br>

## Results
The measured variables in each data file include: Feeder-head voltage/Feeder-head current/RMS voltage at the PV bus/RMS current at the PV bus/angle speed at the PV bus under the corresponding scenario. The fault is applied at 0.3s and never cleared. The time window is from 0.28s to 0.34s.<br>

![image](https://user-images.githubusercontent.com/113486786/205100327-bf760968-2ea3-4d1a-a98d-8b5e865bf8f9.png)
