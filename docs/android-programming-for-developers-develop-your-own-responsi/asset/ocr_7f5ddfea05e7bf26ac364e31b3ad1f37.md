once they are shown, while the immersive sticky mode will hide them after a couple of seconds have passed, returning to the real fullscreen. The recommended mode for games is to use sticky immersion.

The code to put the app in the fullscreen sticky immersion mode is as follows:

```
@Override
public void onWindowFocusChanged(boolean hasFocus) {
    super.onWindowFocusChanged(hasFocus);
    if (hasFocus) {
        View decorView = getWindow().getDecorView();

decorView.setSystemUiVisibility(View.SYSTEM_UI_FLAG_LAYOUT_STABLE
        | View.SYSTEM_UI_FLAG_LAYOUT_FULLSCREEN
        | View.SYSTEM_UI_FLAG_FULLSCREEN
        | View.SYSTEM_UI_FLAG_LAYOUT_HIDE_NAVIGATION
        | View.SYSTEM_UI_FLAG_HIDE_NAVIGATION
        | View.SYSTEM_UI_FLAG_IMMERSIVE_STICKY);
    }
}
```

In this case, as in the previous one, we are requesting the use of a stable layout, and we are making it as if it is fullscreen. This time, we include a flag to make the stable layout the one with no Navigation bar
(SYSTEM_UI_FLAG_LAYOUT_HIDE_NAVIGATION).

We also add the flags to hide the Status bar (fullscreen) and the Navigation bar
(hide navigation). Finally, we ask for the immersive sticky mode. The result is a
real fullscreen game: