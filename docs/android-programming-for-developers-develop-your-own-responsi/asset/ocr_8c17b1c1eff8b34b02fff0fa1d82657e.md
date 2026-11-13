+   * GPS: android.hardware.location.gps
+   * Microphone: android.hardware.microphone
+   * Sensors: android.hardware.sensor

For games only, the touchscreen and sensor aspect may be worrying, but there are games that use location and/or the camera as an integral part of the game. This may be a deal-breaker when it comes to bringing the game to the TV.

# Note

If your game relies on GPS or the Camera, it's probably not a good fit for a TV.

In addition to this, some uses-permission declarations imply a requirement for hardware features. The permissions that implicitly require features are:

+   * RECORD_AUDIO requires the use of android.hardware.microphone.
+   * CAMERA requires:
+     * android.hardware.camera
+     * android.hardware.camera.autofocus
+   * ACCESS_COARSE_LOCATION requires:
+     * android.hardware.location
+     * android.hardware.location.network
+   * ACCESS_FINE_LOCATION requires:
+     * android.hardware.location
+     * android.hardware.location.gps

If you use any of these permissions, you should ensure to mark the appropriate feature as not required and act accordingly when not present.

To detect if a feature is present or not to enable or disable some parts of the game on runtime, you can use the hasSystemFeature method of PackageManager. Like this:

```
getPackageManager().hasSystemFeature("android.hardware.touchscreen"
)
```

Note that getPackageManager is available at the Context level, so you can access it from the Activity.