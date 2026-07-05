# DT80 IO Expansion v02 — Sensor Wiring Map

This board is labelled as a DT80-style sensor terminal base. Use the connector names printed on the silkscreen.

## Field sensors Matt uses

| Sensor | Preferred connector | Pins | Notes |
|---|---|---|---|
| Vaisala HMP115 | J16 RS485 | `+12V / A / B / GND` | HMP115 commonly uses RS-485 / Modbus RTU. Connect power and RS485 pair here. |
| Vaisala HMP155 RS485 model | J16 RS485 | `+12V / A / B / GND` | Use this when the probe is the serial/RS485 output version. |
| Vaisala HMP155 analog model | J11 HMP155 ANALOG | `AI1 / AI2 / GND / +5V` | Use for analog humidity/temp outputs. Check actual output range: 0-1V, 0-5V, or 0-10V. 0-10V needs divider/conditioning before ADS1115. |
| HMP155 PT100/passive temperature output | J14 PT100 4-WIRE | `I+ / V+ / V- / I-` | 4-wire RTD terminal provision. Needs RTD front-end/adapter for accurate readout if not going through a real DT80 universal input. |
| Vaisala BP / barometer, RS485/serial | J16 RS485 | `+12V / A / B / GND` | For PTB-style barometers with RS485 option. RS232-only models need an RS232 adapter, not direct RS485. |
| Vaisala BP / barometer, analog or 4-20mA | J13 BP LOOP | `+12V / AI3 / GND` | Use for 4-20mA/analog pressure output. Add shunt/scale conditioning as needed. |
| Gill WindSonic / Gill digital wind | J16 or J15 | J16: `+12V / A / B / GND`; J15: `+12V / DATA / GND` | Gill models vary: RS232/RS422/RS485/SDI-12/analog. Use J16 for RS485, J15 for SDI-12. RS232/RS422 need adapter. |
| Gill analog wind | J12 | `AI3 / AI4 / GND / +5V` | For analog speed/direction variants. Confirm voltage/current range. |
| R.M. Young wind speed | J19 | `+5V / PULSE / GND` | Wind speed is usually pulse/frequency/Hall/AC depending model. Use pulse/count input. |
| R.M. Young wind direction | J12 | `AI3 / AI4 / GND / +5V` | Direction is commonly potentiometer/analog bridge. Provide excitation and analog readback. |
| PT100 / PT1000 | J14 | `I+ / V+ / V- / I-` | 4-wire RTD terminal. For production, add MAX31865 or precision current-source + ADC frontend. |
| General I2C sensor | J17 | `3V3 / SDA / SCL / GND` | Local low-voltage digital sensors only. Not for long outdoor cable runs. |
| General 1-Wire sensor | J18 | `3V3 / DATA / GND` | DS18B20-style local/short cable sensors. Add pullup/protection for field use. |

## Connector list

- J1: `+12V / GND` board power input
- J2: DT80 base RS485 `A / B`
- J3-J6: simple analog inputs `AI1-AI4 / GND`
- J7-J8: relay/open-collector outputs
- J9-J10: digital/opto inputs
- J11: HMP155 analog / voltage humidity-temp terminal
- J12: Young/Gill analog wind terminal
- J13: BP/barometer 4-20mA or analog loop terminal
- J14: PT100/PT1000 4-wire RTD terminal provision
- J15: SDI-12 bus provision
- J16: RS485/Modbus sensor bus for HMP115/HMP155/Gill/BP
- J17: local I2C/Qwiic-style sensor port
- J18: local 1-Wire sensor port
- J19: Young wind speed pulse/frequency/count input

## Front-end circuits now added

### PT100 / PT1000 RTD frontend

J14 now feeds a MAX31865 RTD-to-digital converter:

- J14 pin 1 `I+` -> `RTD_FORCE+`
- J14 pin 2 `V+` -> `RTD_SENSE+`
- J14 pin 3 `V-` -> `RTD_SENSE-`
- J14 pin 4 `I-` -> `RTD_FORCE-`
- U7 `MAX31865` handles RTD measurement.
- R9 is a 430 ohm 0.1% RTD reference resistor for PT100 use.
- C4/C5 provide local 3V3 decoupling.
- SPI to ESP32:
  - `RTD_CS` -> ESP32 GPIO5
  - `RTD_SCK` -> ESP32 GPIO18
  - `RTD_MISO` -> ESP32 GPIO19
  - `RTD_MOSI` -> ESP32 GPIO23

### BP / barometric pressure 4-20mA frontend

J13 now uses a protected loop-input node instead of feeding ADS1115 directly:

- J13 pin 1: `+12V`
- J13 pin 2: `BP_LOOP_IN`
- J13 pin 3: `GND`
- R10: 165 ohm 0.1% shunt from `BP_LOOP_IN` to `GND`
- R11: 100 ohm ADC filter resistor from `BP_LOOP_IN` to `AI3`
- C6: 100nF filter from `AI3` to `GND`
- D1: TVS protection from `BP_LOOP_IN` to `GND`

165 ohm scaling gives approximately:

| Loop current | ADC voltage |
|---:|---:|
| 4mA | 0.66V |
| 20mA | 3.30V |

### Field protection / termination provisions

- R12: 120 ohm RS485 termination provision across `RS485_A` and `RS485_B` — mark DNP unless this board is at the end of the bus.
- D2/D3: TVS protection from RS485 A/B to GND.
- D4: TVS protection for `SDI12_DATA`.
- R13/C7/D5: pulse input pulldown, filter, and TVS for Young wind speed / pulse-count input.

## Important build notes

This revision now has the connector map, sensor front-end circuits, and silkscreen for Matt's real sensors.

- RS485 sensors should use J16.
- Analog/4-20mA sensors should use J11/J12/J13 depending sensor.
- PT100/PT1000 should use J14 and is read via MAX31865 over ESP32 SPI.
- SDI-12 and pulse inputs now include basic protection provisions.
- RS232/RS422-only sensors still need external adapter/transceiver support; do not connect RS232 directly to RS485 pins.
- PCB DRC currently has 0 errors after front-end placement; remaining warnings are cosmetic silkscreen/legacy symbol-grid warnings.
