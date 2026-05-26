# 5G Teleoperated Driving System — Public Overview

Public technical overview of a teleoperated smart-car prototype controlled through a 5G network using a Raspberry Pi 4, SIM8200EA-M2 5G module, Xbox Series X controller, live camera streaming, Python socket communication, and real-time motor/servo control.

<p align="center">
  <img src="docs/assets/Fig%20-%20Car%20and%20Controller.jpeg" width="760" alt="Teleoperated smart car with 5G module and Xbox Series X controller" />
</p>

The full source code is private due to intellectual property considerations. This repository documents the project's functionality, architecture, technologies, screenshots, and implementation approach without exposing private source code, internal files, or full Git history.

---

## Demo Video

The demo video shows the smart car being controlled through the Xbox Series X controller while the laptop receives a live camera stream from the vehicle.

[Watch the demo video](docs/assets/A%20teleoperated%20driving%20system%20via%205G%20network%281%29.mp4)

> Note: GitHub may display the video as a downloadable media file instead of embedding it directly in the README.

---

## Project Summary

This project implements a small-scale teleoperated driving system using 5G connectivity for remote control and live video feedback.

The vehicle is built around a Raspberry Pi 4 Model B mounted on a custom 3D-printed smart-car chassis. The Raspberry Pi acts as the onboard server and receives control commands from a laptop client. The operator uses an Xbox Series X controller connected to the laptop, while the car receives driving commands over a 5G-based communication path.

The system also includes a Raspberry Pi Camera Module mounted on a servo motor, allowing the operator to view the environment around the car through a live video stream. The car can move forward and backward, steer through a servo-based direction system, and adjust the camera direction independently.

---

## Main Objectives

- Build and assemble a custom smart-car chassis capable of holding the Raspberry Pi, 5G module, camera, motors, power supplies, and wiring.
- Integrate a SIM8200EA-M2 5G module with a Raspberry Pi 4 Model B.
- Configure 5G-based network communication between the vehicle and the client device.
- Implement a server-client architecture for real-time command transmission.
- Read Xbox Series X controller inputs and convert them into driving commands.
- Control DC motors for propulsion and servo motors for steering and camera orientation.
- Stream live video from the Raspberry Pi camera to the operator.
- Test the system in outdoor and indoor 5G environments.

---

## Key Features

### Xbox Controller Teleoperation

The operator controls the smart car using an Xbox Series X controller. The analog trigger input is used for acceleration and the joystick is used for steering, creating a more realistic driving experience than simple on/off remote control.

### 5G-Based Remote Communication

The project uses a 5G communication module to connect the smart car to the network and enable remote command transmission. A virtual private network layer is used so the laptop client and Raspberry Pi server can communicate as if they were on the same local network.

### Raspberry Pi Server

The Raspberry Pi 4 Model B acts as the main onboard computer. It receives control commands, manages GPIO/PWM signals, controls the motors and servos, and streams the camera feed.

### Live Camera Feed

A Raspberry Pi Camera Module 2 NoIR is mounted on the car and provides live video feedback to the operator. The camera is mounted on a servo motor so it can be repositioned without moving the entire vehicle.

### Motor and Steering Control

The propulsion system uses DC motors controlled through an L298N H-bridge motor driver. Steering is handled through an MG90S servo motor, and a second servo motor is used for camera orientation.

### 3D-Printed Chassis

The chassis was modified and 3D printed to support the Raspberry Pi, 5G module, motor driver, power bank, battery holders, switches, camera mount, and the custom wiring layout.

### Indoor and Outdoor Testing

The project was tested outdoors using commercial 5G coverage and indoors using a private 5G environment. The experiments focused on responsiveness, latency, video stability, and real-time control behavior.

---

## Technologies and Components

### Software

| Area | Technologies / Concepts |
|---|---|
| Main language | Python |
| Network communication | Socket programming |
| Video streaming | Flask web server for camera feed |
| Virtual networking | ZeroTier virtual private network |
| Hardware control | Raspberry Pi GPIO and PWM |
| Controller input | Xbox Series X controller input handling |
| Operating system | Raspberry Pi OS / Raspbian |
| Connectivity setup | SIM8200EA-M2 driver configuration and AT commands |

