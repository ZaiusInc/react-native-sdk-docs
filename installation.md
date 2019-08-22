# Installation

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

