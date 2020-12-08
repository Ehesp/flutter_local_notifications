---
title: Overview
description: A cross platform plugin for displaying local notifications.
---

A cross platform plugin for displaying local notifications.

## 📱 Supported platforms

- **Android 4.1+**. Uses the [NotificationCompat APIs](https://developer.android.com/reference/androidx/core/app/NotificationCompat) so it can be run older Android devices
- **iOS 8.0+**. On iOS versions older than 10, the plugin will use the UILocalNotification APIs. The [UserNotification APIs](https://developer.apple.com/documentation/usernotifications) (aka the User Notifications Framework) is used on iOS 10 or newer.
- **macOS 10.11+**. On macOS versions older than 10.14, the plugin will use the [NSUserNotification APIs](https://developer.apple.com/documentation/foundation/nsusernotification). The [UserNotification APIs](https://developer.apple.com/documentation/usernotifications) (aka the User Notifications Framework) is used on macOS 10.14 or newer.

## ✨ Features

- Mockable (plugin and API methods aren't static)
- Display basic notifications
- Scheduling when notifications should appear
- Periodically show a notification (interval based)
- Schedule a notification to be shown daily at a specified time
- Schedule a notification to be shown weekly on a specified day and time
- Retrieve a list of pending notification requests that have been scheduled to be shown in the future
- Cancelling/removing notification by id or all of them
- Specify a custom notification sound
- Ability to handle when a user has tapped on a notification, when the app is the foreground, background or terminated
- Determine if an app was launched due to tapping on a notification
- [Android] Configuring the importance level
- [Android] Configuring the priority
- [Android] Customising the vibration pattern for notifications
- [Android] Configure the default icon for all notifications
- [Android] Configure the icon for each notification (overrides the default when specified)
- [Android] Configure the large icon for each notification. The icon can be a drawable or a file on the device
- [Android] Formatting notification content via ([HTML markup](https://developer.android.com/guide/topics/resources/string-resource.html#StylingWithHTML))
- [Android] Support for the following notification styles
  - Big picture
  - Big text
  - Inbox
  - Messaging
  - Media
    - While media playback control using a `MediaSession.Token` is not supported, with this style you let Android treat the `largeIcon` bitmap as album artwork
- [Android] Group notifications
- [Android] Show progress notifications
- [Android] Configure notification visibility on the lockscreen
- [Android] Ability to create and delete notification channels
- [Android] Retrieve the list of active notifications
- [Android] Full-screen intent notifications
- [iOS (all supported versions) & macOS 10.14+] Request notification permissions and customise the permissions being requested around displaying notifications
- [iOS 10 or newer and macOS 10.14 or newer] Display notifications with attachments

## ⚠ Caveats and limitations

The cross-platform facing API exposed by the `FlutterLocalNotificationsPlugin` class doesn't expose platform-specific methods as its goal is to provide an abstraction for all platforms. As such, platform-specific configuration is passed in as data. There are platform-specific implementations of the plugin that can be obtained by calling the [`resolvePlatformSpecificImplementation`](https://pub.dev/documentation/flutter_local_notifications/latest/flutter_local_notifications/FlutterLocalNotificationsPlugin/resolvePlatformSpecificImplementation.html). An example of using this is provided in the section on requesting permissions on iOS. In spite of this, there may still be gaps that don't cover your use case and don't make sense to add as they don't fit with the plugin's architecture or goals. Developers can fork or maintain their own code for showing notifications in these situations.

### Compatibility with firebase_messaging

Previously, there were issues that prevented this plugin working properly with the `firebase_messaging` plugin. This meant that callbacks from each plugin might not be invoked. This has been resolved since version 6.0.13 of the `firebase_messaging` plugin so please make sure you are using more recent versions of the  `firebase_messaging` plugin and follow the steps covered in `firebase_messaging`'s readme file located [here](https://pub.dev/packages/firebase_messaging)

### Scheduled Android notifications

Some Android OEMs have their own customised Android OS that can prevent applications from running in the background. Consequently, scheduled notifications may not work when the application is in the background on certain devices (e.g. by Xiaomi, Huawei). If you experience problems like this then this would be the reason why. As it's a restriction imposed by the OS, this is not something that can be resolved by the plugin. Some devices may have setting that lets users control which applications run in the background. The steps for these can be vary and but is still up to the users of your application to do given it's a setting on the phone itself.

It has been reported that Samsung's implementation of Android has imposed a maximum of 500 alarms that can be scheduled via the [Alarm Manager](https://developer.android.com/reference/android/app/AlarmManager) API and exceptions can occur when going over the limit.

### iOS pending notifications limit

There is a limit imposed by iOS where it will only keep 64 notifications that will fire the soonest.

### Scheduled notifications and daylight savings
The notification APIs used on iOS versions older than 10 (aka the `UILocalNotification` APIs) have limited supported for time zones.

### Updating application badge

This plugin doesn't provide APIs for directly setting the badge count for your application. If you need this for your application, there are other plugins available, such as the [`flutter_app_badger`](https://pub.dev/packages/flutter_app_badger) plugin.

### Custom notification sounds

[iOS and macOS restrictions](https://developer.apple.com/documentation/usernotifications/unnotificationsound?language=objc) apply (e.g. supported file formats).

### macOS differences

Due to limitations currently within the macOS Flutter engine, `getNotificationAppLaunchDetails` will return null on macOS versions older than 10.14. These limitations will mean that conflicts may occur when using this plugin with other notification plugins (e.g. for push notifications).

The `schedule`, `showDailyAtTime` and `showWeeklyAtDayAndTime` methods that were implemented before macOS support was added and have been marked as deprecated aren't implemented on macOS.

## 📷 Screenshots

| Platform | Screenshot |
| ------------- | ------------- |
| Android | <img height="480" src="https://github.com/MaikuB/flutter_local_notifications/raw/master/images/android_notification.png"> |
| iOS | <img height="414" src="https://github.com/MaikuB/flutter_local_notifications/raw/master/images/ios_notification.png"> |
| macOS | <img src="https://github.com/MaikuB/flutter_local_notifications/raw/master/images/macos_notification.png"> |

## 👏 Acknowledgements

* [Javier Lecuona](https://github.com/javiercbk) for submitting the PR that added the ability to have notifications shown daily
* [Jeff Scaturro](https://github.com/JeffScaturro) for submitting the PR to fix the iOS issue around showing daily and weekly notifications and migrating the plugin to AndroidX
* [Ian Cavanaugh](https://github.com/icavanaugh95) for helping create a sample to reproduce the problem reported in [issue #88](https://github.com/MaikuB/flutter_local_notifications/issues/88)
* [Zhang Jing](https://github.com/byrdkm17) for adding 'ticker' support for Android notifications
* ...and everyone else for their contributions. They are greatly appreciated