# Shelly Virtual Components

The script updates Shelly Virtual Components directly.

Create these number components before starting the script:

| Component | Meaning |
|---|---|
| `number:200` | Voltage |
| `number:201` | Current |
| `number:202` | Active Power |
| `number:203` | Power Factor |
| `number:204` | Frequency |
| `number:205` | Total Active Energy |
| `number:206` | Temperature |
| `number:207` | Humidity |

The relevant script section:

```js
const VC_VOLTAGE      = Virtual.getHandle("number:200");
const VC_CURRENT      = Virtual.getHandle("number:201");
const VC_ACTIVE_POWER = Virtual.getHandle("number:202");
const VC_POWER_FACTOR = Virtual.getHandle("number:203");
const VC_FREQUENCY    = Virtual.getHandle("number:204");
const VC_TOTAL_ENERGY = Virtual.getHandle("number:205");

const VC_SENSOR_TEMP     = Virtual.getHandle("number:206");
const VC_SENSOR_HUMIDITY = Virtual.getHandle("number:207");
```
