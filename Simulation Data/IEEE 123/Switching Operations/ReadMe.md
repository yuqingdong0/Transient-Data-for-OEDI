# IEEE 123 Test Model
## Contents
The data is simulated through IEEE 123 bus system with 14 PVs in ATP-EMTP software. The ATP model is converted from an open-source model [IEEE123_PV.xml](https://github.com/GRIDAPPSD/CIMHub/blob/feature/SETO/OEDI/xml/IEEE123_PV.xml) via [CIMHub](https://github.com/GRIDAPPSD/CIMHub/tree/feature/SETO). The data format is in **CSV** or **COMTRADE**（.cfg and .dat) file.<br>
The data file label is named after the following rules:<br>
* Loading Condition: 0.4/1.0 ➡️ l1/l2<br>
* PV Capacity: 0.4/0.6/0.8/1.0 ➡️ c1/c2/c3/c4<br>
* Breaker Name: sw1/sw2/sw3/sw4/sw5 ➡️ b1/b2/b3/b4/b5
* Switching Phase: three-phase/single-phase(phase A) ➡️ t1/t2<br>

*_For detailed explanation, the switching types and the breaker locations are listed below. The bus defined in ATP model corresponds to the bus No. in XML file, which can be found in [IEEE123_PV.atpmap](https://github.com/yuqingdong0/Transient-Data-for-OEDI/blob/main/Simulation%20Data/IEEE%20123/Switching%20Operations/IEEE123_PV.atpmap)_.<br>
<table style="width:100%">
    <thead>
        <tr>
            <th style="width:40%">Breaker Name</th>
            <th style="width:50%">Connecting Bus</th>
            <th style="width:10%">Switching Type</th>
        </tr>
    </thead>
    <tbody align="center">
        <tr>
            <td>sw1</td>
            <td>from 150r to 149</td>
            <td rowspan=5>Three-phase/Single-phase<br/> load disconnection</td>
        </tr>
        <tr>
            <td>sw2</td>
            <td>from 13 to 152</td>
        </tr>
        <tr>
            <td>sw3</td>
            <td>from 18 to 135</td>
        </tr>
        <tr>
            <td>sw4</td>
            <td>from 60 to 160</td>
        </tr>
        <tr>
            <td>sw5</td>
            <td>from 97 to 197</td>
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

file_name = "ieee123_pv_l1c1b1t1"
rec = Comtrade()
rec.load(f"{file_name}.cfg", f"{file_name}.dat")
print("Trigger time = {}s".format(rec.trigger_time))

for i in range(rec.channels_count):
    plt.plot(rec.time, rec.analog[i])
    plt.legend([rec.analog_channel_ids[i]])
    plt.show()
```

## Results
There are 54 measured variables under each corresponding scenario in one data file. 
* The variables which begin with _SBUS_ are system node voltages or branch currents, following the rules:
  * suffix _V-node_ specifies the feederhead bus voltage in phase _A_/_B_/_C_
  * suffix _I-branch_ specifies the feederhead current in phase _A_/_B_/_C_
* The variables which begin with _TACS_ section delimiter are inverter outputs, following the rules:
    * _PV###_ specifies the inverter number, from 1..999
    * suffix _V_ specifies the inverter's calculated RMS voltage (positive sequence in the case of three-phase); helpful to visualize the operation of undervoltage trip or ridethrough.
    * suffix _I_ specifies the inverter's RMS current injection (positive sequence in the case of three-phase); sufficient to visualize the status of this inverter. If the RMS current drops to zero, it means either a voltage or frequency trip function activated.
    * suffix _W_ specifies the inverter's estimated frequency (rad/s); helpful to diagnose grid synchronization issues.
    * suffixes _A_, _B_, _C_ specify the inverter's instantaneous phase voltages; needed to impute PMU data for this inverter.
    * suffixes _X_, _Y_, _Z_ specify the inverter's instantaneous injected currents; needed to impute PMU data for this inverter.


The switching operation is applied at 0.3s and never cleared. The time window is from 0.0s to 0.6s. Results of **TACS PV001A/X/I I-branch** in "ieee123_pv_l1c1b1t1" are shown below as examples for three-phase load disconnection. The units in *Y*-axis are *Volt* (*V*) for *Voltage* and *Ampere* (*A*) for *Current*.<br>

<img src="https://user-images.githubusercontent.com/113486786/208825883-080af374-9399-437a-8c03-d33e2f883553.png" width="600" height="300">
<img src="https://user-images.githubusercontent.com/113486786/208825941-b2f3f8dc-8cef-4fd6-95a5-ec988bfb4ecb.png" width="600" height="300">
<img src="https://user-images.githubusercontent.com/113486786/208826009-024f90ff-6ce2-4b9a-8ceb-75234c9ea3c2.png" width="600" height="300">
