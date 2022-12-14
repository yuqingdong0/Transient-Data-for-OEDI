# IEEE 13 Test Model
## Contents
The data is simulated through IEEE 13 bus system with 2 PVs and 3 Battery Storages (ST) in ATP-EMTP software. The ATP model is converted from an open-source model [IEEE13_PV.xml](https://github.com/GRIDAPPSD/CIMHub/blob/feature/SETO/OEDI/xml/IEEE13_PV.xml) via [CIMHub](https://github.com/GRIDAPPSD/CIMHub/tree/feature/SETO). The data format is in **CSV** or **COMTRADE**（.cfg and .dat) file.<br>
The data file label is named after the following rules:<br>
* Loading Condition: 0.4/0.7/1.0 ➡️ l1/l2/l3<br>
* PV Capacity: 0.2/1.0 ➡️ c1/c2<br>
* Three-phase Switching Operation: t1<br>
  * Breaker Name: brkr1/fuse1/671692/sw_bat1/sw_bat2 ➡️ b1/b2/b3/b4/b5
* Single-phase (Phase A) Switching Operation: t2<br>
  * Breaker Name: sect1/sw_bat1/sw_bat3 ➡️ b1/b2/b3 

*_For detailed explanation, the switching types and the breaker locations are listed below. The bus defined in ATP model corresponds to the bus No. in XML file, which can be found in [IEEE13_PV.atpmap](https://github.com/yuqingdong0/Transient-Data-for-OEDI/blob/main/Simulation%20Data/IEEE%2013/Switching%20Operations/IEEE13_PV.atpmap)._<br>
<table style="width:100%">
  <thead>
    <tr>
      <th style="width:18%"> Phase Type </th>
      <th style="width:22%"> Switching Type </th>
      <th style="width:20%"> Breaker Name </th>
      <th style="width:30%"> Connecting Bus </th>
    </tr>
  </thead>
  <tbody align="center">
    <tr>
      <td rowspan=5> Three Phase </td>
      <td rowspan=2> Line Trip </td>
      <td>brkr1</td>
      <td>from 650 to brkr</td>
    </tr>
    <tr>
      <td>fuse1</td>
      <td>from 633 to xf1</td>
    </tr>
    <tr>
      <td rowspan=3>Load Disconnection</td>
      <td>671692</td>
      <td>from 671 to 692</td>
    </tr>
    <tr>
      <td>sw_bat1</td>
      <td>from 634 to 634_bat1</td>
    </tr>
    <tr>
      <td>sw_bat2</td>
      <td>from 634 to 634_bat2</td>
    </tr>
    <tr>
      <td rowspan=3>Single Phase</td>  
      <td rowspan=3>Load Disconnection</td>
      <td>sect1</td>
      <td>from 684 to tap</td>
    </tr>
    <tr>
      <td>sw_bat1</td>
      <td>from 634 to 634_bat1</td>
    </tr>
    <tr>
      <td>sw_bat3</td>
      <td>from house to house_bat</td>
    </tr>
  </tbody>
</table>

## For COMTRADE files only
**Python Comtrade** is a module for Python 3 designed to read *Common Format for Transient Data Exchange* (**COMTRADE**) files. Detailed information can be found [here](https://github.com/dparrini/python-comtrade). These consists of oscillography data recorded during power system outages, control systems tests, validation and tests of field equipment, protective relaying logs, etc. The COMTRADE format is defined by IEEE Standards.
### Installation

```python
pip install comtrade
```

### Plot
The example below shows how to open CFG and DAT files to plot (using `pyplot`) analog channel oscillography. The **file_name** can be modified according to the file desired.

```python
import matplotlib.pyplot as plt
from comtrade import Comtrade

file_name = "ieee13_pv_l1c1b1t1"
rec = Comtrade()
rec.load(f"{file_name}.cfg", f"{file_name}.dat")
print("Trigger time = {}s".format(rec.trigger_time))

for i in range(rec.channels_count):
    plt.plot(rec.time, rec.analog[i])
    plt.legend([rec.analog_channel_ids[i]])
    plt.show()
```


## Results
There are 21 measured variables under each corresponding scenario in one data file.
* The variables which begin with _SBUS_ are system node voltages or branch currents, following the rules:
  * suffix _V-node_ specifies the feederhead bus voltage in phase _A_/_B_/_C_
  * suffix _I-branch_ specifies the feederhead current in phase _A_/_B_/_C_
* The variables which begin with _TACS_ section delimiter are inverter outputs, following the rules:
    * _PV###_ specifies the PV number, from 1..999
    * _ST###_ specifies the battery storage number, from 1..999
    * suffix _V_ specifies the inverter's calculated RMS voltage (positive sequence in the case of three-phase); helpful to visualize the operation of undervoltage trip or ridethrough.
    * suffix _I_ specifies the inverter's RMS current injection (positive sequence in the case of three-phase); sufficient to visualize the status of this inverter. If the RMS current drops to zero, it means either a voltage or frequency trip function activated.
    * suffix _W_ specifies the inverter's estimated frequency (rad/s); helpful to diagnose grid synchronization issues.
    
The switching operation is applied at 0.3s and never cleared. The time window is from 0.0s to 0.6s. Results of **TACS PV001V/I/W I-branch** in "ieee13_pv_l1c1b1t1" are shown below as examples for three-phase line trip. The units in *Y*-axis are *Volt* (*V*) for *Voltage*, *Ampere* (*A*) for *Current* and *rad/s* for *Frequency*.<br>

<img src="https://user-images.githubusercontent.com/113486786/208350962-0aa314f6-1344-4563-929d-295a270ca72b.png" width="600" height="300">
<img src="https://user-images.githubusercontent.com/113486786/208351018-e2518327-f4e1-4f7b-b156-766edf37bf91.png" width="600" height="300">
<img src="https://user-images.githubusercontent.com/113486786/208351052-82531ab6-5863-4e98-8d24-8cc6b0568004.png" width="600" height="300">

