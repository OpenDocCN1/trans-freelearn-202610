Consuming the event and handling it should be done with it.

In any other case, we return false (again, not consuming the event) to let the activity handle the key event normally.

Finally, in the case of the pause and game over dialogs that are shown in the GameFragment, we do not need to do anything because the only item that is focusable on that UI is the pause button.

What we will do is set the pause button to not focusable and then let the system handle the keys:

```
<ImageView
 style="@style/iconButton"
 android:focusable="false"
 android:layout_gravity="top|right"
 android:id="@+id/btn_play_pause"
 android:layout_marginTop="@dimen/menu_margin"
 android:layout_marginRight="@dimen/menu_margin"
 android:src="@drawable/pause" />
```

Note that, while ImageView is not focusable by default, we have set that property at the style level, and we can always overwrite the value of the style in the definition of the layout.

With all this changes all the UI of our game is usable with a gamepad and we are finally ready to publish on Android TV.