### Hardware

| Component | Role |
|---|---|
| Raspberry Pi 4 Model B | Main onboard server and control unit |
| SIM8200EA-M2 5G HAT | 5G connectivity module for Raspberry Pi |
| Xbox Series X Wireless Controller | Operator control interface |
| Raspberry Pi Camera Module 2 NoIR | Live video feedback from the car |
| L298N H-bridge motor driver | DC motor speed and direction control |
| DC motors | Propulsion system |
| MG90S servo motors | Steering and camera orientation |
| 3D-printed chassis | Mechanical structure for the vehicle |
| Power bank | Power supply for Raspberry Pi and 5G module |
| 9V batteries | Power supply for motors / motor control components |
| Switches and battery holders | Power control and modular assembly |

---

## System Architecture

<p align="center">
  <img src="docs/assets/Fig%20-%205G%20Schematic.jpg" width="760" alt="5G client-server architecture between laptop, controller, Raspberry Pi, 5G module, and smart car" />
</p>

The architecture follows a client-server model:

```text
Xbox Series X Controller
        ↓
Laptop Client
        ↓
5G / Virtual Private Network
        ↓
Raspberry Pi Server on the Smart Car
        ↓
Motor Driver + Servo Motors + Camera Stream
```

The laptop client reads controller input and sends driving commands to the Raspberry Pi server. The Raspberry Pi receives the commands, maps them to motor and servo actions, and streams live camera footage back to the operator.

---

## Control Flow

```text
1. Operator moves the Xbox controller triggers or joystick.
2. The laptop client reads and processes the controller input.
3. The client sends command data to the Raspberry Pi server over the network.
4. The Raspberry Pi interprets the command packet.
5. GPIO/PWM signals are generated for the motors and servos.
6. The car moves, steers, or adjusts the camera direction.
7. The camera livestream gives visual feedback to the operator.
```

---

## Hardware Architecture

<p align="center">
  <img src="docs/assets/Fig%20-%20Schematic.png" width="760" alt="Wiring diagram for Raspberry Pi, 5G module, camera, L298N motor driver, DC motors, servo motors, power bank, switches, and batteries" />
</p>

The hardware design combines propulsion, steering, camera control, 5G connectivity, and independent power sections.

Main hardware subsystems:

- propulsion through two DC motors;
- motor speed and direction control through the L298N H-bridge;
- steering through a servo motor;
- camera rotation through a second servo motor;
- live video through the Raspberry Pi Camera Module;
- 5G communication through the SIM8200EA-M2 module;
- Raspberry Pi-based command processing;
- separate power handling for motors, Raspberry Pi, and 5G module.

---

## Software Architecture

The software implementation is organized around two main runtime sides.

### Client Side

The client runs on a laptop and is responsible for:

- reading Xbox Series X controller input;
- processing joystick, trigger, and button values;
- connecting to the Raspberry Pi server;
- sending command packets through socket communication;
- displaying or accessing the camera livestream.

### Server Side

The server runs on the Raspberry Pi mounted on the smart car and is responsible for:

- listening for incoming client connections;
- receiving command data from the laptop;
- converting received commands into motor/servo actions;
- controlling GPIO and PWM outputs;
- streaming live camera video through a Flask server;
- managing communication between the software and physical components.

---

## 5G Communication Setup

The project uses a SIM8200EA-M2 5G module connected to the Raspberry Pi. The setup required:

- selecting a compatible Raspberry Pi OS version;
- installing the 5G module drivers;
- checking generated network interfaces such as `wwan0`;
- verifying USB serial ports for the module;
- using AT commands to activate and configure the modem;
- connecting the Raspberry Pi to the 5G network;
- creating a virtual private network so the laptop and Raspberry Pi can communicate over the internet.

This setup allows the vehicle to be controlled without depending on a short-range Bluetooth or Wi-Fi-only connection.

---

## Project Media

### Developed Teleoperated Smart Car

<p align="center">
  <img src="docs/assets/Fig%20-%20Car%20and%20Controller.jpeg" width="720" alt="Developed teleoperated smart car with Raspberry Pi, 5G antennas, camera, wiring, and Xbox controller" />
</p>

