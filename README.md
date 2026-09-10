# Automatic-Barn-Temperature-Controller

An ESP32-based smart livestock monitoring and barn temperature control system that integrates real-time sensing, automated Peltier-based cooling, livestock activity monitoring, milk quantity estimation, and IoT-based data visualization.

## Overview

This project is an ESP32-based smart livestock monitoring and barn temperature control system. The system integrates multiple sensors to monitor livestock temperature, barn temperature, physical activity, and milk quantity.

The ESP32 processes the sensor data and performs basic health-status classification and automated cooling control. A relay is used to control the Peltier-based cooling system when the barn temperature exceeds the defined threshold.

The system also uses Wi-Fi connectivity and Adafruit IO to provide remote monitoring of important parameters and system status.
