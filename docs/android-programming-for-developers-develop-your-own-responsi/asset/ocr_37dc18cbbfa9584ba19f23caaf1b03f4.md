```
else if (mGamepadControllerListener != null) {
if
(mGamepadControllerListener.dispatchGenericMotionEvent(event)) {
return true;
}
}
return super.dispatchGenericMotionEvent(event);
}
```

Finally, we have to update the handling of the back key:

```
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
```

If we are showing a dialog, we just dismiss it. Otherwise we keep the code we
had before: we ask the current fragment to handle the back key press and, if the
event was not consumed, we pass it to the parent class.

Note that passing the back key event to the current fragment is important. In our
game a back press while in the GameFragment means that we want to pause the
game.

To complete this, we have to make the dialogs handle the keys. The default
implementation on BaseCustomDialog is to just return false, meaning that the
event was not consumed:

```
public boolean dispatchKeyEvent(KeyEvent event) {
return false;
}
public boolean dispatchGenericMotionEvent(MotionEvent event) {
return false;
}
```

We are going to do a very simple handling where we will only process the OK