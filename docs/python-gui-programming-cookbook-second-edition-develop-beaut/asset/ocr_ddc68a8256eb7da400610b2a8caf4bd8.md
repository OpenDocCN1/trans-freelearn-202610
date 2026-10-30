After verifying the default labelFrameText, we programmatically set the radio button to index 1 and then invoke the radio button's callback method:

```
self.gui.radVar.set(1)
self.gui.callBacks.radCall()
```

This is basically the same action as clicking the radio button in the GUI, but we do this button click event via code in the unit tests.

Then we verify that our text in the LabelFrame widget has changed as intended.

When we run the unit tests from within Eclipse with the Python PyDev plugin, we get the following output written to the Eclipse console:

```
UnitTests.py
```

![](bbox=[0.27, 0.37, 0.68, 0.52])

Run from a Command Prompt, we get a similar output once we navigate to the folder where our code resides:

![](bbox=[0.04, 0.59, 0.9, 0.75])