# 📱 Tablet Camera Popup (Script Blueprint)

![Type](https://img.shields.io/badge/Type-Script%20Blueprint-blue?style=flat-square)
![Dependency](https://img.shields.io/badge/Dependency-Browser_Mod-orange?style=flat-square)
![Category](https://img.shields.io/badge/Category-Dashboard%20Control-blueviolet?style=flat-square)

A specialized script that forces a live camera feed to pop up on specific wall-mounted tablets. Ideal for visual confirmation when a doorbell rings or motion is detected.

> [!IMPORTANT]
> **Dependency: Browser Mod**
> This blueprint requires the [Browser Mod](https://github.com/thomasloven/hass-browser_mod) integration (available via HACS). You must register your tablets as "Browsers" within that integration to obtain their `browser_id`.

---

## ⚙️ Configuration

### 1. Blueprint Inputs (Setup)
When importing the blueprint, you will configure the global settings for the popup behavior.

| Input | Description | Default |
| :--- | :--- | :--- |
| **Target Browser IDs** | A YAML list of the Browser IDs (found in Browser Mod settings) that should display the popup. | *(Required)* |
| **Popup Timeout** | How long (in milliseconds) the popup remains open before auto-closing. | `11000` (11s) |

**Format for Browser IDs:**
Because this uses the `object` selector, ensure you enter the IDs as a list:
```yaml
- 78245-tablet-kitchen
- 99281-tablet-hallway
```

2. Runtime Fields (Automation)

When calling this script in an automation, you must provide the context for the specific event.

Title Text: The header displayed on the popup (e.g., "Driveway Motion").

Camera Entity: The specific camera. entity you want to stream.

📝 Usage Example
Here is a standard automation example: "When the doorbell is pressed, show the camera on the kitchen tablet."

```YAML
trigger:
  - platform: state
    entity_id: binary_sensor.front_doorbell_ring
    to: "on"
action:
  - action: script.tablet_popup
    data:
      title_text: "Front Door - Ringing!"
      camera_entity: camera.front_door_high_res
```
🔎 Technical Details
Card Type: Uses a standard picture-glance card inside the popup for minimal latency.

Auto-Close: The popup is configured to allow the user to close it manually (dismissable: true), but will also close automatically after the configured timeout.
