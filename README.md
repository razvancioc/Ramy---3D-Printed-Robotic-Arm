# Ramy-6-DOF-3D-Printed-Robotic-Arm(IN PROGRESS)

![Status: Work in Progress](https://img.shields.io/badge/Status-Work_in_Progress-orange)
![Platform: Fusion 360](https://img.shields.io/badge/CAD-Fusion_360-blue)
![Platform: Arduino/ESP32](https://img.shields.io/badge/Controller-TBD-lightgrey)

This repository documents the assembly, build process, and programming of a 6 Degrees of Freedom (6-DOF) robotic arm. The project is currently in the physical prototyping and software development phase.

## About the Project
The 3D printed parts are based on an excellent open-source model found on MakerWorld [https://makerworld.com/en/models/2466519-3d-robotic-arm?from=search]. To better understand the kinematics and prepare for the programming phase, I imported and fully assembled the components in **Autodesk Fusion 360**, where I also conducted a motion study.

## Motion Study & Kinematics
Before starting the physical assembly, I validated the joint limits and basic movements using a Motion Study in Fusion 360. This ensures the mechanical design can achieve the required range of motion without collisions.

<p align="center">
  <img src="Media/motion_study-ezgif.com-gif-maker.gif" alt="Robotic Arm Motion Study" width="600" />
</p>

## Current Status
- [x] 3D printing of mechanical components.
- [x] Virtual assembly in Fusion 360.
- [x] Kinematic simulation (Motion Study).
- [x] Sourcing electronic components (Servos, Controller, Power Supply).
- [x] Physical mechanical assembly.
- [x] Wiring and electronics integration.
- [x] Writing core control code (Forward/Inverse Kinematics).

## Hardware (Bill of Materials - TBD)
*This list will be updated as the project progresses.*
* **3D Printed Parts:** PLA/PETG (30% infill recommended for rigidity)
* **Actuators:** 3 x MG996R & 3 x MG90 MICRO
* **Microcontroller:** ESP32
* **Servo Controller:** I2C PCA9685
* **Position Control** 3 x XY Joystick Module

### Hardware Schematic

![Electrical Schematic](https://github.com/user-attachments/assets/6eb6785e-4bef-4326-b2a8-eec25f9a681f)


## Software
*Planned:* The control software will be written in C++ for ESP32. It will initially cover individual axis control, with future plans to implement an interface for calculating Inverse Kinematics (IK) for precise end-effector positioning.

## Hardware Challenges & Lessons Learned

### The 360° vs. 180° Servo Motor Issue
During the initial prototyping and testing phase of the robotic arm, an unexpected behavior was observed: the joints would rotate continuously at varying speeds instead of holding a specific targeted angle. When the joystick was released to its center position, the motors would not stop accurately.

**The Root Cause:**
The issue was traced back to the hardware type. The project inadvertently used **360-degree continuous rotation servos** instead of **180-degree positional servos** (which are frequently sold under the exact same SG90/MG996R model names by suppliers). 



The fundamental difference lies in the internal feedback loop:
* **Standard 180° Servo:** Contains an internal potentiometer. The ESP32 PWM signal dictates the **absolute position** (e.g., go to 90° and lock there). This is mandatory for a robotic arm to hold objects against gravity.
* **Continuous 360° Servo:** The potentiometer is removed or replaced with fixed resistors. The PWM signal dictates **speed and direction** instead of position. Thus, a "center" signal might cause a slow drift, and the system has no way of knowing the physical angle of the arm.

**The Solution:**
All joint actuators were audited and replaced with true **180-degree positional servos**. This hardware correction, combined with an incremental control logic in the ESP32 code (adding a deadzone for the joystick), allowed the robotic arm to accurately track inputs and firmly hold its position when idle.

---

*This README will be updated regularly with physical build photos and code snippets as the project evolves.*
