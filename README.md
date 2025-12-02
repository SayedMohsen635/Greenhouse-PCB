# Smart Greenhouse PCB System
This repository includes the design files and implementation of a custom Greenhouse Control PCB developed to automate and regulate key environmental functions in a greenhouse system.

---

## ✨ Features

[cite_start]The smart greenhouse system is designed to provide comprehensive, automated environmental control[cite: 10, 11].

* [cite_start]**Automated Environmental Monitoring:** Continuously monitors crucial parameters for plant health, including air temperature, humidity, $Co_{2}$ concentration, and light intensity[cite: 4, 22, 23, 24, 25].
* [cite_start]**Actuator Control:** Controls actuators such as fans, heaters, motors, misting systems, and LEDs[cite: 14]. This includes controlling:
    * [cite_start]Heating (Ceramic Heaters) [cite: 27]
    * [cite_start]Cooling and Ventilation (Fans) [cite: 28]
    * [cite_start]Humidity (Misting Systems) [cite: 29]
    * [cite_start]Grow lights and shading systems (Motors and LEDs) [cite: 30]
* [cite_start]**Dual Control Modes:** Supports both automated logic and user-friendly manual control[cite: 6, 11].
    * [cite_start]**Automatic Mode:** Central **ESP32-WROOM-32D** microcontroller manages data acquisition and control logic[cite: 5, 37].
    * [cite_start]**Manual Override:** Switches/buttons allow the user to manually turn actuators on/off for testing or in case of system failure[cite: 17, 31].
* [cite_start]**Scalability:** The design provides scalability for larger greenhouse applications[cite: 16].

---

## 🛠️ System Architecture & Components

### 1. Central Microcontroller

[cite_start]The system is centered around the **ESP32-WROOM-32D** module[cite: 20, 37].

* [cite_start]**Key Functions:** Selected for its integrated Wi-Fi and Bluetooth connectivity, sufficient processing capability, and wide range of peripheral interfaces[cite: 37]. [cite_start]It manages reliable monitoring and control of environmental parameters[cite: 38].
* [cite_start]**Low-Power Features:** Ensures energy efficiency[cite: 39].

### 2. Sensors

[cite_start]The PCB is designed to interface with the following sensors[cite: 41]:

| Parameter | Sensor (Initial Selection) | Key Specifications |
| :--- | :--- | :--- |
| Temperature & Humidity | **DHT22** | [cite_start]Operates at 5V, output is 40-bit digital data, accuracy is $\pm0.5^{\circ}C$, $\pm2\%RH$[cite: 64, 65, 67, 72]. |
| $CO_{2}$ Concentration | **MQ-135** | [cite_start]Used to measure $Co_{2}$ in air[cite: 43]. [cite_start]*(Note: MQ-135 is generally for air quality; MH-Z19B is the recommended alternative for dedicated $CO_{2}$ measurement)*[cite: 80, 78]. |
| Light Intensity | **BH1750** | [cite_start]Used to measure light intensity[cite: 44]. [cite_start]*(Note: TSL2561 is the recommended alternative for better accuracy and IR/Visible light separation)*[cite: 88, 83]. |

### 3. Actuators and Relays

[cite_start]Actuators are connected via relays for switching high loads[cite: 45]. The system uses:

* [cite_start]**Solid State Relays (SSR) - RE47:** Two SSRs are used, specifically for the 220V ceramic heaters, as they are precise, great for resistive loads, and allow fast switching (PWM)[cite: 51, 55, 105].
    * [cite_start]**Output:** 24-380V AC, 40A max current[cite: 56, 57].
* [cite_start]**Electromagnetic Relays (SLA30):** Eight electromagnetic relays are used for loads, specifically for AC Motor (shading system) and AC Fan (cooling), connected to a contactor before the load[cite: 60, 106].
    * [cite_start]**Max Load:** AC 250V/30A, DV 30V/30A[cite: 61].

---

## 💡 Control Modes and Status Indicators

### Control Modes

[cite_start]Control is managed by **four SPDT switches**[cite: 108, 90]:

* [cite_start]**Global Mode Switch:** A single SPDT switch determines the operating mode (**Automatic** vs. **Manual**)[cite: 93].
* [cite_start]**Actuator Override Switches:** Three actuator-specific SPDT switches enable manual override of the **fan**, **motor**, and **heater**[cite: 94].
* [cite_start]**Safety:** All logic-level signals from the switches are processed by the ESP32, ensuring safe, low-voltage control[cite: 96].

### Status Indicators

[cite_start]The PCB includes indicator LEDs to provide system feedback[cite: 110]:

* [cite_start]**Power/Start LED (Green):** Indicates that the ESP32 board is powered and running[cite: 98].
* [cite_start]**Error LED (Red):** Signals system fault or abnormal operation[cite: 99].
* [cite_start]**Alarm LED (Yellow/Orange):** Activates when environmental thresholds (e.g., high temperature, $CO_{2}$) are exceeded[cite: 100, 102].
* [cite_start]**Actuator Status:** Small LEDs indicators show the on/off status of the actuators[cite: 33, 109].

---

## ⚡ Power System Design

[cite_start]The system requires a stable power distribution network[cite: 114].

* [cite_start]**Primary Power Source:** 220V AC mains input[cite: 116].
* **Step-down Conversion:**
    * [cite_start]220V AC to **12V DC** power supply module for relays and contactors[cite: 118].
    * [cite_start]12V to **5V DC** buck converter to power sensors (e.g., DHT22, MH-Z19B, TSL2561)[cite: 119].
    * [cite_start]12V to **3.3V** LDO or DC-DC buck converter to supply the ESP32-WROOM-32D[cite: 120].
* **Isolation and Protection:**
    * [cite_start]Fuses and circuit breakers for overcurrent protection[cite: 122].
    * [cite_start]**Optocouplers** for isolation between ESP32 GPIOs and relays/SSR[cite: 123].
    * [cite_start]Decoupling capacitors to reduce noise near microcontrollers and sensors[cite: 124].
* [cite_start]**Controls:** Dedicated reset push-button and a reboot button (used with reset for programming mode) for the ESP32[cite: 126, 127, 128].

---

## 🖼️ Design Screenshots

### PCB Schematic

The schematic illustrates the connections between the ESP32-Wroom-32D, sensors, relays, and power management circuits.
![Smart Greenhouse PCB Schematic](images/schematic.png)

### PCB Design

These images shows the finalized component placement and copper trace routing of the custom PCB.
![2D PCB Layout](images/2d_pcb_layout.png)
![3D PCB Layout_Front](images/3d_pcb_layout_1.png)
![3D PCB Layout_Back](images/3d_pcb_layout_2.png)
