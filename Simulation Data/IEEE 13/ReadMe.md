# IEEE 13 Test Model
## Contents
The data is simulated through IEEE 13 bus system with 2 PVs and 3 Battery Storages (ST) in ATP-EMTP software. The ATP model is converted from an open-source model [IEEE13_PV.xml](https://github.com/GRIDAPPSD/CIMHub/blob/feature/SETO/OEDI/xml/IEEE13_PV.xml) via [CIMHub](https://github.com/GRIDAPPSD/CIMHub/tree/feature/SETO). The data format is in **CSV** or **COMTRADE**（.cfg and .dat) file.<br>
The data file label is named after the following rules:<br>
* Loading Condition: 0.4/0.7/1.0 ➡️ l1/l2/l3<br>
* PV Capacity: 0.2/1.0 ➡️ c1/c2<br>
* Fault Location: ADJ1/634_bat1/xf1 ➡️ b1/b2/b3<br>
* Fault Type: Three-phase/Single-phase/Line-to-line phase fault ➡️ f1/f2/f3<br>

*_The fault location defined in ATP model corresponds to the bus No. in XML file, which can be found in [IEEE13_PV.atpmap](https://github.com/yuqingdong0/Transient-Data-for-OEDI/blob/main/Simulation%20Data/IEEE%20123/IEEE123_PV.atpmap). In addition, ADJ1 is a fault location outside the feeder, approximately 250 feet away._<br>

## For COMTRADE files only
**Python Comtrade** is a module for Python 3 designed to read *Common Format for Transient Data Exchange* (**COMTRADE**) files. Deailed information can be found [here](https://github.com/dparrini/python-comtrade). These consists of oscillography data recorded during power system outages, control systems tests, validation and tests of field equipment, protective relaying logs, etc. The COMTRADE format is defined by IEEE Standards.
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


## Results
There are 24 measured variables under the corresponding scenario in each data file, as shown in the table below. 

| Variable Name | Description | Variable Name | Description |
| :---: | :---: | :---: | :---: |
| SBUS A V-node | *Feederhead bus voltage in phase A* | TACS PV002V I-branch | *RMS voltage at PV2* |
| SBUS B V-node | *Feederhead bus voltage in phase B* | TACS PV002I I-branch | *RMS current at PV2* |
| SBUS C V-node | *Feederhead bus voltage in phase C* | TACS PV002W I-branch | *Frequency* (*rad/s*) *at PV2* |
| SBUS A 22 A I-branch | *Feederhead current in phase A* | TACS ST001V I-branch | *RMS voltage at ST1* |
| SBUS B 22 B I-branch | *Feederhead current in phase B* | TACS ST001I I-branch | *RMS current at ST1* |
| SBUS C 22 C I-branch | *Feederhead current in phase C* | TACS ST001W I-branch | *Frequency* (*rad/s*) *at ST1* |
| FAULTA I-branch | *Fault current in phase A* | TACS ST002V I-branch | *RMS voltage at ST2* |
| FAULTB I-branch | *Fault current in phase B* | TACS ST002I I-branch | *RMS current at ST2* |
| FAULTC I-branch | *Fault current in phase C* | TACS ST002W I-branch | *Frequency* (*rad/s*) *at ST2* |
| TACS PV001V I-branch | *RMS voltage at PV1* | TACS ST003V I-branch | *RMS voltage at ST3* |
| TACS PV001V I-branch | *RMS current at PV1* | TACS ST003I I-branch | *RMS current at ST3* |
| TACS PV001V I-branch | *Frequency* (*rad/s*) *at PV1* | TACS ST003W I-branch | *Frequency* (*rad/s*) *at ST3* |


The fault is applied at 0.3s and never cleared. The time window is from 0.0s to 0.6s. Results of **SBUS A/B/C** in "ieee13_pv_l1c1b1f1" are shown as examples. The units in *Y*-axis are *Volt* (*V*).<br>

<div align=center><img src="https://user-images.githubusercontent.com/113486786/205723319-c76e8ecc-ee7c-44fd-981e-7211c38f63d4.png" width="600" height="300">
<div align=center><img src="https://user-images.githubusercontent.com/113486786/205723329-df133944-33df-404c-8e9b-4c7bc3ce1bd1.png" width="600" height="300">
<div align=center><img src="https://user-images.githubusercontent.com/113486786/205723340-6f88c022-e19e-40be-9b6c-9de2e432cc1c.png" width="600" height="300">
