```
<alpha
    android:fromAlpha="0.0"
    android:interpolator="@android:anim/accelerate_interpolator"
    android:toAlpha="1.0" >

<alpha
    android:fromAlpha="1.0"
    android:interpolator="@android:anim/accelerate_interpolator"
    android:toAlpha="0.0" >

<set>
```

Right-click on the anim directory and navigate to **New | Animation resource file**. In the **File name** field, type left_right and then click on **OK**. Delete all the contents and add this code to create the animation:

```
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android"
    <translate
        android:fromXDelta="-500%"
        android:toXDelta="0%"/>
</set>
```

Right-click on the anim directory and navigate to **New | Animation resource file**. In the **File name** field, type right_left and then click on **OK**. Delete all the contents and add this code to create the animation:

```
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android"
    <translate
        android:fillAfter="false"
        android:fromXDelta="500%"
        android:toXDelta="0%"/>
</set>
```

Right-click on the anim directory and navigate to **New | Animation resource file**. In the **File name** field, type top_bot and then click on **OK**. Delete all the contents and add this code to create the animation:

```
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android"
    <translate
        android:fillAfter="false"
```