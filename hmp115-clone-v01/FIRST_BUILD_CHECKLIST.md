# First Build Checklist — HMP115 Clone v01

## Schematic capture
- [ ] Add SHT31 sensor with I2C pullups
- [ ] Add MCU with SWD programming header
- [ ] Add MAX3485 RS485 transceiver
- [ ] Add 5–28V input protection
- [ ] Add 3.3V power rail and decoupling
- [ ] Add RS485 TVS/termination provision
- [ ] Add field connector labels
- [ ] Add address/config pads if useful

## PCB layout
- [ ] Probe-shaped board or clear sensor nose area
- [ ] Sensor at airflow end
- [ ] Power/RS485 at cable end
- [ ] Keep regulator heat away from SHT31
- [ ] Add mounting/enclosure holes
- [ ] Add clean silkscreen: VIN, GND, A, B, address
- [ ] Run DRC until 0 critical violations

## Firmware
- [ ] Bring up MCU blink/status LED
- [ ] Read SHT31 over I2C
- [ ] Print temp/RH over UART debug
- [ ] Add MAX3485 direction control
- [ ] Implement Modbus read input registers
- [ ] Implement configurable address later

## Bench test
- [ ] Power from 12V supply
- [ ] Measure 3.3V rail
- [ ] Confirm SHT31 responds on I2C
- [ ] Confirm RS485 communication with USB-RS485 dongle
- [ ] Compare temp/RH against reference

## Decision gate
Do not order assembled boards until:
- [ ] schematic reviewed
- [ ] DRC clean
- [ ] connector pinout confirmed
- [ ] enclosure concept chosen
- [ ] firmware pinout confirmed
