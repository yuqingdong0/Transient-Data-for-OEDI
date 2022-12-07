# IEEE 123 Test Model
## Contents
The PMU data is converted from the Point On Wave (POW) data in [IEEE 123/COMTADE file](https://github.com/yuqingdong0/Transient-Data-for-OEDI/tree/main/Simulation%20Data/IEEE%20123/COMTRADE%20file) folder.
The PMU used in the demo is the IEEE standard P-Class PMU with a sampling rate of 960Hz, a nominal frequency of 60Hz, and a report rate of 60Hz.

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

file_name = "IEEE123_PV_L1C1B1F1_pmu"
rec = Comtrade()
rec.load(f"{file_name}.cfg", f"{file_name}.dat")
print("Trigger time = {}s".format(rec.trigger_time))

for i in range(rec.channels_count):
    plt.plot(rec.time, rec.analog[i])
    plt.legend([rec.analog_channel_ids[i]])
    plt.show()
```

## Results
There are 122 measured variables in one data file.
* The variables begin with _SBUS_ or _FAULT_ are system branch currents or node voltages, shown as below:

| Variable Name | Description | Variable Name | Description |
| :---: | :---: | :---: | :---: |
| SBUS A V-node | *Feederhead bus voltage in phase A* | SBUS C 25 C I-branch | *Feederhead current in phase C* |
| SBUS B V-node | *Feederhead bus voltage in phase B* | FAULTA I-branch | *Fault current in phase A* | 
| SBUS C V-node | *Feederhead bus voltage in phase C* | FAULTB I-branch | *Fault current in phase B* | 
| SBUS A 25 A I-branch | *Feederhead current in phase A* | FAULTC I-branch | *Fault current in phase C* | 
| SBUS B 25 B I-branch | *Feederhead current in phase B* | FALTBC 112 C I-branch | *Fault current in phase C of line to line fault* | 

* The varibales begin with _TACS_ section delimiter are inverter outputs, following the rules:
    * _PV###_ specifies the inverter number, from 1..999
    * suffix _V_ specifies the inverter's calculated RMS voltage (positive sequence in the case of three-phase);helpful to visualize the operation of undervoltage trip or ridethrough.
    * suffix _I_ specifies the inverter's RMS current injection (positive sequence in the case of three-phase);sufficient to visualize the status of this inverter. If the RMS current drops to zero, it means either a voltageor frequency trip function activated.
    * suffix _W_ specifies the inverter's estimated frequency (rad/s); helpful to diagnose grid synchronization issues.
    * suffixes _A_, _B_, _C_ specify the inverter's instantaneous phase voltages; needed to impute PMU data for this inverter.
    * suffixes _X_, _Y_, _Z_ specify the inverter's instantaneous injected currents; needed to impute PMU data for this inverter.

### 
The fault is applied at 0.3s and never cleared. The time window is from 0.0s to 0.6s. Results of the *Magnitude* and *Phase angle* of **SBUS A/B/C** are shown below as examples.

<div align=center><img src="https://user-images.githubusercontent.com/113486786/206096429-81d9650c-2006-4cca-a5cd-0614c56aa76f.png" width="400" height="200"/><div<div align=center><img src="https://user-images.githubusercontent.com/113486786/206096440-7f214fca-e46f-4f5a-a88b-e7d78b74749a.png" width="400" height="200"/></div>
<div align=center><img src="https://user-images.githubusercontent.com/113486786/206098449-872a4d29-7141-4236-bf8d-e7d0bdd1246e.png" width="400" height="200"/><div<divalign=center><img src="https://user-images.githubusercontent.com/113486786/206098464-78d4dac8-e8c3-425c-a9af-45512799fa1f.png" width="400" height="200"/></div>
<div align=center><img src="https://user-images.githubusercontent.com/113486786/206099086-8f903c36-f113-440f-8390-98289d3a4b93.png" width="400" height="200"/><div<divalign=center><img src="https://user-images.githubusercontent.com/113486786/206099134-04b2ec1b-1c45-4e3d-a82f-dcae7253350b.png" width="400" height="200"/></div>


