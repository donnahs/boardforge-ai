# HMP155-Env — Vaisala HMP155 Clone Design Brief

## What It Is
A DIY clone/reference design of the **Vaisala HMP155** temperature + relative humidity sensor.
Uses the same sensor elements (PT1000 RTD + capacitive RH sensor) at a fraction of the cost.

## Why
- HMP155 costs $500–$800
- The sensor elements inside are cheap ($20–$50):
  - **SHT45** (Sensirion) or **HDC3020** (TI) for RH
  - **PT1000 Class B** or **TMP117** for temperature
- For hydroponics / environmental monitoring, a $50 DIY sensor is good enough

## Target Specs
| Feature | Spec |
|---------|------|
| Temperature | ±0.2°C accuracy (TMP117 or PT1000 + ADS1115) |
| RH | ±1.5% accuracy (SHT45 or HDC3020) |
| Output | Analog 0–10V or SDI-12 or Modbus RTU |
| Power | 12V or 5V input |
| Protection | IP65 enclosure, solar radiation shield |
| Connector | M12 4-pin or screw terminals |
| PCB | 2-layer, 30×50mm, KiCad, JLCPCB |

## Sensor Options
| Sensor | Type | Price (LCSC) | Accuracy |
|--------|------|-------------|----------|
| SHT45 | I2C RH+T | ~$8 | ±1% RH, ±0.1°C |
| HDC3020 | I2C RH+T | ~$6 | ±1.5% RH, ±0.1°C |
| TMP117 | I2C Temp | ~$5 | ±0.1°C |
| PT1000 | RTD | ~$3 | ±0.3°C (Class B) |

## Block Diagram
```
┌─────────────────────────────┐
│  Solar Radiation Shield     │
│  (3D-printed, Stevenson)    │
│         │                   │
│  ┌──────▼──────┐           │
│  │  SHT45      │── I2C ────┤
│  │  (RH + T)   │           │
│  └─────────────┘           │
│         │                   │
│  ┌──────▼──────┐           │
│  │  MCU (STM32 │           │
│  │  or ATTINY) │           │
│  │             │── RS485 ──┤
│  └─────────────┘   /SDI-12 │
│         │                   │
│  12V ───┴── Buck 3.3V      │
└─────────────────────────────┘
```

## Status
- [ ] Sensor comparison test
- [ ] Schematic
- [ ] PCB layout
- [ ] Firmware (I2C → analog/SDI-12 output)
- [ ] Enclosure + radiation shield
