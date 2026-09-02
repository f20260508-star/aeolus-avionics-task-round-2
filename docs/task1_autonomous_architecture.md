Task 1 — Autonomous Architecture


## 1. Selected Architecture

I selected **Build 1: Forward-Facing Stereo Camera + NVIDIA Jetson Orin Nano** for the autonomous search-and-rescue drone.

This build uses a Waveshare/custom IMX219-83 stereo camera and an NVIDIA Jetson Orin Nano companion computer. The stereo camera is mounted facing forward so that the drone can observe the area in front of it during the search mission.

## 2. Purpose of Each Component

| Component | Purpose in the drone |
|---|---|
| IMX219-83 stereo camera | Captures two images of the same scene from slightly different positions and hence helps understand the distance of objects or people from the drone |
| NVIDIA Jetson Orin Nano | Processes the camera images, detects a person, calculates depth and identifies obstacles and then sends the relevant data or commands to the Pixhawk flight controller  |
| Pixhawk 6C Mini | Stabilizes the drone, manages flight modes and controls the motors through the ESCs |

## 3. Basic Working

1. The left and right cameras capture the same scene at the same time.
2. The Jetson compares the two images to estimate the distance of objects in front of the drone.
3. The Jetson runs an AI model to identify a possible person and then process the image data produced by the camera.
4. If a person or obstacle is detected, Jetson sends high-level information to Pixhawk.
5. Pixhawk controls the drone flight, motor outputs and safety functions.

## 4. Simple System Flow

```text
Left IMX219 Camera ──┐
                     ├──> NVIDIA Jetson Orin Nano ──> Pixhawk 6C Mini ──> ESCs ──> Motors
Right IMX219 Camera ─┘
```

## 5. Why This Build Fits the Mission

The mission is to search for Odysseus autonomously. Build 1 is selected as the most suitable architecture for the autonomous search-and-rescue drone.
The forward-facing stereo camera provides environmental and depth perception, while the NVIDIA Jetson Orin Nano provides strong onboard processing 
capability. Although the architecture may have disadvantages in terms of power consumption and cost, its suitability for autonomous sensing and 
processing gives it the highest overall weighted score.

## 6. Compute

Compute refers to the processing capability available for camera processing, depth estimation and artificial-intelligence tasks.

The NVIDIA Jetson Orin Nano is selected as the companion computer because it provides much higher AI processing capability than a basic single-board computer. It can process the two stereo-camera image streams, calculate depth, run a person-detection model and track a detected person or any other obstacles in real time.

The Jetson Orin Nano 8 GB version provides up to 40 TOPS of AI performance. This makes it suitable for computer-vision tasks such as:

- Person or obstacle detection during the search mission
- Stereo-depth estimation
- Target tracking
- Image processing and video recording

The Jetson has two MIPI CSI-2 camera connectors. Therefore, the two cameras in the IMX219-83 stereo system can be connected as camera inputs to the Jetson using suitable compatible CSI cables/adapters.

For the search-and-rescue mission, high compute capability is important because the drone must process camera data while flying, detect a possible person quickly and estimate the distance of obstacles or the target. The Jetson provides enough processing margin for these tasks, making Build 1 a strong autonomous architecture.

## 7. Power

Power consumption is important because the drone must fly for more than 12 minutes while remaining below the 2.5 kg weight limit. Higher electronics power consumption requires a larger battery, which can increase drone weight and reduce the payload margin.

The NVIDIA Jetson Orin Nano is more power-hungry than a Raspberry Pi-based solution. The Jetson Orin Nano 8 GB module can operate in 7 W and 15 W power modes. During the search mission, the Jetson must process two camera streams and run person-detection software, so the higher-performance mode is appropriate.

For a conservative Task 1 estimate, the power budget of the autonomous subsystem is assumed as follows:

| Component | Estimated Power |
|---|---:|
| NVIDIA Jetson Orin Nano during AI processing | 15 W |
| IMX219-83 stereo camera pair | 2 W |
| Cooling fan and carrier-board overhead | 2 W |
| DC-DC converter loss and wiring margin | 3 W |
| **Estimated autonomous subsystem total** | **22 W** |

