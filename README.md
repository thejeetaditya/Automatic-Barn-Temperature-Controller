# Automatic-Barn-Temperature-Controller

An ESP32-based smart livestock monitoring and barn temperature control system that integrates real-time sensing, automated Peltier-based cooling, livestock activity monitoring, milk quantity estimation, and IoT-based data visualization.

## Overview

This project is an ESP32-based smart livestock monitoring and barn temperature control system. The system integrates multiple sensors to monitor livestock temperature, barn temperature, physical activity, and milk quantity.

The ESP32 processes the sensor data and performs basic health-status classification and automated cooling control. A relay is used to control the Peltier-based cooling system when the barn temperature exceeds the defined threshold.

The system also uses Wi-Fi connectivity and Adafruit IO to provide remote monitoring of important parameters and system status.

## Objectives

- Develop an ESP32-based system for real-time livestock and barn monitoring.
- Monitor livestock body temperature and barn environmental temperature.
- Monitor livestock physical activity using an accelerometer.
- Estimate milk quantity using a load cell and HX711 module.
- Implement automated barn cooling using Peltier modules and relay control.
- Classify basic livestock health and activity conditions using sensor data.
- Enable remote monitoring of system parameters through Adafruit IO.

## System Architecture

The system consists of an ESP32 microcontroller connected to multiple sensors and actuators. Sensor data is processed by the ESP32 to determine livestock and environmental conditions and to control the cooling system.

```text
                    ┌──────────────────────┐
                    │        ESP32         │
                    │   Main Controller    │
                    └──────────┬───────────┘
                               │
          ┌────────────────────┼────────────────────┐
          │                    │                    │
          ▼                    ▼                    ▼
       LM35                  DHT11               MPU6050
   Livestock Temp          Barn Temp             Activity
          │                    │                    │
          └────────────────────┼────────────────────┘
                               │
                               ▼
                     Sensor Data Processing
                               │
              ┌────────────────┴────────────────┐
              │                                 │
              ▼                                 ▼
       Health Classification              Cooling Control
              │                                 │
              │                               Relay
              │                                 │
              │                                 ▼
              │                          Peltier Module
              │
              │
              └─────────────────────────────────┐
                                                │
                                                ▼
                                         System Monitoring

          HX711 + Load Cell
                  │
                  ▼
           Milk Quantity
             Estimation

                    ESP32
                      │
                      ▼
                Wi-Fi Network
                      │
                      ▼
                 Adafruit IO
                      │
                      ▼
              Remote Dashboard
```

## Hardware Components

| Component | Purpose |
|---|---|
| ESP32 | Main microcontroller and data-processing unit |
| LM35 | Livestock temperature measurement |
| DHT11 | Barn temperature measurement |
| MPU6050 | Livestock movement/activity monitoring |
| HX711 | Load-cell signal amplification and processing |
| Load Cell | Milk quantity measurement |
| Relay Module | Switching the Peltier cooling system |
| Peltier Module | Barn cooling |
| Power Supply | Provides power to the system components |

## Pin Configuration

| Component | ESP32 Pin |
|---|---|
| LM35 | GPIO 34 |
| MPU6050 SDA | GPIO 25 |
| MPU6050 SCL | GPIO 26 |
| DHT11 | GPIO 13 |
| Relay | GPIO 27 |
| HX711 DOUT | GPIO 21 |
| HX711 CLK | GPIO 22 |

## Working Principle

### 1. Livestock Temperature Monitoring

The LM35 is connected to the ESP32 analog input and is used to measure livestock temperature.

The ESP32 reads the sensor voltage through its ADC and converts the reading into a temperature value.

The measured temperature is then used as one of the parameters for basic health-status classification.

### 2. Barn Temperature Monitoring

A DHT11 sensor continuously measures the temperature inside the barn.

The measured barn temperature is compared with a predefined threshold to determine whether cooling is required.

### 3. Livestock Activity Monitoring

An MPU6050 accelerometer is used to monitor livestock movement.

The acceleration values along the X, Y and Z axes are processed by the ESP32 to calculate an approximate movement level.

Based on the measured movement, the system classifies activity into:

- Resting
- Walking
- Active

### 4. Health Status Classification

The system combines livestock temperature and activity information to determine a basic health status.

The implemented classifications include:

- Normal
- Fever Alert
- Low Temperature
- Low Activity

The health status is updated based on the sensor readings processed by the ESP32.

### 5. Automatic Peltier Cooling

The DHT11 barn temperature measurement is used to control the Peltier cooling system.

When the barn temperature exceeds the predefined threshold, the ESP32 activates the relay and turns the cooling system ON.

When the temperature falls below the threshold, the relay is switched OFF.

