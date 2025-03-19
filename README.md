# Home Assistant Automation: Door Sensor Alarm Control

## Overview
This automation manages an alarm system based on the status of the rear door sensor and a bypass button. It ensures that the alarm triggers when the rear door is opened unless the bypass mode is active.

This system was created to **monitor individuals who may be at risk of unknowingly exiting a door**. It provides an additional layer of security without modifying an existing home or commercial alarm system. This alarm can be used **at any time** for a **specific door**, avoiding the need to tamper with an overall security system.

## Hardware Components Used
- **Shelly Button 1** – Used to toggle the alarm bypass mode.
- **Shelly 1 Plus Relay (Status Light)** – Controls the status indicator.
- **Shelly 1 Plus Relay (12V Siren)** – Controls the siren activation.
- **12V DC Power Adapter** – Powers the combined siren and flashing red light.
- **Aqara Door Sensor** – Detects when the rear door is opened or closed.
- **SMLIGHT SLZB-06M Zigbee Coordinator** – Manages Zigbee-based devices, including the Aqara door sensor.
- **UniFi Cameras** – Integrated with Home Assistant to provide real-time monitoring of the rear door.

## Home Automation Setup & Raspberry Pi Performance

- **Performance Considerations:** The Raspberry Pi 4 has more memory and processing power than previous models, making it well-suited for running Home Assistant, HACS (Home Assistant Community Store), and multiple integrations efficiently.
- **Device Networking:** All applicable devices have static IP addresses.
- **Backup Considerations:** Backups are crucial due to the risk of SD card corruption. A high-quality SanDisk SD card is used.
- **Heat Management:** The Raspberry Pi 4 tends to get hot, which may require additional cooling solutions.
- **Display Connectivity:** Raspberry Pi 4 requires a **Micro HDMI to HDMI cable** to connect to an external display. If using a **VGA monitor**, an additional **HDMI-to-VGA adapter** is needed.
- **Home Assistant Core:** Manages all automations and integrations.
- **Integrations Used:**
  - **Shelly Integration** – Manages Shelly Button 1 and relays.
  - **ZHA (Zigbee Home Automation)** – Handles the Aqara door sensor via the SMLIGHT SLZB-06M Zigbee Coordinator.
  - **UniFi Integration** – Provides real-time camera feeds, including monitoring of the rear door and status light.

## Dashboard & Battery Monitoring
- A **custom Lovelace dashboard** is set up in Home Assistant to:
  - Display the status of the **rear door sensor**.
  - Show the **status light** in real time.
  - Stream the **UniFi camera feed** for immediate visual confirmation of door activity.

## Automation Details

### Battery Monitoring & Replacement Guidelines
- The Aqara Door Sensor uses a battery, and its battery level is monitored in Home Assistant.
- The battery status has been added to the Lovelace dashboard to ensure timely replacement and prevent unexpected failures.
- Batteries should be replaced when they reach **60% or below** to maintain optimal performance and prevent device disconnections.
- **Alias:** 11 - Door Sensor
- **Description:** Controls alarm based on rear door status and bypass button.

## Triggers
1. **Button Pressed**
   - Triggered when the Shelly Button 1 is pressed.
2. **Door Opened**
   - Triggered when `binary_sensor.rear_door_sensor_opening` is `on` for 2 seconds.
3. **Door Closed**
   - Triggered when `binary_sensor.rear_door_sensor_opening` is `off` for 2 seconds.

## Actions
### Button Press Handling
- If the alarm bypass is **off**, pressing the button will:
  - Turn **on** the `input_boolean.alarm_bypass`.
  - Turn **on** the alarm switch.
- If the alarm bypass is **on**, pressing the button will:
  - Turn **off** the `input_boolean.alarm_bypass`.
  - Turn **off** the alarm switch.

### Door Open Handling
- If the alarm bypass is **off**, opening the rear door will turn **on** the alarm switch, activating the siren and flashing red light.

### Door Close Handling
- Closing the rear door will:
  - Turn **off** the alarm switch.
  - Ensure the alarm system is fully deactivated.

## Mode
- The automation runs in **restart** mode, meaning it will restart if triggered again while running.

## Variables
- `bypass_active`: Tracks whether the bypass mode is active.

## Notes
- The automation ensures the alarm only triggers when the door is opened and the bypass is inactive.
- The bypass button allows temporary disabling of the alarm system.
- The alarm system resets when the door is closed.
- The status light provides a visual indication of the alarm system state.
- This alarm is designed to work alongside an existing home or commercial security system **without requiring modifications**, providing additional protection for a specific door when needed.

## Problems & Considerations
### 1. **Sonoff Button Issues**
- The **Sonoff Zigbee button** did not work reliably and was replaced with the **Shelly 1 WiFi button**, which is also **hard-wired**.

### 2. **Zigbee Device Stability**
- **Aqara door sensor** sometimes disconnects; considering **Konnected Alarm Panel** for hard-wired integration:  
  [Konnected Alarm Panel - Wired Alarm System Conversion Kit](https://konnected.io/products/konnected-alarm-panel-wired-alarm-system-conversion-kit)

### 3. **System Reboot for Stability**
- Rebooting the system periodically enhances overall stability. Restarting Home Assistant and the Zigbee Coordinator helps refresh connections and ensures smooth operation.

## Home Assistant YAML Configuration, Customization & Community Contributions
This automation is configured in Home Assistant's YAML file under Automations and can be managed via the UI.

### Custom YAML Code & Entity ID Changes
We modified our YAML configuration to ensure flexibility and ease of maintenance. The entity IDs were carefully renamed for better readability and consistency. Some key changes include:
- Renaming `binary_sensor.rear_door_sensor_opening` to a more intuitive ID.
- Ensuring that `input_boolean.alarm_bypass` aligns with other related automations.
- Optimizing the conditions to make the system more efficient and reduce unnecessary triggers.

### Improvements and Features
- **Real-time Feedback with UniFi Cameras**: The automation integrates UniFi cameras, allowing users to visually confirm the alarm status through the Lovelace dashboard.
- **Hard-wired Shelly 1 WiFi Button**: Instead of relying on battery-powered Zigbee devices, we switched to a hard-wired Shelly button for more reliable operation.
- **Logical Reset Approach**: The automation ensures that once the door closes, the alarm resets seamlessly, preventing unnecessary manual intervention.
- **Zigbee Stability Improvement**: Best practices like rebooting the Zigbee Coordinator first before HA have been implemented to enhance system reliability.

These improvements make the automation more robust, user-friendly, and efficient.

### Community Contributions & Feedback
We welcome feedback and ideas from the Home Assistant community. If you have suggestions to improve this automation or experiences to share, please feel free to contribute. Whether it’s optimizing YAML code, enhancing stability, or integrating additional features.

