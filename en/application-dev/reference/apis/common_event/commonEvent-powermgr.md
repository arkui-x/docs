# Common Events of the Power Management Subsystem
This document lists the common system events provided by the power management subsystem to applications. Applications can use [APIs](../js-apis-commonEventManager.md) to subscribe to common system events.

## COMMON_EVENT_BATTERY_LOW
Indicates that the battery level is low.

- Value: usual.event.BATTERY_LOW
- Required subscriber permissions: none

When the battery level drops to lower than the low battery level set for the device, the event notification service is triggered to publish this event.

## COMMON_EVENT_BATTERY_OKAY
Indicates that the battery level is normal.

- Value: usual.event.BATTERY_OKAY
- Required subscriber permissions: none

When the battery level changes from the low level to normal level, the event notification service is triggered to publish this event.

## COMMON_EVENT_SCREEN_ON
Indicates that the device screen is on and the device is in the active state.

- Value: usual.event.SCREEN_ON
- Required subscriber permissions: none

When the device screen is turned on and in the active state, the event notification service is triggered to publish this event.

## COMMON_EVENT_POWER_SAVE_MODE_CHANGED
Indicates that the system power saving mode has changed.

- Value: usual.event.POWER_SAVE_MODE_CHANGED
- Required subscriber permissions: none

When the system power saving mode changes, the event notification service is triggered to publish this event.
