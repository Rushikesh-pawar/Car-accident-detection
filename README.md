# Accident Detection and Alert System

An embedded systems project that detects vehicle accidents using a vibration sensor and automatically sends SMS alerts to emergency contacts (police, ambulance, family) over a GSM network. Built on the 8051 microcontroller as part of a Microcontrollers course project at KIT's College of Engineering, Kolhapur (B.Tech, Sem-V).

The motivation: in many road accidents — especially in regions with limited emergency response infrastructure — survival depends on how quickly help reaches the scene. This project explores a low-cost, low-power solution that any vehicle owner could retrofit, designed to send location-tagged alerts within seconds of a crash.

## Overview

When a vehicle is involved in a collision, the on-board vibration sensor crosses a configurable threshold. The 8051 microcontroller detects this signal, activates a buzzer, displays the accident status on a 16x2 LCD, and instructs the GSM module to transmit pre-formatted SMS messages to three registered phone numbers — typically a family member, an ambulance service, and a local police station.

The full flow from impact to first SMS dispatch is **under 5 seconds** in bench testing.

## System architecture

| Component | Role |
|---|---|
| **AT89C51 (8051) microcontroller** | Central controller — reads sensor input, drives LCD, controls GSM via UART |
| **Vibration sensor** | Detects impact; crosses threshold during a collision |
| **GSM modem (SIM800-class)** | Transmits SMS over cellular network using AT commands |
| **LCD module (16x2)** | Displays system state ("Initialising", "Vibration: NO/YES", "Sending MSG", "MSG Sent") |
| **Buzzer** | Activated on impact for local audible alert |
| **Power regulation** | 5V regulated supply for SIM card and microcontroller |

## What I implemented

- **Embedded C firmware** for the 8051 (compiled in Keil IDE), covering UART initialization, LCD driver routines, and the main accident-detection loop.
- **GSM communication layer** using AT commands (`AT+CMGF=1` for text mode, `AT+CMGS` for sending) — including character-by-character serial transmission and delay handling around modem responses.
- **LCD driver** for command/data writes, line addressing (0x80 / 0xC0), and status messaging throughout the workflow.
- **Three-recipient alert dispatch**: hardcoded phone numbers for fast response, with each SMS sent sequentially and confirmed via LCD update.
- **Hardware simulation** in Proteus before physical assembly, validating the circuit and firmware against expected behavior.

## Repository contents

- `accident_detection.c` — main firmware (Embedded C, 8051)
- `circuit/` — Proteus simulation files
- `report/` — academic report covering problem statement, system architecture, hardware description, and circuit diagrams

## Running it

You'll need:
- **Keil µVision IDE** to compile the firmware for the 8051
- **Proteus** to simulate the circuit, or physical hardware (8051 dev board, GSM module with active SIM, vibration sensor, 16x2 LCD, buzzer, 5V regulated supply)
- Update the three target phone numbers in the `#define NUMBER1/2/3` macros before flashing

## Limitations and what I'd do differently today

This is an undergrad project from 2019 and reflects the toolset of that course. A modern version would:

- Add a **GPS module** so the SMS includes coordinates (the original sends only "ACCIDENT DETECTED" — recipient has to guess location). The report does mention GPS as a planned extension.
- Replace the binary vibration threshold with an **MPU-6050 accelerometer** and a tuned threshold over a sliding window, to reduce false positives from potholes and speed bumps.
- Add a **30-second cancellation window** with a button so the driver can abort a false alarm before alerts go out.
- Use a more capable MCU (ESP32, STM32) for over-the-air updates, configurable contacts, and a companion mobile app.

Listing these honestly, since the project was scoped to demonstrate microcontroller-driven sensor-to-network integration, not a production safety system.

## Authors

Original team project at KIT's College of Engineering, Kolhapur (Sem-V Microcontroller course, 2019), under the guidance of Prof. V. B. Gundavade:

- Divyani Deepak Nangare
- **Rushikesh Pradeep Pawar** (this repository)
- Swarada Chandrashekhar Phadnis
- Shradhha Santosh Shigaonkar
