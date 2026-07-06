# Project Brief — HMP115 Clone v01

## What it is
A low-cost HMP115-style temperature and relative-humidity probe for environmental monitoring, hydroponics, and data logger use.

It should behave like a field probe: powered from a wide DC supply, protected against wiring mistakes, communicating over RS485/Modbus, and physically built so air reaches the sensor while electronics are protected.

## Who would use/pay for it
- Matt / environmental monitoring workbench testing
- hydroponics growers needing temp/RH monitoring
- makers replacing expensive humidity probes
- data logger users wanting a cheap Modbus RH/T probe
- schools/STEM labs building weather stations

## Money path
- v01: internal prototype / portfolio project
- v02: small PCB + enclosure kit
- v03: sell as assembled Modbus RH/T probe or open-source hardware design pack
- digital product: BoardForge design pack showing how the sensor was made

## Target specs

| Item | v01 target |
|---|---|
| RH range | 0–100 %RH |
| RH accuracy | sensor-limited target ±2–3 %RH before external calibration |
| Temp range | −40 to +60 °C practical enclosure target |
| Temp accuracy | sensor-limited target ±0.2–0.5 °C before external calibration |
| Supply | 5–28 V DC input |
| Interface | RS485 2-wire half-duplex |
| Protocol | Modbus RTU, 9600 8N1 default |
| Connector | 4-pin: VIN, GND, RS485_A, RS485_B |
| Enclosure | vented/filter cap with hydrophobic membrane if possible |

## What v01 will NOT do
- It will not be a certified Vaisala replacement.
- It will not claim Vaisala HUMICAP performance.
- It will not emulate every HMP115 command until the exact target logger protocol is confirmed.
- It will not be suitable for condensation-heavy outdoor exposure without enclosure/filter testing.

## Disposition
A — Do Now as hardware prototype / F career portfolio / D later if turned into BoardForge-style product.
