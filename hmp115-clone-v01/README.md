# HMP115 Clone v01 — Temperature + RH Probe

## Purpose
Create a practical HMP115-style humidity/temperature probe that measures **relative humidity** and **temperature** and can be used with field controllers/data loggers.

This is a **functional clone / compatible replacement direction**, not a copy of Vaisala internal hardware or firmware.

## Target behaviour
- Measure: 0–100 %RH
- Measure: temperature, target field range −40 to +60 °C for enclosure/probe use
- Power input: 5–28 V DC field supply
- Primary output: 2-wire half-duplex RS485 Modbus RTU
- Optional later outputs: 0–1 V or 0–5 V analogue temp/RH, if needed for legacy loggers
- Probe format: small PCB in vented/filter-cap enclosure with electronics protected from condensation

## v01 Architecture

```text
5–28V IN
   │
   ├─ reverse/TVS/fuse protection
   │
   ├─ buck/LDO to 3.3V
   │
   ├─ STM32C011 MCU
   │     ├─ I2C → SHT31-DIS RH/T sensor
   │     ├─ UART → MAX3485 RS485 transceiver
   │     ├─ SWD programming header
   │     └─ optional address/config pads
   │
   └─ RS485 A/B + GND + VIN connector
```

## Selected core parts

| Function | Part | LCSC | Why |
|---|---|---:|---|
| RH/T sensor | Sensirion SHT31-DIS-B2.5kS | C80862 | Factory calibrated, ±2%RH, ±0.2°C, I2C |
| MCU | STM32C011F6P6 | C5456198 | Cheap, TSSOP-20, UART/I2C, enough flash/RAM |
| RS485 | MAX3485ED-compatible | C558792 | 3.3V half-duplex RS485 |
| 3.3V regulator | AMS1117-3.3 | C6186 | easy/basic part; v02 should use lower-Iq regulator |

## Files
- `HMP115-Clone-v01.kicad_pro` — KiCad project scaffold
- `PROJECT_BRIEF.md` — product requirements
- `ELECTRICAL_DESIGN.md` — circuit architecture
- `BOM_DRAFT.csv` — draft sourcing BOM
- `FIRMWARE_SPEC.md` — firmware behaviour and Modbus map
- `ENCLOSURE_AND_CALIBRATION.md` — mechanical/filter/calibration plan
- `FIRST_BUILD_CHECKLIST.md` — what to build/test first

## Current status
Design package/scaffold created. Next step is schematic capture + PCB routing for v01 probe board.
