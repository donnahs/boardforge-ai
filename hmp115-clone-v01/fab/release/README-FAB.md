# HMP115 Clone v01 — Fabrication Package

## Status
- Schematic ERC: 0 errors, warnings only (off-grid/library-table warnings from generated schematic)
- PCB DRC: 0 violations
- Unconnected pads: 0
- Footprint errors: 0

## Board purpose
Prototype HMP115-style RH/T probe with:
- Sensirion SHT31 RH/T sensor
- STM32C011 MCU
- MAX3485 RS485 transceiver
- 5–28V input concept
- RS485 Modbus output concept

## Human review before ordering
- Confirm SHT31 DFN pad/pin orientation against datasheet.
- Confirm STM32C011F6P6 TSSOP20 pin map and firmware alternate functions.
- Confirm AMS1117 thermal/power dissipation from highest VIN; v02 should use a buck/low-Iq regulator for 24V field supply.
- Confirm RS485 A/B polarity with target logger/cable.
- Confirm enclosure/filter cap strategy before field use.

Prototype only. Not a certified Vaisala replacement.
