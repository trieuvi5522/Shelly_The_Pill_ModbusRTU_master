# Modbus Register Map

## Serial settings

```text
Baudrate: 9600
Mode: 8N1
```

## Energy meter

Slave ID: `1`  
Function code: `FC04 - Read Input Registers`

| Value | Register | Quantity | Type | Scale | Decimals | Virtual Component |
|---|---:|---:|---|---:|---:|---|
| Voltage | `0x0000` | 1 | `u16` | `0.1` | 1 | `number:200` |
| Current | `0x0003` | 1 | `u16` | `0.01` | 2 | `number:201` |
| Active Power | `0x0008` | 1 | `s16` | `1` | 0 | `number:202` |
| Power Factor | `0x0014` | 1 | `u16` | `0.001` | 2 | `number:203` |
| Frequency | `0x001A` | 1 | `u16` | `0.01` | 1 | `number:204` |
| Total Active Energy | `0x001D` | 2 | `u32` | `0.01` | 2 | `number:205` |

## RS485 temperature and humidity sensor

Slave ID: `2`  
Function code: `FC03 - Read Holding Registers`

| Value | Register | Quantity | Type | Scale | Decimals | Virtual Component |
|---|---:|---:|---|---:|---:|---|
| Humidity | `0x0000` | 1 | `u16` | `0.1` | 2 | `number:207` |
| Temperature | `0x0001` | 1 | `u16` | `0.1` | 1 | `number:206` |
