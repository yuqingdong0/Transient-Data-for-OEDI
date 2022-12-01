# IEEE 123 Test Model
The data is simulated through IEEE 123 bus system with 14 PVs in ATP-EMTP software. The data format is in COMTRADE file（.cfg and .dat). The fault is applied at 0.3s and never cleared. The time window is from 0.28s to 0.34s.<br>
The data file label is named after the following rules:<br>
* Loading Condition: 0.4/1.0 --> l1/l2<br>
* PV Capacity: 0.4/0.6/0.8/1.0 --> c1/c2/c3/c4<br>
* Fault Location: ADJ1/197/450/82 --> b1/b2/b3/b4<br>
* Fault Type: Three-phase/Single-phase/Line-to-line phase fault --> f1/f2/f3<br>

*ADJ1 is 250 feet awy from the feeder.<br>

The measured variables in each data file include: Feeder-head voltage / Feeder-head current / RMS current at three selected PV buses under the corresponding scenario.

![image](https://user-images.githubusercontent.com/113486786/205101071-91c863a6-51fc-4c40-a5e6-274aad9b0a14.png)
