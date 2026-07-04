# DT80 IO-Expansion Board v0.1

**Status:** Schematic placed, footprints assigned, power wired → **Needs signal routing in KiCad GUI**

---

## What This Board Does

RS485 Modbus expansion for **DataTaker DT80** data loggers. Adds:
- **4× analog inputs** (0-10V / 4-20mA via ADS1115 16-bit ADC)
- **2× relay outputs** (ULN2003 driven, 24V/500mA)
- **2× opto-isolated digital inputs** (TLP281, 12-24V)
- **Modbus RTU slave** (ESP32 + MAX485) → DT80 reads via RS485

---

## Component Placement (Schematic Coordinates)

| Ref | Component | Position (mm) | Footprint |
|-----|-----------|---------------|-----------|
| **U1** | MAX485 RS485 transceiver | (39.37, 22.86) | SOIC-8 |
| **U2** | ESP32-WROOM-32D | (69.85, 36.83) | ESP32-WROOM-32 |
| **U3** | ADS1115 4ch ADC | (110.49, 22.86) | VSSOP-10 |
| **U4** | ULN2003 relay driver | (110.49, 57.15) | SOIC-16 |
| **U5** | MP1584 12V→5V buck | (39.37, 77.47) | SOIC-8-1EP |
| **U6** | AMS1117-3.3 5V→3V3 | (69.85, 77.47) | SOT-223 |
| **Q1** | TLP281 optocoupler DI1 | (110.49, 77.47) | SOP-4 |
| **Q2** | TLP281 optocoupler DI2 | (110.49, 82.55) | SOP-4 |
| **J1** | 12V/GND power in | (17.78, 30.48) | Screw terminal 2P |
| **J2** | RS485 A/B to DT80 | (20.32, 17.78) | Screw terminal 2P |
| **J3–J6** | AI1–AI4 inputs | (139.70, 12.70–27.94) | Screw terminal 2P ×4 |
| **J7,J8** | REL1, REL2 outputs | (139.70, 52.07–57.15) | Screw terminal 2P ×2 |
| **J9,J10** | DI1, DI2 inputs | (139.70, 77.47–82.55) | Screw terminal 2P ×2 |
| **SW1** | Modbus addr DIP switch | (20.00, 20.16) | 4-position DIP |
| **C1** | 100µF 25V input bulk | (27.94, 90.17) | Radial electrolytic |
| **C2** | 10µF 16V 5V bulk | (46.99, 90.17) | Radial electrolytic |
| **C3** | 10µF 16V 3V3 bulk | (77.47, 90.17) | Radial electrolytic |
| **R1–R4** | Pull-ups/pull-downs | (120.00, 75–90) | 0603 SMD |

---

## Wiring Instructions (Complete These in KiCad)

### 1. MAX485 ↔ ESP32 UART

Connect U1 (MAX485) to U2 (ESP32) UART pins:

| U1 Pin | Signal | → | U2 Pin | ESP32 GPIO |
|--------|--------|---|--------|------------|
| Pin 1 (RO) | RX | → | Pin 34 | RXD0 (GPIO3) |
| Pin 4 (DI) | TX | → | Pin 35 | TXD0 (GPIO1) |
| Pin 2 (RE) | RX Enable | → | Pin 28 | IO17 (GPIO17) |
| Pin 3 (DE) | TX Enable | → | Tie to RE (both controlled by GPIO17) |
| Pin 6 (A) | RS485 A | → | J2 Pin 1 | RS485_A net |
| Pin 7 (B) | RS485 B | → | J2 Pin 2 | RS485_B net |

**In KiCad:**
1. Place wire from U1 Pin 1 to U2 Pin 34 → label `UART_RX`
2. Place wire from U1 Pin 4 to U2 Pin 35 → label `UART_TX`
3. Short U1 Pin 2 and Pin 3 together → label `RS485_DE`
4. Wire U1 Pin 6 to J2 Pin 1 → net `RS485_A`
5. Wire U1 Pin 7 to J2 Pin 2 → net `RS485_B`

### 2. ADS1115 ↔ ESP32 I2C

| U3 Pin | Signal | → | U2 Pin | ESP32 GPIO |
|--------|--------|---|--------|------------|
| Pin 9 (SDA) | I2C SDA | → | Pin 33 | IO21 (GPIO21) |
| Pin 10 (SCL) | I2C SCL | → | Pin 36 | IO22 (GPIO22) |
| Pin 1 (ADDR) | Address select | → | GND | Sets I2C addr 0x48 |
| Pin 8 (VDD) | Power | → | +5V | |
| Pin 3 (GND) | Ground | → | GND | |

**In KiCad:**
1. Wire U3 Pin 9 to U2 Pin 33 → label `I2C_SDA`
2. Wire U3 Pin 10 to U2 Pin 36 → label `I2C_SCL`
3. Wire U3 Pin 1 to PWR3 (GND symbol)
4. Wire U3 Pin 8 to PWR1 (+5V symbol)
5. Wire U3 Pin 3 to PWR3 (GND symbol)

### 3. ULN2003 ↔ ESP32 + Relays

