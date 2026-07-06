# Enclosure and Calibration Plan — HMP115 Clone v01

## Mechanical concept
A small probe-style PCB where the sensor sits at the exposed end and electronics/power/RS485 sit at the cable end.

```text
[filter/vent cap]  [SHT31 airflow zone]  [MCU/RS485/power]  [cable gland/connector]
```

## Enclosure requirements
- Air must reach SHT31 quickly.
- Liquid water/condensation should not sit directly on the sensor.
- Electronics should be conformal-coated or separated from airflow path if used outdoors.
- Cable strain relief required.
- Plastic enclosure preferred to avoid thermal mass and electrical issues.

## Sensor protection options

### Indoor/lab v01
- vent holes or slotted cap
- no membrane initially
- keep away from direct heat sources

### Better v02
- PTFE/hydrophobic membrane/filter cap
- replaceable filter nose
- conformal coat electronics except SHT31 sensing window
- optional desiccant/weep path depending enclosure

## Calibration plan

### Basic check
1. Compare against a trusted RH/T meter at room conditions.
2. Log 10 minutes of readings.
3. Apply single-point offsets if needed.

### Two-point RH check
- low point: saturated salt chamber around 33% RH using MgCl2 (if available)
- high point: saturated salt chamber around 75% RH using NaCl
- allow enough stabilization time
- record temperature because RH references depend on temp

### Temperature check
- compare against calibrated thermometer or good reference probe
- avoid touching sensor/enclosure during test
- check at ambient and one warm/cool condition if possible

## Field test
- run next to known HMP115/HMP155 or existing monitoring station sensor for 24–72 hours
- compare drift, lag, daily min/max, noise
- inspect for condensation or enclosure heat bias

## Warnings
- Do not claim Vaisala-level calibration without traceable calibration.
- SHT31 is good, but enclosure and airflow will dominate real-world performance.
- Regulator heat can bias temperature if layout is poor.
