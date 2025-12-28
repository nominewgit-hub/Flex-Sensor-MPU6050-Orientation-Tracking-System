# Flex Sensor + MPU6050 Orientation Tracking System

## 📌 Overview
This project implements a real-time motion tracking system using:
- A **Flex Sensor** for bend angle measurement
- An **MPU6050 (Accelerometer + Gyroscope)** for 3-axis orientation tracking
The system runs on **Arduino** and outputs live data to the serial monitor.  
It uses **sensor fusion (complementary filter)** and **active gyro drift compensation** to improve accuracy and stability.

---

## 🔧 Hardware Components
- Arduino Uno / Nano / Mega
- Flex Sensor
- MPU6050 (I2C)
- Resistors (for flex sensor voltage divider)
- Jumper wires
- Breadboard (optional)

---

## 📐 Features
✔ Flex sensor angle measurement (0° – 90°)  
✔ MPU6050 roll, pitch, yaw estimation  
✔ Complementary filter for sensor fusion  
✔ Active gyro drift learning and correction  
✔ Noise reduction using averaging  
✔ Real-time serial output  

---

## ⚙️ How It Works

### 1. Flex Sensor
- Reads analog values from pin **A0**
- Averages multiple samples for noise reduction
- Maps ADC values to a bending angle
- Outputs a **negative angle intentionally** (application-specific)

### 2. MPU6050
- Reads accelerometer and gyroscope data via I2C
- Gyroscope is calibrated at startup
- Orientation is calculated by integrating gyro data
- Accelerometer provides absolute reference
- A complementary filter combines both

### 3. Drift Compensation
- Detects when the system is stationary
- Learns gyro drift gradually
- Applies counter-drift correction automatically

---

## 🧮 Output Format
Data is printed to the Serial Monitor as:


## Code Structure & Functionality
The code implements a real-time sensor fusion system with the following components:

## 1. Flex Sensor Section
Reads analog input from pin A0 Averages 10 samples for noise reduction Calibrates between 0° (adc0 = 621.51) and 90° (adc90 = 388.05) bend anglesReturns negative angle (intentional for application-specific use)

## 2. MPU6050 IMU Section
Initialization: Wakes up the MPU6050 via I2C (address 0x68)
Calibration: Collects 300 gyro samples at startup to establish offsetsData Reading: Fetches raw accelerometer and gyroscope dataOrientation Tracking: Uses complementary filter (α = 0.98) combining:Gyro integration for short-term accuracy
Accelerometer absolute reference for long-term stability

## 3. Active Drift Compensation
Detects stationary periods (gyro rates < 0.04°/s)
Gradually learns and corrects gyro drift (learning rate = 0.002)Applies counter-drift to maintain accuracy over time

## 4. Output Format
Serial output: flex_angle, (roll, pitch, yaw)

## Key Technical Details
Sensor Fusion: Complementary filter balances gyro (98%) and accelerometer (2%)

# Timing:
 10ms loop with delta-time calculation for integration
Calibration: Manual ADC values for flex sensor (may need adjustment)

# Drift Learning: 
Adaptive compensation when device is still
Potential Issues/Improvements

# File Extension:
 Rename to .ino for Arduino IDE compatibility

# Flex Calibration:
 Hardcoded ADC values may not be universal

# I2C Error Handling:
 No checks for MPU6050 communication failures

# Memory Usage:
 Uses floating-point math (consider fixed-point for optimization)

# Yaw Calculation: 
Z-axis integration without magnetometer may drift over time


## Hardware Requirements
Arduino board with I2C support
Flex sensor on A0 with appropriate voltage divider
MPU6050 connected to SDA/SCL pins