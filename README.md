# Industrial-Vibration-Monitoring-Gateway
# Industrial Vibration Monitoring Gateway

## Overview

The **Industrial Vibration Monitoring Gateway** is a proposed embedded hardware system for monitoring vibration and temperature in industrial machinery. It combines a three-axis vibration sensor, a PT100 temperature sensor interface, embedded processing, a local OLED display, and wireless data transfer.

The schematic and four-layer PCB are being developed in **Altium Designer**. This README describes the intended design; measured performance and final hardware validation are not yet claimed.

## Objectives

- Measure three-axis vibration using the ADXL354 precision analog-output accelerometer.
- Measure temperature using a PT100 RTD and MAX31865 RTD-to-digital converter.
- Use the ESP32-P4 as the main controller for sensor acquisition and data handling.
- Display measurements and status on an OLED display.
- Transfer measurement data to an ESP-12F Wi-Fi module over UART.
- Convert a 12 V DC input to the intended 3.3 V supply using an MP1584 buck converter.
- Include input protection and power/status indication.
- Develop the circuit schematic and four-layer PCB in Altium Designer.

## System Architecture

### Power path

**12 V DC input → Input protection → MP1584 buck converter → 3.3 V supply → Controller and peripherals**

### Measurement and communication paths

- **Vibration:** ADXL354 analog X, Y, and Z outputs connect to compatible ESP32-P4 ADC inputs.
- **Temperature:** PT100 connects to the MAX31865, which communicates with the ESP32-P4 over SPI.
- **Local display:** The ESP32-P4 updates the OLED over I²C.
- **Wireless link:** The ESP32-P4 transfers formatted readings to the ESP-12F over UART; the ESP-12F provides Wi-Fi connectivity.

## Hardware Components

| Component | Role in the design |
|---|---|
| ESP32-P4 | Main controller for sensor acquisition, processing, display updates, and communication |
| ADXL354 | Measures acceleration/vibration along the X, Y, and Z axes using analog outputs |
| PT100 RTD | Measures machine-surface temperature through its temperature-dependent resistance |
| MAX31865 | Interfaces the PT100 and provides digital measurement data over SPI |
| ESP-12F | Provides Wi-Fi connectivity; receives data from the ESP32-P4 over UART |
| OLED display | Displays measurements and system status locally over I²C |
| MP1584 buck converter | Steps the 12 V DC input down to the intended 3.3 V rail |
| Input protection circuit | Planned input fuse, reverse-polarity protection, TVS surge protection, and filtering |
| Status LEDs | Provide power indication and firmware-controlled status indication |
| CP2102N USB-to-UART bridge | Included in the report for USB serial programming/debugging; confirm whether it remains in the final design |

## Communication Interfaces

| Interface | Connection | Purpose |
|---|---|---|
| Analog input / ADC | ADXL354 → ESP32-P4 | Acquire vibration signals |
| SPI | ESP32-P4 ↔ MAX31865 | Read temperature-converter data |
| I²C | ESP32-P4 ↔ OLED | Update the display |
| UART | ESP32-P4 ↔ ESP-12F | Transfer measurement data |
| Wi-Fi | ESP-12F ↔ network | Wireless data transmission |

## Power Design

The planned input is **12 V DC**, with an **MP1584 buck converter** intended to generate a **3.3 V rail**. Input protection is planned ahead of the converter.

Before fabrication, verify the regulator reference design, output-voltage setting, inductor saturation current, diode and capacitor ratings, thermal performance, protection-device ratings, and the supply's ability to support the ESP-12F's Wi-Fi current peaks. The stated supply and current figures are design targets, not measured results.

## PCB Design

The schematic and PCB are being developed in **Altium Designer**. The proposed four-layer stack-up is:

1. **L1 — Top:** Signals and components
2. **L2 — Ground:** Ground plane
3. **L3 — Power / routing:** Power plane or selected routing
4. **L4 — Bottom:** Signals

The ground plane is intended to provide a low-impedance return path. Keep the ADXL354 analog routes away from switching-regulator nodes and other noisy signals. Confirm the finished stack-up and board thickness with the PCB manufacturer's fabrication specifications.

## Planned Firmware Workflow

1. Power up and initialise the ESP32-P4.
2. Initialise the accelerometer path, MAX31865, OLED, and UART.
3. Sample the ADXL354 analog outputs at a suitable rate.
4. Read PT100 temperature data through the MAX31865.
5. Validate and process the readings.
6. Update the OLED with measurements and system status.
7. Send formatted measurements to the ESP-12F over UART.
8. Transmit data over Wi-Fi using the protocol and receiver selected during firmware development.
9. Report sensor or communication errors through the display and/or status LED.

The sampling rate, calibration procedure, data format, wireless protocol, destination server, and alarm thresholds still need to be defined and verified. Potential vibration processing includes DC-offset removal, per-axis RMS calculation, and later frequency analysis.

## Project Images

Upload your images to the `images/` folder in this repository. Then change the example paths below to match the filenames you upload. Until those image files exist at these paths, the images will not display.

### System Block Diagram

![System Block Diagram](images/system_block_diagram.png)

### Circuit Schematic

![Circuit Schematic](images/schematic.png)

### PCB Layout

![PCB Layout](images/pcb_layout.png)

### Four-Layer PCB Stack-Up

![Four-Layer PCB Stack-Up](images/pcb_stackup.png)

## Verification Before Fabrication

The following items need to be checked and tested before the design is treated as complete:

- ESP32-P4 minimum-system circuitry, required memory, clock, reset/boot circuitry, and power rails
- ADXL354 pin connections, analog output range, filtering, and ADC compatibility
- PT100 wiring, MAX31865 reference resistor, and measurement configuration
- MP1584 output regulation, stability, thermal performance, and component ratings
- Input fuse, reverse-polarity protection, TVS selection, and capacitor voltage ratings
- ESP-12F boot-strapping pins, decoupling, and Wi-Fi supply-current capability
- OLED supply, logic levels, I²C pull-ups, and address
- USB programming/debug circuit, if retained
- PCB stack-up and manufacturing tolerances
- Sensor calibration, measurement accuracy, and communication reliability

## Expected Outcome

The intended outcome is a compact hardware gateway that combines three-axis vibration sensing, PT100 temperature measurement, local OLED readout, and Wi-Fi data transfer for industrial condition-monitoring applications.

## Tools

- **Altium Designer** — schematic development and PCB layout
- **Embedded firmware development tools** — to be selected for programming and testing

## Repository Structure

After adding the project images, the repository can be organised as follows:

```text
Industrial-Vibration-Monitoring-Gateway/
├── README.md
└── images/
    ├── system_block_diagram.png
    ├── schematic.png
    ├── pcb_layout.png
    └── pcb_stackup.png
```

## Author

Add your name or team details here.
