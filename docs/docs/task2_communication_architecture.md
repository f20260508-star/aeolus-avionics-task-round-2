# Task 2 — Communication Architecture

## Objective

This task selects the RC control system, telemetry system and video-transmission system for the autonomous 10-inch search-and-rescue drone.

The communication system must provide:

- Manual RC control and emergency pilot override
- Telemetry between the drone and ground-control station
- Live video for search and target confirmation
- Reliable communication with the Pixhawk 6C Mini flight controller


## 1. RC Transmitter and Receiver

### Purpose

The RC transmitter and receiver provide manual control and emergency override for the autonomous drone. The pilot can arm/disarm the drone, select flight modes, manually control the drone, and take over if the autonomous system detects a fault or uncertain target.

### Selected Models

| Component | Selected Model | Reason for Selection |
|---|---|---|
| RC transmitter | RadioMaster TX16S Mark II with internal ExpressLRS 2.4 GHz | Provides multiple switches for flight modes and reliable manual control |
| RC receiver | RadioMaster RP3 ExpressLRS 2.4 GHz Diversity Receiver | Designed for reliable RC control |

### Protocol

The receiver communicates with the Pixhawk flight controller using the CRSF serial protocol.

CRSF is preferred over PWM or PPM because it sends multiple RC channels, link-quality information and telemetry through one UART serial connection. This
avoids using multiple PWM signal wires and avoids a PWM/PPM protocol mismatch.

### Connection to Pixhawk 6C Mini

The ExpressLRS receiver must be connected to a free UART port on the Pixhawk 6C Mini.

| RC Receiver Pin | Pixhawk UART Pin |
|---|---|
| TX | RX |
| RX | TX |
| 5V | 5V output |
| GND | GND |

TX and RX are crossed because the receiver transmit pin must connect to the Pixhawk receive pin, and the receiver receive pin must connect to the Pixhawk
transmit pin.

### Power Source

The RC receiver is powered from the regulated 5 V rail of the Pixhawk or a dedicated 5 V BEC. The ground of the receiver, Pixhawk and battery system must
be common.

### Range

The exact range depends on antenna placement, transmit power, terrain, legal transmission limits and interference. A conservative design target is a 
reliable line-of-sight RC link of at least 1 km.


## 2. Telemetry Module

### Purpose

The telemetry module provides a two-way data link between the drone and the ground-control station. It allows the operator to monitor the GPS location,
speed, battery status, altitude, flight mode and warning messages during the mission.
It can also receive waypoint updates and mission commands from the ground-control station.

### Selected Model

| Component | Selected Model | Reason for Selection |
|---|---|---|
| Telemetry radio set | Holybro SiK Telemetry Radio V3, 433 MHz | Designed for Pixhawk telemetry, supports MAVLink communication |

The telemetry set contains two radios:

- An air radio mounted on the drone
- A ground radio connected to the laptop by USB

### Protocol and Frequency

The telemetry radio communicates with Pixhawk through a UART serial connection using MAVLink messages. The air radio and ground radio communicate
wirelessly at 433 MHz.

MAVLink carries small flight-data and command messages, such as GPS location, altitude, battery status, flight mode, waypoints and warnings. MAVLink is
not used to transmit live video.

### Connection to Pixhawk 6C Mini

The air telemetry radio is connected to the Pixhawk TELEM1 port.

| SiK Telemetry Radio Pin | Pixhawk TELEM1 Pin |
|---|---|
| VCC | 5V |
| TX | RX |
| RX | TX |
| GND | GND |

The TX and RX pins are crossed because the telemetry radio transmit pin must connect to the Pixhawk receive pin, and the telemetry radio receive pin must connect to the Pixhawk transmit pin.

### Power Source

The air telemetry radio is powered from the Pixhawk TELEM1 5 V output. The ground telemetry radio is powered from the operator through USB.

### Range

The standard Holybro SiK Telemetry Radio V3 typically provides more than 300 m range out of the box. The actual range depends on frequency variant,
antenna placement, output power, terrain, interference and legal radio regulations.


## 3. Video Transmitter (VTX)

### Purpose

The VTX transmits live video from the drone to the ground operator. The live video allows the operator to inspect the search area, confirm a person detected by the Jetson AI system and support safe manual takeover if required.

### Selected Models

| Component | Selected Model | Reason for Selection |
|---|---|---|
| FPV camera | RunCam Phoenix 2 analog FPV camera | Analog camera suitable for outdoor FPV viewing |
| Video transmitter | Rush Tank Solo 5.8 GHz analog VTX | Supports analog CVBS video input, adjustable output power and a wide 7–36 V input range |
| Ground video receiver | Compatible 5.8 GHz analog FPV goggles or monitor receiver | Receives the live analog video feed |

### Analog versus Digital Video

An analog VTX is selected for this design because it provides low latency, has simple wiring and is independent of the Jetson vision system. The separate
analog FPV camera and VTX provide a direct live feed for the ground operator.

This separation improves safety: even if the Jetson software fails or is overloaded, the operator can still receive a live FPV video feed for manual
control and target confirmation.

### Connections

| Connection | Wiring |
|---|---|
| FPV camera video output | Connect to VTX video input |
| FPV camera power | Connect to regulated 5 V output and GND |
| VTX power input | Connect to battery voltage or a regulated supply within 7–36 V and GND |
| VTX antenna | Connect to 5.8 GHz antenna before powering the VTX |
| Optional VTX control | Connect VTX SmartAudio control pad to a spare Pixhawk UART TX pin if supported |

The analog FPV camera sends a CVBS video signal directly to the VTX. The VTX transmits this video at 5.8 GHz to a compatible ground receiver, monitor or
FPV goggles.

### Power Source

The Rush Tank Solo VTX accepts 7–36 V DC and can be powered from the main battery voltage if the battery voltage remains within this range. The RunCam
Phoenix 2 camera is powered from a regulated 5 V supply.

The VTX, camera, Pixhawk and battery system must share a common ground for the analog video signal to work correctly.

### Protocol and Range

The video system uses an analog CVBS video signal between the camera and VTX. The wireless link operates at 5.8 GHz.

Video range depends strongly on VTX output power, antenna type, antenna placement, line of sight, terrain, interference and local legal transmit-power
limits. For this project, a conservative design target is at least 500 m of clear line-of-sight video range at a legal VTX power setting.
