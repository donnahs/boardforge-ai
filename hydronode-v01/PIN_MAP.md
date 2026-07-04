# HydroNode v0.1 ESP32 Pin Map

| Function | ESP32 pin | Net |
|---|---:|---|
| pH module analogue signal | GPIO34 / IO34 | `PH_ADC_GPIO34` |
| EC/TDS module analogue signal | GPIO35 / IO35 | `EC_ADC_GPIO35` |
| DS18B20 data | GPIO4 / IO4 | `TEMP_DS18B20_GPIO4` |
| Water level input | GPIO27 / IO27 | `LEVEL_GPIO27` |
| Flow sensor input | GPIO26 / IO26 | `FLOW_GPIO26` |
| Pump output 1 gate | GPIO16 / IO16 | `PUMP1_GPIO16` -> `R1` -> `PUMP1_GATE_DRV` |
| Pump output 2 gate | GPIO17 / IO17 | `PUMP2_GPIO17` -> `R2` -> `PUMP2_GATE_DRV` |
| Pump output 3 gate | GPIO18 / IO18 | `PUMP3_GPIO18` -> `R3` -> `PUMP3_GATE_DRV` |
| Pump output 4 gate | GPIO19 / IO19 | `PUMP4_GPIO19` -> `R4` -> `PUMP4_GATE_DRV` |
| I2C SDA | GPIO21 / IO21 | `I2C_SDA_GPIO21` |
| I2C SCL | GPIO22 / IO22 | `I2C_SCL_GPIO22` |
| Supply | VDD33 | `+3V3` |
| Ground | GND pins | `GND` |

## Notes
- GPIO34/35 are input-only and suit analogue module outputs.
- IO0/EN programming/reset circuitry remains a next-rev task.
- I2C pullups are included as placeholders R10/R11; make jumper-selectable later.
