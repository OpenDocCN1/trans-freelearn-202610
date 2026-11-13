```
android:fromYDelta="-100%"
android:toYDelta="0%"
</set>
```

Right-click on the anim directory and navigate to New | **Animation resource file**. In the **File name** field, type flash and then click on **OK**. Delete all the contents and add this code to create the animation:

```
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android">
    <alpha android:fromAlpha="0.0"
        android:toAlpha="1.0"
        android:interpolator="@android:anim/accelerate_interpolator"
        android:repeatMode="reverse"
        android:repeatCount="10">
</set>
```

Right-click on the anim directory and navigate to New | **Animation resource file**. In the **File name** field, type bounce and then click on **OK**. Delete all the contents and add this code to create the animation:

```
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android"
    android:fillAfter="true"
    android:interpolator="@android:anim/bounce_interpolator">
<scale
    android:fromXScale="1.0"
    android:fromYScale="0.0"
    android:toXScale="1.0"
    android:toYScale="1.0" >
</set>
```

Right-click on the anim directory and navigate to New | **Animation resource file**. In the **File name** field, type rotate_left and then click on **OK**. Delete all the contents and add this code to create the animation. Here, we see something new, pivotX="50%" and pivotY="50%". This makes the rotating animation central on the widget that will be animated. We can think of this as setting the pivot point of the animation:

```
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android">
    <rotate android:fromDegrees="360"
```