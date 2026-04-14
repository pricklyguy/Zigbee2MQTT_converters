# 🪟 Smart Motor Roller Shade — Zigbee2MQTT External Converter

> Add proper `cover` entity support with battery reporting for **Smart Motor** Zigbee roller shades in Home Assistant + Zigbee2MQTT.

[![Zigbee2MQTT](https://img.shields.io/badge/Zigbee2MQTT-External_Converter-blue?logo=zigbee&logoColor=white)](https://www.zigbee2mqtt.io/advanced/support-new-devices/01_support_new_devices.html)
[![Home Assistant](https://img.shields.io/badge/Home_Assistant-Compatible-41BDF5?logo=homeassistant&logoColor=white)](https://www.home-assistant.io/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

---

## 📋 Overview

Smart Motor roller shades show up as **"Unsupported"** in Zigbee2MQTT because they broadcast a generic manufacturer ID. This converter maps them correctly as a `cover` entity with:

- ✅ **Lift control** (open/close/stop/position)
- ✅ **Battery reporting**
- ✅ No custom firmware or Zigbee coordinator changes needed

---

## 📁 File Structure

After setup, your Zigbee2MQTT config directory should look like this:

```
/config/zigbee2mqtt/
├── configuration.yaml
└── external_converters/
    └── smart_motor.js        ← the file we're creating
```

---

## 🔧 Installation

> **Note:** The standard HA File Editor often restricts new folder creation. Use one of the two methods below.

<details>
<summary><strong>Method 1 — Terminal / SSH (Recommended)</strong></summary>

<br>

1. Open a terminal (Terminal & SSH add-on, or VS Code Server integrated terminal).

2. Run the following commands:

```bash
cd /config/zigbee2mqtt
mkdir -p external_converters
cd external_converters
touch smart_motor.js
```

3. Open `smart_motor.js` in Studio Code Server and paste the converter code below.

</details>

<details>
<summary><strong>Method 2 — VS Code Server File Browser</strong></summary>

<br>

1. In VS Code Server, navigate to `/config/zigbee2mqtt/`
2. Create a new folder: `external_converters`
3. Inside that folder, create a new file: `smart_motor.js`
4. Paste the converter code below into the file.

</details>

---

## 📄 Converter Code

Paste this into `smart_motor.js`:

```js
const m = require('zigbee-herdsman-converters/lib/modernExtend');

const definition = {
    zigbeeModel: ['Smart Motor'],
    model: 'Smart Motor',
    vendor: 'Smart Motor',
    description: 'Zigbee Roller Shade Motor',
    extend: [
        m.battery(),
        m.windowCovering({ controls: ['lift'] }),
    ],
};

module.exports = definition;
```

---

## ✅ Verification

1. **Restart** the Zigbee2MQTT add-on (not full HA — just the add-on).
2. Open **Zigbee2MQTT → Logs** and look for:
   ```
   Loaded external converter: smart_motor.js
   ```
3. Re-pair or re-interview your shade device.
4. The device should now appear as a `cover` entity with a battery sensor in Home Assistant.

---

## 🐛 Troubleshooting

| Symptom | Fix |
|---|---|
| Still shows "Unsupported" | Confirm `smart_motor.js` is in `/config/zigbee2mqtt/external_converters/` |
| Converter not loading | Check Z2M logs for JS syntax errors |
| `cover` entity missing | Re-interview the device after the converter loads |
| Battery not reporting | Some hardware revisions require a wake event — open/close the shade once |

---

## 📎 References

- [Zigbee2MQTT — Supporting New Devices](https://www.zigbee2mqtt.io/advanced/support-new-devices/01_support_new_devices.html)
- [zigbee-herdsman-converters `modernExtend` API](https://github.com/Koenkk/zigbee-herdsman-converters)
- [Home Assistant Zigbee2MQTT Add-on](https://github.com/zigbee2mqtt/hassio-zigbee2mqtt)
