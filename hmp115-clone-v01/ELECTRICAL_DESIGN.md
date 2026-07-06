# Electrical Design — HMP115 Clone v01

## Top-level blocks

1. **Input protection**
   - 4-pin field terminal: VIN, GND, RS485_A, RS485_B
   - resettable fuse or series protection
   - reverse polarity protection diode/P-MOSFET in v02
   - TVS diode on VIN/GND
   - ESD/TVS protection on RS485 A/B

2. **Power**
   - Input: 5–28 V DC
   - v01 simple path: buck module or regulator stage down to 5V/3.3V
   - local 3.3V rail for MCU/SHT31/MAX3485
   - Decoupling at every IC: 100nF close to VDD, bulk 10uF per rail

3. **Sensor**
   - SHT31-DIS-B2.5kS, LCSC C80862
   - I2C address default 0x44, optional ADDR pad for 0x45 if exposed
   - SDA/SCL pullups: 4.7k to 3.3V
   - Sensor placed at board nose / airflow zone
   - Keep heat sources away from sensor
   - Copper keepout around sensor if practical to reduce thermal conduction

4. **MCU**
   - STM32C011F6P6, LCSC C5456198
   - I2C to SHT31
   - UART to MAX3485
   - GPIO for DE/RE direction control
   - BOOT0/NRST/SWD header pads for programming
   - address/config solder pads optional

5. **RS485**
   - MAX3485ED-compatible, LCSC C558792
   - Half-duplex A/B lines
   - 120R termination provision as DNP/solder jumper
   - bias resistors optional/DNP depending network topology
   - TVS/ESD protection near connector

## Proposed MCU pin allocation

| Function | MCU signal |
|---|---|
| SHT31 SDA | I2C_SDA |
| SHT31 SCL | I2C_SCL |
| RS485 RX | UART_RX |
| RS485 TX | UART_TX |
| RS485 DE/RE | GPIO_RS485_DIR |
| Status LED | GPIO_LED optional |
| Address select 0 | GPIO_ADDR0 optional |
| Address select 1 | GPIO_ADDR1 optional |
| SWDIO/SWCLK | programming header |

Exact STM32 package pins to be locked during schematic capture from datasheet/pinout.

## Connector pinout

### J1 Field connector

| Pin | Net | Notes |
|---:|---|---|
| 1 | VIN_5_28V | field supply |
| 2 | GND | common ground |
| 3 | RS485_A | differential bus A |
| 4 | RS485_B | differential bus B |

## Thermal/layout notes
- Sensor must be physically isolated from regulator/MCU heat.
- Put regulator and RS485 at cable end; sensor at exposed airflow end.
- Use slots/cutouts or narrow neck if probe shape allows.
- Do not flood copper under the SHT31 unless datasheet recommends otherwise.
- Add mounting/probe alignment holes if using a tube/filter cap.
