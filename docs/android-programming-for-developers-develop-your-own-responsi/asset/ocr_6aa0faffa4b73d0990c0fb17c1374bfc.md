between -1 and 1; thus, whenever the distance is larger than mMaxDistance, we do not consider it.

Finally, time to connect this new controller to the GameEngine. It is quite simple.
We just have to update the layout for fragment_game.xml, including
view_vjoystick.xml instead of view_keypad.xml, and then update the
initialization of the GameEngine:

```
<FrameLayout
xmlns:android="http://schemas.android.com/apk/res/android"
xmlns:tools="http://schemas.android.com/tools"
android:layout_width="match_parent"
android:layout_height="match_parent"
android:orientation="vertical"
android:paddingTop="@dimen/activity_vertical_margin"
android:paddingLeft="@dimen/activity_horizontal_margin"
android:paddingRight="@dimen/activity_horizontal_margin"
tools:context="com.plattysoft.yass.counter.GameFragment">

<TextView
android:layout_gravity="top|left"
android:id="@+id/txt_score"
android:layout_width="wrap_content"
android:layout_height="wrap_content"
android:text="@string/hello_world" >

<Button
android:layout_gravity="top|right"
android:id="@+idbtn_play_pause"
android:layout_width="wrap_content"
android:layout_height="wrap_content"
android:text="@string/pause" />

<include layout="@layout/view_vjoystick" />
</FrameLayout>
```

As a reminder, the initialization of the GameEngine is done inside
onViewCreated of the GameFragment. We only need to create an instance of the
proper InputController:

```
mGameEngine = new GameEngine(getActivity());
mGameEngine.setInputController(new
VirtualJoystickInputController(get View()));
mGameEngine.addGameObject(new Player(getView()));
```