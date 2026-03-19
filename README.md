# Aqara Cube Magic Oracle

A Magic 8-Ball style oracle blueprint for Home Assistant using the **Aqara Cube T1 Pro** via Zigbee2MQTT.

Ask the cube a yes/no question, perform the ritual, and let the oracle speak.

---

## How it works

1. **Shake** the cube — lights dim, the ritual begins
2. **Place** the cube on any face — the oracle reads which side is up
3. **Rotate** the cube left or right to channel your answer
4. **Stop** — the oracle decides
5. **Reveal** — lights turn green, orange, or red; the oracle speaks

The outcome is determined by a combination of which face you land on and how far you rotate, making it genuinely unpredictable and difficult to game.

---

## Requirements

- **Aqara Cube T1 Pro** (model CTP-R01) in scene mode
- **Home Assistant** with MQTT integration configured
- **Zigbee2MQTT** with the cube paired and publishing
- An **Assist Satellite** (e.g. a voice assistant speaker)
- One or more **lights** to use as oracle atmosphere *(optional — leave blank for announcement only)*

---

## Installation

### 1. Add the blueprint

Copy `aqara_cube_oracle.yaml` into your Home Assistant blueprints folder:

```
config/blueprints/automation/aqara_cube_oracle.yaml
```

Or in the HA UI: **Settings → Automations → Blueprints → Import Blueprint** and point to the file.

### 2. Find your Zigbee2MQTT topic

In Z2M, open your cube device → **About** tab. The topic is shown there, typically:

```
zigbee2mqtt/aqara_cube
```

### 3. Find your Action Angle sensor entity

In Home Assistant, go to **Settings → Devices & Services → Zigbee2MQTT** and open your cube device. Look for a sensor named:

```
sensor.<friendly_name>_action_angle
```

For example, if your cube's friendly name in Z2M is `cube`, the entity will be:

```
sensor.cube_action_angle
```

### 4. Create the automation

**Settings → Automations → Create Automation → Use Blueprint → Aqara Cube Oracle**

Fill in the four fields:

| Field | Description | Example |
|---|---|---|
| **Cube MQTT Topic** | Z2M topic for your cube | `zigbee2mqtt/aqara_cube` |
| **Oracle Lights** | *(Optional)* Lights to use for atmosphere | `light.living_room` |
| **Assist Satellite** | Speaker to announce the answer | `assist_satellite.living_room` |
| **Action Angle Sensor** | The `_action_angle` sensor entity | `sensor.cube_action_angle` |

Save the automation.

---

## Using the oracle

1. Think of a yes/no question
2. **Shake** the cube — your lights will dim
3. **Place** the cube flat on any face
4. **Wait 1 second** for the cube to settle
5. **Rotate** left or right — keep going until you feel ready to stop
6. **Stop rotating** and hold still — the oracle responds within a few seconds
7. The lights will flash and the oracle will speak

### Responses

| Light colour | Category | Meaning |
|---|---|---|
| Green | Positive | Yes, favourable |
| Orange | Neutral | Uncertain, try again |
| Red | Negative | No, unfavourable |

---

## Troubleshooting

**Automation triggers but nothing happens after the shake**
- Check that your MQTT topic matches exactly what Z2M publishes
- Open Z2M → your cube device → **Exposes** and shake it — you should see the `action: shake` payload appear

**Lights dim but the oracle never speaks**
- The cube didn't report a `side_up` event within 3 seconds of the shake
- Place the cube flat immediately after shaking; don't hold it in the air

**The oracle speaks but lights don't change colour**
- Your lights may not support colour. The automation works without colour-capable lights; brightness will still change.
