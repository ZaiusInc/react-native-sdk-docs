# Install & Initialize

## NPM Module

This page will take you through the steps to set up the Zaius SDK module in your React Native application.

The Zaius SDK module was written using TypeScript and so is fully compatible with both JavaScript and TypeScript applications. If you are developing a new React Native app and wish to integrate with Zaius, it's recommended you use TypeScript.react-native init Project --template typescript

```bash
npm install --save @zaiusinc/react-native-sdk @react-native-community/async-storage react-native-push-notification
```

### iOS

For an iOS project you'll also need to run the following:

```bash
npm install --save @react-native-community/push-notification-ios
cd <project>/ios && pod install
```

```bash
react-native link
```

After this, you will need to allow your app's Bundle ID to have the Push Notifications capability \(in Xcode, click on Capabilities and scroll down to Push Notifications\).

### Android

Inside `<project>/android/app/src/main/AndroidManifest.xml`, integrate the following XML to allow proper permissions to allow the app to manage Push Notifications.

```markup
<receiver
     android:name="com.google.android.gms.gcm.GcmReceiver"
     android:exported="true"
     android:permission="com.google.android.c2dm.permission.SEND" >
     <intent-filter>
         <action android:name="com.google.android.c2dm.intent.RECEIVE" />
         <category android:name="${applicationId}" />
     </intent-filter>
 </receiver>
 <!-- < Only if you're using GCM or localNotificationSchedule() > -->

 <receiver android:name="com.dieam.reactnativepushnotification.modules.RNPushNotificationPublisher" />
 <receiver android:name="com.dieam.reactnativepushnotification.modules.RNPushNotificationBootEventReceiver">
     <intent-filter>
         <action android:name="android.intent.action.BOOT_COMPLETED" />
     </intent-filter>
 </receiver>
 <service android:name="com.dieam.reactnativepushnotification.modules.RNPushNotificationRegistrationService"/>

 <!-- < Only if you're using GCM or localNotificationSchedule() > -->
 <service
     android:name="com.dieam.reactnativepushnotification.modules.RNPushNotificationListenerServiceGcm"
     android:exported="false" >
     <intent-filter>
         <action android:name="com.google.android.c2dm.intent.RECEIVE" />
     </intent-filter>
 </service>
 <!-- </ Only if you're using GCM or localNotificationSchedule() > -->

 <!-- < Else > -->
 <service
     android:name="com.dieam.reactnativepushnotification.modules.RNPushNotificationListenerService"
     android:exported="false" >
     <intent-filter>
         <action android:name="com.google.firebase.MESSAGING_EVENT" />
     </intent-filter>
 </service>
 <!-- </Else> -->
```

You will also need to install your `google-services.json` according to Google's instructions. From the `google-services.json` file, you will need the `project_number` field, which you will use as the "Sender ID".

## Initialize

In order to use the Zaius SDK, it must first be configured. You will need two required and one optional piece of information: First, you will need the Short Identifier from the Mobile Apps integration. This will be your App ID. Next, the API Key can be obtained from the "APIs" section in Zaius. Your API Key will be in the "key" field under "Public". Lastly, you will optionally need the Sender ID as mentioned above. This isn't required for iOS apps, but is required for Android apps.

With this information, you can use the main entry point for the SDK, `Zaius.configure()`

```typescript
Zaius.configure({
    appId: string,
    apiKey: string,
    appVersion?: string = '',
    baseUrl?: string = 'https://api.zaius.com/v3',
    onNotification?: (notification: PushNotification) => void = () => undefined,
    onRegister?: (push_token: PushToken) => void = () => undefined,
    platform?: 'ios' | 'ios-sandbox' | 'android' | 'tvos' | 'tvos_sandbox' | 'unknown' // = [calculated from Platform.OS]
    requestPermissions?: boolean = true,
    senderID?: string = '',
    startQueue?: boolean = true,
}) : Promise<Zaius>
```

The arguments of `configure` are as follows:

* `appId`: **\[Required\]** The App ID as mentioned above, the Shortened Name of your project.
* `apiKey`: **\[Required\]** The API Key from the Public tab in the APIs section of the Zaius app.
* `appVersion`: The version of your app, sent with each request to help you keep track of which customers are using which version.
* `baseUrl`: Where the Zaius API is located. This should not normally by changed.
* `onNotification`: A callback function that is called when a Push Notification is recieved by the app \(Android\) or tapped by the user \(both Android and iOS\). See [Push Notifications](push-messaging/push-notifications.md) for more information.
* `onRegister`: A callback function that is called when permission for Push Notifications has been granted. Once this callback is called, everything is set up on the phone side for Pushes to work.
* `platform`: Which platform the app is running on. Will be detected if it's not supplied.
* `requestPermissions`: If this is `false`, then it is up to the app author to call `Zaius.requestPermission()` in order for Push Notifications to be initialized. See [Push Notifications](push-messaging/push-notifications.md) for more.
* `senderID`: **\[Required for Android\]** Required for Push Notifications on Android. The Sender ID is found in the `google-services.json` file you can obtain this file from Firebase by following [Google directions](https://support.google.com/firebase/answer/7015592?hl=en#android). The `Sender ID` is the value found at `project_info` -&gt; `project_number`.
* `startQueue`: When set to true, the SDK will start processing the Event queue as soon as it can. See [Events](tagging/events.md) for more information about the Event queue. 

 The return type of `Zaius.configure` is a `Promise` and so it must be handled as a `Promise` or called from an `async` function.

Once you have the `Zaius` object from `configure()` the following functions are available on both the object returned from `Zaius.configure()` and as functions directly on the global `Zaius` object \(e.g. `Zaius.customer()`\).

* `Zaius.getConfig()`: Returns the configuration that the `Zaius` object is using.
* `Zaius.getVUID()`: Return the `VUID` that identifies the current user. See [VUIDs](tagging/customers/#vuids) for more info.
* `Zaius.anonymize()`: Generates a new VUID, effectively anonymizing the user.
* `Zaius.event()`: Queues an event into the Event queue. See [Events](tagging/events.md) for more info.
* `Zaius.customer()`: Updates information about this Customer with Zaius. See [Customers](tagging/customers/#updating-customer-information) for more info.
* `Zaius.identify()`: Updates Customer identification information. See [Customers](tagging/customers/#updating-customer-identifiers) for more info.

