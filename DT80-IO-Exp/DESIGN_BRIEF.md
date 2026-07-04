# DT80 IO Expansion Board — Design Brief

## What It Is
A cheap external IO expansion board for dataTaker DT80/DT85 loggers.
Connects via the DT80's **Serial Sensor Port (RS232/RS485)** to add:
- Extra analog inputs (4–8 channels, 0–10V / 4–20mA)
- Extra digital inputs/outputs (relay-driven)
- SDI-12 sensor interface
- Modbus RTU slave for external sensors

## Why DT80 Owners Need This
- DT80 base units are expensive ($3k–$5k)
- DT80 expansion modules (CEM20, DSM) are expensive ($500–$1k each)
- DT80 has a Serial Sensor Port that can address Modbus slaves
- A $50 DIY expansion board beats a $500 OEM module

## Target Specs
| Feature | Spec |
|---------|------|
| Interface | RS485 (2-wire) or RS232 (D9) to DT80 Serial Sensor Port |
| Power | 12V from DT80 terminal block |
| Analog Inputs | 4x 16-bit, 0–10V / 4–20mA (via ADS1115 or MCP3424) |
| Digital IO | 4x opto-isolated inputs + 2x relay outputs |
| Protocol | Modbus RTU slave (address configurable via DIP switch) |
| Optional | SDI-12 interface for environmental sensors |
| MCU | ESP32-S3 or STM32F103 (handles ADC + Modbus) |
| PCB | 2-layer, 100×60mm, KiCad, JLCPCB fab |

## Block Diagram
```
DT80 Serial Sensor Port (RS485)
        │
        ▼
┌─────────────────────────────────┐
│  RS485 Transceiver (MAX485)     │
│         │                       │
│  ┌──────▼──────┐               │
│  │  ESP32-S3   │◄── 4x AI      │
│  │  or STM32   │    (ADS1115)  │
│  │             │◄── 4x DI      │
│  │             │──► 2x Relay   │
│  │             │    (opto+driver)
│  └─────────────┘               │
│         │                       │
│    12V ─┴─ Buck 3.3V           │
└─────────────────────────────────┘
```

## Reference Docs
- DT80 User Manual (RS232/RS485 serial sensor port)
- Modbus RTU slave library for ESP32: `emelianov/modbus-esp8266`
- ADS1115 4-channel 16-bit ADC (I2C)

## Status
- [ ] Schematic (planned)
- [ ] PCB layout (planned)
- [ ] Firmware (Modbus slave + ADC polling)
- [ ] Enclosure design
