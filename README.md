# Public Communication Protocols
This repo shall contain the communication protocols to communicate with some products of NIST Control Systems as listed below.

## Motorscope Related Protocols
### Direct UART TTL communication
#### Related Products
- All Motorscope [three phase](https://nistcontrol.com/product-category/three-phase/) motor protection relays
- All Motorscope [single phase](https://nistcontrol.com/product-category/single-phase/) motor protection relays
#### Protocol Doc
- [NIST_StatusOnly_Serial_Protocol.pdf](https://github.com/NISTControlSystems/Public_Communication_Protocols/blob/main/Docs/Motorscope/UART/NIST_StatusOnly_Serial_Protocol.pdf)  This file describes the protocol used on the 4 pin TTL UART port, situated next to the three status LEDs of all Motorscope products.

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
