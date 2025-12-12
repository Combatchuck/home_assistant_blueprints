# 📺 Apple TV Notify (Script Blueprint)

![Type](https://img.shields.io/badge/Type-Script%20Blueprint-blue?style=flat-square)
![Integration](https://img.shields.io/badge/Integration-Apple%20TV-black?style=flat-square&logo=apple)
![Logic](https://img.shields.io/badge/Logic-Smart%20Filtering-success?style=flat-square)

A Home Assistant blueprint designed to send **Text-to-Speech (TTS)** announcements to Apple TVs without being intrusive.

## 🧠 Why use this? (Smart Filtering)
Standard notification scripts often blindly send audio to all devices. If an Apple TV is off (sleeping), sending audio will wake the device and turn on the attached TV via HDMI-CEC.

**This blueprint checks the state of the device first.** It will silently skip any device that is not currently active.

| TV State | Result |
| :--- | :--- |
| **▶️ Playing** | ✅ **Announcement plays** (Audio ducks automatically) |
| **⏸️ Paused** | ✅ **Announcement plays** |
| **🟢 On / Idle / Buffering** | ✅ **Announcement plays** |
| **⚫ Off / Standby** | 🔇 **Skipped** (TV remains off) |

---

## ⚙️ Configuration

### 1. Blueprint Inputs (Setup)
When importing the blueprint, you will be asked to configure the default targets.

* **Default Apple TVs (Helper Group):** Select a Home Assistant **Group** or a list of entities containing all your Apple TVs. This acts as the default distribution list for announcements.

### 2. Runtime Fields (Automation)
When calling this script in an automation, you can use the following fields:

* **Message (Required):** The text string you want the voice to read.
* **Target Override (Optional):** If you want to announce *only* to a specific room (ignoring the default group), select specific entities here. They will still be subject to the "Smart Filtering" logic.

---

## 📝 Usage Example

Here is how to call this script in your automations:

```yaml
action: script.apple_tv_notify
data:
  message: "The washing machine has finished."
  target_override:
    - media_player.living_room_apple_tv
```

> [!IMPORTANT]
> **TTS Provider Configuration**
> This blueprint is hardcoded to use the `google_translate` TTS service. If you use a different provider (e.g., `tts.cloud_say`, Piper), you must manually edit the `media_content_id` line in the blueprint's YAML file to match your provider.

> [!NOTE]
> **Entity Requirements**
> The `target_override` and `default_apple_tvs` inputs must be `media_player` entities created by the official Home Assistant Apple TV integration.
