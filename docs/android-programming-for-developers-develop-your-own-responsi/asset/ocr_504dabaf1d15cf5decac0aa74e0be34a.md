```
Update the application module build < PROJECT_DIRECTORY
>/app/build.gradle:

apply plugin: 'com.android.application'

android {
    compileSdkVersion 23
    buildToolsVersion "21.1.1"
    defaultConfig {
        applicationId "com.packpublishing.asynchronousandroid"
        minSdkVersion 9
        targetSdkVersion 23
    }
}

dependencies {
    ...
    compile 'com.google.android.gms:play-services-gcm:8.3.0'
}
apply plugin: 'com.google.gms.google-services'
```

To use GCM on your Android application you need to have a device with Android API 8 or higher with the Google Play Store installed, or a device with API level 9 if you want to use the new GCM features delivered by the Google Play Services.

With the Google Services library dependencies declared on our build files, we can start to bootstrap the GCM infrastructure on our application.

To use GCM in your application, you have to register for the following permissions on your application AndroidManifest.xml file:

```
<uses-permission android:name="android.permission.INTERNET" >
<!--- Required to wakeup the device and deliver messages --->
<uses-permission android:name="android.permission.WAKE_LOCK" >

<permission android:name="<Package>.permission.C2D_MESSAGE"
    android:protectionLevel="signature">
<uses-permission android:name="<Package>.permission.C2D_MESSAGE">
...
</manifest>
```

# Note

Notice that you should replace <Package> with your unique application package,