| U4 Pin | Signal | → | Destination |
|--------|--------|---|-------------|
| Pin 1 (1B) | Relay 1 control | → | U2 Pin 29 (IO5) |
| Pin 2 (2B) | Relay 2 control | → | U2 Pin 30 (IO18) |
| Pin 16 (1C) | Relay 1 coil + | → | J7 Pin 1 |
| Pin 15 (2C) | Relay 2 coil + | → | J8 Pin 1 |
| Pin 8 (E) | Emitter (GND) | → | GND |
| Pin 9 (COM) | Common (flyback diodes) | → | +5V |

**In KiCad:**
1. Wire U4 Pin 1 to U2 Pin 29 → label `REL1_CTRL`
2. Wire U4 Pin 2 to U2 Pin 30 → label `REL2_CTRL`
3. Wire U4 Pin 16 to J7 Pin 1 → net `REL1`
4. Wire U4 Pin 15 to J8 Pin 1 → net `REL2`
5. Wire U4 Pin 8 to PWR3 (GND)
6. Wire U4 Pin 9 to PWR1 (+5V) — **IMPORTANT: this is the flyback diode common, NOT power supply**

### 4. TLP281 Optocouplers ↔ ESP32 + Digital Inputs

| Q1 Pin | Signal | → | Destination |
|--------|--------|---|-------------|
| Pin 1 | LED Anode | → | J9 Pin 2 (via current limiting resistor R3=1kΩ) |
| Pin 2 | LED Cathode | → | J9 Pin 1 (GND) |
| Pin 4 | Phototransistor Collector | → | U2 Pin 26 (IO4) |
| Pin 3 | Phototransistor Emitter | → | GND |

| Q2 Pin | Signal | → | Destination |
|--------|--------|---|-------------|
| Pin 1 | LED Anode | → | J10 Pin 2 (via current limiting resistor R4=1kΩ) |
| Pin 2 | LED Cathode | → | J10 Pin 1 (GND) |
| Pin 4 | Phototransistor Collector | → | U2 Pin 27 (IO16) |
| Pin 3 | Phototransistor Emitter | → | GND |

**In KiCad:**
1. Place R3 (1kΩ) between J9 Pin 2 and Q1 Pin 1
2. Wire Q1 Pin 2 to J9 Pin 1 → net `GND`
3. Wire Q1 Pin 4 to U2 Pin 26 → label `DI1`
4. Wire Q1 Pin 3 to PWR3 (GND)
5. Place R4 (1kΩ) between J10 Pin 2 and Q2 Pin 1
6. Wire Q2 Pin 2 to J10 Pin 1 → net `GND`
7. Wire Q2 Pin 4 to U2 Pin 27 → label `DI2`
8. Wire Q2 Pin 3 to PWR3 (GND)

### 5. ADS1115 Analog Inputs

| U3 Pin | Signal | → | Destination |
|--------|--------|---|-------------|
| Pin 4 (AIN0) | AI1 | → | J3 Pin 1 |
| Pin 5 (AIN1) | AI2 | → | J4 Pin 1 |
| Pin 6 (AIN2) | AI3 | → | J5 Pin 1 |
| Pin 7 (AIN3) | AI4 | → | J6 Pin 1 |

**In KiCad:**
1. Wire U3 Pin 4 to J3 Pin 1 → net `AI1`
2. Wire U3 Pin 5 to J4 Pin 1 → net `AI2`
3. Wire U3 Pin 6 to J5 Pin 1 → net `AI3`
4. Wire U3 Pin 7 to J6 Pin 1 → net `AI4`

### 6. DIP Switch → ESP32 (Modbus Address)

| SW1 Pin | Signal | → | U2 Pin | Function |
|---------|--------|---|--------|----------|
| Pin 1 | Addr bit 0 | → | Pin 25 (IO0) | LSB |
| Pin 2 | Addr bit 1 | → | Pin 24 (IO2) | |
| Pin 3 | Addr bit 2 | → | Pin 23 (IO15) | |
| Pin 4 | Addr bit 3 | → | Pin 22 (SD1) | MSB |

**In KiCad:**
1. Wire SW1 Pin 1 to U2 Pin 25 → label `ADDR0`
2. Wire SW1 Pin 2 to U2 Pin 24 → label `ADDR1`
3. Wire SW1 Pin 3 to U2 Pin 23 → label `ADDR2`
4. Wire SW1 Pin 4 to U2 Pin 22 → label `ADDR3`
5. Place 4× 10kΩ pull-down resistors (R1, R2, and 2 more) from each ADDR line to GND

---

## Next Steps

1. **Open project in KiCad:** `dt80-io-exp-v01/DT80-IO-Exp-v01.kicad_pro`
2. **Complete signal wiring** using the table above
3. **Run ERC** (Tools → Electrical Rules Check)
4. **Sync schematic → board** (Tools → Update PCB from Schematic)
5. **Place footprints** on PCB (100×80mm board)
6. **Autoroute** with Freerouting
7. **Run DRC** and fix violations
8. **Export Gerbers** → JLCPCB

---

## Power Budget

| Rail | Source | Load | Current |
|------|--------|------|---------|
| 12V_IN | J1 | U5 (MP1584) | ~300mA |
| +5V | U5 | U6, U1, U3, U4, relays | ~200mA |
| +3V3 | U6 | U2 (ESP32) | ~100mA peak |

---

## GitHub

Repository: `https://github.com/donnahs/boardforge-ai`
Branch: `main`
