# **AID Formats**

## **XTC-2 and XTC-2E Equipment AIDs**
### **XTC-2 and XTC-2E Equipment AIDs (Shelf A)**

| Equipment Type | Equipment Description| AID Format | Valid Values |
|------|-------------------------------|---------------------------------------------------|-------------|
|Chassis|Chassis of the network element. |`<chassis>`|`<chassis>= {1-254}` |
|Shelf|Shelf on the chassis. |`<chassis>-<shelf>`|<ul><li>`<chassis> = {1-254}`</li> <li>`<shelf> = {A}` </li></ul>|
|XCMH|XTN Control Module. |`<chassis>-<shelf>-<slot>`|<ul><li>`<chassis> = {1-254}`</li> <li>`<shelf> = {A}` </li><li>`<slot> = {9A, 9B}` </li></ul>|
|OTXM|OTN Tributary Switching Module. |`<chassis>-<shelf>-<slot>`|<ul><li>`<chassis> = {1-254}`</li> <li>`<shelf> = {A}` </li><li>`<slot> = {1, 5}` </li></ul>|
|OTSM|OTN Tributary Static  Module. |`<chassis>-<shelf>-<slot>`|<ul><li>`<chassis> = {1-254}`</li> <li>`<shelf> = {A}` </li><li>`<slot> = {1, 5}` </li></ul>|
|TIM|Tributary Interface Module. |`<chassis>-<shelf>-<slot>-<sub-slot>`|<ul><li>`<chassis> = {1-254}`</li> <li>`<shelf> = {A}` </li><li>`<slot> = {1, 5}` </li><li>`<sub-slot> = {1-12} for TIM-5-10GM/10GX,TIM-5B-10GM` </li><li>`<sub-slot> = {1,3,5,7,9} for LIM-1-100GE/100GX, TIM-1-100G/100GE/100GM/100GX,TIM-1B-100GE, TIM-16-2.5GM` </li><li>`<sub-slot> = {3, 9} for PXM-16-10GE, PXM-1-100GE` </li></ul>|
|LIM|Line Interface Module. |`<chassis>-<shelf>-<slot>-<sub-slot>`|<ul><li>`<chassis> = {1-254}`</li> <li>`<shelf> = {A}` </li><li>`<slot> = {1, 5}` </li><li>`<sub-slot> = {1-12} for TIM-5-10GM/10GX,TIM-5B-10GM` </li><li>`<sub-slot> = {1,3,5,7,9} for LIM-1-100GE/100GX, TIM-1-100G/100GE/100GM/100GX,TIM-1B-100GE, TIM-16-2.5GM` </li><li>`<sub-slot> = {3, 9} for PXM-16-10GE, PXM-1-100GE` </li></ul>|
|OFX|Advanced OTN FlexChannel Switching Line Module (AOFX-100). |`<chassis>-<shelf>-<slot>-<sub-slot>`|<ul><li>`<chassis> = {1-254}`</li> <li>`<shelf> = {A}` </li><li>`<slot> = {9A, 9B}` </li><li>`<sub-slot> = {1, 3, 5, 7, 9, 11}` </li></ul>|
|OFM|Advanced OTN FlexChannel Line Module (AOFM-100). |`<chassis>-<shelf>-<slot>-<sub-slot>`|<ul><li>`<chassis> = {1-254}`</li> <li>`<shelf> = {A}` </li><li>`<slot> = {9A, 9B}` </li><li>`<sub-slot> = {1, 3, 5, 7, 9, 11}` </li></ul>|
### **XTC-2E Equipment AIDs (Shelf B)**
| Equipment Type | Equipment Description| AID Format | Valid Values |
|------|-------------------------------|---------------------------------------------------|-------------|
|Chassis|Chassis of the network element. |`<chassis>`|`<chassis>= {1-254}` |
|Shelf|Shelf on the chassis. |`<chassis>-<shelf>`|<ul><li>`<chassis> = {1-254}`</li> <li>`<shelf> = {A}` </li></ul>|
|FAN|Fan tray. |`<chassis>-FANSHELF-<shelf>-<fantray>`|<ul><li>`<chassis> = {1-254}`</li> <li>`<shelf> = {B}` </li><li>`<fantray> = {1}` </li></ul>|
|PEM|Power Entry Module. |`<chassis>-PEMSHELF-<shelf>-<PEM>`|<ul><li>`<chassis> = {1-254}`</li> <li>`<shelf> = {B}` </li><li>`<PEM> = {1-2}` </li></ul>|
|FRM-4D|FlexROADM Module. |`<chassis>-<shelf>-<slot>`|<ul><li>`<chassis> = {1-254}`</li> <li>`<shelf> = {B}` </li><li>`<slot> = {1-5}` </li></ul>|
|FMM-C-5 |FlexROADM Multiplexing Module. |`<chassis>-<shelf>-<slot>`|<ul><li>`<chassis> = {1-254}`</li> <li>`<shelf> = {B}` </li><li>`<slot> = {1-6}` </li></ul>|
|FMM-C-12 |FlexROADM Multiplexing Module ( FMM-C-12-EC-TR ). |`<chassis>-<shelf>-<slot>`|<ul><li>`<chassis> = {1-254}`</li> <li>`<shelf> = {B}` </li><li>`<slot> = {1-6}` </li></ul>|
|IAM-2 |ILS Amplifier Module (IAM-2). |`<chassis>-<shelf>-<slot>`|<ul><li>`<chassis> = {1-254}`</li> <li>`<shelf> = {B}` </li><li>`<slot> = {1-5}` </li></ul>|
|OTDM |Optical Time Domain Reflectometer Module. |`<chassis>-<shelf>-<slot>`|<ul><li>`<chassis> = {1-254}`</li> <li>`<shelf> = {B}` </li><li>`<slot> = {1-6}` </li></ul>|
---
**NOTE**

For Infinera nodes, the possible value range for Chassis is {1-254}. However, the maximum number of chassis supported in a multi-chassis node is less than 254. See the Release Notes document for each product release for the multi-chassis limitations in that release.
