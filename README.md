# Multi-Node CAN-Based Vehicle Monitoring and Reverse Safety System

## Overview

The **Multi-Node CAN-Based Vehicle Monitoring and Reverse Safety System** is an embedded systems project designed to improve vehicle monitoring and driver safety using multiple ARM7-based LPC2129 microcontrollers communicating over a Controller Area Network (CAN) bus.

The system monitors engine temperature, manages vehicle indicators, detects obstacles while reversing, and provides visual and audible alerts. CAN communication allows individual nodes to exchange information reliably instead of placing every function on a single controller.

## Objectives

- Monitor engine temperature using a DS18B20 temperature sensor.
- Detect obstacles behind the vehicle using an HC-SR05 ultrasonic sensor.
- Control left, right, and hazard indicators.
- Exchange sensor readings and control information between nodes using CAN.
- Display relevant information on an LCD.
- Provide alerts using LEDs and a buzzer when required.

## System Architecture

The project is organized into multiple functional nodes. The exact node allocation can be adapted to the available hardware.

| Node / function | Responsibility |
|---|---|
| Temperature monitoring node | Reads engine temperature and sends the reading over CAN. |
| Reverse-safety node | Measures the distance to an obstacle using the HC-SR05 and sends distance information over CAN. |
| Indicator-control node | Reads indicator switches and controls the corresponding indicator outputs. |
| Main display / alert node | Receives CAN messages, displays information, and activates alerts as configured. |

## Hardware Requirements

- LPC2129 ARM7 microcontroller boards
- CAN transceiver interface (MCP2551, where used; some development boards may have a CAN interface built in)
- DS18B20 temperature sensor
- HC-SR05 ultrasonic distance sensor
- LCD module (for example, a 20×4 LCD)
- Indicator LEDs and other status LEDs
- Buzzer
- Push buttons or switches
- Appropriate power supply, wiring, and common ground

**Important:** Verify the board's CAN interface and transceiver arrangement before wiring. Connect CANH to CANH and CANL to CANL, use suitable bus termination at the two physical ends of the bus, and follow the board and transceiver voltage requirements.

## Software and Tools

- Embedded C
- Keil µVision IDE
- Flash Magic for programming supported LPC devices
- Proteus for simulation, where the required components and models are available
- Git and GitHub for version control and project hosting

## How It Works

1. The temperature node reads the DS18B20 sensor.
2. The reverse-safety node triggers the HC-SR05, measures the echo duration, and calculates obstacle distance.
3. Indicator inputs are read and the corresponding indicator outputs are controlled.
4. Each node sends its data or status through CAN messages.
5. The receiving node checks the message identifier and processes the received data.
6. The display and alert outputs are updated according to the received data and the project's configured thresholds.

## CAN Message Plan

The following identifiers are an example plan and should match the identifiers used in the source code.

| CAN identifier | Example message |
|---|---|
| `0x101` | Engine temperature |
| `0x102` | Reverse obstacle distance |
| `0x103` | Left indicator status |
| `0x104` | Right indicator status |
| `0x105` | Hazard indicator status |

Ensure that the sender and receiver agree on each identifier, payload format, data length, and units.

## Repository Structure

An example modular structure:

```text
Multi-Node-CAN-Vehicle-Safety/
├── README.md
├── temperature_node/
│   ├── main.c
│   ├── ds18b20.c
│   └── ds18b20.h
├── reverse_safety_node/
│   ├── main.c
│   ├── ultrasonic.c
│   └── ultrasonic.h
├── indicator_node/
│   └── main.c
├── display_alert_node/
│   ├── main.c
│   ├── lcd.c
│   └── lcd.h
├── common/
│   ├── can.c
│   ├── can.h
│   ├── delay.c
│   └── delay.h
└── docs/
    ├── block_diagram.png
    └── circuit_diagram.png
```

This is a suggested layout; update it to reflect the files actually present in your repository.

## Build and Run

1. Install Keil µVision and the device support needed for the LPC2129.
2. Open the relevant node project in Keil.
3. Add the source and header files required by that node.
4. Verify processor clock settings, peripheral pin assignments, CAN bit timing, and sensor connections.
5. Build each node project and resolve any compiler errors.
6. Program each controller using a supported programming method, such as Flash Magic.
7. Connect the nodes to the CAN bus and check that the CAN transceiver, wiring, and termination are correct.
8. Power the setup and test each function individually before testing the integrated system.
9. Use an oscilloscope, logic analyzer, UART debug output, or CAN monitoring tool if available to diagnose communication problems.

## Testing Checklist

- [ ] LCD initialization and text display
- [ ] DS18B20 temperature reading
- [ ] HC-SR05 distance measurement
- [ ] Left and right indicator operation
- [ ] Hazard indicator operation
- [ ] CAN transmission and reception between nodes
- [ ] Correct handling of CAN message identifiers and payloads
- [ ] Display updates from received messages
- [ ] Buzzer / LED alerts at configured thresholds
- [ ] Integrated system test with all nodes connected

## Safety and Limitations

This is an educational prototype, not a certified automotive safety system. Test it on a bench or controlled setup. Do not rely on it as the sole safety mechanism in a real vehicle. Sensor accuracy, timing, CAN error handling, power protection, and fault conditions should be evaluated before any real-world deployment.

## Future Enhancements

- Add fault detection and communication timeouts.
- Record sensor readings for diagnostics.
- Add configurable alert thresholds.
- Improve display messages and system status reporting.
- Test bus-off recovery and other CAN error conditions.

## Author

**Project:** Multi-Node CAN-Based Vehicle Monitoring and Reverse Safety System  
**Platform:** LPC2129 ARM7  
**Language:** Embedded C  
**IDE:** Keil µVision

---

*Update the repository structure, wiring diagrams, pin assignments, CAN configuration, and author details to match your final implementation.*
