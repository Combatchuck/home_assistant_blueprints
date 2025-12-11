# 📘 Home Assistant Blueprints

![Home Assistant](https://img.shields.io/badge/Home%20Assistant-Blueprints-blue?style=for-the-badge&logo=home-assistant)
![Language](https://img.shields.io/badge/Language-YAML-red?style=for-the-badge)
![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-green.svg?style=for-the-badge)

A collection of advanced, logic-heavy blueprints designed to standardize **AI vision handling**, **smart notifications**, and **media player control** across the Home Assistant ecosystem.

These blueprints go beyond simple automations by introducing **state awareness** (smart filtering), **AI integration** (Gemini/Ollama), and **hardware-specific features** (Browser Mod/Apple TV).

---

## 📂 Blueprint Catalog

| Category | File | Description | Type |
| :--- | :--- | :--- | :--- |
| **👁️ AI Vision** | `camera_event_manager.yaml` | **The Core Engine.** Unifies snapshots, AI analysis, and multi-target notifications. | Automation |
| **📲 Routing** | `phone_notification_manager.yaml` | Routes notifications to specific people based on presence (Home/Away). | Script |
| **🏠 Orchestrator** | `house_notification_manager.yaml` | Central hub that routes TTS to Standard Speakers vs Apple TVs. | Script |
| **🔊 Audio** | `make_announcement.yaml` | Robust TTS announcements with volume ducking and state restoration. | Script |
| **📺 Apple TV** | `apple_tv_notify.yaml` | Sends TTS to Apple TVs only if they are active (skips sleeping devices). | Script |
| **📱 Dashboard** | `tablet_popup.yaml` | Forces a camera popup on wall tablets using Browser Mod. | Script |

---

## 🚀 Key Features

### 1. 👁️ Camera Event Manager (AI Vision)
* **Dual-Path Snapshots:** Saves high-res images to local storage for history *and* `www` for mobile notifications.
* **Generative AI:** Feeds snapshots to AI agents (Gemini/Ollama) to generate natural language descriptions of the event.
* **Context Memory:** Logs events to a database to give the AI long-term memory of home activity.

### 2. 📺 Smart Media Filtering
Stop waking up the house with announcements.
* **Apple TV Notify:** Checks if the Apple TV is `playing`, `paused`, or `on`. If the device is `off` or `sleeping`, the announcement is silently skipped.
* **Audio Ducking:** Announcements snapshot the current volume, lower the media, speak the text, and then restore the previous volume and content perfectly.

### 3. 📱 Dashboard Integration
* **Tablet Popups:** Leverages **Browser Mod** to create immediate visual interruptions on wall-mounted dashboards when critical events (like a doorbell ring) occur.

---

## 📥 Installation

There are two ways to import these blueprints into your Home Assistant instance:

### Option A: Direct Import (Button)
1. Copy the URL of the specific YAML file in this repository.
2. Go to **Settings** → **Automations & Scenes** → **Blueprints**.
3. Click **Import Blueprint** and paste the URL.

### Option B: Manual Installation
1. Download the `.yaml` files from this repository.
2. Upload them to your Home Assistant `config/blueprints/automation/` (for automations) or `config/blueprints/script/` (for scripts) folder.
3. Reload Automations in Developer Tools.

---

## ⚠️ Dependencies & Requirements

> [!IMPORTANT]
> Some blueprints require specific integrations to function correctly.

* **Browser Mod:** Required for `tablet_popup.yaml`. You must have the integration installed via HACS and your tablet IDs registered.
* **External Access:** For notification images to load on mobile devices outside your network, your `www` folder must be accessible via your external URL.
* **Apple TV Integration:** The notification scripts rely on the official Apple TV integration for state detection.
