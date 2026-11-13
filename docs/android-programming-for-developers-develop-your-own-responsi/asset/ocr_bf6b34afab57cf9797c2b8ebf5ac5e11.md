So you might expect the widgets to be on top of one another?

Previously I suggested taking a close look at the three highlighted lines in the code. Let's examine the first highlighted line, which is from the first TextView that we added below TextView that was already there (the one with the text Hello world!). Here is the highlighted line of code again:

```
android:layout_below="@+id/textView"
```

What this is saying is place me below the widget with the id of textView. If you look at the main code listing again you will indeed see that the id property of TextView containing the text Hello world! is set to textView with the following line of XML:

```
android:id="@+id/textView" />
```

This method of describing where the contents of a layout go is for RelativeLayout only. It is perfect for some types of practical app designs (perhaps forms) and extremely awkward for others.

Let's explore some other layout types and then we will build some layouts that take advantage of each layout type we have experimented with.

# Using LinearLayout

With this layout the clue is in the name. All the widgets contained in LinearLayout will be displayed in the order in which they are added. We can certainly still add margins, padding, and so on, but the order is fixed. It is linear (or sequential). LinearLayouts applies either vertical or horizontal ordering. Let's quickly make a project and combine a few LinearLayouts with some widgets.

The code for this mini app can be found in the Chapter 4/Linear Layout Experiment folder:

+   1. Create a new project in Android Studio. Call it Linear Layout Experiment, choose a Blank Activity, and leave all the other settings at their defaults.

+   2. Let's start with a completely clean sheet. Right-click on the layout folder in the project explorer. From the menu, choose New | Layout resource file as