# HydroNode v0.1 Bring-up Procedure

## Before power
- Inspect board for shorts, wrong orientation, solder bridges.
- Confirm no mains wiring is attached.
- Confirm external PSU current limit is enabled.
- Confirm pump outputs are unloaded for first power.

## First power checks
1. Set bench supply to 3.3V, 100mA current limit, output OFF.
2. Connect to 3.3V/GND test points or logic rail injection point.
3. Enable for max 5 seconds.
4. Shut down if current exceeds 80mA.
5. Verify no heating.
6. Repeat for 5V rail at 150mA limit.
7. Repeat for 12V input at 100mA no-load limit.

## Firmware smoke test later
- Compile HAL-based firmware.
- Confirm all pump GPIOs boot OFF.
- Confirm serial status heartbeat.
- Simulate low-water state and verify all pumps remain disabled.

## Output channel test later
- Use dummy 12V load, not a pump first.
- Limit current to 500mA max per channel.
- Pulse each output briefly and verify MOSFET temperature.
