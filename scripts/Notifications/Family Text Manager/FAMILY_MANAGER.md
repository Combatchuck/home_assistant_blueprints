# 📲 Phone Notification Manager (Script Blueprint)

![Type](https://img.shields.io/badge/Type-Script%20Blueprint-blue?style=flat-square)
![Category](https://img.shields.io/badge/Category-Notifications-green?style=flat-square)
![Logic](https://img.shields.io/badge/Logic-Presence%20Aware-orange?style=flat-square)

A smart routing engine for mobile notifications. Instead of blasting every phone in the house for every event, this script routes messages based on **user roles** and **location status**.

## 🧠 The Logic (Smart Routing)
This blueprint divides users into two categories: the **Primary User** (e.g., the Home Assistant Admin) and **Family Members**.

| Role | User | Logic Behavior |
| :--- | :--- | :--- |
| **Primary** | **Person 1** | **Always Notified.** If selected in the target list, they receive the message regardless of where they are. |
| **Family** | **Person 2 & 3** | **Conditionally Notified.** They only receive the message if their presence entity is `home`. If they are away, they are skipped to reduce notification fatigue. |

---

## ⚙️ Configuration (One-Time Setup)

When importing this blueprint, you will map your family members to the logic slots.

### 1. Notification Services
The blueprint asks for the **Service Name** (not the entity).
* *Format:* `notify.mobile_app_your_phone_name`
* *Where to find it:* Go to **Developer Tools** → **Services** and search for "notify".

### 2. Presence Entities
For Persons 2 and 3, you must select their `person.` entity (e.g., `person.wife`, `person.child`). This is used to check if they are `home`.

---

## 📝 Usage (Automation)

When calling this script in an automation, you can use the following fields:

### 🔹 Required Fields
* **Title:** The notification header.
* **Message:** The body text.
* **Who to Notify:** A dropdown selection to determine targets.
    * `All`
    * `Person 1`
    * `Person 2`
    * `Person 3`
    * `Person 1/Person 2`
    * `Person 1/Person 3`
    * `Person 2/Person 3`

### 🔹 Optional Fields
* **Image Path:** Attaches a rich image to the notification.
    * *Note:* Use `/local/filename.jpg` for images stored in your `www` folder.
* **Notification Tag:** A unique string ID.
    * *Feature:* If you send a new notification with the *same* tag, it replaces the old one on the phone screen (great for updating camera snapshots so you don't get a stack of 10 old images).

---

## 💡 YAML Examples

### Example 1: The "Everyone" Announcement
Useful for generic home events (e.g., "Washer is done"). Person 1 gets it anywhere; Persons 2 & 3 only get it if they are home.

```yaml
action: script.phone_notification_manager
data:
  title: "Laundry Room"
  message: "The Washing Machine has finished."
  who_to_notify: "All"
```
Example 2: Security Alert (With Image)

Sends a high-priority alert with a camera snapshot.
```YAML
action: script.phone_notification_manager
data:
  title: "⚠️ Motion Detected"
  message: "Movement detected in the backyard."
  who_to_notify: "Person 1/Person 2"
  image_path: "/local/snapshots/backyard_latest.jpg"
  tag: "backyard-motion"
```
⚠️ Requirements<br>

External Image Access If you attach an image using image_path, your Home Assistant instance must be accessible externally (via Nabu Casa or distinct URL) for the image to load on a phone that is not on your WiFi.

If you use local paths (e.g., /local/img.jpg), ensure your mobile app is configured correctly to resolve internal URLs.
