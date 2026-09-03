# Task 3 — Propulsion System

## Objective
The final propulsion system must provide more than 12 minutes of endurance while keeping the total drone weight below 2.5 kg.

## Fixed Components

| Component | Specification |
|---|---|
| Frame | 10-inch quadcopter frame |
| Motors | 4 × Holybro S500 V2 2216–920 KV motors |
| Propellers | 4 × 1045 (10 × 4.5 inch) propellers |
| Flight controller | Pixhawk 6C Mini |
| Maximum all-up weight | 2.5 kg |
| Minimum endurance | More than 12 minutes |

## Selected Battery

I selected a **4S 8000 mAh 20C or higher LiPo battery with XT60 or XT90 connector**.

A 4S LiPo battery has a nominal voltage of 14.8 V and a fully charged voltage of 16.8 V. The Holybro S500 V2 2216–920 KV motor supports 3S to 4S LiPo batteries, so a 4S battery is compatible with the motor and provides sufficient voltage for a 10-inch propulsion system.
An 8000 mAh capacity is selected instead of 5000 mAh because the drone must achieve more than 12 minutes of endurance while carrying the Jetson-based autonomous vision system.

## Selected ESC

I selected **4 × 20 A BLHeli-S ESCs**, with at least **30 A burst current rating**, 3–4S LiPo support and DShot/PWM input compatibility.

A 20 A ESC is selected because the expected maximum motor current with a 10 × 4.5 propeller on a 4S battery is approximately 16.37 A per motor. The 20 A continuous ESC rating provides approximately 22% current margin.

\[
\text{ESC current margin} =
\frac{20 - 16.37}{16.37} \times 100
= 22.2\%
\]

## Battery Current Capability

The selected battery is a 4S 8000 mAh 20C LiPo battery.

\[
\text{Battery capacity} = 8000\text{ mAh} = 8\text{ Ah}
\]

\[
\text{Maximum continuous current} =
\text{Capacity} \times C\text{-rating}
\]

\[
I_{\text{battery,max}} = 8\text{ Ah} \times 20C = 160\text{ A}
\]

The maximum tested current of one 2216–920 KV motor with a 10 × 4.5 propeller on 4S is approximately 16.37 A.

\[
I_{\text{motor,max,total}} = 4 \times 16.37 = 65.48\text{ A}
\]

Adding a conservative 5 A battery-side equivalent for Jetson, cameras, Pixhawk, GPS, RC receiver, telemetry, VTX and regulator losses:

\[
I_{\text{system,max}} = 65.48 + 5 = 70.48\text{ A}
\]

\[
160\text{ A} > 70.48\text{ A}
\]

Therefore, the 4S 8000 mAh 20C battery has sufficient continuous-current capability for the propulsion system and avionics.

## Endurance Calculation

To calculate a conservative endurance estimate, the drone is assumed to have the maximum permitted all-up weight of 2.5 kg.

\[
\text{Required hover thrust per motor} =
\frac{2500\text{ g}}{4}
= 625\text{ g}
\]

A 2216–920 KV motor with a 10 × 4.5 propeller produces approximately 729 g thrust at 65% throttle and draws approximately 6.78 A at 16 V.

\[
\text{Total thrust at 65% throttle} =
4 \times 729
= 2916\text{ g}
\]

\[
2916\text{ g} > 2500\text{ g}
\]

Therefore, 65% throttle provides sufficient thrust margin for a 2.5 kg drone.

\[
I_{\text{motors}} =
4 \times 6.78
= 27.12\text{ A}
\]

The estimated Jetson and stereo-camera power consumption is 22 W.

\[
I_{\text{Jetson/camera}} =
\frac{22\text{ W}}{14.8\text{ V}}
= 1.49\text{ A}
\]

A further 0.4 A is reserved for Pixhawk, GPS, RC receiver, telemetry radio, VTX and regulator margin.

\[
I_{\text{average}} =
27.12 + 1.49 + 0.4
= 29.01\text{ A}
\]

Only 80% of LiPo capacity is used to avoid over-discharging the battery.

\[
C_{\text{usable}} =
0.8 \times 8
= 6.4\text{ Ah}
\]

\[
t =
\frac{C_{\text{usable}}}{I_{\text{average}}}
=
\frac{6.4}{29.01}
= 0.221\text{ hours}
\]

\[
t =
0.221 \times 60
= 13.2\text{ minutes}
\]

Therefore, the estimated endurance is **approximately 13.2 minutes**, which satisfies the requirement of more than 12 minutes.

## Weight Budget

The drone must remain below the maximum permitted all-up weight of 2.5 kg. The following weight budget uses estimated component masses for the selected configuration.

| Component | Quantity | Mass per Unit | Total Mass |
|---|---:|---:|---:|
| 10-inch frame with landing gear | 1 | 440 g | 440 g |
| Holybro S500 V2 2216–920 KV motor | 4 | 63 g | 252 g |
| 20 A BLHeli-S ESC | 4 | 8 g | 32 g |
| 10 × 4.5 propeller | 4 | 12 g | 48 g |
| 4S 8000 mAh 20C LiPo battery | 1 | 530 g | 530 g |
| Pixhawk 6C Mini | 1 | 42 g | 42 g |
| GPS and compass module | 1 | 35 g | 35 g |
| Jetson Orin Nano, stereo camera, regulator and mounting | 1 | 178 g | 178 g |
| ELRS RC receiver | 1 | 5 g | 5 g |
| SiK telemetry air radio and antenna | 1 | 25 g | 25 g |
| FPV camera, VTX and antenna | 1 | 35 g | 35 g |
| Power module, BECs, wiring, connectors and mounts | 1 | 150 g | 150 g |
| **Estimated total all-up weight** | — | — | **1772 g** |

