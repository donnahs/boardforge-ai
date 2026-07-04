# HydroNode v0.1 PCB Preflight

## Must fix before ordering
- [ ] Complete MP1584 buck reference design passives.
- [ ] Add input fuse/polyfuse and reverse polarity protection.
- [ ] Add decoupling capacitors near ESP32 and regulators.
- [ ] Add EN/IO0 programming/reset handling.
- [ ] Add PWR_FLAG symbols or equivalent ERC handling.
- [ ] Review ESP32 antenna keepout.
- [ ] Separate analogue pH/EC area from pump output current paths.
- [ ] Widen +12V and pump switched traces.
- [ ] Add mounting holes and silkscreen labels.
- [ ] Run ERC and DRC.

## Prototype accept criteria
- KiCad project opens.
- ERC blockers documented or fixed.
- DRC blockers documented or fixed.
- BOM has real JLC/LCSC part IDs.
- No mains voltage appears on schematic or silkscreen.
