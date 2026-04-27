# Capstone
Code Storage for Capstone Project
# Pump Control System – Arduino + Python GUI

A safe, serial‑controlled pump management system with manual/automatic modes.  
The Arduino runs the pump (PWM control), and the Python GUI provides a user‑friendly interface.

---

## Table of Contents

- [Overview](#overview)
- [Hardware Requirements](#hardware-requirements)
- [Pin Connections (Arduino)](#pin-connections-arduino)
- [Software Requirements](#software-requirements)
- [Installation & Setup](#installation--setup)
- [How to Use](#how-to-use)
  - [Arduino Firmware](#arduino-firmware)
  - [Python GUI](#python-gui)
- [Serial Commands (for direct terminal use)](#serial-commands-for-direct-terminal-use)
- [Safety Features](#safety-features)
- [Troubleshooting](#troubleshooting)
- [Extending the System](#extending-the-system)

---

## Overview

| Component          | Role                                                                 |
|--------------------|----------------------------------------------------------------------|
| **Arduino**        | Receives serial commands, controls the pump via pin 5 (PWM).         |
| **Python GUI**     | Sends commands to the Arduino, displays logs, and provides buttons for ON/OFF/AUTO. |
| **Pump / Actuator**| Connected to the Arduino through a driver (MOSFET, relay, motor driver). |

The system supports:

- Manual pump ON / OFF  
- Automatic toggling with a user‑defined interval (seconds, minutes, hours, days)  
- Emergency RESET that stops the pump and clears auto mode  
- A background serial reader that logs all Arduino replies

---

## Hardware Requirements

- **Arduino board** (Uno, Nano, Mega, etc.)  
- **Pump / DC motor / solenoid**  
- **External driver** (relay module, MOSFET, or motor driver) – **DO NOT connect the pump directly to the Arduino pin**  
- **Flyback diode** (if the pump is inductive)  
- **USB cable** for programming and serial communication  
- **Power supply** for the pump (separate from the Arduino if current > 40 mA)

---

## Pin Connections (Arduino)

| Arduino Pin | Connected to                | Notes                                                                 |
|-------------|-----------------------------|-----------------------------------------------------------------------|
| **D5 (PWM)** | Control input of driver (MOSFET gate / relay IN) | Use a current‑limiting resistor (e.g. 220 Ω) on the gate.            |
| GND         | Ground of driver & pump power supply | Common ground between Arduino, driver, and pump supply.              |
| VIN (or external) | Driver VCC (if needed) | Some relay modules need 5 V from Arduino or an external supply.      |

> **Important:** Pin 5 must be capable of PWM if you want variable speed. For plain ON/OFF you can use any digital pin (but the code uses pin 5 and `analogWrite`).

---

## Software Requirements

- **Arduino IDE** (to upload the firmware)  
- **Python 3.7+** with the following packages:
Install pyserial via pip:
    ```bash
    pip install pyserial

Installation & Setup
1. Upload the Arduino firmware
Copy the provided Arduino code (the one with pumpPin = 5, loop(), etc.) into a new sketch.

Select your board and port in Arduino IDE.

Upload the code.

Open the Serial Monitor (115200 baud) to verify you see SYSTEM READY.

2. Run the Python GUI
Save the Python script as pump_control.py.

In a terminal, run:

bash
python pump_control.py
Select the correct COM port (Windows) or /dev/ttyUSB0 (Linux) / /dev/cu.usbmodem* (macOS).

Baud rate must be 115200 (matches the Arduino).

Click Connect. The GUI will automatically send MANUAL and DIR CCW to ensure a safe start.

Use the buttons to control the pump.
