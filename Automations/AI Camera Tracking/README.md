# 👁️ Camera Event Trigger (Automation Blueprint)

![Type](https://img.shields.io/badge/Type-Automation%20Blueprint-blue?style=flat-square)
![Integration](https://img.shields.io/badge/Integration-Sensors%2C%20AI%20Vision-blueviolet?style=flat-square)
![Logic](https://img.shields.io/badge/Logic-Event%20Trigger-success?style=flat-square)

This blueprint is the starting point for all camera-based AI analysis. It's designed to watch a trigger entity—like a motion sensor, doorbell, or person detector—and then activate the main `Camera Event Manager` script.

---

## 🔗 Dependencies

This automation acts as a trigger for the main `Camera Event Manager` script. A fully functional setup requires several interconnected blueprints.

### Primary Dependency
- **[Camera Event Manager](../../scripts/Notifications/Camera%20Event%20Manager/CAMERA_EVENT_MANAGER.md):** This automation's main purpose is to call the script created from this blueprint. You **MUST** have this installed and configured.

### Sub-Dependencies (Required by Camera Event Manager)
The `Camera Event Manager` relies on the following "worker" scripts to handle notifications. You will need to have these installed and configured as well:

| Script Blueprint | Role |
| :--- | :--- |
| **`family_notification_manager.yaml`** | Sends mobile alerts with smart routing. |
| **`browsermod_popups_for_cameras.yaml`** | Forces a live camera feed onto dashboards. |
| **`notify_media_players.yaml`** | Handles general audio announcements. |
| **`notify_apple_tvs.yaml`** | Sends alerts specifically to Apple TVs. |


---

## ⚙️ Configuration (Blueprint Inputs)

When you create an automation from this blueprint, you will be asked to fill in the following fields to define the trigger and the subsequent actions.

### Trigger Configuration
| Input | Description | Default |
| :--- | :--- | :--- |
| **Trigger Entity** | The `binary_sensor` that starts the event (e.g., a motion sensor or doorbell). | *(Required)* |
| **Trigger Duration**| How long the sensor must be 'on' to trigger. Helps prevent false positives. | `0` seconds |
| **Cooldown Time** | Time to wait before this automation can be triggered again. Prevents spam. | `30` seconds |

### Core Logic
| Input | Description | Example |
| :--- | :--- | :--- |
| **Manager Script** | Select the `script.camera_event_manager` entity you created. | `script.camera_event_manager` |
| **Camera Entity** | The camera to snapshot for analysis. | `camera.front_door` |
| **Event Title** | A friendly name for notifications (e.g., "Front Door"). | *(Required)* |
| **Notification Tag**| A unique tag to group or replace notifications on mobile devices. | `front-door-motion` |

### AI & Notification Settings
| Input | Description | Default |
| :--- | :--- | :--- |
| **AI Instructions** | The prompt for the vision model. | `"Describe the image..."` |
| **Who to Notify** | Target for mobile alerts (must match your Notification Manager).| `Person 1` |
| **Enable AI?** | Toggle AI analysis on/off for this trigger. | `true` |
| **Enable Tablet Popup?**| Toggle `browsermod` popups on/off. | `true` |
| **Enable Phone...** | Toggle mobile notifications on/off. | `true` |
| **Enable Audio...** | Toggle TTS announcements on/off. | `false` |
| **Enable Apple TV...**| Toggle Apple TV notifications on/off. | `false` |
| **TTS Override** | Speak a fixed phrase instead of the AI summary. | `""` |

### Custom Actions
| Input | Description |
| :--- | :--- |
| **Pre-Analysis Actions** | A sequence of actions to run **immediately** upon trigger, *before* the AI analysis begins. Perfect for turning on lights or ringing a physical chime. |

---

## 📝 Usage Example

1. Go to **Settings > Automations & Scenes > Blueprints**.
2. Find the "Camera Event - AI, LLM Vision" blueprint and click **Create Automation**.
3. Fill in the inputs as described above. For example:
    - **Trigger Entity:** `binary_sensor.driveway_motion`
    - **Manager Script:** `script.camera_event_manager`
    - **Camera Entity:** `camera.driveway`
    - **Event Title:** "Driveway"
    - **Pre-Analysis Actions:**
        ```yaml
        - service: light.turn_on
          target:
            entity_id: light.driveway_floodlight
        ```
4. Save the automation. Now, whenever the driveway motion sensor is activated, it will turn on the floodlight and then trigger the full AI analysis and notification workflow.
