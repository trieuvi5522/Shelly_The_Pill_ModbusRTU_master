# Home Server Power & Environment Monitoring with Shelly The Pill + Modbus RTU

## 1. Summary

This project uses **Shelly The Pill** as a compact **Modbus RTU gateway** to monitor my home server rack.

Shelly The Pill reads data from:

- A single-phase Modbus RTU energy meter
- An RS485 temperature and humidity sensor

The data is decoded by a Shelly Script and displayed using **Shelly Virtual Components**.

The project started as a home server monitoring system, but the same concept can be reused for many other Modbus RTU devices such as solar inverters, battery systems, ultrasonic level sensors, optical sensors, industrial sensors, water meters, heat meters, or any device that requires stable wired communication.

<!-- IMAGE 1: Insert the updated home server rack photo here. Recommended file: Home Server(1).png -->
![Updated home server rack](PASTE_IMAGE_HERE)

---

## 2. The problem I wanted to solve

I run a small home server rack for IoT, automation, database, and monitoring services.

I wanted to monitor:

- Real-time power consumption
- Total energy usage
- Voltage, current, active power, power factor, and frequency
- Temperature around the rack
- Humidity around the rack

I also wanted the system to use a **wired and reliable communication method** instead of relying only on Wi-Fi sensors.

Modbus RTU was a good choice because it is stable, noise-resistant, scalable, and widely used by energy meters, sensors, inverters, and industrial equipment.

The goal was not only to monitor one home server rack, but also to demonstrate how **Shelly The Pill can connect the Shelly ecosystem with the Modbus RTU world**.

---

## 3. Hardware used

Main hardware used in this project:

- Shelly The Pill
- Shelly The Pill IO breakout
- UART TTL to RS485 module
- Single-phase Modbus RTU energy meter
- RS485 temperature and humidity sensor
- Electrical protection and terminal blocks
- Home server rack with network equipment and mini PCs

<!-- IMAGE 2: Insert the electrical cabinet photo here. Recommended file: Elec Cabinet.jpg -->
![Electrical cabinet with energy meter and RS485 wiring](PASTE_IMAGE_HERE)

---

## 4. System architecture

The architecture is simple:

```text
Shelly The Pill
      |
      | UART TX/RX
      v
UART TTL to RS485 Module
      |
      | RS485 A/B Bus
      v
+-------------------------------+
| Modbus RTU Energy Meter       |
| Slave ID: 1                   |
| Function: FC04                |
+-------------------------------+

+-------------------------------+
| RS485 Temp/Humidity Sensor    |
| Slave ID: 2                   |
| Function: FC03                |
+-------------------------------+

Shelly Script
      |
      v
Shelly Virtual Components
      |
      v
Shelly App / Shelly Dashboard
```

Shelly The Pill works as the **Modbus RTU master**.

The energy meter and temperature/humidity sensor work as **Modbus RTU slave devices**.

The script reads the Modbus devices sequentially, decodes the values, applies scaling, and updates the corresponding Shelly Virtual Components.

---

## 5. Wiring Overview

Shelly The Pill is connected to the UART TTL to RS485 module through the IO breakout.

```text
Shelly The Pill 5V   ->  RS485 Module VCC
Shelly The Pill GND  ->  RS485 Module GND
Shelly The Pill IO1  ->  RS485 Module RXD
Shelly The Pill IO2  ->  RS485 Module TXD
```

The RS485 side is connected to the Modbus devices:

```text
RS485 A+  ->  Energy Meter A / Sensor A
RS485 B-  ->  Energy Meter B / Sensor B
```

Important checks when wiring:

- Check TX/RX direction
- Check RS485 A/B polarity
- Check baudrate
- Check slave ID
- Check register map
- Check shared GND if required by the module
- Keep high-voltage wiring and signal wiring separated as much as possible

<!-- IMAGE 3: Insert the UART TTL to RS485 wiring diagram here. Recommended file: module UART TTL to RS485.png -->
![Shelly The Pill to UART TTL RS485 wiring](PASTE_IMAGE_HERE)

---

## 6. Setting Script and Virtual Components

The Shelly Script configures UART communication as:

```js
modbus_config({
  Baudrate: 9600,
  Mode: "8N1"
});
```

The script reads two Modbus RTU devices.

### Energy Meter - Slave ID 1

Function code: `FC04 - Read Input Registers`

| Value | Register | Type | Scale | Virtual Component |
|---|---:|---|---:|---|
| Voltage | `0x0000` | `u16` | `0.1` | `number:200` |
| Current | `0x0003` | `u16` | `0.01` | `number:201` |
| Active Power | `0x0008` | `s16` | `1` | `number:202` |
| Power Factor | `0x0014` | `u16` | `0.001` | `number:203` |
| Frequency | `0x001A` | `u16` | `0.01` | `number:204` |
| Total Active Energy | `0x001D` | `u32` | `0.01` | `number:205` |

### Temperature / Humidity Sensor - Slave ID 2

Function code: `FC03 - Read Holding Registers`

| Value | Register | Type | Scale | Virtual Component |
|---|---:|---|---:|---|
| Humidity | `0x0000` | `u16` | `0.1` | `number:207` |
| Temperature | `0x0001` | `u16` | `0.1` | `number:206` |

The script logic is:

```text
Start Modbus cycle
      |
      v
Read energy meter values
      |
      v
Update Shelly Virtual Components number:200 - number:205
      |
      v
Read temperature and humidity sensor
      |
      v
Update Shelly Virtual Components number:206 - number:207
      |
      v
Wait 5 seconds
      |
      v
Repeat
```

The script handles:

- Modbus CRC16
- FC03 read holding registers
- FC04 read input registers
- FC06 write single register
- FC16 write multiple registers
- Timeout handling
- Sequential polling
- Scaling and decimal conversion
- Virtual Component updates

Full script file:

```text
Shelly The Pill.js
```

Repository link:

```text
PASTE_YOUR_REPOSITORY_LINK_HERE
```

---

## 7. Result

The final result is a compact monitoring system for my home server rack.

It shows:

- Voltage
- Current
- Active power
- Power factor
- Frequency
- Total active energy
- Temperature
- Humidity

All values are available directly in Shelly through Virtual Components.

This project proves that Shelly The Pill can be used not only for simple DIY automation, but also as a small Modbus RTU gateway for more accurate and reliable wired monitoring.

The same architecture can be reused with:

- Solar inverters
- Battery systems
- Ultrasonic tank level sensors
- Optical / photoelectric sensors
- Industrial temperature sensors
- Pressure sensors
- Water meters
- Heat meters
- Other Modbus RTU devices

For me, the most valuable part of this project is that Shelly The Pill becomes a bridge between the Shelly smart home ecosystem and professional Modbus RTU devices.

<!-- IMAGE 4: Optional. Insert a Shelly dashboard or Virtual Components screenshot here if available. -->
![Shelly dashboard with Virtual Components](PASTE_IMAGE_HERE)
