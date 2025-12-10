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
  <br>
  
Helper Scripts:<br>
The following helper scripts are required for this blueprint to work correctly:<br>
<br>
Phone Notification Manager<br>
Tablet Popup<br>
Audio Announcement<br>
Apple TV Notify
