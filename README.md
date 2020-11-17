# Public Communication Protocols
This repo shall contain the communication protocols to communicate with some products of NIST Control Systems as listed below.

- [NIST_StatusOnly_Serial_Protocol.pdf](https://github.com/NISTControlSystems/Public_Communication_Protocols/blob/main/NIST_StatusOnly_Serial_Protocol.pdf)  This file describes the protocol used on the 4 pin TTL UART port, situated next to the three status LEDs of all Motorscope products.
- [Display_Values_Calculations_From_BLE_Characteristics.pdf](https://github.com/NISTControlSystems/Public_Communication_Protocols/blob/main/Display_Values_Calculations_From_BLE_Characteristics.pdf) This file describes how to use the values contained in the xml files of BLE services and characteristics to convert the raw bytes to meaningful information.
- [MS_Status_PresentStatus-Spec.xlsx](https://github.com/NISTControlSystems/Public_Communication_Protocols/blob/main/MS_Status_PresentStatus-Spec.xlsx)  This file illustrates how to interpret the data in [PresentStatus](https://github.com/NISTControlSystems/Public_Communication_Protocols/blob/main/com.nistcontrol.characteristic.present_status.xml) (BLE Characteristic), which is in [MS_Status](https://github.com/NISTControlSystems/Public_Communication_Protocols/blob/main/com.nistcontrol.service.ms_status.xml) (BLE Service).
 

These protocols may be used **free of charge**.  

From these documents you should be able to create your own APIs and code libraries to facilitate integration of our products with your digital monitoring solution.  

# Known integrators
As digital monitoring providers make use of these documents to interface with our products, we'll add a link to their websites in this section.

# Quick start libraries
In future, we'll post some code libraries in this repo to speed up your integration efforts.
