# **PW-Gruppo2-LovelyCommit**

<p align="center">
    <a href="./LICENSE"><img src="https://img.shields.io/badge/License-GPLv3-blue.svg"></a>
</p>

<div align="center">  
<h4>
    <a href="#project-trace"> Project Trace </a>
  | <a href="#schemas"> Schemas </a>
  | <a href="#ucontroller"> uController</a>
  | <a href="#raspberrypi"> RaspberryPi </a>
  | <a href="#cloud"> Cloud </a>
  | <a href="#documents"> Documents </a>
</h4>
</div>
Project Work 2022 Group 2 Corso Diqu
Faraoni Matteo, Gatto Alessandro, Pizzato Davide, Scapolan Davide

## **Project Trace**

***
The **UAR**, Upper Adriatic Railroad, requires the creation of a system to monitor the status of the wagons and to be able to control their behavior both from a booth inside the train and remotely.
It was decided to implement an RS485 BUS system that connects each carriage, in which there is a custom uC card capable of detecting **temperature**, **humidity**, and **door closing status**, in addition to verifying if someone's in the **toilet** compartment.
In addition, the microcontroller can **independently control the air conditioning** for each wagon, the **opening of the doors** and the **emergency signals**. An LCD display is also required for each wagon, it must be capable of displaying messages sent by the remote controller, reading the sensors and modifying the operating parameters.

All this information then is sent and received with a proprietary protocol by an RPI that acts as a Gateway connected to a network with internet access, then it will publish it. Publishing it will allow a remote operator to check the status and possibly modify the states of the actuators.
An information queue must also be provided if the connection with the cloud fails while crossing tunnels.

Additional features may be proposed by individual groups.

## **Schemas**

***

To be able to view any .drowio file open [this link](https://draw.io), than open the file from the repository

<!-- |![Flow schema of the system](./Images/FlowSchema.jpg)|
|-|-->
<div align="center">  
<img src="./Images/FlowSchema.jpg " 
     alt="Flow Schema"
     width="400" 
     height="auto" />
</div>

## **uController**

***

### **Hardware component**

*Elements on the list supplied*:

* GY-BME280 barometric sensor for temperature, humidity and air pressure

* Motion sensor HC-SR501 PIR motion detection module
 `Used for automatic presence detection in bathrooms` 

*External element*:

* 20 microswitches with debouncing pullup components
 `Used for open/closed detection of all the doors in the wagon`

* 24V bulb with power supply etc (provided you mount the relays)

### **Protocol PIC-RPI Structure**

<table>
<thead>
<tr>
<th style="text-align:center"></th>
<th style="text-align:center">PIC</th>
<th style="text-align:center">RaspberryPI</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:center">Data sent from PIC to a RPI</td>
<td style="text-align:center">id(RPI)_Payload()_CRC</td>
<td style="text-align:center"></td>
</tr>
<tr>
<td style="text-align:center">Data sent from RPI to a PIC</td>
<td style="text-align:center"></td>
<td style="text-align:center">id(PIC/Broadcast)_Payload_CRC</td>
</tr>
<tr>
<td style="text-align:center">Handashake between PIC and RPI</td>
<td style="text-align:center">id(RPI)_handshakeCode_CRC</td>
<td style="text-align:center">id(RPI)_newId(PIC)_CRC</td>
</tr>
</tbody>
</table>

#### **Payload Telemetry**

The payloads will be sent in this (static) order: 
<table>
<thead>
<tr>
<th style="text-align:center">1° Byte</th>
<th style="text-align:center">2° Byte</th>
<th style="text-align:center">3° Byte</th>
<th style="text-align:center">4° Byte</th>
<th colspan="3"style="text-align:center">5° Byte</th>
</tr>
</thead>
<tbody>
<tr>
<th style="text-align:center"></th>
<th style="text-align:center"></th>
<th style="text-align:center"></th>
<th style="text-align:center"></th>
<th style="text-align:center">5° Byte 3° bit</th>
<th style="text-align:center">5° Byte 2° bit</th>
<th style="text-align:center">5° Byte 1° bit</th>
</tr>
<tr>
<td colspan="2"style="text-align:center">Temperature</td>
<td colspan="2"style="text-align:center">Humidity</td>
<td style="text-align:center">Status back door</td>
<td style="text-align:center">Status front door</td>
<td style="text-align:center">Status bathroom</td>
</tr>
</tbody>
</table>

#### **Payload Commands**

<table>
<thead>
<tr>
<th style="text-align:center">1° Byte</th>
<th style="text-align:center">2° Byte</th>
<th colspan="2" style="text-align:center">3° Byte </th>
<th style="text-align:center">4° Byte</th>
<th style="text-align:center">5° Byte</th>
<th style="text-align:center">X° Byte</th>
</tr>
</thead>
<tbody>
<tr>
<th style="text-align:center"></th>
<th style="text-align:center"></th>
<th style="text-align:center">2° bit</th>
<th style="text-align:center">1° bit</th>
<th style="text-align:center"></th>
<th style="text-align:center"></th>
<th style="text-align:center"></th>
</tr>
<tr>
<td colspan="2" style="text-align:center">Target temperature</td>
<td style="text-align:center">Status back door</td>
<td style="text-align:center">Status front door</td>
<td style="text-align:center">Length of the message (number of byte)</td>
<td  colspan="2"style="text-align:center">Message (if te Length is greater then 0)</td>
</tr>
</tbody>
</table>

#### **CRC Calc**

Library?

## **RaspberryPi**

***

### **AMQP RPI**

### **Mqtt RPI-Azure**

## **Cloud**

***

## **Documents**

***
* [uController section](./PIC/)
* [RaspberryPi section](./RPI/)
* [Cloud section](./Cloud/)

## **LICENSE**

***
LovelyCommit is provided under the [GNU General Public License Version 3](./License)