### 5G Communication Schematic

<p align="center">
  <img src="docs/assets/Fig%20-%205G%20Schematic.jpg" width="760" alt="5G communication schematic showing client, server, laptop, controller, Raspberry Pi, 5G module, and smart car" />
</p>

### Hardware Wiring Schematic

<p align="center">
  <img src="docs/assets/Fig%20-%20Schematic.png" width="760" alt="Hardware wiring schematic for the Raspberry Pi, 5G module, motors, servos, camera, batteries, switches, and power bank" />
</p>

### 3D-Printed Chassis Parts

<p align="center">
  <img src="docs/assets/Fig%20-%20More%20Printed%20Parts.png" width="720" alt="3D printed chassis parts used for the smart car" />
</p>

### 3D-Printed Camera Protection and Stand

<p align="center">
  <img src="docs/assets/Fig%20-%20Camera%203D-Parts.png" width="720" alt="3D printed camera protection box and camera stand components" />
</p>

### Outdoor Experiment Setup

<p align="center">
  <img src="docs/assets/Fig%20-%20Experiment%20Outdoor.jpeg" width="560" alt="Outdoor experiment setup with Xbox controller, laptop, livestream, and 5G teleoperated smart car" />
</p>

### Indoor Experiment Setup

<p align="center">
  <img src="docs/assets/Fig%20-%20Experiment%20Indoor3.jpg" width="760" alt="Indoor private 5G experiment setup with smart car, laptop, controller, 5G module, and indoor RAN dots" />
</p>

### Outdoor Latency — Student Dormitory Area

<p align="center">
  <img src="docs/assets/Fig%20-%20Latency%20Camin.png" width="720" alt="5G latency result near the student dormitory area" />
</p>

### Outdoor Latency — University Rectorate Park

<p align="center">
  <img src="docs/assets/Fig%20-%20Latency%20Rectorat.png" width="720" alt="5G latency result near the university rectorate park" />
</p>

---

## Experimental Validation

The system was validated through outdoor and indoor tests.

### Outdoor Testing

The outdoor experiment tested the car in real conditions using 5G connectivity. The system was evaluated based on command responsiveness, video feed usability, and measured latency in different locations.

Two outdoor tests were documented:

- near the student dormitory area, with approximately 222 Mb/s maximum download, 16.8 Mb/s maximum upload, and around 41 ms minimum latency;
- near the university rectorate park, with approximately 174 Mb/s maximum download, 38.7 Mb/s maximum upload, and around 19 ms minimum latency.

### Indoor Testing

The indoor experiment used a controlled private 5G setup with indoor RAN dots. This environment provided more stable conditions and demonstrated that the system can maintain real-time responsiveness when a reliable 5G connection is available.

---

## Challenges Solved

Important challenges addressed during the project included:

- insufficient power for the DC motors;
- configuring the SIM8200EA-M2 module with Raspberry Pi;
- selecting a compatible Raspberry Pi OS version;
- solving USB compatibility and driver issues;
- creating a working virtual network between client and server;
- keeping camera streaming and command communication active at the same time;
- reducing data loss during transmission;
- improving servo precision and reset behavior.

---

## Future Improvements

Planned or proposed improvements include:

- adding an optical sensor for indoor 2D localization;
- adding headlights, stop lights, and turning signals;
- implementing a return-to-home mode;
- adding obstacle avoidance using ultrasonic or LiDAR sensors;
- developing a VR-based operator interface;
- improving telemetry and monitoring;
- extending the system toward more advanced teleoperation scenarios.

---

## Why This Project Matters

This prototype demonstrates how embedded systems, 5G networking, real-time control, and live video feedback can be combined into a practical teleoperated driving platform.

The project is not only a remote-controlled car. It is a complete small-scale teleoperation system that includes:

- real hardware integration;
- 5G connectivity;
- client-server communication;
- live video feedback;
- controller-based driving;
- physical motor and servo control;
- real-world testing;
- documented engineering trade-offs and improvements.

---

## Source Code Availability

The full implementation is private due to intellectual property considerations.

This public overview focuses on the system design, architecture, component integration, testing process, demo media, and implementation approach.

A technical walkthrough or selected implementation details can be provided upon request.
