# Industrial IoT Integration Lab

This repository is my practical lab for industrial communication, field device integration, edge computing, and OT/IT integration.

The goal is to organize real industrial experience, test notes, troubleshooting records, and learning experiments into reusable technical documentation.

This is a lab repository.

It is not a finished product, and it is not a claim that every topic is fully mastered.  
Some content comes from real field experience, and some content will be added step by step as I continue learning and building.

## Why This Repository Exists

Industrial systems are not only about writing PLC programs.

A real system often includes:

- Field devices
- PLCs and I/O modules
- Communication protocols
- Serial and Ethernet networks
- Edge computers
- Data collection
- Dashboards
- Cloud platforms
- Monitoring and troubleshooting

This repository is used to document how these parts can be connected and tested in practical situations.

## Focus Areas

### Industrial Communication

- Modbus TCP
- Modbus RTU
- OPC UA
- MQTT
- RS-232 / RS-485
- TCP/IP networking

### Field Device Integration

- PLC integration
- I/O module testing
- Sensor and actuator communication
- Industrial gateway testing
- Device register mapping
- Base 0 / Base 1 address conversion

### Edge Computing

- Edge computer setup
- Docker-based services
- Data collection
- Local dashboard
- Edge-to-cloud data flow

### Troubleshooting

- Ping and network quality testing
- Wireshark packet capture
- Serial communication debugging
- Timeout and polling issues
- Gateway and routing problems
- Device response verification

## Experience Map

### Already Experienced

These areas come from my industrial automation and field support experience:

- PLC and field device integration
- Modbus TCP / RTU testing
- OPC UA data connection
- MQTT-based data flow
- WAGO PLC / Edge device usage
- RS-232 / RS-485 troubleshooting
- Docker basics on edge devices
- Grafana / InfluxDB / Node-RED testing
- On-site network troubleshooting
- Industrial data collection concepts

### Currently Organizing

These are areas I am turning into reusable notes and examples:

- Device test records
- Communication troubleshooting steps
- Industrial protocol examples
- Edge data flow examples
- README-based technical documentation
- Case-based learning structure

### Currently Learning

These are areas I am gradually improving:

- More structured Go backend integration
- OpenTelemetry and observability
- Scalable edge-to-cloud architecture
- Better dashboard and alert design
- AI-assisted industrial data analysis
- Security basics for OT/IT systems

## Repository Structure

```text
industrial-iot-integration-lab/
├── README.md
├── docs/
│   ├── modbus.md
│   ├── opcua.md
│   ├── mqtt.md
│   ├── rs485.md
│   ├── edge-computing.md
│   └── network-troubleshooting.md
├── cases/
│   ├── banner-k30-modbus-rtu/
│   ├── wago-edge-data-platform/
│   ├── opcua-to-dashboard/
│   └── network-diagnosis/
├── devices/
│   ├── wago-750-8212.md
│   ├── wago-752-9800.md
│   ├── wago-750-653.md
│   └── banner-k30.md
└── templates/
    └── case-template.md
```

## Case Study Format

Each case study should include:

- Background
- Hardware
- Software
- Network topology
- Communication settings
- Register / address mapping
- Test procedure
- Problems encountered
- Troubleshooting process
- Final result
- Lessons learned

## Planned Cases

### Banner K30 Modbus RTU Test

A case study for testing Banner K30 touch indicator lights through Modbus RTU.

Planned topics:

- RS-485 wiring
- Modbus RTU parameters
- Slave ID setting
- Register address mapping
- Base 0 / Base 1 conversion
- Read / write test
- Polling stability
- Troubleshooting communication timeout

### WAGO Edge Data Platform

A case study for using WAGO Edge Computer as an edge data platform.

Planned topics:

- Docker services
- OPC UA data source
- InfluxDB
- Grafana dashboard
- Node-RED / Telegraf data flow
- Edge monitoring

### OPC UA to Dashboard

A case study for collecting PLC data through OPC UA and displaying it in a dashboard.

Planned topics:

- OPC UA server
- Data collection
- Time-series database
- Dashboard design
- Alarm and monitoring concept

### Network Diagnosis

A case study for testing industrial network quality and troubleshooting connection problems.

Planned topics:

- Ping test
- ARP table
- Default gateway issue
- Wireshark capture
- TCP connection testing
- Timestamp-based troubleshooting

## Technical Principles

### 1. Document Real Problems

The most valuable notes come from real problems, not perfect examples.

### 2. Keep Tests Reproducible

Each case should include enough information to reproduce the test.

### 3. Separate Devices, Protocols, and Cases

Device notes, protocol notes, and case studies should be organized separately.

### 4. Record Troubleshooting Steps

Problems, wrong assumptions, and final solutions are all important.

### 5. Connect OT Experience with Software Engineering

This lab is not only about industrial automation.

It is also a bridge toward modern software systems, cloud platforms, and AI-assisted workflows.

## Roadmap

### Stage 1 — Basic Structure

- Create repository structure
- Add README
- Add case template
- Add first protocol notes
- Add first device notes

### Stage 2 — Case Documentation

- Add Banner K30 Modbus RTU case
- Add WAGO Edge data platform case
- Add network troubleshooting case
- Add screenshots and diagrams where useful

### Stage 3 — Software Integration

- Add simple scripts or tools
- Add data collection examples
- Add dashboard examples
- Add Docker compose examples

### Stage 4 — Edge-to-Cloud Integration

- Add MQTT data flow example
- Add API integration example
- Add observability notes
- Add AI-assisted analysis notes

## Long-Term Direction

This repository is part of my long-term direction from industrial automation toward cloud-ready industrial platforms.

The goal is to build a practical knowledge base that connects:

- Industrial devices
- Communication protocols
- Edge computing
- Cloud systems
- Dashboards
- Monitoring
- AI-assisted analysis

The final direction is to turn real field experience into stable, maintainable, and useful industrial software systems.
