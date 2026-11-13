
else if (mGamepadControllerListener != null) {
    if (mGamepadControllerListener.dispatchGenericMotionEvent(event)) {
        return true;
    }
}
return super.dispatchGenericMotionEvent(event);
}

Finally, we have to update the handling of the back key:

@Override
public void onBackPressed() {
if (mCurrentDialog != null && mCurrentDialog.isShowing()) {
mCurrentDialog.dismiss();
return;
}
final YassBaseFragment fragment = (YassBaseFragment)
getFragmentManager().findFragmentByTag(TAG_FRAGMENT);
if (fragment == null || !fragment.onBackPressed()) {
super.onBackPressed();
}
}

If we are showing a dialog, we just dismiss it. Otherwise we keep the code we
had before: we ask the current fragment to handle the back key press and, if the
event was not consumed, we pass it to the parent class.

Note that passing the back key event to the current fragment is important. In our
game a back press while in the GameFragment means that we want to pause the
game.

To complete this, we have to make the dialogs handle the keys. The default
implementation on BaseCustomDialog is to just return false, meaning that the
event was not consumed:

public boolean dispatchKeyEvent(KeyEvent event) {
return false;
}
public boolean dispatchGenericMotionEvent(MotionEvent event) {
return false;
}

We are going to do a very simple handling where we will only process the OK click when one of the buttons on the dialog is selected, but you could also prevent the focus from going to views outside the dialog with a more elaborate handling.

For ControllerHelpDialog, we will just dismiss it whenever an **OK** is pressed, regardless of which view is in focus:

public boolean dispatchKeyEvent(KeyEvent event) {
    if (event.getKeyCode() == KeyEvent.KEYCODE_BUTTON_A ||
    event.getKeyCode() == KeyEvent.KEYCODE_ENTER ||
    event.getKeyCode() == KeyEvent.KEYCODE_DPAD_CENTER) {
    dismiss();
    return true;
    }
    return false;
}

Note that, from the controller point of view, the **A** button is the **OK** button, but certain controllers also have the D-Pad center button and sometimes the *Enter* key, so we accept any of those key events as an **OK**.

In the case of the Quit dialog, the code is slightly different:

public boolean dispatchKeyEvent(KeyEvent event) {
    if (event.getKeyCode() == KeyEvent.KEYCODE_BUTTON_A ||
    event.getKeyCode() == KeyEvent.KEYCODE_ENTER ||
    event.getKeyCode() == KeyEvent.KEYCODE_DPAD_CENTER) {
    if (findViewById(R.id.btn_resume).isFocused() ||
    findViewById(R.id.btn_exit).isFocused()) {
    // Return false, so a proper click is sent
    return false;
    }
    return true;
    }
    return false;
}

In this dialog, whenever we get an OK event, we check if one of our buttons is focused. If that is the case, we return false, meaning that we let the upper level process it. That will pass the event to the activity, when it will be processed and sent back as a click.

If none of our actions are focused, we return true, meaning that we are consuming the event and nothing else should be done with it

Consuming the event and handling it should be done with it.

In any other case, we return false (again, not consuming the event) to let the activity handle the key event normally.

Finally, in the case of the pause and game over dialogs that are shown in the GameFragment, we do not need to do anything because the only item that is focusable on that UI is the pause button.

What we will do is set the pause button to not focusable and then let the system handle the keys:

<ImageView
 style="@style/iconButton"
 android:focusable="false"
 android:layout_gravity="top|right"
 android:id="@+id/btn_play_pause"
 android:layout_marginTop="@dimen/menu_margin"
 android:layout_marginRight="@dimen/menu_margin"
 android:src="@drawable/pause" />

Note that, while ImageView is not focusable by default, we have set that property at the style level, and we can always overwrite the value of the style in the definition of the layout.

With all this changes all the UI of our game is usable with a gamepad and we are finally ready to publish on Android TV.

## Beyond this book

We made it! We have built a game from scratch using the Android SDK. We have built a game engine that has a separate UpdateThread and DrawThread, we have sprites that can move around, we have handled touch and gamepad controllers, and added collision detection, sound, and even particles.

We have also used the components of the Android framework to build our menus and dialogs and made use of the animation tools to make it more dynamic.

YASS also has achievements and leaderboards using Google Play Game Services and now it is ready to be played on Android TV as well.

At this point, there are a few things that can be done to improve the game and that can be a good exercise to help you move forward, if you feel like it. Some ideas are:

- Spawn multiple waves with increasing difficulty
- Include bigger asteroids that break in smaller parts when hit
- Add ship selection: provide several ships with different characteristics (speed, fire ratio, and so on)
- Implement power-ups like shields or improved lasers

While this book focuses on the development of a game, there are other aspects of making a game that have been overlooked but are also very important. The most relevant of them are game design and monetization.

### Note

Game design and monetization are key aspects of making a game.

**Game design** is a discipline-independent from platform and code and it requires high-level considerations about the game itself. Game design is about making a game that is fun and engages the user. There are many books that are dedicated solely to this concept and it is beyond the scope of this book.

**Monetization** is also a big topic nowadays. If you plan to release a game just for fun you do not need to worry about it, but if you mean to make some money out of it, it is something to be evaluated.

The main options to monetize a game are:

- **Premium**: Make it a paid game
- **Ads**: Monetize the game by showing ads to the user, either banners or interstitials
- **In-App Purchases**: Offer the user the chance to purchase items inside the game

The advantage of making a paid game is that it is much simpler to build and it will give you fewer headaches.

Some examples of premium games that have done well on mobile are Monument Valley, Threes, and The Room (1 and 2).

The advantage of a free game, either with ads or In-App Purchases, is that the install barrier is the lowest there is. If someone is interested in the game, installing it is free. Paying for a game requires a conscious decision and many people will opt out, regardless of the price.

If you are planning to go the Free2Play road, be ready to worry about retention and conversion of the players. You will get familiar with terms like DAU, MAU, ARPU, and ARPPU, to name a few.

Some examples of successful free games with different monetization schemes are Candy Crush Saga, Clash of Clans, and Angry Birds.

Most studies will tell you that people make more money with In-App Purchases than with paid apps. While that it is true, creating a Free2Play game that monetizes well requires time and effort to tune it right; so does designing compelling In-App Purchases.

In my experience, making a good monetization design for a Free2Play game takes about as much time as making the game itself. If you do not design it carefully, you can get really low conversion rates, which turns into little or no revenue.

As a rule of thumb. do not push a monetization model in a game that does not

As a rule of thumb, do not push a monetization model in a game that does not feel right and invest your efforts into making the game fun.

## Summary

We have our game ready for Android TV. We have updated the manifest to provide an activity to launch from the Leanback interface as well as a banner to show on the UI, and we have marked it as a game. We also reviewed the permissions to be sure that the game will be available on Google Play for Android TV, mainly marking touchscreen capability and Leanback as requested but not required.

From the code point of view, we added some extra padding to all the screens when running on the TV, to make sure the important content is never cut off because of overscan, and we tweaked the code on the dialogs to make them work nicely with controllers.

Finally we mentioned some pointers about how to improve the game further and talked briefly about game design and monetization.

It is time to start making your own game. Good luck and have fun!
