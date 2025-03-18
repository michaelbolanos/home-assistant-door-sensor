# Home Assistant Automation: Door Sensor Alarm Control

## Overview
This automation manages an alarm system based on the status of the rear door sensor and a bypass button. It ensures that the alarm triggers when the rear door is opened unless the bypass mode is active.

## Automation Details
- **Alias:** 11 - Door Sensor
- **Description:** Controls alarm based on rear door status and bypass button.

## Triggers
1. **Button Pressed**
   - Triggered when the button on a ZHA remote is short-pressed.
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
- If the alarm bypass is **off**, opening the rear door will turn **on** the alarm switch.

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

### Home Assistant YAML Configuration
This automation is configured in Home Assistant's YAML file under Automations and can be managed via the UI.
The YAML is usually edited in code view, but the UI can be used to check and review.

