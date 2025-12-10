# 🏠 House Notification Manager (Script Blueprint)

![Type](https://img.shields.io/badge/Type-Script%20Blueprint-blue?style=flat-square)
![Role](https://img.shields.io/badge/Role-Orchestrator-purple?style=flat-square)
![Dependency](https://img.shields.io/badge/Requires-Worker%20Scripts-orange?style=flat-square)

The centralized "Conductor" for all home audio announcements.

Instead of adding complex logic to every single automation in your house, you call this single manager script. It intelligently routes your text-to-speech (TTS) message to the correct sub-systems (Standard Speakers vs. Apple TVs) based on your preferences.

Configuration (One-Time Setup)
Because this script acts as a manager, you must "wire it up" to your existing hardware groups and worker scripts during the Blueprint Import process.


## 🔗 The Architecture

This blueprint does not speak directly. It orchestrates **other scripts** to do the work:

1. **Standard Speakers:** Calls the **"Make Announcement"** script (always fires).
2. **Apple TVs:** Calls the **"Apple TV Notify"** script (conditional).

```mermaid
graph LR
    A[Automation] -->|Call Script| B(House Manager)
    B -->|Always| C[Standard Speakers]
    B -->|If Enabled| D[Apple TVs]


