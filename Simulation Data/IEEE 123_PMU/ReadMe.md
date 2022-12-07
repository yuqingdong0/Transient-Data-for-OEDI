# IEEE 123 Test Model
## Contents
The PMU data is converted from the Point On Wave (POW) data in [Transient-Data-for-OEDI/Simulation Data/IEEE 123/COMTADE file](https://github.com/yuqingdong0/Transient-Data-for-OEDI/tree/main/Simulation%20Data/IEEE%20123/COMTRADE%20file) in this repository.
The results are in **float32 formatted Comtrade** files (according to Annex H) and will contain magnitude and phase angle for each channel in the corresponding POW data file, as well as positive sequence phasors, frequency, and ROCOF estimates when a three phase signal is presented. The PMU used in the demo is the IEEE standard P-Class PMU with a sampling rate of 960Hz, a nominal frequency of 60Hz, and a report rate of 60Hz.<br>
*_The CSV file format for PMU data is unavailable now._

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
The order is stored as follows: 
* Variables begin with ***SBUS***
    * specify the variables on the feederhead bus
    * _abs_(_Va_), _angle_(_Va_), _abs_(_Vb_), _angle_(_Vb_), _abs_(_Vc_), _angle_(_Vc_), _abs_(_Vp_), _angle_(_Vp_), _abs_(_Ia_), _angle_(_Ia_), _abs_(_Ib_), _angle_(_Ib_), _abs_(_Ic_), _angle_(_Ic_), _abs_(_Ip_), _angle_(_Ip_), _Frequency_, _ROCOF_
* Variables begin with ***FAULT/FALT***
    * specify the fault currents
    * _abs_(_Ia_), _angle_(_Ia_), _abs_(_Ib_), _angle_(_Ib_), _abs_(_Ic_), _angle_(_Ic_) for single-phase fault; _abs_(_Ic_), _angle_(_Ic_) for line-to-line fault
* Variables begin with ***TACS*** 
    * specify the inverter outputs
    * _PV###_ specifies the inverter number, from 1..999
    * suffixes _A_: _abs_(_Va_), _angle_(_Va_)
    * suffixes _X_: _abs_(_Ia_), _angle_(_Ia_)
    * suffixes _I_: _abs_(_Irms_), _angle_(_Irms_) for single-phase PVs; _abs_(_Ip_), _angle_(_Ip_) for three-phase PVs
    * suffixes _V_: _abs_(_Vp_), _angle_(_Vp_)
    * suffixes _W_: the inverter's estimated frequency (rad/s)


### 
The fault is applied at 0.3s and never cleared. The time window is from 0.0s to 0.6s. Results of the *Magnitude* and *Phase angle* of **SBUS A/B/C** in "IEEE123_PV_L1C1B1F1_pmu" are shown below as examples.

<div align=center><img src="https://user-images.githubusercontent.com/113486786/206096429-81d9650c-2006-4cca-a5cd-0614c56aa76f.png" width="400" height="200"/><div<div align=center><img src="https://user-images.githubusercontent.com/113486786/206096440-7f214fca-e46f-4f5a-a88b-e7d78b74749a.png" width="400" height="200"/></div>
<div align=center><img src="https://user-images.githubusercontent.com/113486786/206098449-872a4d29-7141-4236-bf8d-e7d0bdd1246e.png" width="400" height="200"/><div<divalign=center><img src="https://user-images.githubusercontent.com/113486786/206098464-78d4dac8-e8c3-425c-a9af-45512799fa1f.png" width="400" height="200"/></div>
<div align=center><img src="https://user-images.githubusercontent.com/113486786/206099086-8f903c36-f113-440f-8390-98289d3a4b93.png" width="400" height="200"/><div<divalign=center><img src="https://user-images.githubusercontent.com/113486786/206099134-04b2ec1b-1c45-4e3d-a82f-dcae7253350b.png" width="400" height="200"/></div>


