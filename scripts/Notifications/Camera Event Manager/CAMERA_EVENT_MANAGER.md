# 👁️ Camera Event Manager (Script Blueprint)

![Type](https://img.shields.io/badge/Type-Script%20Blueprint-blue?style=flat-square)
![Integration](https://img.shields.io/badge/Integration-AI%20Task%2C%20LLM%20Vision-blueviolet?style=flat-square)
![Logic](https://img.shields.io/badge/Logic-Multi--Channel%20Notifications-success?style=flat-square)

**The core engine of the vision system.** This blueprint is a centralized script for handling all camera-related events. It manages taking snapshots, triggering AI analysis, and distributing rich notifications to multiple targets including phones, tablets, speakers, and Apple TVs.

---

## ⚠️ Critical Setup & Dependencies

> [!WARNING]
> **Hardcoded Audio Group:** The "Enable Audio Announcement" feature is hardcoded to target a specific entity: `media_player.alert_notifications_group`. You **MUST** create a [Media Player Group](https://www.home-assistant.io/integrations/group/) with this exact name, containing the speakers you want for announcements, otherwise the TTS step will fail.

> [!IMPORTANT]
> **Allowed Directories:** This blueprint saves snapshots to two specific locations. You **MUST** add these paths to your `configuration.yaml` file and restart Home Assistant.
> ```yaml
> homeassistant:
>   allowlist_external_dirs:
>     - "/config/www/images/llmvision"
>     - "/media/llmvision/snapshots"
> ```

### Required Helper Scripts
This blueprint acts as a manager that delegates tasks to other, more specialized "worker" scripts. You must have the other blueprints from this repository installed and configured first.

| Script Type | Role | Recommended Blueprint |
| :--- | :--- | :--- |
| **Phone Notification** | Sends mobile alerts with smart routing (who to notify). | `family_notification_manager.yaml` |
| **Tablet Popup** | Forces a live camera feed onto wall-mounted dashboards. | `browsermod_popups_for_cameras.yaml` |
| **Audio Announcement** | Handles TTS with volume ducking and state restoration. | `notify_media_players.yaml` |
| **Apple TV Notify** | Sends alerts to Apple TVs only if they are active. | `notify_apple_tvs.yaml` |

---

## ⚙️ Configuration (Blueprint Inputs)

When importing this blueprint, you must link your helper scripts and define your environment.

| Input | Description | Example |
| :--- | :--- | :--- |
| **Script: Phone...** | Select the corresponding worker script you created. | `script.phone_notification_manager` |
| **Script: Tablet...** | Select the corresponding worker script you created. | `script.tablet_popup` |
| **Script: Audio...** | Select the corresponding worker script you created. | `script.make_announcement` |
| **Script: Apple TV...**| Select the corresponding worker script you created. | `script.apple_tv_notify` |
| **Default AI Model** | The `ai_task` entity to use for generating descriptions. | `ai_task.gemini_flash` |
| **Base URL** | Public-facing URL for accessing snapshot images on mobile. | `https://my-ha.ui.nabu.casa/local/images/llmvision`|

---

## 📝 Usage (Runtime Fields)

When you call this script from an automation, you can control its behavior using these fields.

| Field | Description | Default |
| :--- | :--- | :--- |
| **`camera_entity`** | The camera to snapshot. | *(Required)* |
| **`title_prefix`** | The title for notifications (e.g., "Front Door"). | *(Required)* |
| **`prompt`** | The instruction for the AI model. | `"Describe the image..."` |
| **`notification_tag`**| A unique ID to update mobile notifications in-place. | `""` |
| **`who_to_notify`**| Who to send mobile alerts to (must match your Phone Manager).| `"Person 1"` |
| **`enable_ai`** | Toggle AI analysis on/off for this run. | `true` |
| **`ai_model`** | (Override) Use a different AI model for this specific event. | Blueprint Default |
| **`enable_popup`** | Toggle tablet popups on/off. | `true` |
| **`enable_notification`**| Toggle mobile notifications on/off. | `true` |
| **`enable_tts`** | Toggle audio announcements on/off. | `false` |
| **`enable_appletv`** | Toggle Apple TV notifications on/off. | `false` |
| **`enable_memory`** | Toggle saving the event to LLM Vision Memory. | `true` |
| **`tts_override`** | Speak this custom text instead of the AI summary. | `""` |

---

## 💡 YAML Example

This example shows how you might call this script from a doorbell automation.

```yaml
# This action would be part of a larger automation
action:
  # Call the camera manager script
  - service: script.camera_event_manager # This is the entity created from the blueprint
    data:
      # --- Required ---
      camera_entity: camera.front_door
      title_prefix: "Front Door"
      
      # --- AI & Notification ---
      prompt: "A doorbell was rung. Describe who is at the door. Be concise."
      notification_tag: "front-door-ring"
      who_to_notify: "All"

      # --- Enable Specific Actions ---
      enable_popup: true
      enable_tts: true
      enable_appletv: true
      
      # --- Custom Message ---
      tts_override: "Someone is at the front door."
```


