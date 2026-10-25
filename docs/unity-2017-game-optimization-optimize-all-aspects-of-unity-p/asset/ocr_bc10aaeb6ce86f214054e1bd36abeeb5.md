While holding down the right-mouse button in the **Scene** window, we can use the _W_, _A_, _S_,
and _D_ keys to fly around with the Camera, in typical first-person Camera control style. The
_Q_ and _E_ keys can also be used to fly up and down, respectively.

# ## Interface

We can press _Alt_ and click on any **Hierarchy** window arrow (the small gray arrow to the
left of any parent object name) to expand the object's entire hierarchy rather than just the
next level in the **Hierarchy** window. This works on `GameObjects` in the **Hierarchy**
window, folders and Prefabs within the **Project** window, lists in the **Inspector** window, and
so on.

We can save and restore object selections in the **Hierarchy** or **Project** windows much like a
typical RTS game. Make the selection and press _Ctrl_ + _Alt_ + <0-9> (_Command_ + _Alt_ + <0-9>)
to save the selection. Press _Ctrl_ + _Shift_ + <0-9> (_Command_ + _Shift_ + <0-9>) to restore it. This is
exceptionally useful if we find ourselves selecting the same handful of objects over and over
again while we make adjustments.

Pressing _Shift_ + spacebar will expand the current window to fill the entire Editor screen.
Pressing it again will shrink the window and restore it to its previous location.

Pressing _Ctrl_ + _Shift_ + _P_ (_Command_ + _Shift_ + _P_) will toggle the **Pause** button while in **Play**
**Mode**. This is usually an awkward key combination to press if we're trying to pause in a
hurry, so it often helps to create a custom hotkey for pausing:

```
void Update() {
    if (Input.GetKeyDown(KeyCode.P)) {
        Debug.Break();
    }
}
```

# # In-editor documentation

We can quickly access the documentation of any Unity keyword or class by highlighting it
in MonoDevelop and pressing _Ctrl_ + ’ (_Command_ + ’). This will open the default browser and
perform a search on the Unity documentation for the given keyword or class.

> > Note that users with European keyboards may need to also hold down the
> _Shift_ key for this feature to work.