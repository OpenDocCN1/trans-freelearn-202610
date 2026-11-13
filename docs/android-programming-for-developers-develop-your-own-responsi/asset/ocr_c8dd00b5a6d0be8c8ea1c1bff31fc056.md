# # Defining the animations in XML

First, let's define two animations in XML that we can use. A FADE IN animation, which is the normal behavior when a note appears in the list and a flash, which will be the behavior that can be tweaked from the settings screen. If the note is important, the user will be able to set it to either flash fast, flash slow, or not flash at all.

Right-click on the res folder and navigate to New | Android resource directory. Enter anim in the Directory name field and click on OK.

Now, right-click on the new anim directory and navigate to New | Animation resource file. In the File name field, type fade_in and then click on OK. Delete all the contents and add this code to create the animation:

```
<set xmlns:android="http://schemas.android.com/apk/res/android">
<alpha
android:fromAlpha="0.0"
android:toAlpha="1.0"
android:duration="500"
android:interpolator="@android:anim/accelerate_interpolator">
</alpha>
</set>
```

Now, right-click on the new anim directory and navigate to New | Animation resource file. In the File name field, type flash and then click on OK. Delete all the contents and add this code to create the animation:

```
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android">
<alpha android:fromAlpha="0.0"
android:toAlpha="1.0"
android:interpolator="@android:anim/accelerate_interpolator"
android:repeatMode="reverse"
android:repeatCount="infinite">
</set>
```

Our XML animations are ready, so let's add the Java code.