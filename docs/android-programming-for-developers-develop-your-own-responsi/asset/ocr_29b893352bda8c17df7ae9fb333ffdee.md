created dialog and also to modify the logic of when to show it:

```
private void displayGamepadHelp() {
    showDialog(new ControllerHelpDialog(getYassActivity()));
}

private boolean shouldDisplayGamepadHelp() {
    PackageManager pm = getYassActivity().getPackageManager();
    boolean isLeanback =
    pm.hasSystemFeature("android.software.leanback");
    if (isLeanback) {
        boolean shownAlready = YassActivity.sGamepadHelpShown;
        YassActivity.sGamepadHelpShown = true;
        return !shownAlready;
    }
    if (isGameControllerConnected()) {
    return
    PreferenceManager.getDefaultSharedPreferences(getActivity())
    .getBoolean(PREF_SHOULD_DISPLAY_GAMEPAD_HELP, true);
    }
    return false;
}
```

In the method shouldDisplayGamePadHelp we will check if we use the Leanback feature to know if we are on a TV or not.

If the Leanback feature is not present, we use the same code as before: we display the dialog if a controller is connected and we have not displayed it before.

As mentioned before, we will keep the logic we had for phones, but for TVs we will always display the dialog.

In the case of an Android TV, we want to show it when the app is opened. Since shouldDisplayGamepadHelp is executed during the onResume method of the fragment, we will store the fact that it has been shown already in a static variable, so the dialog is not shown again when we come back to the fragment after a game over. Then we return true if the dialog has not been shown yet.