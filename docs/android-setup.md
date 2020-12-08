---
title: Android Setup
description: Flutter Local Notifications setup for Android devices
---

## ⚙️ Android Setup

Before proceeding, please make sure you are using the latest version of the plugin. The reason for this is that since version 3.0.1+4, the amount of setup needed has been reduced. Previously, applications needed changes done to the `AndroidManifest.xml` file and there was a bit more setup needed for release builds. If for some reason, your application still needs to use an older version of the plugin then make use of the release tags to refer back to older versions of readme.

### Custom notification icons and sounds

Notification icons should be added as a drawable resource. The example project/code shows how to set default icon for all notifications and how to specify one for each notification. It is possible to use launcher icon/mipmap and this by default is `@mipmap/ic_launcher` in the Android manifest and can be passed `AndroidInitializationSettings` constructor. However, the offical Android guidance is that you should use drawable resources. Custom notification sounds should be added as a raw resource and the sample illustrates how to play a notification with a custom sound. Refer to the following links around Android resources and notification icons.

 * [Notifications](https://developer.android.com/studio/write/image-asset-studio#notification)
 * [Providing resources](https://developer.android.com/guide/topics/resources/providing-resources)
 * [Icon design status bar](https://developer.android.com/guide/practices/ui_guidelines/icon_design_status_bar)

When specifying the large icon bitmap or big picture bitmap (associated with the big picture style), bitmaps can be either a drawable resource or file on the device. This is specified via a single property (e.g. the `largeIcon` property associated with the `AndroidNotificationDetails` class) where a value that is an instance of the `DrawableResourceAndroidBitmap` means the bitmap should be loaded from an drawable resource. If this is an instance of the `FilePathAndroidBitmap`, this indicates it should be loaded from a file referred to by a given file path.

⚠️ For Android 8.0+, sounds and vibrations are associated with notification channels and can only be configured when they are first created. Showing/scheduling a notification will create a channel with the specified id if it doesn't exist already. If another notification specifies the same channel id but tries to specify another sound or vibration pattern then nothing occurs.

### Full-screen intent notifications

If your application needs the ability to schedule full-screen intent notifications, add the following attributes to the activity you're opening. For a Flutter application that is typically only ony activity extends from `FlutterActivity`. These attributes ensure the screen turns on and shows when the device is locked.
```xml
<activity
    android:showWhenLocked="true"
    android:turnScreenOn="true">
```

For reference, the example app's `AndroidManifest.xml` file can be found [here](https://github.com/MaikuB/flutter_local_notifications/blob/master/flutter_local_notifications/example/android/app/src/main/AndroidManifest.xml).

Note that when a full-screen intent notification actually occurs (as opposed to a heads-up notification that the system may decide should occur), the plugin will act as though the user has tapped on a notification so handle those the same way (e.g. `onSelectNotification` callback) to display the appropriate page for your application.

### Release build configuration

Before creating the release build of your app (which is the default setting when building an APK or app bundle) you will need to customise your ProGuard configuration file as per this [link](https://developer.android.com/studio/build/shrink-code#keep-code). Rules specific to the GSON dependency being used by the plugin will need to be added. These rules can be found [here](https://github.com/google/gson/blob/master/examples/android-proguard-example/proguard.cfg). The example app has a consolidated Proguard rules (`proguard-rules.pro`) file that combines these together for reference [here](https://github.com/MaikuB/flutter_local_notifications/blob/master/flutter_local_notifications/example/android/app/proguard-rules.pro).

⚠️ Ensure that you have configured the resources that should be kept so that resources like your notification icons aren't discarded by the R8 compiler by following the instructions [here](https://developer.android.com/studio/build/shrink-code#keep-resources). If you fail to do this, notifications might be broken. In the worst case they will never show, instead silently failing when the system looks for a resource that has been removed. If they do still show, you might not see the icon you specified. The configuration used by the example app can be found [here](https://github.com/MaikuB/flutter_local_notifications/blob/master/flutter_local_notifications/example/android/app/src/main/res/raw/keep.xml) where it is specifying that all drawable resources should be kept, as well as the file used to play a custom notification sound (sound file is located [here](https://github.com/MaikuB/flutter_local_notifications/blob/master/flutter_local_notifications/example/android/app/src/main/res/raw/slow_spring_board.mp3)).
