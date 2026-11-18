# 🔥 Temperature Control Module
### From System Identification to Real-Time Embedded Control

🚀 **Featured by Arduino**  
[Read the official article](https://blog.arduino.cc/2026/02/26/from-theory-to-hardware-cristian-castro-lagos-on-control-engineering-with-arduino/)


Engineering-grade closed-loop temperature control platform integrating ARX system identification, digital PI controller design, deterministic real-time implementation, and experimental validation using Arduino Leonardo.

![Platform](https://img.shields.io/badge/Platform-Arduino%20Leonardo-blue)
![Language](https://img.shields.io/badge/Language-C%2FC++-green)
![Control](https://img.shields.io/badge/Control-Digital%20PI-orange)
![Modeling](https://img.shields.io/badge/System%20Identification-ARX-purple)
![License](https://img.shields.io/badge/License-MIT-lightgrey)

## 🔎 Why This Project Matters

This platform demonstrates the complete professional workflow used in real-world control system development:

Data acquisition → System identification → Controller design → Discretization → Embedded implementation → Experimental validation.

It bridges control theory with physical hardware under real sampling, saturation, and noise constraints.


## 📖 Overview
This low-cost module was designed and built to perform hands-on experiments in Digital Control. The system is controlled using an Arduino Leonardo, heat is modulated with a TIP31C transistor, and temperature is measured using a TMP36 sensor.  

It provides a simple and practical platform for learning control theory in a laboratory environment.

## 📂 Contents
- `/HardwareV2` → New Version schematic, Gerbers and BOM+POS for PCBA.
- `/Hardware` → schematic, PCB, Gerbers.
- `/control_temp_LC` → C code for Arduino.

## 📊 Project Status

| Component | Status |
|------------|--------|
| Temperature Sensor Interface | ✅ Completed |
| Signal Filtering and Smoothing | ✅ Completed |
| PID Control Implementation | ✅ Completed |
| Migration from MATLAB Data Logging to Processing 4 GUI | ✅ Completed |
| Data Storage and CSV Export | ✅ Completed  |
| Processing 4 GUI (Monitoring & Logging) | ✅ Completed |
|  PCB Design with Improved Measurement Stability  | ✅ Completed |

> 💻 The project now features a Processing 4 graphical interface that replaces the previous MATLAB-based monitoring, data recording and data logging system.  
> 🔧 This new GUI allows real-time temperature monitoring and data recording, alongside the ongoing development of a custom PCB to improve measurement stability and reduce signal noise.

## ⚙️ System Description
- **Controller:** Arduino Leonardo or Mega  
- **Sensor:** TMP36 analog temperature sensor  
- **Actuator:** TIP31C transistor controlling a heating element  
- **Control Strategy:** Digital PI temperature regulation  
- **Sampling period:** 0.1 seconds  
- **Data transmission:** Serial communication for data logging and Processing 4 GUI monitoring  
- **PCB:** Custom-designed board for power and measurement stability  

## 🔄 Control Loop

<p align="center">
<img src="https://github.com/user-attachments/assets/af323e33-f114-40ee-a15d-8e0bd7f2776f" alt="Diagrama sistema" width="700">
</p>

### Controlled Variables
- **Temperature** → Temperature value control (stabilization)

## 📐 Digital PID Control

The PID controllers implemented in this project are **incremental (velocity form)** and use **trapezoidal integration** for the integral term.  
This ensures:
- Accurate discrete-time implementation suitable for microcontrollers.
- Consistency between simulation and embedded hardware behavior.
- Avoidance of integral windup when combined with actuator saturation or anti-windup mechanisms.

The Module uses a discrete PI controller implemented on a Arduino microcontroller.  
The control law in the digital domain is expressed as:

$$
u(n) = u(n-1) + K_0 e(n) + K_1 e(n-1)
$$

Digital PI controller implemented for temperature measured,

$$
V_{PWM}(n) = V_{PWM}(n-1) + K_0 e(n) + K_1 e(n-1)
$$

### Parameters:
The parameters are adjusted for temperature measured,

$$
K_0 = K_p + \frac{K_p}{2T_i} T_s
$$

$$
K_1 = -K_p + \frac{K_p}{2T_i} T_s
$$

## 🔉 Signal Processing: Low-Pass IIR Filter (1st Order)

To reduce measurement noise, a **first-order IIR low-pass filter** was applied to the temperature signal before feeding it to the controller and the ARX model.  

### 🔹 Filter Equation  

$$
y(k) =(1 - \alpha)  \cdot x(k) +  \alpha \cdot y(k-1)
$$  

Where:  
- $$x(k)$$: raw sensor measurement at time step $$k$$  
- $$y(k)$$: filtered output  
- $$\alpha$$: smoothing factor, $$(0<\alpha<1)$$ 


## 🧪 PI Control Test and ARX Model Comparison  

A test was performed using a **PI controller** applied to the temperature control module.  
The experimental results were compared against a **linear ARX model** identified from system data.  

### 🔹 General ARX Model Equation  

The ARX (Auto-Regressive with eXogenous input) model is defined as:  

$$
y(k) + a_1 y(k-1) + a_2 y(k-2) + \dots + a_{n_a} y(k-n_a) =
b_1 u(k-1) + b_2 u(k-2) + \dots + b_{n_b} u(k-n_b) + e(k)
$$  

Where:  
- $$y(k)$$: system output at sample $$k$$ 
- $$u(k)$$: input signal at sample $$k$$  
- $$( a_i, b_j)$$: ARX parameters  
- $$( n_a, n_b)$$: model orders  
- $$e(k) $$: disturbance/noise  

### 🔹 Results Overview  
- ✅ The PI controller improved **temperature tracking** and reduced overshoot.  
- 📉 The ARX model captured the main dynamics, with small discrepancies due to nonlinear heat transfer effects.  
- ⚖️ The comparison confirms the applicability of ARX models for **thermal system control design**.  


### 🔹 Experimental Plots  

<p align="center">
<img src="https://github.com/user-attachments/assets/ce3281f3-fd1a-4895-82a5-0ad519f0561b" alt="ARX vs Experimental Comparison (Temperature)" width="500">
</p>

## 🖥️ GUI — Monitoring and Data Logging

The graphical user interface (GUI) shown in the photograph is developed using **Processing 4**.  
It is designed **only for monitoring** the temperature module in real time and for **recording experimental data**.  

<p align="center">
<img src="https://github.com/user-attachments/assets/aeaa1f78-48c5-424e-8d03-00733b628413" alt="GUI preview" width="500">
</p>

Key features:
- Real-time plot of temperature and setpoint.
- Display of control output (PWM or equivalent).
- Logging of measurements to files for offline analysis.
- Simple visualization of system behavior during experiments.

> ⚠️ **Note:** The GUI is for observation and data recording only; it does **not** modify the control system or send commands to the hardware.

> ⚠️ **Status:** Work In Progress — the GUI is being actively developed and will be added to the repository once sufficiently stable.


## 📉 Measurement Stability Improvement — PCB v2 vs PCB v3

The new PCB revision focused on improving the **temperature signal stability** without changing the control algorithm.

### 🔧 Issues Observed in PCB v2
In the previous revision, the temperature reading from the TMP36 sensor showed perturbations during heater actuation by the TIP31C transistor.  
This was mainly due to lack of proper analog stabilization near the sensor.

### ✅ Modifications Introduced in PCB v3
To address this, the following hardware changes were implemented according to TMP36 manufacturer guidelines:

- ✔ **Decoupling capacitor added** close to TMP36 power pins
- ✔ **Analog conditioning resistors changed to SMD**
- ✔ **All related passive components migrated to SMD footprint** to minimize parasitics and noise pickup

No changes were made to the controller firmware or sensor type — only the PCB-level conditioning was improved.

### 📊 Comparative Experimental Results

| Version | Condition | Plot |
|--------|------------|------|
| **PCB v2 (Old)** | Visible ripple & switching noise on temperature signal | <img src="https://github.com/user-attachments/assets/3ed6fd51-2adb-488d-b22b-0521f7768437" width="600"> |
| **PCB v3 (New)** | Stable and smooth measurement — disturbances eliminated | <img src="https://github.com/user-attachments/assets/dc860728-dc21-43cf-b59e-123ecb7ae722" width="600"> |

> ✅ The addition of the capacitor and SMD passives significantly reduced noise, improving the quality of temperature measurements and the overall stability of the closed-loop response.


## 🖼️ Render 3D PCB 

<table>
  <tr>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/d7cba087-c42c-4e02-afc7-a82ab77ad624" alt="PCB Render - Version 3" width="500"><br>
      <sub>PCB Render - Version 3 - NEW VERSION </sub>
    </td>
    <td align="center">
        <img src="https://github.com/user-attachments/assets/6689b137-0e90-4bba-a3c9-acc6b8084fe7" alt="PCB Render - Version 2" width="550"><br>
      <sub>PCB Render - Version 2- OLDER VERSION </sub>
    </td>
  </tr>
</table>

## ⚡ Physical Prototype

The updated PCB and system assembly have been tested successfully.  
The following image shows the complete setup of the **new prototype version**:

<p align="center">
  <img src="https://github.com/user-attachments/assets/3754cb93-b701-40c8-a1e7-902c2dc6c7cc" alt="Complete Prototype Setup - Version 3" width="400">
</p>

The following images show the complete setup of the **previous prototype version**:

<table>
  <tr>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/c50fd3b0-34b9-4b80-b290-66d5855d7d41" alt="Foto del prototipo" width="400"><br>
      <sub>Device connections</sub>
    </td>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/ebcdc46f-b9ea-460e-a8e4-280b313c55d5" alt="Foto del prototipo" width="400"><br>
      <sub>Complete setup</sub>
    </td>
  </tr>
</table>


## 🔗 ROS 2 Integration

This embedded temperature control module can be connected to a ROS 2 system
for supervision, telemetry streaming, remote setpoint control and online metrics.

👉 **ROS 2 Bridge Repository:**  
[Ros integration](https://github.com/dev2439/Temp_ControlMod_ROS_integration)

The ROS integration provides:

- UART ↔ ROS 2 bridge
- Real-time topics (/temp, /u, /ref)
- Remote setpoint commands
- Online control performance metrics (IAE, ISE, ITAE, etc.)
- rosbag logging and visualization tools

This separation keeps:
- this repo → embedded/firmware/control
- ROS repo → middleware/supervision/integration


## 📜 License
MIT License  
