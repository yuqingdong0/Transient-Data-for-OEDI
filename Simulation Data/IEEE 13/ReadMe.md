# IEEE 13 Test Model
## Contents
The data is simulated through IEEE 13 bus system with 2 PVs in ATP-EMTP software. The ATP model is converted from an open-source model [IEEE13_PV.xml](https://github.com/GRIDAPPSD/CIMHub/blob/feature/SETO/OEDI/xml/IEEE13_PV.xml) via [CIMHub](https://github.com/GRIDAPPSD/CIMHub/tree/feature/SETO). The data format is in COMTRADE file.<br>
The data file label is named after the following rules:<br>
* Loading Condition: 0.4/0.7/1.0 --> l1/l2/l3<br>
* PV Capacity: 0.2/1.0 --> c1/c2<br>
* Fault Location: ADJ1/634_bat1/xf1 --> b1/b2/b3<br>
* Fault Type: Three-phase/Single-phase/Line-to-line phase fault --> f1/f2/f3<br>

*_ADJ1 is 250 feet awy from the feeder_.<br>

## How to Use
**Python Comtrade** is a module for Python 3 designed to read *Common Format for Transient Data Exchange* (COMTRADE) files. Deailed information can be found [here](https://github.com/dparrini/python-comtrade).
### Installation

```python
pip install comtrade
```

### Plot
The example below shows how to open CFG and DAT files to plot (using `pyplot`) analog channel oscillography. The **file_name** can be modified according to the file desired.

```python
import matplotlib.pyplot as plt
from comtrade import Comtrade

file_name = "ieee13_pv_l1c1b1f1"
rec = Comtrade()
rec.load(f"{file_name}.cfg", f"{file_name}.dat")
print("Trigger time = {}s".format(rec.trigger_time))

for i in range(rec.channels_count):
    plt.plot(rec.time, rec.analog[i])
    plt.legend([rec.analog_channel_ids[i]])
    plt.show()
```


### Results
The measured variables under the corresponding scenario in each data file are shown in the table below. 

| Variable | Description |
| --- | --- |
| SBUS A | Feederhead instantaneous voltage in phase A |
| SBUS B| Feederhead instantaneous voltage in phase B |
| SBUS C| Feederhead instantaneous voltage in phase C |
| SBUS A 22 A I-branch| Bus 22 instantaneous voltage in phase A |
| SBUS B 22 B I-branch| Bus 22 instantaneous voltage in phase B |
| SBUS C 22 C I-branch| Bus 22 instantaneous voltage in phase C |
| FAULTA I-branch| Fault current in phase A |
| FAULTB I-branch| Fault current in phase B |
| FAULTC I-branch| Fault current in phase C |
| TACS PV001V I-branch| RMS voltage at PV1 |
| TACS PV001I I-branch| RMS current at PV1 |
| TACS PV001W I-branch| Angle speed at PV1 |
| TACS PV002V I-branch| RMS voltage at PV2 |
| TACS PV002I I-branch| RMS current at PV2 |
| TACS PV002W I-branch| Show file differences that haven't been staged |
| TACS ST001V I-branch| Show file differences that haven't been staged |
| TACS ST001I I-branch| Show file differences that haven't been staged |
| TACS ST001W I-branch| Show file differences that haven't been staged |
| TACS ST002V I-branch| Show file differences that haven't been staged |
| TACS ST002I I-branch| Show file differences that haven't been staged |
| TACS ST002W I-branch| Show file differences that haven't been staged |
| TACS ST003V I-branch| Show file differences that haven't been staged |
| TACS ST003I I-branch| Show file differences that haven't been staged |
| TACS ST003W I-branch| Show file differences that haven't been staged |


The fault is applied at 0.3s and never cleared. The time window is from 0.0s to 0.6s.<br>

![image](https://user-images.githubusercontent.com/113486786/205100327-bf760968-2ea3-4d1a-a98d-8b5e865bf8f9.png)
