# 🤖 Two-Wheeled Self-Balancing Robot (ESP32)

**Author:** Vivan Vishal Jadhav  


## Project Overview
This project is an inverted pendulum robot powered by an ESP32 microcontroller, an MPU6050 6-axis IMU, and an L298N motor driver. The core objective is to maintain vertical equilibrium using a custom tuned 'Proportional Integral Derivative' (PID)controller.

---

## Hardware & Chassis Upgrade
Achieving balance in an inverted pendulum relies heavily on physical geometry. A significant part of this project involved upgrading and optimizing the chassis weight distribution to give the motors maximum headroom.

* **The Original Issue:** The initial build suffered from severe center of mass misalignment. The heavy components caused a steep forward/backward tilt, forcing the TT motors to run at near-maximum capacity just to fight gravity, leaving zero overhead power to actually correct a fall.
* **The Upgrade:** The electronics were rehoused into a rigid tin chassis to eliminate flex. Most importantly, the heaviest components (the dual 18650 Li-ion battery pack) were shifted to sit **dead-center over the wheel axles**. 
* **The Result:** This hardware modification allows the robot to rest naturally at a near 0-degree angle during a "pinch test," meaning gravity does zero work when the robot is upright. The motors can now dedicate 100% of their torque to rapid PID corrections.

---

##  The Balancing Mechanism

### 1. Sensor Data & Filtering (MPU6050)
The robot determines its current tilt using an MPU6050. Because raw accelerometer data is incredibly noisy (especially with motor vibrations) and gyroscope data drifts over time, a **Complementary Filter** is applied. This filter relies 98% on the gyroscope for fast, short-term reactions, and 2% on the accelerometer to correct long-term drift, outputting a clean, stable pitch angle.

### 2. The PID Controller
A control loop running strictly at 100Hz calculates the error between the robot's current tilt and its desired `setpoint` (perfectly upright).
* **Proportional (Kp):** The "muscle." Provides immediate motor power directly proportional to how far the robot is falling.
* **Integral (Ki):** The "memory." Accumulates micro-errors over time to correct slight mechanical drifts (currently minimized to prevent windup).
* **Derivative (Kd):** The "brakes." Calculates the speed of the fall. As the robot swings back toward the center, the derivative term artificially slows the motors down to prevent overshooting (the "expanding wobble").


---

