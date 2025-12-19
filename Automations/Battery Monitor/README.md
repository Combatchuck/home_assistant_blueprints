# 🔋 Low Battery Alert Manager

![Type](https://img.shields.io/badge/Type-Automation%20Blueprint-blue?style=flat-square)
![Category](https://img.shields.io/badge/Category-Notification-orange?style=flat-square)
![Feature](https://img.shields.io/badge/Feature-Dynamic%20Detection-success?style=flat-square)

Tired of your smart devices dying without warning? This blueprint is a centralized and automated battery watchdog for your entire Home Assistant setup. It periodically scans all your battery-powered devices and sends a single, convenient notification listing every device that needs attention.

## 🔄 How it Works

This automation runs on a schedule to prevent spamming you with notifications.

1.  **⏰ Scan on a Schedule:** At a frequency you define (e.g., every 4 hours), the automation wakes up and scans all `sensor` entities.
2.  **🔎 Filter for Low Batteries:** It identifies all sensors with `device_class: battery` and checks if their power level (which must be a number) has fallen below your specified threshold.
3.  **📢 Consolidate & Alert:** If one or more devices are running low, the blueprint gathers them into a single list and sends it to your phone, so you know exactly what needs charging or new batteries.

---

## ⚙️ Configuration

Set your preferences once when creating the automation.

| Input | Description | Default |
| :--- | :--- | :--- |
| **Notification Service** | The service to call for sending the alert. | `notify.mobile_app_your_phone` |
| **Low Battery Threshold**| The percentage (%) below which a device is considered to have a low battery. | `20%` |
| **Check Interval (Hours)** | How often (in hours) the automation should run to check on your devices. | `4 hours` |
| **Notification Title** | The title of the alert message. You can customize this if you like. | `🔋 Low Battery Alert` |

---

## 📝 Usage

1.  Import the blueprint.
2.  Create a new automation based on it.
3.  Choose your notification service and adjust the threshold or check-in interval if needed.
4.  Save the automation, and you're done!

> [!IMPORTANT]
> For this automation to work correctly, your battery-powered devices **must**:
> 1. Be correctly classified in Home Assistant with `device_class: battery`.
> 2. Report their battery level as a numeric percentage (e.g., `25`), not as text like "Low" or "Good".
