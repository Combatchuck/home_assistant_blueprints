# 🔊 Make Announcement (Script Blueprint)

![Type](https://img.shields.io/badge/Type-Script%20Blueprint-blue?style=flat-square)
![Category](https://img.shields.io/badge/Category-Media%20Control-red?style=flat-square)
![Feature](https://img.shields.io/badge/Feature-Audio%20Ducking-success?style=flat-square)
![Feature](https://img.shields.io/badge/Feature-State%20Restoration-success?style=flat-square)

A robust Text-to-Speech (TTS) engine that solves the biggest annoyance with home automation announcements: **ruining your music.**

This script performs a full "Snapshot -> Speak -> Restore" cycle. It saves the current state of your media players (volume, current track, play status), plays your announcement at a set volume, and then puts everything back exactly how it was.

## 🔄 How it Works

1. **📸 Snapshot:** Instantly records the state of all target speakers (Volume: 20%, Playing: "Spotify").
2. **🔊 Set Volume:** Raises the volume to the announcement level (e.g., 50%) to ensure the message is heard.
3. **🗣️ Speak:** Plays the TTS message.
4. **⏳ Wait:** Calculates the duration of the text (Length / 10 + Buffer) and pauses the script.
5. **🔙 Restore:** Reapplies the snapshot (Volume: 20%, Playing: "Spotify").

---

## ⚙️ Configuration (One-Time Setup)

When importing the blueprint, you can set defaults to avoid repetitive configuration later.

| Input | Description | Default |
| :--- | :--- | :--- |
| **Default Volume** | The standard volume level (0.0 - 1.0) for announcements if one isn't specified in the automation. | `0.4` (40%) |
| **TTS Provider** | The internal name of your TTS service. Common examples: `google_translate`, `cloud` (Nabu Casa), `piper`. | `google_translate` |

---

## 📝 Usage (Automation)

When calling this script in an automation, you **must** provide the message and the target speakers.

### 🔹 Required Fields
* **Message:** The text string to announce.
* **Target Players:** The list of `media_player` entities or groups to speak on.

### 🔹 Optional Fields
* **Volume:** Override the default volume for *this specific* announcement. (e.g., set to `0.8` for a security alarm, or `0.2` for a night-time whisper).

---

## 💡 YAML Examples

### Example 1: Standard Announcement
Uses the blueprint's default volume (40%).

```yaml
action: script.make_announcement
data:
  message: "Dinner is ready."
  target_players:
    - media_player.kitchen_sonos
    - media_player.living_room_echo
```

Example 2: High-Priority Alarm

Overrides the volume to 90% to ensure it is heard over loud music or TV.

```YAML
action: script.make_announcement
data:
  message: "WARNING: Water leak detected in the basement."
  target_players: media_player.whole_house_audio
  volume: 0.9
```

⚠️ Notes & Limitations
Wait Calculation: The script estimates the spoken duration using the formula: (Character Count / 10) + 4 seconds. This is a general approximation. 
<br>
Very slow speech rates might get cut off, while very fast speech might leave a few seconds of silence before music resumes.
<br> <br> <br>
Scene Overwrite: This script uses a temporary scene ID scene.temp_announcement_snapshot. If multiple announcements fire exactly at the same time, they might overwrite each other's snapshot.
