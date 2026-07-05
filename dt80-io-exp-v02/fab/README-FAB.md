# DT80-IO-Exp-v02 Fabrication Package

Generated: 2026-07-05

## Status

- PCB DRC: 0 violations
- Unconnected pads: 0
- Footprint errors: 0
- Routing: completed via Freerouting Java 25 Docker run, SES imported
- Board: DT80-style IO/sensor expansion for HMP115/HMP155, BP/barometer, Gill wind, Young wind, PT100/PT1000

## Included

- `gerbers/` — copper, mask, paste, silkscreen Gerbers + `.gbrjob`
- `drill/` — PTH/NPTH drill files and drill maps
- `DT80-IO-Exp-v02-bom.csv` — schematic BOM export
- `DT80-IO-Exp-v02-position.csv` — pick-and-place / position export
- `../DRC-final-clean.rpt` — final clean DRC report

## Notes

- R12 is RS485 termination provision and should be treated as DNP unless this board is installed at the physical end of the RS485 bus.
- RS232-only sensors still need an RS232 adapter/transceiver; do not wire RS232 directly to RS485 A/B.
- This is a prototype-grade design. Review connector pin order against the exact sensor cable drawings before ordering a production batch.
