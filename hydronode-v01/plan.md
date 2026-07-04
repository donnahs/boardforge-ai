# HydroNode v0.1 PCB Design Plan

## Goal
Create a KiCad-openable low-voltage ESP32 hydroponics controller starter board.

## Locked scope
- ESP32 controller module/devkit footprint path for prototype simplicity.
- 12V DC input with fuse/reverse-polarity placeholder.
- 5V and 3.3V regulator blocks.
- pH analogue module header.
- EC/TDS analogue module header.
- DS18B20 water temperature header.
- Water level input header.
- Optional flow sensor input header.
- 4x low-side 12V pump/solenoid MOSFET outputs.
- I2C/Qwiic expansion header.
- UART/debug header.
- No mains AC on board.

## Deliverables this session
- KiCad project files under `kicad/`.
- Design brief, pin map, living BOM seed, safety profile.
- Initial schematic starter with labels and connector blocks.
- Initial PCB file with board outline and synced footprints where possible.
- Verification report with what passed/failed and next fixes.

## Guardrails
- Treat as prototype starter, not production-ready.
- Do not claim ready to order until ERC/DRC and manual power/safety review pass.
- Keep all loads low-voltage DC only.
- Use JLC/LCSC parts where practical, but allow generic KiCad components for the starter.