\[
\text{Weight margin} =
2500\text{ g} - 1772\text{ g}
= 728\text{ g}
\]

The estimated all-up weight is 1772 g, which is below the 2.5 kg maximum limit. The design retains approximately 728 g margin for differences in actual component mass, landing gear, fasteners, vibration mounts and manufacturing tolerances.

The battery must be weighed before final assembly. If the selected 4S 8000 mAh battery is substantially heavier than 530 g, the full weight budget and endurance calculation must be checked again.

## Final Propulsion System Selection

The selected propulsion and power system for the 10-inch autonomous search-and-rescue quadcopter is:

| Item | Final Selection |
|---|---|
| Motors | 4 × Holybro S500 V2 2216–920 KV motors |
| Propellers | 4 × 1045 (10 × 4.5 inch) propellers |
| Battery | 4S 8000 mAh 20C LiPo battery |
| ESCs | 4 × 20 A BLHeli-S ESCs with at least 30 A burst rating |
| Flight controller output protocol | DShot or PWM from Pixhawk 6C Mini |

The Holybro S500 V2 2216–920 KV motors are compatible with 3S–4S LiPo batteries. The 4S battery was selected because it provides a nominal voltage of 14.8 V and is compatible with the motor and 10 × 4.5 inch propeller combination.

Each motor can draw approximately 16.37 A at maximum throttle with a 10 × 4.5 inch propeller on a 4S battery. Therefore, a 20 A ESC provides sufficient continuous-current margin, while the 30 A burst rating provides additional protection during rapid throttle changes.

The estimated average current draw during a conservative hover/cruise condition is approximately 29.01 A. With 80% usable capacity from an 8000 mAh battery, the estimated endurance

The estimated all-up weight is approximately 1772 g, which is below the 2500 g maximum limit. The design therefore has an estimated mass margin of approximately 728 g.

### Final Result

| Requirement | Result | Status |
|---|---:|---|
| Maximum drone weight | 1772 g < 2500 g | Pass |
| Minimum endurance | 13.2 min > 12 min | Pass |
| Motor voltage compatibility | 4S supported | Pass |
| ESC current capability | 20 A > 16.37 A per motor | Pass |
| Battery current capability | 160 A > 70.48 A maximum-system estimate | Pass |

Therefore, a **4S 8000 mAh 20C LiPo battery with four 20 A BLHeli-S ESCs** is selected as the recommended propulsion-system build for this design.

## Practical Safety Notes

- Verify the actual motor thrust/current data using the exact final propeller and battery before flight.
- Measure the real all-up weight after final assembly.
- Perform a restrained motor test before flight and verify that ESC and motor temperatures remain safe.
- Configure low-battery failsafe, RC-loss failsafe and Return-to-Launch in Pixhawk.
- Do not power the Jetson from a small Pixhawk telemetry-port supply; use its dedicated regulator.

## Bonus: Alternative Motor for Improved Thrust Efficiency

The given Holybro 2216–920 KV motor with a 10 × 4.5 inch propeller is suitable for the selected 4S propulsion system. However, an endurance-focused search-and-rescue drone can potentially improve hover efficiency by using a larger, lower-KV motor together with a larger, lower-pitch propeller.

### Suggested Alternative Build

| Component | Alternative Selection |
|---|---|
| Motor | T-Motor MN3110 700 KV |
| Propeller | 11 × 3.7 inch or 12 × 3.8 inch two-blade carbon-fibre propeller |
| Battery | 4S or 6S LiPo, selected after final thrust testing |
| ESC | ESC selected with sufficient margin from the final motor-propeller current test |

The MN3110 700 KV motor is a larger, lower-KV motor intended for multirotor UAV applications. Low-KV motors are suited to turning larger propellers at lower rotational speed. A larger, lower-pitch propeller can move more air efficiently during hover, which is useful for a slow search-and-rescue mission where endurance and stable flight are more important than high speed.

The MN3110 700 KV motor weighs approximately 80 g and has a maximum continuous current rating of approximately 21 A. Manufacturer data shows that, with an 11 × 3.7 inch propeller, the motor produces approximately 280 g thrust at 19.98 W and approximately 380 g thrust at 33.30 W in a 3S test. This indicates good low-throttle efficiency for hover-oriented flight.

### Engineering Trade-Off

This alternative motor is heavier than the original 63 g Holybro 2216–920 KV motor. It also requires a frame that can safely clear an 11-inch or 12-inch propeller. Therefore, it cannot be used unchanged with the fixed 10-inch frame and 10 × 4.5 inch propeller specified in the main task.

The proposed alternative is a redesign option for a larger endurance-focused aircraft. Before use, the final motor, propeller, battery and ESC combination must be tested together to verify thrust, current, temperature, vibration and total aircraft weight.
