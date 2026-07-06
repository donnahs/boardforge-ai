# Firmware Spec — HMP115 Clone v01

## Goal
Read SHT31 temperature/RH over I2C and expose values over RS485 Modbus RTU.

## Defaults
- Baud: 9600
- Data: 8N1
- Modbus address: 1
- Update interval: 1 second
- Interface: RS485 half-duplex

## Measurement cycle
1. Wake/init SHT31.
2. Trigger high-repeatability measurement.
3. Read raw temperature and humidity.
4. Convert to engineering units.
5. Apply calibration offsets.
6. Update Modbus registers.
7. Serve Modbus requests.

## Holding/input register map

Use input registers for measured values.

| Register | Name | Type | Scale | Example |
|---:|---|---|---:|---|
| 30001 | Temperature_C_x100 | signed int16 | °C × 100 | 2534 = 25.34°C |
| 30002 | RH_x100 | uint16 | %RH × 100 | 5067 = 50.67%RH |
| 30003 | DewPoint_C_x100 | signed int16 | °C × 100 | optional calculated |
| 30004 | Sensor_Status | uint16 | flags | 0 = OK |
| 30005 | Supply_mV | uint16 | mV | optional if ADC divider added |

Holding registers for configuration:

| Register | Name | Type | Notes |
|---:|---|---|---|
| 40001 | Modbus_Address | uint16 | 1–247 |
| 40002 | Baud_Code | uint16 | 0=9600,1=19200,2=38400 |
| 40003 | Temp_Offset_x100 | int16 | calibration offset |
| 40004 | RH_Offset_x100 | int16 | calibration offset |
| 40005 | Sample_Period_ms | uint16 | default 1000 |

## Status flags

| Bit | Meaning |
|---:|---|
| 0 | SHT31 read error |
| 1 | CRC error from SHT31 |
| 2 | RH out of range |
| 3 | Temp out of range |
| 4 | calibration values invalid |

## Calibration
v01 supports simple 1-point offsets:
- temperature offset in 0.01°C
- RH offset in 0.01%RH

v02 can add 2-point RH calibration and serial-numbered calibration report.

## Firmware implementation options
- STM32CubeIDE / STM32 HAL
- bare-metal C
- Arduino core only if STM32C011 support is convenient

## Minimum firmware acceptance test
- reads SHT31 successfully
- returns temp/RH via Modbus read input registers
- handles RS485 direction correctly
- survives power cycle with default address
- reports sensor error if SHT31 disconnected
