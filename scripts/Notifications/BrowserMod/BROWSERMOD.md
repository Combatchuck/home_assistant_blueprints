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
