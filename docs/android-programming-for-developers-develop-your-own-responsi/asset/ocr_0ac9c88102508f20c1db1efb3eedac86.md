# Declaring Leanback support

To allow Google Play to list our game when searching from an Android TV, we need to specifically declare that we support the Leanback UI. To do that, we have to declare that we use the Leanback feature:

```
<uses-feature android:name="android.software.leanback"
    android:required="false" />
```

Note that we declare that we use it but we mark it as not required. This is important because otherwise the game will not appear for devices that do not have the Leanback feature. If we were to have two separate APKs, we should mark this feature as required on the TV build variant only.

Declaring a uses-feature means that your app makes use of it. Marking it as not required means that it can do without it, but it will make use of it if present.

# Note

Declaring a uses-feature means that your app makes use of it. Marking it as not required means that it can do without it.

If you are using separate APKs for mobile and TV, you should declare the uses-feature for Leanback only on the TV build variant and do nothing on the mobile one. By doing that, the TV APK will appear only on devices with Leanback UI and the mobile APK will appear only on devices without Leanback support.

The default state for the Leanback feature is to not be required.