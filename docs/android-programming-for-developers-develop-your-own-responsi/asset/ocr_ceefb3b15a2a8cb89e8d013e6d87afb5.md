# # Declaring a TV Activity

Android TV uses the Leanback interface, which is designed for big screens controlled by a remote. This UI displays apps and games in a different way from phones. The launcher is also different. Because of this, apps and games need to declare an Activity with a particular intent filter so the Leanback launcher can find them and make them available to the user.

This intent filter will declare the app to be available for launch on a Leanback interface.

The easiest way to proceed is to create a new Activity that extends from our normal activity and then override some methods if necessary to adapt to the TV interface. For now, we will create YassTvActivity that just extends YassActivity:

```
```java
public class YassTvActivity extends YassActivity {
}
```
```

We will then declare it on AndroidManifest.xml with the proper intent filter:

```
```xml
<activity
    android:name=".YassTvActivity"
    android:label="@string/app_name"
    >
    <intent-filter>
        <action android:name="android.intent.action.MAIN" >
        <category
        android:name="android.intent.category.LEANBACK_LAUNCHER" >
    </intent-filter>
</activity>
```
```

As you can see, the intent filter is almost the same as the one for the normal launch activity except that the category has a different name (LEANBACK_LAUNCHER instead of LAUNCHER).

Note that on Android you cannot declare the same activity twice on the manifest, but you can declare multiple intent filters for the same activity. You can use the same activity for both mobile and TV, like this:

```
```xml
<activity
```
```