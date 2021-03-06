---
layout: page
title: "APNS"
description: "Instructions how to add APNS notifications to Home Assistant."
date: 2016-09-05 23:00
sidebar: true
comments: false
sharing: true
footer: true
ha_category: Notifications
---


The `APNS` platform uses the Apple Push Notification service (APNS) to deliver notifications from Home Assistant.

To use the APNS service you will need an apple developer account
and you will need to create an App to receive push notifications.
For more information see the apple developer documentation.

```yaml
# Example configuration.yaml entry
notify:
  name: NOTIFIER_NAME
  platform: apns
  sandbox: true or false
  cert_file: cert_file.pem
```

Configuration variables:

- **name** (*Required*): The name of the notifier. The notifier will bind to the service `notify.NOTIFIER_NAME`.
- **sandbox** (*Optional*): If true notifications will be sent to the sandbox (test) notification service. Default false.
- **cert_file** (*Required*): The certificate to use to authenticate with the APNS service.

 The APNS platform will register two services, `notify.NOTIFIER_NAME` and `apns.NOTIFIER_NAME`.

### apns.NOTIFIER_NAME

This service will register device id's with home assistant. In order to receive a notification a device must be registered. The app on the device can use this service to send an id to Home Assistant during startup, the id will be stored in `[NOTIFIER_NAME]_apns.yaml`.

See didRegisterForRemoteNotificationsWithDeviceToken in the [Apple developer documentation](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIApplicationDelegate_Protocol/#//apple_ref/occ/intfm/UIApplicationDelegate/application:didRegisterForRemoteNotificationsWithDeviceToken:) for more information about how to obtain a device id.

### notify.NOTIFIER_NAME

This service will send messages to a registered device. The following parameters can be used:

- **message**: The message to send

- **target**: The desired state of the device, only devices that match the state will receive messages. To enable state tracking a registered device must have a `tracking_device_id` attribute added to the `[NOTIFIER_NAME]_apns.yaml` file. If this id matches a device in known_devices.yaml the device state will be tracked.

- **data**:
	* **badge**: The number to display as the badge of the app ic
	* **sound**: The name of a sound file in the app bundle or in the Library/Sounds folder.
	* **category**: Provide this key with a string value that represents the identifier property of the UIMutableUserNotificationCategory
	* **content_available**: Provide this key with a value of 1 to indicate that new content is available.
