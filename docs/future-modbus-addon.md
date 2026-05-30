# Future: Shelly The Pill Modbus Add-on

The current project uses a generic UART TTL to RS485 module.

This repository is prepared for a future migration path when the Shelly The Pill Modbus add-on becomes available and tested.

## Planned documentation updates

- Add photos of the Shelly The Pill Modbus add-on
- Add wiring diagram for the add-on
- Compare generic TTL-RS485 module vs Shelly Modbus add-on
- Update setup instructions
- Add compatibility notes
- Add test results
- Add known limitations

## Current migration idea

Current wiring:

```text
Shelly The Pill -> IO breakout -> UART TTL RS485 module -> RS485 bus
```

Future wiring:

```text
Shelly The Pill -> Shelly Modbus add-on -> RS485 bus
```

## Important note

Do not publish non-public hardware details, firmware behavior, pinout, or internal documentation unless it is allowed by Shelly or already public.

For now, this repository can safely describe the current working implementation and keep a placeholder for future official add-on documentation.
