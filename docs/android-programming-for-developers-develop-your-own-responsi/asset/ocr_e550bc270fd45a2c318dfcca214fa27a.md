```
```xml
<shape xmlns:android="http://schemas.android.com/apk/res/android"
android:shape="rectangle">
<solid
android:color="@android:color/transparent" >
<padding android:bottom="@dimen/game_frame_width"
android:left="@dimen/game_frame_width"
android:right="@dimen/game_frame_width"
android:top="@dimen/game_frame_width"/>
<stroke android:color="@color/game_view_frame"
android:width="@dimen/game_frame_width" >
<shape>
```
```

On the left we have a RelativeLayout, which we will use to place the controls for the score and lives. In front of this we have the virtual joystick we were already using, and that covers the entire screen. Finally, we have the pause button, which has to remain in the foreground, otherwise the virtual joystick will capture the touch event. As we have already mentioned, the pause button is now using the same look and feel as the music and sound buttons on the main menu.

![](bbox=[0.11, 0.44, 0.89, 0.75])

Now that we have the space, let's start adding items there! We are going to just display scores and lives, but you could use the space for anything relevant to the game, from top scores to In-App Purchases buttons.