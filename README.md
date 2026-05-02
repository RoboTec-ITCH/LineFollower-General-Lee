[English](README.md) | [Español](README-es.md)

# RoboTec - High-Speed Line Follower (Architecture & Hardware Showcase)

![Project Type](https://img.shields.io/badge/Project-Technical_Showcase-blue.svg)
![Platform](https://img.shields.io/badge/Platform-Arduino_Nano-00979D.svg)
![License](https://img.shields.io/badge/License-MIT-green.svg)
![Status](https://img.shields.io/badge/Status-Competition_Ready-orange.svg)

*Note: This repository serves as a technical showcase and documentation portfolio for the project. The core proprietary C++ firmware is maintained in a private repository by the RoboTec engineering team.*

This project showcases the hardware architecture and control methodology developed for the High-Speed Line Follower competitive robotics category. It relies on a custom Proportional-Integral-Derivative (PID) control system featuring real-time dynamic tuning, high-inertia predictive braking, and multiplexed analog processing of a 16-sensor reflective array.

---

## Engineering Features

- **Configurable PID Profiles:** Supports multiple user-configurable PID profiles (speed and aggressiveness) that can be switched in real-time via a hardware control or the start-module input. This enables on-the-fly tuning without reflashing the microcontroller in the pit lane.
- **Differential Predictive Braking:** Extreme-angle detection algorithm for 90-degree curves that applies an internal reverse "anchor", neutralizing centrifugal force at peak speeds.
- **Software Optical Inversion:** Logic integration to toggle the algorithm between standard tracks (black line / white background) and inverted tracks (white line / black background).
- **Sentinel Mode:** Low-speed active self-centering routine triggered by logic signals from the starting module.

---

## Hardware Architecture & Integration

This project relies on specific hardware modules working synchronously. Below is the technical breakdown of each system and its role in the robot's performance.

### 1. Chassis and Powertrain
<div align="center">
<img src="images/20260428_190512.jpg" width="300" alt="Robot Chassis">
</div>

The robot is driven by two 3000 RPM DC motors controlled by a TB6612FNG motor driver. This driver was selected for its capability to handle 1.2A continuous current and up to 3A peaks, safely operating within the 6V to 8.4V range provided by the 2S LiPo battery. The driver's internal voltage regulator (up to 500mA) powers the logic components.

### 2. "Hermes" Sensor Array
<div align="center">
<img src="images/hermes.png" width="300" alt="Hermes Sensor Array">
</div>

Designed and assembled by Alexander Armando Martinez Gil, the "Hermes" board is a custom frontal array of 16 analog reflective sensors. To bypass the Arduino Nano's pin limitations, the board integrates a **CD74HC4067 multiplexer**. The system cycles through the 16 channels via 4 control pins (S0-S3) and reads a single analog output (COM), applying a center-weighted average algorithm to calculate the exact position error.

### 3. Competition Start Module
<div align="center">
<img src="images/control.png" width="500" alt="Start Control Module">
</div>

To comply with official competition standards, the system interfaces with an *Ingeniero Maker* RF receiver module. It provides two critical logic signals:
* **Ready Signal (Pin D12):** Triggers the "Sentinel Mode" to keep the robot centered on the line at low speeds before the race. It also acts as the manual switch to cycle through the PID profiles during pit setup.
* **Go Signal (Pin D10):** Unlocks the main PID control loop, launching the robot at competitive speeds.

---

## Bill of Materials (BOM)

| Component | Specifications | System Role |
| :--- | :--- | :--- |
| **Microcontroller** | Arduino Nano (ATmega328P) | Core processing and PID control loop. |
| **Sensor Array** | Custom "Hermes" Board | Frontal 16-sensor array. |
| **Multiplexer** | CD74HC4067 | 16-channel analog multiplexing. |
| **Motor Driver** | TB6612FNG | PWM power control. |
| **Powerplant** | 2x DC Motors (3000 RPM) | Differential traction. |
| **Power Supply** | LiPo 2S (7.4V nominal) | Main power source. |
| **Start Module** | Ingeniero Maker Module | RF receiver for Ready/Go signals. |

---

## Wiring Schematic (Pinout)

**Motors & TB6612FNG Driver**
* `AIN1`: D8 | `AIN2`: D7 | `PWMA`: D5 (Right Motor)
* `BIN1`: D9 | `BIN2`: D4 | `PWMB`: D6 (Left Motor)

**"Hermes" Sensor Array (CD74HC4067)**
* `S0`: A3 | `S1`: A2 | `S2`: A1 | `S3`: A0
* `COM` (MUX Analog Output): A4

**Start Module (Ingeniero Maker)**
* `RDY`: D12
* `GO`: D10

---

## Competition Operation (Pit Lane Setup)

1. Power on the robot. The system defaults to the first stored PID profile.
2. If the `GO` signal is not received, the ready/control input or onboard button is used to cycle through the stored PID profiles. The onboard LED indicates the currently selected profile.
3. Upon receiving the `Ready` signal, the robot enters **Sentinel Mode**, locking onto the track line at low speed to ensure perfect alignment.
4. Upon receiving the `GO` signal, the main high-speed PID control loop is enabled and the race sequence begins.

---

## Credits & Team

* **Software Architecture & Control:** Mauricio Gómez Márquez
* **Hardware Engineering & PCB Design:** Alexander Armando Martinez Gil
* **Organization:** RoboTec Robotics Club

---

## License

The documentation and hardware architecture presentations in this repository are released under the MIT License. See [LICENSE](LICENSE) for details. The underlying control firmware remains proprietary to the organization.
