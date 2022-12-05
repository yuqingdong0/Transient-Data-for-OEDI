# IEEE 13 Test Model
## Contents
The data is simulated through IEEE 13 bus system with 2 PVs and 3 Battery Storages in ATP-EMTP software. The ATP model is converted from an open-source model [IEEE13_PV.xml](https://github.com/GRIDAPPSD/CIMHub/blob/feature/SETO/OEDI/xml/IEEE13_PV.xml) via [CIMHub](https://github.com/GRIDAPPSD/CIMHub/tree/feature/SETO). The data format is in COMTRADE file.<br>
The data file label is named after the following rules:<br>
* Loading Condition: 0.4/0.7/1.0 ➡️ l1/l2/l3<br>
* PV Capacity: 0.2/1.0 ➡️ c1/c2<br>
* Fault Location: ADJ1/634_bat1/xf1 ➡️ b1/b2/b3<br>
* Fault Type: Three-phase/Single-phase/Line-to-line phase fault ➡️ f1/f2/f3<br>

*_ADJ1 is 250 feet away from the feeder_.<br>

## How to Use
**Python Comtrade** is a module for Python 3 designed to read *Common Format for Transient Data Exchange* (COMTRADE) files. Deailed information can be found [here](https://github.com/dparrini/python-comtrade). These consists of oscillography data recorded during power system outages, control systems tests, validation and tests of field equipment, protective relaying logs, etc. The COMTRADE format is defined by IEEE Standards.
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

| Variable | Description | Variable | Description |
| --- | --- | --- | --- |
| SBUS A | *Feederhead instantaneous voltage in phase A* | TACS PV001V I-branch | *RMS voltage at PV1* |
| SBUS B | *Feederhead instantaneous voltage in phase B* | TACS PV001I I-branch | *RMS current at PV1* |
| SBUS C | *Feederhead instantaneous voltage in phase C* | TACS PV001W I-branch | *Angle speed at PV1* |
| SBUS A 22 A I-branch | *Bus 22 instantaneous voltage in phase A* | TACS PV002V I-branch | *RMS voltage at PV2* |
| SBUS B 22 B I-branch | *Bus 22 instantaneous voltage in phase B* | TACS PV002I I-branch | *RMS current at PV2* |
| SBUS C 22 C I-branch | *Bus 22 instantaneous voltage in phase C* | TACS PV002W I-branch | *Angle speed at PV2* |
| FAULTA I-branch | *Fault current in phase A* | TACS ST001V I-branch | *RMS voltage at ST1* |
| FAULTB I-branch | *Fault current in phase B* | TACS ST001I I-branch | *RMS current at ST1* |
| FAULTC I-branch | *Fault current in phase C* | TACS ST001W I-branch | *Angle speed at ST1* |
| TACS ST002V I-branch | *RMS voltage at ST2* | TACS ST003V I-branch | *RMS voltage at ST3* |
| TACS ST002I I-branch | *RMS current at ST2* | TACS ST003I I-branch | *RMS current at ST3* |
| TACS ST002W I-branch | *Angle speed at ST2* | TACS ST003W I-branch | *Angle speed at ST3* |


The fault is applied at 0.3s and never cleared. The time window is from 0.0s to 0.6s. Results of **SBUB A/B/C** are shown below.<br>
![file_name_1](https://user-images.githubusercontent.com/113486786/205723319-c76e8ecc-ee7c-44fd-981e-7211c38f63d4.png)
![file_name_2](https://user-images.githubusercontent.com/113486786/205723329-df133944-33df-404c-8e9b-4c7bc3ce1bd1.png)
![file_name_3](https://user-images.githubusercontent.com/113486786/205723340-6f88c022-e19e-40be-9b6c-9de2e432cc1c.png)


