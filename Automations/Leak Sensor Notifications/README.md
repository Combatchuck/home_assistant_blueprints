# 💧 Leak Sensor Alert Manager

![Type](https://img.shields.io/badge/Type-Automation%20Blueprint-blue?style=flat-square)
![Category](https://img.shields.io/badge/Category-Notification-orange?style=flat-square)
![Feature](https://img.shields.io/badge/Feature-Dynamic%20Detection-success?style=flat-square)

A set-and-forget automation that automatically monitors **every** moisture sensor in your Home Assistant instance. If any of them detect a leak, it instantly sends a notification listing all affected sensors. No need to manually add new sensors to your automations ever again.

## 🔄 How it Works

This blueprint uses a dynamic template to achieve its "zero-configuration" goal.

1.  **🔎 Monitor:** The trigger continuously scans all `binary_sensor` entities in your system.
2.  **💧 Trigger:** The automation runs the moment it finds any sensor with `device_class: moisture` that has a state of `'on'`.
3.  **📢 Alert:** It then gathers the friendly names of all triggered sensors and sends a single, consolidated notification to your chosen device.

---

## ⚙️ Configuration

When importing the blueprint, you only need to tell it *where* to send the alert.

| Input | Description | Example |
| :--- | :--- | :--- |
| **Notification Service** | The service to call for sending the alert. This is typically a mobile device. | `notify.mobile_app_your_phone` |
| **Notification Title** | The title of the alert message. You can customize this if you like. | `💦 Leak Detected!` (Default) |

---

## 📝 Usage

1.  Import the blueprint.
2.  Create a new automation based on it.
3.  Select your notification service from the dropdown list.
4.  Save the automation. That's it!

> [!IMPORTANT]
> For this automation to work, your leak sensors **must** be correctly classified in Home Assistant with `device_class: moisture`. If your sensor is just a generic "on/off" binary sensor, this automation will not see it.

> [!TIP]
> This blueprint is designed to be a "catch-all." You can still create more specific automations for certain leak sensors if you need them to trigger different actions, like shutting off a water valve.
