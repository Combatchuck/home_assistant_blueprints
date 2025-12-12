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

## ⚙️ Configuration (Blueprint Inputs)

When importing this blueprint, you will map your family members to the logic slots.

> [!TIP]
> The blueprint asks for the **Notification Service Name** (e.g., `notify.mobile_app_your_phone_name`). You can find this in **Developer Tools** → **Services**.

| Input | Description | Example |
| :--- | :--- | :--- |
| **Person 1 Notify** | The notification service for the Primary User. | `notify.mobile_app_chucks_iphone` |
| **Person 2 Notify** | The notification service for Family Member 2. | `notify.mobile_app_wife_iphone` |
| **Person 2 Tracker** | The `person` entity for Family Member 2's presence. | `person.wife` |
| **Person 3 Notify** | The notification service for Family Member 3. | `notify.mobile_app_childs_iphone` |
| **Person 3 Tracker** | The `person` entity for Family Member 3's presence. | `person.child` |

---

## 📝 Usage (Runtime Fields)

When calling this script in an automation, you can use the following fields.

| Field | Description | Required |
| :--- | :--- | :--- |
| **`title`** | The notification header text. | ✅ Yes |
| **`message`** | The notification body text. | ✅ Yes |
| **`who_to_notify`** | Dropdown to select the target users for this message. | ✅ Yes |
| **`image_path`** | (Optional) URL or path to an image to attach. | ❌ No |
| **`tag`** | (Optional) A unique ID to update existing notifications. | ❌ No |

**`who_to_notify` Options:** `All`, `Person 1`, `Person 2`, `Person 3`, `Person 1/Person 2`, `Person 1/Person 3`, `Person 2/Person 3`.

---

## 💡 YAML Examples

### Example 1: Generic Announcement
This sends a message to "All". Person 1 gets it anywhere; Persons 2 & 3 only get it if they are home.

```yaml
action:
  - service: script.phone_notification_manager
    data:
      title: "Laundry Room"
      message: "The Washing Machine has finished."
      who_to_notify: "All"
```

### Example 2: Security Alert with Image
This sends a high-priority alert to Persons 1 and 2 with a camera snapshot, and uses a `tag` to ensure it can be updated.
```yaml
action:
  - service: script.phone_notification_manager
    data:
      title: "⚠️ Motion Detected"
      message: "Movement detected in the backyard."
      who_to_notify: "Person 1/Person 2"
      image_path: "/local/snapshots/backyard_latest.jpg"
      tag: "backyard-motion"
```

> [!NOTE]
> **External Image Access:** For images to load on phones off your local WiFi, your Home Assistant instance must be accessible externally (e.g., via Nabu Casa) and your app must be configured to resolve local URLs. Images should typically be placed in the `/config/www` directory, which is accessed via `/local/` in the URL.
