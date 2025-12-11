# Camera Event Manager Blueprint

## Description

**Unified Camera Handler** for Home Assistant that manages snapshots, AI analysis, and multiple notification channels. It enables customizable behavior based on camera events, including popup notifications, mobile alerts, audio announcements, and integration with Apple TV. It also stores event details in LLM Memory for future reference.

### Workflow:
1. Takes snapshots to `/media/llmvision/snapshots/` and `/config/www/images/llmvision/`.
2. Sends a "Analyzing..." notification.
3. Generates an AI description of the image.
4. Distributes the result via Popup, Phone, Audio, or Apple TV.
5. Saves the event to LLM Memory for future reference.

## ⚠️ CRITICAL SETUP REQUIRED

- **File Paths:** 
  This blueprint forces snapshots to be stored in the specified paths. Ensure these directories exist and are accessible in your `configuration.yaml`.

- **Required Directories:**
  You **MUST** ensure the following folders exist and are allowed in the configuration:

  ```yaml
  homeassistant:
    allowlist_external_dirs:
      - "/config/www/images/llmvision"
      - "/media/llmvision/snapshots"

  ```
After adding this, restart Home Assistant.

2. Required Helper Scripts

This blueprint does not handle notifications directly; it acts as a "General" delegating tasks to "Soldiers." 
<br>
You must have the following scripts installed (via the other blueprints in this repo) and ready to select during import:

| Script          | Role                     | Recommended Blueprint     | Why?                                                        |
|-----------------|---------------------------|----------------------------|-------------------------------------------------------------|
| Phone Notify    | Phone Notification Manager | Handles smart routing      | Handles smart routing (Who to notify & when).              |
| Tablet Popup    | Tablet Popup               | Tablet Popup               | Forces the camera feed onto wall-mounted dashboards.       |
| Audio Announce  | Make Announcement          | Audio Announcement         | Handles TTS with volume ducking and restoration.           |
| Apple TV Notify | Apple TV Notify            | Apple TV Notify            | Sends alerts to Apple TVs only if they are active.         |

---

## 🔄 Workflow Overview

When triggered (such as by a motion sensor or doorbell), the automation executes the following sequence:

### 📸 1. Dual Snapshot  
Captures high-resolution images to:
- **`/media/`** → archived storage  
- **`/www/`** → instantly accessible from mobile devices  



### 📲 2. Instant Alert  
Sends an immediate notification:  
**“Motion Detected… Analyzing…”**  
This ensures you know something is happening *before* AI finishes processing.



### 🧠 3. AI Analysis  
The captured image is processed through your AI provider (e.g., **Gemini**, **Ollama**) using your custom prompt.



### 📝 4. Notification Update  
Once the AI returns a summary (e.g., *“A delivery driver left a package”*),  
the phone notification is **updated in-place** with the final result.



### 📺 5. Multi-Channel Distribution

- **📱 Tablets:** Forces a popup showing the live camera feed  
- **🔊 Speakers:** Announces the AI summary via TTS  
- **📺 Apple TVs:** Displays the summary directly on screen  



### 💾 6. Memory Storage  
The event is saved into the **LLM Vision Memory Database**, enabling long-term context and future automations to reference it.

---

## ⚙️ Configuration (One-Time Setup)

When importing the blueprint, you will be asked to map your environment.

---

### 🔗 Link Your Scripts
You will see inputs such as **Script: Phone Notification Manager**.  
Use the entity picker to select the specific scripts you created earlier.

---

### 🤖 AI Model
Select the `ai_task` entity you want to use  
(e.g., `ai_task.gemini_flash`).

---

### 🌐 Base URL
The blueprint needs a URL that your phone can access from outside your home.  
This is used to generate links to snapshot images.

**Format:**
https://<YOUR_EXTERNAL_URL>/local/images/llmvision
<br>
DO NOT INCLUDE A TRAILING SLASH

---
## 📝 Usage (Automation)

Here is how to configure the automation trigger and fields when creating an automation from this blueprint.

---

### 🔹 Core Fields

- **Camera Entity**  
  The camera to take snapshots from.

- **Title / Event Name**  
  Used for the notification header (e.g., `"Front Door"`).

- **AI Instructions (Prompt)**  
  Tells the AI what to look for.

  **Default:**  
  `"Describe the image in detail."`

  **Examples:**  
  - `"Is there a package visible?"`  
  - `"Is it a person or a car?"`

---

### 🔹 Toggles (On/Off)

Customize the behavior of each automation:

- **Enable AI Analysis** (Default: `true`)  
- **Enable Tablet Popup**  
- **Enable Phone Notification**  
- **Enable Audio Announcement**  
- **Enable Apple TV Notification**  
- **Enable LLM Memory**

---

### 🔹 Advanced Overrides

- **TTS Override Message**  
  Provide a custom phrase for audio announcements  
  (e.g., `"Ding dong"`).  
  This replaces the AI-generated summary when announced via speakers.

- **Notification Tag** *(Highly Recommended)*  
  Enter a unique ID (e.g., `front_door_motion`).  
  This ensures your phone **updates** an existing notification instead of spamming multiple alerts when the camera triggers repeatedly.

---

💡 YAML Example
Scenario: Front Doorbell Logic.

Notify phones with AI description.

Pop up on the kitchen tablet.

Announce on speakers.

```YAML
alias: "Security - Front Doorbell Ring"
description: ""
use_blueprint:
  path: your_name/camera_event_manager.yaml
  input:
    camera_entity: camera.front_door_high_res
    title_prefix: "Front Door"
    prompt: "A doorbell was rung. Describe who is at the door. Be concise."
    notification_tag: "front-door-ring"
    enable_tts: true
    enable_popup: true
    enable_appletv: true
    # Note: Requires a group named 'media_player.alert_notifications_group' 
    # for the audio announcement step, or edit the blueprint to match your setup.
```


