# IEEE LV_Network Model
## Contents
The data is simulated through IEEE LV_Network system with 8 PVs in ATP-EMTP software. The ATP model is converted from an open-source model [IEEE390_PV.xml](https://github.com/GRIDAPPSD/CIMHub/blob/feature/SETO/OEDI/xml/IEEE390_PV.xml) via [CIMHub](https://github.com/GRIDAPPSD/CIMHub/tree/feature/SETO). The data format is in **CSV** or **COMTRADE**（.cfg and .dat) file.<br>
The data file label is named after the following rules:<br>
* Loading Condition: 0.4/0.7/1.0 ➡️ l1/l2/l3<br>
* PV Capacity: 0.6/0.8/1.0 ➡️ c1/c2/c3<br>
* Fault Location: ADJ1/p6/s128 ➡️ b1/b2/b3<br>
* Fault Type: Three-phase/Single-phase/Line-to-line phase fault ➡️ f1/f2/f3<br>

*_The fault location defined in ATP model corresponds to the bus No. in XML file, which can be found in [LV_Network.atpmap](https://github.com/yuqingdong0/Transient-Data-for-OEDI/blob/main/Simulation%20Data/IEEE%20LVN/LV_Network.atpmap). In addition, ADJ1 is a fault location outside the feeder, approximately 250 feet away._<br>

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

file_name = "ieee123_pv_l1c1b1f1"
rec = Comtrade()
rec.load(f"{file_name}.cfg", f"{file_name}.dat")
print("Trigger time = {}s".format(rec.trigger_time))

for i in range(rec.channels_count):
    plt.plot(rec.time, rec.analog[i])
    plt.legend([rec.analog_channel_ids[i]])
    plt.show()
```

## Results
There are 33 measured variables under each corresponding scenario in one data file.
* The variables which begin with _SBUS_ are system node voltages or branch currents, following the rules:
  * suffix _V-node_ specifies the feederhead bus voltage in phase _A_/_B_/_C_
  * suffix _I-branch_ specifies the feederhead current in phase _A_/_B_/_C_
* The variables which begin with _FAULT_ are fault current in phase _A_/_B_/_C_
* The varibales which begin with _TACS_ section delimiter are inverter outputs, following the rules:
    * _PV###_ specifies the inverter number, from 1..999
    * suffix _V_ specifies the inverter's calculated RMS voltage (positive sequence in the case of three-phase); helpful to visualize the operation of undervoltage trip or ridethrough.
    * suffix _I_ specifies the inverter's RMS current injection (positive sequence in the case of three-phase); sufficient to visualize the status of this inverter. If the RMS current drops to zero, it means either a voltage or frequency trip function activated.
    * suffix _W_ specifies the inverter's estimated frequency (rad/s); helpful to diagnose grid synchronization issues.
   
The fault is applied at 0.3s and never cleared. The time window is from 0.0s to 0.6s. Results of **TACS PV001V/I/W I-branch** in "lv_network_l1c1b1f1" are shown below as examples. The units in *Y*-axis are *Ampere* (*A*) for.<br>

<div align=center><img src="https://user-images.githubusercontent.com/113486786/206629389-5fcbe976-8d16-4c90-9f54-bb571649b32a.png" width="600" height="300">
<div align=center><img src="https://user-images.githubusercontent.com/113486786/206629522-227ea90e-6c7d-4b95-968c-7f8178aff833.png" width="600" height="300">
<div align=center><img src="https://user-images.githubusercontent.com/113486786/206629581-a1f89cbe-74c9-4eb3-a950-86a321bff45f.png" width="600" height="300">
    