```text
Barn Temperature
       │
       ▼
   ESP32 reads
   temperature
       │
       ▼
 Is temperature
 above threshold?
     /     \
   YES      NO
    │        │
    ▼        ▼
Relay ON   Relay OFF
    │        │
    ▼        ▼
Peltier ON Peltier OFF
```

### 6. Milk Quantity Monitoring

A load cell connected through an HX711 module is used to estimate milk quantity.

The ESP32 reads the load-cell measurement and converts the measured value into an approximate milk quantity.

The system classifies the measured quantity as:

- Normal Milk
- Reduced Milk
- Low Milk

### 7. IoT Monitoring

The ESP32 connects to a Wi-Fi network and communicates with Adafruit IO.

The following parameters are sent to individual Adafruit IO feeds:

- Livestock temperature
- Barn temperature
- Activity
- Health status
- Cooling status

This allows the system parameters to be monitored remotely through an IoT dashboard.

## Software

- Arduino IDE
- Embedded C/C++
- ESP32
- Adafruit IO
- Wi-Fi
- MATLAB/Simulink (if used for supporting system analysis)

## Libraries Used

The project uses the following Arduino libraries:

```text
Wire
Adafruit MPU6050
Adafruit Unified Sensor
DHT
HX711
WiFi
Adafruit IO
```

## Control Logic

The system operates continuously using the ESP32 main control loop.

The overall sequence is:

```text
Start
  │
  ▼
Initialize Sensors
  │
  ▼
Connect to Wi-Fi / Adafruit IO
  │
  ▼
Read Livestock Temperature
  │
  ▼
Read Barn Temperature
  │
  ▼
Read Activity
  │
  ▼
Determine Health Status
  │
  ▼
Check Barn Temperature
  │
  ├──── Temperature High ────► Peltier ON
  │
  └──── Temperature Normal ──► Peltier OFF
  │
  ▼
Read Milk Quantity
  │
  ▼
Update IoT Dashboard
  │
  ▼
Repeat
```

## Adafruit IO Monitoring

The system uses separate feeds for different monitored parameters.

| Feed | Parameter |
|---|---|
| `cowtemperature` | Livestock temperature |
| `barntemperature` | Barn temperature |
| `activity` | Livestock activity |
| `healthstatus` | Health classification |
| `coolingstatus` | Peltier cooling status |

## Features

- Real-time livestock temperature monitoring
- Real-time barn temperature monitoring
- Activity classification
- Basic health-status classification
- Automatic Peltier cooling
- Milk quantity estimation
- Wi-Fi connectivity
- IoT-based remote monitoring
- Serial monitoring for debugging and system observation

## Project Structure

```text
Automatic-Barn-Temperature-Controller/
│
├── README.md
│
├── src/
│   └── peltier.ino
│
├── images/
│   ├── prototype.jpg
│   ├── circuit.jpg
│   └── dashboard.jpg
│
└── docs/
    ├── system_architecture.png
    └── circuit_diagram.png
```

## Setup and Installation

### 1. Install Arduino IDE

Install the Arduino IDE and configure the ESP32 development board.

### 2. Install Required Libraries

Install the following libraries through the Arduino IDE Library Manager:

- Adafruit MPU6050
- Adafruit Unified Sensor
- DHT sensor library
- HX711
- Adafruit IO Arduino

### 3. Configure Wi-Fi and Adafruit IO

Before uploading the program, provide your own Wi-Fi and Adafruit IO credentials.

Do not upload personal credentials or API keys to a public repository.

Example:

```cpp
#define WIFI_SSID       "YOUR_WIFI_SSID"
#define WIFI_PASS       "YOUR_WIFI_PASSWORD"

#define IO_USERNAME     "YOUR_ADAFRUIT_USERNAME"
#define IO_KEY          "YOUR_ADAFRUIT_IO_KEY"
```

### 4. Upload the Program

Connect the ESP32 to the computer, select the appropriate board and COM port in Arduino IDE, and upload the program.

### 5. Monitor the System

Open the Serial Monitor at:

```text
115200 baud
```

The system displays livestock temperature, barn temperature, activity, health status, cooling status and milk quantity.

## Future Improvements

- Implement PID-based temperature control for improved cooling regulation.
- Improve sensor calibration and measurement accuracy.
- Add more robust livestock health classification.
- Improve power management of the Peltier cooling system.
- Add data logging for long-term livestock monitoring.
- Develop a dedicated web or mobile monitoring interface.
- Add alerts for abnormal temperature, activity and milk production.
- Improve the control strategy using historical sensor data.

## Author

**Vijayaditya Annadurai**

Electrical and Computer Engineering Undergraduate  
Amrita Vishwa Vidyapeetham

- GitHub: [Add your GitHub profile]
- LinkedIn: [Add your LinkedIn profile]
- Email: vijayaditya.annadurai@gmail.com
- Classify basic livestock health and activity conditions using sensor data.
- Enable remote monitoring of system parameters through Adafruit IO.
