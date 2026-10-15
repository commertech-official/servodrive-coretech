# IoT Edge Platform with ATmega2560 and ESP8266

A pet project exploring a complete IoT stack — from a resource-constrained 8-bit microcontroller to a Linux-based local gateway and cloud.

## Overview

This project demonstrates a modular IoT architecture where an **ATmega2560** handles sensors and actuators, an **ESP8266** provides Wi-Fi/MQTT connectivity via UART, and a **local Linux server** acts as an MQTT broker, data logger, and cloud bridge. It supports telemetry (e.g., temperature), remote commands, and motorized valve control using a **NEMA 17 stepper motor with a planetary gearbox**.

## Architecture

- **Edge layer**: ATmega2560 reads sensors (e.g., DS18B20), controls relays and stepper drivers, and sends simple text commands over UART to the ESP8266.
- **Gateway layer**: ESP8266 runs an MQTT client, publishes/subscribes to topics, and handles Wi-Fi reconnection.
- **Local server**: Linux single-board computer (e.g., Raspberry Pi) runs Mosquitto MQTT broker, a Python bridge service, and a local database.
- **Cloud**: Aggregated data is forwarded via MQTT bridge or REST API.

## Hardware

- Arduino Mega 2560 with integrated ESP8266 (or separate modules)
- NEMA 17 stepper motor + planetary gearbox (4:1, 5:1, 10:1, 20:1, or 50:1)
- Motorized gate / butterfly valve with a custom coupling
- DS18B20 temperature sensor (or similar)
- Stepper driver (A4988, DRV8825, or TMC2209)

## Software Stack

- **Firmware**: C++ (Arduino framework) for ATmega2560; C++ for ESP8266 (PubSubClient)
- **Broker**: Eclipse Mosquitto
- **Bridge**: Python 3 with paho-mqtt
- **Database**: SQLite (or InfluxDB for time-series data)
- **Cloud**: MQTT bridge / REST API (configurable)

## MQTT Topics

- `device/{id}/sensor/{type}` — telemetry (e.g., `device/atmega1/sensor/temp`)
- `device/{id}/cmd` — commands to edge
- `device/{id}/status` — online/offline/heartbeat
- `cloud/{id}/cmd` — commands from cloud

## Features

- Reliable UART protocol between ATmega and ESP8266
- MQTT publish/subscribe with reconnection logic
- Local data logging and cloud forwarding
- Motorized valve control with optional position feedback
- Modular design: swap sensors, actuators, or cloud backends

## Getting Started

1. Flash ATmega2560 with sensor/actuator firmware.
2. Flash ESP8266 with MQTT client firmware.
3. Install and configure Mosquitto on the Linux server.
4. Run `bridge.py` to subscribe to topics and log to SQLite.
5. Verify with `mosquitto_sub -t 'device/atmega1/sensor/temp'`.

## Roadmap

- [ ] Add TLS/authentication for MQTT
- [ ] Implement OTA updates for ESP8266
- [ ] Add web dashboard for real-time monitoring
- [ ] Support multiple edge devices and dynamic registration
- [ ] Integrate a cloud IoT platform (AWS IoT, ThingsBoard, etc.)
