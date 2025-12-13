# AIRMAN – Level 2 AHRS Telemetry System

**Author:** Prajen
**Role:** Embedded / Firmware Engineer – Technical Assessment (Level 2)
**Submission:** AHRS computation + enhanced telemetry + real-time visualization

---

## 📌 Overview

This project implements the **Level-2 Advanced Extension** of the AIRMAN firmware assessment.

It builds on a telemetry pipeline by adding a **full Attitude and Heading Reference System (AHRS)**, enhanced telemetry framing, robust checksum validation, and real-time visualization.

The system closely resembles a **real embedded flight-control / robotics telemetry stack**, covering:
- Sensor fusion
- Orientation estimation
- Communication robustness
- Ground-station style visualization and logging

---

## ✔ Features Implemented

### **1. AHRS Algorithm (C – Madgwick Filter)**

A **Madgwick AHRS filter** is implemented to estimate orientation using:
- Accelerometer
- Gyroscope
- Magnetometer



**Outputs:**
- Roll (deg)
- Pitch (deg)
- Heading / Yaw (deg)

Key characteristics:
- Quaternion-based orientation tracking
- Continuous normalization for numerical stability
- Suitable for real-time embedded execution
- Widely used in UAVs, robotics, and IMU systems

This satisfies the Level-2 requirement for a **recommended AHRS algorithm**.

---

### **2. Enhanced Level-2 Telemetry Protocol**

Telemetry frames are transmitted at **20 Hz (50 ms)** in the following format:

`$L2,<timestamp_ms>,<roll>,<pitch>,<heading>,<alt>,<temp>*<CHK>`

Where:
- `$` → Start of frame
- `*` → Start of checksum
- `<CHK>` → **CRC16-CCITT checksum**

Compared to Level-1:
- XOR checksum is replaced with **CRC16** for stronger error detection
- Payload carries **AHRS outputs** instead of raw IMU values

This mirrors real-world embedded telemetry protocols used in aerospace and robotics.

---

### **3. Real-Time Visualization & Logging (Python)**

A Python-based receiver performs:
- Frame reception via **STDIN (pipe mode)**
- **CRC16 checksum validation**
- Parsing of AHRS telemetry
- Deterministic logging into **CSV**
- Real-time plotting (≥10 Hz) using Python visualization tools

The receiver behaves like a **lightweight Ground Control Station (GCS)** used during system integration and testing.

---

## 📁 File Structure

```yaml
level2/
│
├── ahrs.c / ahrs_filter.c # Madgwick AHRS implementation
├── telemetry_tx_level2.c # Level-2 telemetry transmitter (C)
├── uart_rx_level2_logger.py # Python telemetry receiver & CSV logger
├── plot_live.py # Real-time AHRS visualization
├── level2_telemetry.csv # Generated flight log
└── README.md # Documentation (this file)
