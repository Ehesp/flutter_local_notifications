---
title: iOS Setup
description: Flutter Local Notifications setup for iOS devices
---

## ⚙️ iOS setup

### General setup

Add the following lines to the `didFinishLaunchingWithOptions` method in the AppDelegate.m/AppDelegate.swift file of your iOS project

<Tabs
  defaultValue="objc"
  values={[
    {label: 'Objective-C', value: 'objc'},
    {label: 'Swift', value: 'swift'},
  ]}>
  <TabItem value="objc">

```objc
if (@available(iOS 10.0, *)) {
  [UNUserNotificationCenter currentNotificationCenter].delegate = (id<UNUserNotificationCenterDelegate>) self;
}
```
  
  </TabItem>
  <TabItem value="swift">

```swift
if #available(iOS 10.0, *) {
  UNUserNotificationCenter.current().delegate = self as? UNUserNotificationCenterDelegate
}
```

  </TabItem>
</Tabs>

### Handling notifications whilst the app is in the foreground

By design, iOS applications *do not* display notifications while the app is in the foreground unless configured to do so.

For iOS 10+, use the presentation options to control the behaviour for when a notification is triggered while the app is in the foreground. The default settings of the plugin will configure these such that a notification will be displayed when the app is in the foreground.

For older versions of iOS, you need to handle the callback as part of specifying the method that should be fired to the `onDidReceiveLocalNotification` argument when creating an instance `IOSInitializationSettings` object that is passed to the function for initializing the plugin. 

Here is an example:

```dart
// initialise the plugin. app_icon needs to be a added as a drawable resource to the Android head project
const AndroidInitializationSettings initializationSettingsAndroid =
    AndroidInitializationSettings('app_icon');
final IOSInitializationSettings initializationSettingsIOS =
    IOSInitializationSettings(
        onDidReceiveLocalNotification: onDidReceiveLocalNotification);
final InitializationSettings initializationSettings = InitializationSettings(
    android: initializationSettingsAndroid,
    iOS: initializationSettingsIOS);
flutterLocalNotificationsPlugin.initialize(initializationSettings,
    onSelectNotification: onSelectNotification);

...

Future onDidReceiveLocalNotification(
    int id, String title, String body, String payload) async {
  // display a dialog with the notification details, tap ok to go to another page
  showDialog(
    context: context,
    builder: (BuildContext context) => CupertinoAlertDialog(
      title: Text(title),
      content: Text(body),
      actions: [
        CupertinoDialogAction(
          isDefaultAction: true,
          child: Text('Ok'),
          onPressed: () async {
            Navigator.of(context, rootNavigator: true).pop();
            await Navigator.push(
              context,
              MaterialPageRoute(
                builder: (context) => SecondScreen(payload),
              ),
            );
          },
        )
      ],
    ),
  );
}
```