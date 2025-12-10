# 📘 Home Assistant Blueprints

![Home Assistant](https://img.shields.io/badge/Home%20Assistant-Blueprints-blue?style=for-the-badge&logo=home-assistant)
![Language](https://img.shields.io/badge/Language-YAML-red?style=for-the-badge)
![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-green.svg?style=for-the-badge)

This repository contains custom blueprints designed to standardize **advanced notifications**, **AI vision handling**, and **media player control** across the home ecosystem.

---

## 👁️ 1. Camera Event Manager (AI Vision)

**File:** `camera_event_manager.yaml`

The core engine for the home's security system. It unifies snapshot generation, AI analysis, and multi-channel notifications into a single, cohesive workflow.

### Capabilities
* **Dual-Path Snapshots:** Saves images to `/media/llmvision/snapshots/` (for HA history/timeline) and `/config/www/` (for mobile notifications).
* **AI Integration:** Sends images to `ai_task` entities (e.g., Gemini, Ollama) to generate natural language descriptions of events.
* **Multi-Target Notification:** Simultaneously notifies Phones, Wall Tablets (Popup), Standard Speakers (TTS), and Apple TVs.
* **LLM Memory:** Stores significant events into a memory database to provide future context for AI agents.

> [!WARNING]
> **Configuration Requirement**
> Your `configuration.yaml` must allow external access (whitelist) to the snapshot directories defined in this blueprint for notifications to display images correctly.

  allowlist_external_dirs:
    - /config/www/images
    - /media/llmvision/snapshots

---

## 📲 2. Notification Managers

**Files:** `family_notification_manager.yaml`, `house_notification_manager.yaml`, `notify_apple_tvs.yaml`

A routed notification system that ensures the right person gets the message based on presence and device status.

### 👤 Phone Notification Manager
Handles logic for delivering notifications to mobile devices.
* **Person 1 (Charles):** Always notified regardless of location.
* **Persons 2 & 3 (Petra/Ethan):** Only notified if their `person` entity status is `home`.
* **Features:** Supports actionable tags and rich image attachments.

### 🏠 House Notification Manager
Broadcasts TTS (Text-to-Speech) announcements across the home audio system.
* **Standard Speakers:** Always announce.
* **Apple TVs:** Conditionally announce based on the logic below.

### 📺 Apple TV Notify
Smart filtering to prevent interruptions during quiet times or when the TV is off.
| State | Action |
| :--- | :--- |
| **On / Playing** | ✅ Announce |
| **Paused / Buffering** | ✅ Announce |
| **Off / Sleeping** | ❌ Silent |

I recommend creating a helper group with all of your appl tv containied within, then targeting that.

---

## 📟 3. Tablet & Media Control

**Files:** `browsermod_popups_for_cameras.yaml`, `notify_media_players.yaml`

### Tablet Popup
Leverages `browser_mod` to force a live camera feed popup on specific wall-mounted dashboard tablets immediately when motion is detected.

### Make Announcement (Robust TTS)
A smart script that handles audio ducking and state restoration to ensure announcements are heard without ruining the media experience.
1. **Snapshot:** Records
