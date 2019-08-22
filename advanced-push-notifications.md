# Advanced Push Notifications

Push Notifications are capable of more behaviors than simply notifying the user in a standard way. You can specify custom sounds and custom icons for your notifications to use when displaying to the user.

### Custom Notification Sounds

You will need to create a sound for your notification and save it in MP3 format. Remember not to make the sound too long. Then follow the instructions for your platform:

#### iOS

1. Add the mp3 of the sound to the Xcode project at the root level.
2. Modify the `Custom Sound` attribute in the "iOS Settings" of the Zaius

   Campaign using the full name of the sound \(e.g. `bell.mp3`\).

#### Android

1. Add the mp3 of the sound to the Android project at

   `<root>/android/app/src/main/res/raw/bell.mp3`

2. Under the Android Options, add a key/value pair with the key of `soundName`

   and the value of the filename \(e.g. `bell.mp3`\)

### Custom Icons

On Android, you can specify custom Icons in a manner similar to specifying sounds. Place appropriately sized images into the correct folders under `<project>/android/app/src/main/res`

| Directory | Dimensions |
| :--- | :--- |
| mipmap-mdpi | 48x48 |
| mipmap-hdpi | 72x72 |
| mipmap-xhdpi | 96x96 |
| mipmap-xxhdpi | 144x144 |
| mipmap-xxxhdpi | 192x192 |

Then give the Zaius campaign a new field named `largeIcon` with the name of the image \(but without the extension, e.g. `icon` instead of `icon.png`\).

