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

## 11. Architecture Rating and Weightage

For this search-and-rescue mission, compute capability and mission suitability are given the highest weightage. The drone must reliably detect a person, estimate distance using stereo vision and make safe high-level decisions while flying autonomously.

| Criterion | Weightage | Reason |
|---|---:|---|
| Compute | 30% | Real-time person detection and stereo-depth processing are required |
| Use case suitability | 30% | The architecture must support autonomous search, target detection and obstacle awareness |
| Power consumption | 15% | Avionics power affects battery endurance |
| Weight | 15% | The complete drone must remain below 2.5 kg |
| Cost | 10% | Cost matters, but rescue capability and safety are more important |
| **Total** | **100%** |  |

A rating of 5 indicates the best performance in that criterion, while a rating of 1 indicates the weakest performance.

| Build | Compute /5 | Use Case /5 | Power /5 | Weight /5 | Cost /5 | Weighted Score /5 |
|---|---:|---:|---:|---:|---:|---:|
| **1. Stereo camera + Jetson Orin Nano** | **5** | **5** | 3 | 3 | 2 | **4.10** |
| 2. OAK-D Lite + Raspberry Pi 5 | 4 | 3 | 5 | 4 | 4 | 3.80 |
| 3. LiDAR + IMX219 camera + Raspberry Pi 5 | 3 | 4 | 3 | 3 | 3 | 3.35 |
| 4. Stereo camera + Raspberry Pi 5 | 3 | 3 | 4 | 4 | 4 | 3.45 |
| 5. RealSense D435i + Jetson Orin Nano | 5 | 4 | 3 | 3 | 2 | 3.95 |

### Build 1 Score Calculation

\[
\text{Weighted score} =
(5 \times 0.30) +
(5 \times 0.30) +
(3 \times 0.15) +
(3 \times 0.15) +
(2 \times 0.10)
\]

\[
= 1.50 + 1.50 + 0.45 + 0.45 + 0.20
\]

\[
= 4.10/5
\]

## 12. Bonus: Camera Orientation

The IMX219-83 stereo camera should be mounted at the front centre of the drone, facing forward. The two cameras must remain side-by-side with a horizontal baseline because stereo-depth estimation uses the left-right difference between the two images.

### Recommended Orientation

| Parameter | Recommended Value | Reason |
|---|---|---|
| Camera position | Front centre of the drone | Provides an unobstructed forward view |
| Stereo baseline | Horizontal, left camera and right camera side-by-side | Supports standard stereo matching and depth estimation |
| Yaw alignment | Aligned with the drone forward direction | Makes target direction easier to relate to drone heading |
| Mounting method | Rigid mount with vibration isolation | Reduces motion blur and preserves stereo calibration |

The camera should be mounted high enough and far enough forward that the propellers, frame arms and landing gear do not block the camera field of view.
The camera should also be kept away from ESC power wires and motors as much as possible to reduce vibration and electrical interference. The selected
orientation provides a balance between forward search coverage and near-field obstacle awareness.

## 13. Bonus: Alternate Build

The alternate architecture I recommend is **Build 5: Intel RealSense D435i depth camera + NVIDIA Jetson Orin Nano**.

The Intel RealSense D435i provides depth information directly, which reduces the amount of stereo-camera calibration and image-processing setup required compared with a custom stereo-camera pair, which is used in Build 1.

The Jetson Orin Nano remains useful in this alternate build because it can run advanced person-detection and target-tracking AI models using the RealSense RGB and depth data.

| Advantage of Build 5 | Limitation of Build 5 |
|---|---|
| Ready-made depth camera | Higher cost than the IMX219-83 stereo camera |
| Easier depth-camera integration and calibration | Adds weight to the drone |
| Jetson provides strong AI compute for person detection | Higher power consumption than Raspberry Pi-based options |

Build 5 is a strong alternate option when the team wants a more ready-to-use depth camera and has a higher budget. However, I selected Build 1 as the
primary architecture because the IMX219-83 stereo pair is lighter, lower-cost and provides a flexible camera system, while the Jetson Orin Nano still
supplies strong AI compute for the search-and-rescue mission.
