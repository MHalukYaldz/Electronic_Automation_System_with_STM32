# STM32-Based Electronic Automation System

A modular STM32-based electronic automation system designed for environmental data acquisition, actuator control, Modbus RTU/RS-485 communication, GSM/SMS notification, and multi-board PCB implementation.

## Project Overview

This project is a modular embedded automation system based on STM32F103 microcontrollers. The system is designed to acquire environmental data from sensors, transfer the measured values between boards using Modbus RTU over RS-485, make threshold-based decisions, control physical actuators, and notify the user through GSM/SMS when required.

The system consists of three main boards: Sensor Board, Control Board, and Server/Gateway Board. Each board has a dedicated function in the overall architecture, making the system easier to test, debug, and expand.


## System Architecture

The system is structured as a distributed multi-board architecture:

- Sensor Board: Collects temperature, gas, vibration, and pressure data.
- Server/Gateway Board: Reads sensor data over RS-485, evaluates alarm conditions, sends SMS notifications, and transfers control commands.
- Control Board: Drives physical loads such as fan, lamp, buzzer, and optional external switching outputs.

Data flow is mainly organized through Modbus register-based communication. Sensor data is transferred from the Sensor Board to the Server/Gateway Board, while control commands are sent from the Server/Gateway Board to the Control Board.

## Hardware Design

The hardware design was developed as a PCB-based modular system. The boards were designed by considering power distribution, microcontroller support circuitry, sensor/actuator interfaces, communication interfaces, and test/debug points.

The system operates from a 12 V input supply. Required voltage levels such as 5 V, 3.3 V, and the GSM module supply voltage are generated on the related boards using regulator and converter stages.

## Sensor Board

The Sensor Board is responsible for acquiring environmental data. It includes the sensor interfaces required for temperature, gas, vibration, and pressure measurements.

Used sensors:

- LM35 temperature sensor
- MQ-2 gas sensor
- ADXL345 accelerometer
- BMP180 pressure sensor

The acquired sensor values are processed by the STM32 microcontroller and mapped into Modbus registers so that they can be read by the Server/Gateway Board.

## Control Board

The Control Board is responsible for driving the physical output elements of the system. It receives control information from the Server/Gateway Board through Modbus RTU over RS-485 and activates the related output stages.

Controlled outputs include:

- 12 V fan
- 12 V lamp
- 5 V buzzer
- Optional external switching interface

MOSFET-based driver circuits are used for load control. In addition, an optional BJT-based output is provided to support external switching elements such as a relay or contactor when required.

## Server / Gateway Board

The Server/Gateway Board acts as the central management unit of the system. It collects sensor data from the Sensor Board over the RS-485 line, evaluates threshold-based events, sends GSM/SMS notifications, and transfers control commands to the Control Board.

The board includes:

- STM32F103 microcontroller
- RS-485 communication interface
- SIM800L GSM module interface
- Power supply stages for logic and GSM sections

The GSM supply stage is separated from the main logic supply to improve system stability during GSM current bursts.

## Modbus RTU / RS-485 Communication

Inter-board communication is implemented using Modbus RTU over the RS-485 physical layer. RS-485 was selected due to its differential signaling structure and suitability for industrial environments.

The Modbus register map is used to organize sensor values, alarm states, and control commands in a structured way. CRC verification is considered in the Modbus RTU frame structure to improve communication reliability.

## Embedded Software

The embedded software is organized according to the role of each board. The Sensor Board performs sensor reading and register updating. The Server/Gateway Board manages data collection, decision-making, GSM/SMS communication, and command generation. The Control Board receives control commands and drives the related outputs.

Main software functions include:

- Sensor data acquisition
- Modbus RTU communication
- Register-based data transfer
- Threshold-based alarm evaluation
- Actuator control
- GSM/SMS notification

## PCB Design

The project was designed at PCB level instead of remaining only as a software prototype. PCB design decisions were made by considering power paths, regulator placement, decoupling capacitors, communication lines, connector placement, and test/debug accessibility.

The modular PCB approach separates sensing, control, and gateway functions into different boards. This makes the system easier to debug, maintain, and expand.

## Test Results

Several functional tests were performed to verify the system operation:

- MOSFET-based load driving test
- Optional relay/external switching interface test
- Sensor Board functional verification
- Sensor and Server/Gateway Board integration test
- Modbus RTU holding register reading test

The test results show that sensor data can be transferred through the Modbus register structure and that actuator control scenarios can be executed according to the system logic.

