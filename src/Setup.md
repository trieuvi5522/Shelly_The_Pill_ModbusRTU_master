# Setup Guide

This guide explains how to reproduce the current version of the project.

## 1. Prepare the Modbus devices

Current device configuration:

| Device | Slave ID | Function code |
|---|---:|---|
| Single-phase energy meter | `1` | `FC04` |
| RS485 temperature/humidity sensor | `2` | `FC03` |

Serial settings:

```text
Baudrate: 9600
Mode: 8N1
```

## 2. Wire the RS485 bus

Connect the UART TTL to RS485 module to Shelly The Pill through the IO breakout.

```text
Shelly The Pill 5V   ->  RS485 Module VCC
Shelly The Pill GND  ->  RS485 Module GND
Shelly The Pill IO1  ->  RS485 Module RXD
Shelly The Pill IO2  ->  RS485 Module TXD
```

Connect the RS485 side:

```text
RS485 A+  ->  Energy Meter A / Sensor A
RS485 B-  ->  Energy Meter B / Sensor B
```

## 3. Create Shelly Virtual Components

Create these number components before running the script:

| Virtual Component | Meaning |
|---|---|
| `number:200` | Voltage |
| `number:201` | Current |
| `number:202` | Active Power |
| `number:203` | Power Factor |
| `number:204` | Frequency |
| `number:205` | Total Active Energy |
| `number:206` | Temperature |
| `number:207` | Humidity |

## 4. Upload the script

Upload this file to Shelly The Pill:

```text
src/shelly-the-pill-modbus-rtu.js
```

Start the script and check the script console.

Expected log pattern:

```text
Modbus UART configured. Baud=9600, Mode=8N1
========== MODBUS CYCLE START ==========
Read Energy Meter Voltage
Read Energy Meter Current
...
========== MODBUS CYCLE DONE ==========
```

## 5. Troubleshooting

If no values appear:

- Swap RS485 A/B
- Check TX/RX direction
- Confirm Slave ID
- Confirm baudrate and mode
- Confirm function code
- Confirm register address
- Check power supply
- Check if the RS485 module requires shared GND
- Test one Modbus device at a time
