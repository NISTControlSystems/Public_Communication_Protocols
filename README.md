# Public Communication Protocols
This repo shall contain the communication protocols to communicate with some products of NIST Control Systems as listed below.

## Motorscope Related Protocols

### Remote Monitoring Protocols
As of June 2025 our new FREE [Remote Monitoring portal](https://nistcontrol.com/my-devices) is live. Through the portal you may monitor and control all of your linked Motorscope devices and even do a remote event log download from the device in the field.  This portal is proving to be incredibly useful for installers with remote sites as well as end-users who wants to know more about the status of their pumps.  The system also offers paid optional services like push notifications to alert the user of specific (self configured) status changes with configurable notification sounds. Purchases are done on the web at the management page of each specific device page and push notifications are handled by the NCS Toolbox app installed on the user's mobile device.

#### ModBus over TCP/IP
##### Scope
This protocol is activated by default and serves as a means of communication (monitoring and control) over local networks. 
**NOTE:** The device IP is default dynamic and should be changed to static via the NCS Toolbox mobile app and your mobile device's BLE connection for best local stability.

##### Related Products
- All Motorscope [three phase](https://nistcontrol.com/product-category/three-phase/) motor protection relays
- All Motorscope [single phase](https://nistcontrol.com/product-category/single-phase/) motor protection relays

**NOTE:** Since May of 2025 it is all of the above listed products.  Before that, it was only the [Supra-R3](https://www.nistcontrol.com/product/supra-r3-motorscope/).

##### Protocol Doc
Please see the [public document](https://docs.google.com/spreadsheets/d/1sIbtdHM-CrX7nf4pJT6-C6Qk-nNGZ0I_L-UR9Xv2ENg).

#### Secure MQTT over TCP/IP
At present, the built-in MQTT communication protocol is used exclusively as a direct secure encrypted connection to our server and therefore the protocol is proprietary.  In future there will be a comms forwarding service launched which will be managed from within your online remote monitoring account.  Due to the bandwidth requirements for this, the comms forwarding service will (just like the push-notifications) be a paid service.  Please contact us if you would like to add your remote monitored devices to our present beta proof of concept comms forwarding service.

##### Related Products
- All Motorscope [three phase](https://nistcontrol.com/product-category/three-phase/) motor protection relays
- All Motorscope [single phase](https://nistcontrol.com/product-category/single-phase/) motor protection relays

**NOTE:** Since May of 2025 it is all of the above listed products.  Before that, it was only the [Supra-R3](https://www.nistcontrol.com/product/supra-r3-motorscope/).

##### Protocol Info
The comms forwarding device data packets will consist of the following key value pairs in a JSON-string:
| KEY | Description |
| --- | --- |
| "timestamp" | my Node-Red server's timestamp of the data |
| "msgType" | "ddata" = device data. alternatively there is also a "logdata" which is the event log data which is sent from the device in response to the download command. |
| "devID" | The device ID |
| "type" | our product type. All Motorscopes currently connected to server will be the MS4 type. Later there will be SLR and IOT etc. |
| "model" | a string describing the firmware variant |
| "fwVer" | the firmware version string |
| "recID" | the present event record ID. Each event (start / stop / power-up etc) increments this number and thus it could be used to see how many total events occurred in the lifespan of the unit.  Max is uint32 max. This could also be helpful if say the unit is in an auto recovery status and goes offline due to bad internet connection or something and then it does an auto-resume after the error was cleared an cycle repeats a couple of times while offline. Then comes online when status is again in the same value as previously.  User could then be made aware of it that while the device was offline, the status did change and the unit is thus not stuck in the same state forever. |
| "s" | This nested-object contains the STATUS related data <table><tr><td>KEY</td><td>Description</td></tr><tr><td>"b"</td><td>Bluetooth Connectable: true/false. When false, it means that someone is on site presently connected to the device via BLE.  Could be a nice to have to log when this status changes to take note of when someone on site connected with the device via BLE and possibly busy making changes to the protection limits.</td></tr><tr><td>"t"</td><td>Status Text</td></tr><tr><td>"c"</td><td>background colour of the status div on the mobile and web app UI when the device is in this status</td></tr><tr><td>"f"</td><td>text colour in the status div on the mobile and web app UI when the device is in this status</td></tr><tr><td>"tr"</td><td>newer firmware versions have the time remaining in status parameter added. Time remaining in status.  This is a string with more info on how long the device is expected to stay the present status</td></tr></table> |
| "auxT" | Aux Terminal status: 0= Inactive. 1= Active. |
| "auxS" | Sequencer/Scheduler Virtual Aux status. 0= inactive, 1= Active |
| "auxR" | Remote Virtual Aux status: 0=inactive, 1=active |
| "lastCmd" | a sub-object related to the last command sent to the device. <table><tr><td>KEY</td><td>DESCRIPTION</td></tr><tr><td>"ts"</td><td>timestamp of command sent to the device</td></tr><tr><td>"cmd"</td><td>Command String sent to the device</td></tr></table> |
| "pwrHL" | The POWER high Limit string. Value concatenated with the appropriate unit (W, kW, MW) |
| "pwrT" | The POWER last measured. Value concatenated with the appropriate unit (W, kW, MW) |
| "pwrLL" | The POWER low Limit. Value concatenated with the appropriate unit (W, kW, MW) |
| "vHL" | The VOLTAGE high Limit [unit = Vrms (phase to Neutral)] |
| "v1" | The measured VOLTAGE [unit = Vrms (phase to Neutral)] |
| "v2" and "v3" | These are also added here if the unit is a three-phase model. For single phase models, these values are omitted as it is N/A |
| "vLL" | The VOLTAGE low Limit [unit = Vrms (phase to Neutral)] |
| "iHL" | The CURRENT high Limit [unit = Arms] |
| "i1" | The measured CURRENT [unit = Arms] |
| "i2" and "i3" | ---- note that three phase models does not include i2 and i3 values. These values are thus not implemented at present. |
| "elHL" | ---- some models include earth leakage measurements and trips. Only included for those models. Earth Leakage trip level (higher than this would cause a trip) [unit = milli-Amps] |
| "elM" |  ---- some models include earth leakage measurements and trips. Only included for those models. Measured Earth Leakage current [unit = milli-Amps] |
| "phiv1i1" | The measured PHASE ANGLE [unit = degrees] |
| "phiviLL" | ---- note that some models does not implement minimum phase angle limits, and thus omits it in the dataset. If the model does monitor and react to minimum phase angle limit, this parameter will be included with [unit = degrees] |
| "auxTcfg" | The config of the Aux Terminal active state: "OPENED" or "CLOSED" |
| "motTRT" | Motor total run time since firmware was loaded at our OEM factory. This parameter is displayed as either a Seconds number (like: "motTRT":219260) OR as a string like: "motTRT":"60h54m20s". |

### Direct UART TTL communication
#### Related Products
- All Motorscope [three phase](https://nistcontrol.com/product-category/three-phase/) motor protection relays
- All Motorscope [single phase](https://nistcontrol.com/product-category/single-phase/) motor protection relays

#### Protocol Doc
- [NIST_StatusOnly_Serial_Protocol.pdf](https://github.com/NISTControlSystems/Public_Communication_Protocols/blob/main/Docs/Motorscope/UART/NIST_StatusOnly_Serial_Protocol.pdf)  This file describes the protocol used on the 4 pin TTL UART port, situated next to the three status LEDs of all Motorscope products.

#### 4 pin TTL UART pinout
![image](https://github.com/NISTControlSystems/Public_Communication_Protocols/assets/40263983/377b86b9-1e38-45a0-ba14-de1eafeb5210)

**NOTES:**
- Depending on the model, V+ could be anything between 5VDC and 18VDC.
- V+ is a low-current output (do not draw more than 50mA)
- RX (data into the Motorscope) is 5V tolerant.
- Depending on the model, TX (data out of the Motorscope) may be around 5V or 3.3V when HIGH.

#### Default UART Communication Settings
<table>
  <tr>
    <th>Baudrate</th>
    <td>9600</td>
  </tr>
  <tr>
    <th>Data bits</th>
    <td>8</td>
  </tr>
  <tr>
    <th>Stop bits</th>
    <td>1</td>
  </tr>
  <tr>
    <th>Parity</th>
    <td>None</td>
  </tr>
  <tr>
    <th>Flow Control</th>
    <td>None</td>
  </tr>
</table>





### BLE Communication
#### Related Products
All Motorscope [single phase](https://nistcontrol.com/product-category/single-phase/) and [three phase](https://nistcontrol.com/product-category/three-phase/) motor protection relays which has the [Mobi Board](https://nistcontrol.com/product/mobi-board/) (TTL UART to BLE converter board) plugged into it.
#### Protocol Docs
- [Display_Values_Calculations_From_BLE_Characteristics.pdf](https://github.com/NISTControlSystems/Public_Communication_Protocols/blob/main/Docs/Motorscope/BLE/Display_Values_Calculations_From_BLE_Characteristics.pdf) This file describes how to use the values contained in the xml files of BLE services and characteristics to convert the raw bytes to meaningful information.
- [MS_Status_PresentStatus-Spec.xlsx](https://github.com/NISTControlSystems/Public_Communication_Protocols/blob/main/Docs/Motorscope/BLE/MS_Status_PresentStatus-Spec.xlsx)  This file illustrates how to interpret the data in [PresentStatus](https://github.com/NISTControlSystems/Public_Communication_Protocols/blob/main/Docs/Motorscope/BLE/Characteristics/com.nistcontrol.characteristic.present_status.xml) (BLE Characteristic), which is in [MS_Status](https://github.com/NISTControlSystems/Public_Communication_Protocols/blob/main/Docs/Motorscope/BLE/Services/com.nistcontrol.service.ms_status.xml) (BLE Service).

## IO Transceiver Related Protocol
### Related Product
The [IO Transceiver](https://nistcontrol.com/product/io-transceiver/) (a LoRa peer-to-peer TDMA MESH RF-transceiver with programmable output control logic)
### Protocol Docs
-  [IOTransceiverBLEDebugHelper_Public](https://docs.google.com/spreadsheets/d/1ERCwcc7Mkw_FeGg6xAvlH5duZCD3stSa8VJfmg2aX0w/edit?usp=sharing) helps with the decoding of frames from certian [BLE Services](https://github.com/NISTControlSystems/Public_Communication_Protocols/tree/main/Docs/IOTransceiver/BLE/Services) and [BLE Characteristics](https://github.com/NISTControlSystems/Public_Communication_Protocols/tree/main/Docs/IOTransceiver/BLE/Characteristics)  

These protocols may be used **free of charge**.  

From these documents you should be able to create your own APIs and code libraries to facilitate integration of our products with your digital monitoring solution.  

# Known integrators
As digital monitoring providers make use of these documents to interface with our products, we'll add a link to their websites in this section.

# Quick start libraries
In future, we'll post some code libraries in this repo to speed up your integration efforts.
