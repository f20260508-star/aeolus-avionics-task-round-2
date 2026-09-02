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
