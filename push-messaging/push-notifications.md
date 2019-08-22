# Push Notifications

In order to use Push Notifications as part of a Zaius campaign, your phone apps must be set up to recieve them. Detailed instructions about getting started with Zaius Push can be found on the Getting Started pages of the iOS and Android SDK documentation. Futher instructions about installing this React Native module in your app can be found in the [Installation instructions](../installation.md).

#### Multiple Sources

There is no limitation that Zaius must be your only source of Push Notifications. If you set them up as normal, any other source may still operate correctly with your application \(e.g. if you use the same `p12` keys when setting up a service to send iOS notifications\).

What you have to be careful of, however, is that you can handle payloads of various shapes from various services without causing problems.

### Permissions

By default, the call to `Zaius.configure()` will prompt the user for Push Notification access. While this is often acceptable, sometimes you don't want to prompt the user as the first thing the app does. To change this, pass `requestPermissions: false` as an option to `Zaius.configure()`.

Please note, however, that you **must** call `Zaius.requestPermissions()` at some point in the future, or you will not be able to receive Push Notifications. Similarly, the `onRegister` callback will not be called until `Zaius.requestPermissions()` is called.

### Callbacks

The Zaius SDK allows for some custom behavior at certain critical parts of the app's and notification's lifecycle.

#### onNotification

The `onNotification` callback is called when a Push Notification gets to the phone. By default, the `onNotification` option sent to `Zaius.configure()` does nothing. You can override this by sending a function:

```text
import { Zaius, PushNotification } from '@zaiusinc/react-native-sdk'

Zaius.configure({
    ...
    onNotification: (notification: PushNotification) => {
       ...
    },
    ...
})
```

Due to how the underlying module handles Push Notification payloads, the shape of the JSON payload that is available as `notification` is unfortunately not very strict and is different between iOS and Android.

On iOS, the notification will not be displayed to the user if the app is open. It will only be shown if the app is in the background. 

On Android, the notification will be shown regardless of app status.

**Note:** This callback will always be called, even if the notification is not interacted with. The `notification` argument will reliably contain a field called `userInteraction` which will be `true` if the user has tapped on the notification and `false` otherwise. When in the foreground on iOS, the app will still receive a callback on this function, and the `userInteraction` field will be `false`.

**Note:** When the user interacts with the notification, the SDK will [automatically queue an Event](../tagging/events.md#automatic-events) that lets Zaius know that the interaction took place.

#### onRegister

The `onRegister` callback is called when Push Notifications are fully enabled on the phone's side. That is, the callback will be called with a push token that can uniquely identify the phone. Having this token does not necessarily mean the server is capable os sending a notification, but the phone is capable of receiving one.

```text
import { Zaius, PushToken } from '@zaiusinc/reaxt-native-sdk'

Zaius.configure({
    ...
    onRegister: (token: PushToken) => {
        ...
    },
    ...
})
```

**Note:** When this callback is invoked, the SDK will also [automatically queue an Event](../tagging/events.md#automatic-events) to tell Zaius about this push token, so it knows where to send Pushes.

