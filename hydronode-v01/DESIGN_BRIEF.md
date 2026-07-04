# HydroNode v0.1 Design Brief

## What it is
HydroNode v0.1 is a low-voltage ESP32 hydroponics controller starter PCB for monitoring pH/EC interface modules, water temperature, water level, optional flow input, and switching four small 12V DC loads such as pumps, solenoids or fans.

## Hard safety scope
- Prototype hardware only.
- 12V DC loads only.
- No mains AC on this board.
- Mains pumps/lights must use certified external relay modules/contactors.
- Do not order/fabricate until ERC/DRC and manual power review pass.

## Major blocks
1. 12V DC input terminal.
2. MP1584 12V-to-5V buck placeholder block.
3. AMS1117 5V-to-3.3V regulator block.
4. ESP32-WROOM-32D controller.
5. Sensor terminals: pH module, EC module, DS18B20, level, flow.
6. Four AO3400 low-side MOSFET output channels.
7. Flyback diodes on pump outputs.
8. I2C expansion terminal.

## Known prototype limitations
- MP1584 support passives are not completed yet; current schematic uses controller symbol as a placeholder.
- ESP32 boot/programming circuitry still needs EN/IO0 buttons or dev/programming strategy.
- pH/EC are external module inputs, not precision analogue front ends.
- Power flags and complete decoupling/capacitor network still need to be added.
