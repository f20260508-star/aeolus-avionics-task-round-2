# Aeolus Avionics Task — Round 2

**Name:** Saachi Saket Tiwari  
 **Mission:** Autonomous search-and-rescue drone for locating Odysseus

## Mission Constraints

- Frame: 10-inch quadcopter frame
- Maximum drone weight: 2.5 kg
- Minimum endurance: More than 12 minutes
- Flight controller: Pixhawk 6C Mini
- Motors: Holybro S500 V2 2216–920 KV
- Propellers: 10 × 4.5 inch

## Selected Architecture

**Build 1: Forward-facing IMX219-83 stereo camera + NVIDIA Jetson Orin Nano**

The stereo camera pair captures two views of the environment and understands how far away objects are. The cameras produce lots of image data, Jetson Orin Nano processes the images for person detection, stereo depth estimation and obstacle awareness. It shares high-level data with the Pixhawk flight controller, while Pixhawk controls drone stabilization, navigation and motors.

## Work Sections

| Task | Description
|---|---|---|
| Task 1 | Autonomous architecture selection 
| Task 2 | Communication architecture 
| Task 3 | Propulsion, battery and ESC selection 

## Repository Structure

- `docs/` — Written task answers and sources
- `diagrams/` — Block diagrams, communication diagram and exported images
- `calculations/` — Battery, power, endurance and weight calculations
- `kicad/` — KiCad project and netlist, if created
- `assets/` — Datasheets and component images