Therefore, the estimated power requirement for Build 1 is approximately **22 W** during active autonomous operation.

The system should be powered from a dedicated regulated supply connected to the main battery. The Jetson must not be powered from the Pixhawk telemetry
port because Jetson requires significantly more current than a telemetry port is designed to provide. The additional power is accepted because Build 1 
provides stronger AI compute for real-time person detection, stereo-depth processing and target tracking.

## 8. Use Case According to the Mission

The mission is to autonomously search for Odysseus in an unknown outdoor area while keeping the drone safe. Therefore, the drone must identify a possible person, estimate the distance to obstacles and send useful information to the flight controller.

Build 1 supports this mission in the following way:

1. The forward-facing stereo camera continuously captures the area in front of the drone during the search pattern.
2. The Jetson Orin Nano runs a person-detection AI model on the camera images.
3. When a possible person is detected, the Jetson calculates the target position in the image and the confidence of the detection.
4. The Jetson compares the left and right stereo images to generate a depth estimate. This helps estimate how far the detected person or obstacle is from the drone.
5. The Jetson sends high-level target and obstacle information to Pixhawk using MAVLink communication.
6. Pixhawk remains responsible for stable flight, waypoint navigation, return-to-launch and motor control.

The stereo camera is useful because it provides distance information without adding a separate depth camera or LiDAR. A close object appears at a larger left-right shift in the two images, while a far object appears at a smaller shift. Jetson uses this difference to estimate depth.

## 9. Weight

Weight is a critical constraint because the complete 10-inch drone must remain below the maximum all-up weight of 2.5 kg. The mass budget must include the frame, four motors, four ESCs, propellers, battery, Pixhawk, GPS, communication equipment, wiring and the autonomous perception system.

Build 1 is heavier than the Raspberry Pi-based architectures because it uses a Jetson Orin Nano, a carrier board and active cooling. However, the additional mass is manageable if a compact flight-ready carrier board is used instead of a larger desktop-style development kit.

The estimated mass is shown below:

| Component | Estimated Mass |
|---|---:|
| IMX219-83 stereo camera module | 20 g |
| Jetson Orin Nano module | 28 g |
| Compact carrier board, heatsink and cooling fan | 80 g |
| CSI camera cables/adapters | 15 g |
| 5 V regulator, wiring and mounting hardware | 35 g |
| **Estimated autonomous subsystem total** | **178 g** |

The Jetson Orin Nano module itself weighs approximately 28 g. The full official developer kit is approximately 175–176 g before adding the camera, 
regulator, wiring and mounting hardware. Therefore, the developer kit should not be used directly in the final aircraft if weight is limited. Instead, a
compact carrier board with the Jetson module should be selected for the final flight configuration.
An estimated autonomy-subsystem weight of approximately 178 g is reasonable within a 2.5 kg drone, but the complete weight budget must be verified in 
Task 3 after selecting the battery, ESCs and communication components.

## 10. Cost

Build 1 has a higher cost than the Raspberry Pi-based architectures because the NVIDIA Jetson Orin Nano is an AI-focused embedded computer and requires additional supporting hardware such as a carrier board, cooling system, storage and a high-current voltage regulator.

The IMX219-83 stereo camera is a relatively low-cost camera module. However, the complete Build 1 cost is increased mainly by the Jetson Orin Nano and the hardware required to operate it reliably on a drone.

In comparison:

| Architecture Type | Relative Cost |
|---|---|
| Stereo camera + Raspberry Pi 5 | Low to medium |
| OAK-D Lite + Raspberry Pi 5 | Medium |
| LiDAR + camera + Raspberry Pi 5 | Medium to high |
| **Stereo camera + Jetson Orin Nano (Build 1)** | **High** |
| RealSense D435i + Jetson Orin Nano | Very high |

Build 1 is therefore not the lowest-cost option. However, it provides a strong compute-to-capability trade-off because the Jetson can run more capable
real-time person-detection, stereo-depth and target-tracking algorithms than a Raspberry Pi-only architecture.Therefore, the higher cost of Build 1 is
justified by its stronger AI capability and future expandability.
