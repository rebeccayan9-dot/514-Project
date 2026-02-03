# FocusFlow: AI Desktop Wellness Monitor

**FocusFlow** (formerly Rest Radar) is a privacy-first, AI-powered workspace companion designed to help you balance focus and rest. Unlike intrusive software notifications, it uses a physical desktop gauge to visualize your sedentary time and encourages healthy breaks through calm, ambient feedback.


## 📖 Overview

The system consists of two wireless desktop nodes:
1.  **The Vision Node (Sensor):** A smart camera that sits on your desk. It uses Edge AI to detect if you are present and recognizes hand gestures for touch-free control.
2.  **The Gauge Node (Display):** A mechanical dial that fills up as you work (0-60 mins). It provides a physical representation of your "focus stamina" and alerts you when it's time to stretch.

> **Privacy Note:** All video processing is done locally on the device (Edge Computing). No video or images are ever stored or transmitted.

## ✨ Key Features

* **👀 Presence Detection:** Automatically starts the timer when you sit down and pauses when you leave.
* **👋 Gesture Control:** Control the timer without breaking your flow:
    * **✋ Palm:** Pause/Resume Timer (e.g., for phone calls).
    * **👌 OK Sign:** Reset Session (finish a break).
* **🕰 Analog Visualization:** A stepper motor gauge provides a distraction-free, quick-glance status of your session.
* **💡 Ambient Feedback:** An RGB LED ring indicates daily focus achievements and connection status.

## 🛠 Hardware Architecture

The project uses a split **Sensor-Display** architecture communicating via **BLE (Bluetooth Low Energy)**.

### 1. Vision Sensor Node (Transmitter)
* **Core:** Seeed Studio XIAO ESP32C3
* **AI Module:** Grove Vision AI Module V2
* **Camera:** OV5647 (Connected via CSI ribbon)
* **Power:** USB-C (Continuous power)

### 2. Gauge Display Node (Receiver)
* **Core:** Seeed Studio XIAO ESP32C3
* **Actuator:** 28BYJ-48 Stepper Motor + ULN2003 Driver
* **Input:** Capacitive Touch Button / Physical Button (SW1) for manual backup
* **Feedback:** WS2812B RGB LED
* **Power:** 3.7V LiPo Battery (Portable)

## 📸 Wiring & Schematics

### Vision Sensor Pinout
| XIAO Pin | Connected To | Function |
| :--- | :--- | :--- |
| **D4** | Vision AI (D4) | I2C SDA |
| **D5** | Vision AI (D5) | I2C SCL |
| **3V3** | Vision AI (3V3) | Power |
| **GND** | Vision AI (GND) | Ground |

### Display Gauge Pinout
| XIAO Pin | Connected To | Function |
| :--- | :--- | :--- |
| **D0 - D3** | ULN2003 (IN1-IN4)| Stepper Motor Control |
| **D6** | RGB LED (DIN) | Status Light |
| **D7** | Button (SW1) | Manual Input |
| **BAT+/-** | LiPo Battery | Power |

> **Schematic Reference:**
> Please check the `/images` folder for detailed wiring diagrams:
> * `sensor_schematic.png` (Vision Node)
> * `display_schematic.png` (Gauge Node)

## 🧠 System Logic

1.  **Idle:** System waits for user presence.
2.  **Active:** Camera detects user -> Timer starts incrementing.
    * *Gauge moves 0% -> 100% (Green to Red zone).*
3.  **Interaction:**
    * User shows ✋ -> Timer Pauses (LED blinks Blue).
    * User shows 👌 -> Timer Resets to 0 (LED blinks Green).
4.  **Alert:** Timer hits 60 mins -> LED pulses Red -> Motor vibrates needle to signal break time.
5.  **Away:** User leaves desk > 5 mins -> Auto-Reset.

## 📦 Bill of Materials

* 2x Seeed Studio XIAO ESP32C3
* 1x Grove Vision AI Module V2
* 1x OV5647 Camera Module
* 1x 28BYJ-48 Stepper Motor & ULN2003 Driver Board
* 1x WS2812B RGB LED
* 1x 3.7V LiPo Battery
* 3D Printed Enclosures (STL files in `/3d_models`)

## 📂 Project Structure
