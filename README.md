# Aeolus Avionics Task — Round 2

**Name:** Saachi Saket Tiwari  
**Batch:** 2026  
**Track:** Avionics  

## Mission

Design an autonomous 10-inch quadcopter for the search-and-rescue mission of locating Odysseus. The drone must use onboard perception, communication links and a suitable propulsion system while remaining below the maximum permitted weight.

## Mission Constraints

| Parameter | Requirement |
|---|---|
| Frame size | 10-inch quadcopter frame |
| Maximum all-up weight | Below 2.5 kg |
| Minimum endurance | More than 12 minutes |
| Flight controller | Pixhawk 6C Mini |
| Motors | Holybro S500 V2 2216–920 KV |
| Propellers | 1045 (10 × 4.5 inch) |
| Autonomous mission | Search, person detection, obstacle awareness and target confirmation |

## Final Design Summary

| System | Final Selection |
|---|---|
| Autonomous architecture | Build 1: Forward-facing IMX219-83 stereo camera + NVIDIA Jetson Orin Nano |
| Flight controller | Pixhawk 6C Mini |
| RC control | RadioMaster TX16S Mark II + RadioMaster RP3 ExpressLRS receiver |
| RC protocol | ExpressLRS / CRSF over UART |
| Telemetry | Holybro SiK Telemetry Radio V3, 433 MHz |
| Telemetry protocol | MAVLink over UART |
| FPV video camera | RunCam Phoenix 2 analog FPV camera |
| Video transmitter | Rush Tank Solo 5.8 GHz analog VTX |
| Video signal | CVBS analog video |
| Battery | 4S 8000 mAh 20C LiPo battery |
| ESC | 4 × 20 A BLHeli-S ESCs with at least 30 A burst rating |
| Estimated endurance | Approximately 13.2 minutes |
| Estimated all-up weight | Approximately 1772 g |
| Weight margin | Approximately 728 g below the 2.5 kg limit |

## System Overview

```text
IMX219-83 Stereo Camera Pair
              │
              │ CSI camera data
              ▼
     NVIDIA Jetson Orin Nano
              │
              │ MAVLink over UART
              ▼
       Pixhawk 6C Mini
              │
              ▼
       4 × 20 A ESCs
              │
              ▼
4 × Holybro 2216–920 KV Motors
              │
              ▼
    10 × 4.5 inch Propellers
```

## Communication Overview

```text
RadioMaster TX16S
        │
        │ 2.4 GHz ExpressLRS
        ▼
RadioMaster RP3 ELRS Receiver
        │
        │ CRSF over UART
        ▼
Pixhawk 6C Mini
        │
        ├── TELEM1: Holybro SiK Telemetry Radio
        │             ↕ 433 MHz MAVLink radio link
        │             Ground SiK Radio ↔ Laptop / QGroundControl
        │
        ├── TELEM2: NVIDIA Jetson Orin Nano
        │             MAVLink over UART
        │
        └── RunCam Phoenix 2 → Rush Tank Solo VTX
                                  ↕ 5.8 GHz analog video
                                  FPV goggles / monitor
```

## Task Deliverables

| Task | Description | Link |
|---|---|---|
| Task 1 | Autonomous architecture selection, evaluation and bonus discussion | [Open Task 1](docs/task1_autonomous_architecture.md) |
| Task 2 | RC, telemetry, VTX and communication architecture | [Open Task 2](docs/task2_communication_architecture.md) |
| Task 3 | Battery, ESC, endurance, weight budget and alternative motor analysis | [Open Task 3](docs/task3_propulsion_system.md) |
| Diagrams | System and communication diagrams | [Open diagrams](diagrams/) |
| KiCad | KiCad communication schematic and netlist | [Open KiCad files](kicad/) |

## Key Calculations

### Battery Current Capability

\[
I_{\text{battery,max}} = 8\text{ Ah} \times 20C = 160\text{ A}
\]

### Maximum System Current Estimate

\[
I_{\text{motor,max}} = 4 \times 16.37 = 65.48\text{ A}
\]

\[
I_{\text{system,max}} = 65.48 + 5 = 70.48\text{ A}
\]

\[
160\text{ A} > 70.48\text{ A}
\]

Therefore, the selected battery has sufficient current capability.

### Endurance Estimate

\[
I_{\text{average}} = 29.01\text{ A}
\]

\[
C_{\text{usable}} = 0.8 \times 8 = 6.4\text{ Ah}
\]

\[
t = \frac{6.4}{29.01} \times 60 = 13.2\text{ minutes}
\]

The estimated endurance is greater than the required 12 minutes.

### Weight Estimate

\[
\text{Estimated all-up weight} = 1772\text{ g}
\]

\[
\text{Weight margin} = 2500 - 1772 = 728\text{ g}
\]

The estimated all-up weight is below the 2.5 kg mission limit.

## Safety Considerations

- Pixhawk remains responsible for stabilization, motor control, failsafes and Return-to-Launch.
- Jetson performs high-level perception tasks such as person detection, stereo depth and obstacle awareness.
- The pilot retains manual RC override through the ELRS RC link.
- The drone should hover or loiter at a safe distance after detecting a possible person and send target information to the ground operator.
- Jetson must use a dedicated high-current power regulator and must not be powered from a Pixhawk telemetry-port supply.
- All components must share a common electrical ground.
- Actual motor thrust, current, temperature and final all-up mass must be verified physically before flight.
- Frequency, output-power and equipment approval requirements must be checked before real-world radio operation.

## Completion Status

- [x] Task 1 — Autonomous architecture selection and evaluation
- [x] Task 1 bonus — Camera orientation and alternate architecture
- [x] Task 2 — RC transmitter/receiver selection
- [x] Task 2 — Telemetry module selection
- [x] Task 2 — Analog VTX/video system selection
- [ ] Task 2 — KiCad communication diagram and netlist upload
- [x] Task 3 — Battery and ESC selection
- [x] Task 3 — Current, power and endurance calculations
- [x] Task 3 — Weight budget
- [x] Task 3 bonus — Alternative motor discussion
- [] Final diagram export and GitHub file check

## References

- Team Aeolus, *Avionics Task Round 2* — provided task brief.
- Holybro, *S500 V2 Development Kit and Spare Parts*.
- Holybro, *Pixhawk 6C Mini Documentation*.
- NVIDIA, *Jetson Orin Nano Series*.
- Waveshare, *IMX219-83 Stereo Camera Module*.
- PX4, *Companion Computer and Pixhawk Communication Documentation*.
- Holybro, *SiK Telemetry Radio V3 Documentation*.
- ExpressLRS, *Receiver Wiring Documentation*.
- RushFPV, *Tank Solo VTX Specifications*.
- RunCam, *Phoenix 2 FPV Camera Documentation*.
- T-Motor, *MN3110 700 KV Motor Specifications*.
