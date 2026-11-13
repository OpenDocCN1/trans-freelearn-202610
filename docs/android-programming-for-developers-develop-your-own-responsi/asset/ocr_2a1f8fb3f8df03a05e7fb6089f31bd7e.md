Change the preceding line of code to this:

```
android:id="@+id/button2"
```

+   - Now we have two distinct buttons with unique IDs. We will see in Chapter 9, Object-Oriented Programming how to use these IDs to manipulate buttons in our UI by referring to these IDs in our Java code. Take a look at the visual design. Here, we can still see only one button. Look at the following line of XML code that is present in both our buttons:

```
android:layout_below="@+id/textView"
```

+   - This is the line of code that describes where the button should go in the layout to Android. As with our IDs, until we changed them, this line of code is identical in both the buttons. So, it is not surprising that the buttons are in the exact same place, and this is the reason we can only see one of them. Again, in the second button, change the line of code in question to the following:

```
android:layout_below="@+id/button"
```

+   - Now, the second button is below the first. We will examine what has happened a little more closely when we have edited just one more property. Here

![](bbox=[0.64, 0.49, 0.79, 0.61])

is a close-up figure of our two buttons on our UI design:

+   - Let's change one more thing about our second button before we consider our UI for this project complete. Identify the following line of code at the end of the code for the second button:

```
android:onClick="topClick" />
```

Note that it has the name of our soon-to-be-written method, topClick, within double quotation marks "".

+   - Be sure that you have identified the code in the second button and then change the word top to bottom. Now, we must create two Java methods (topClick and bottomClick) to avoid crashes when our user starts clicking on buttons.

+   - Run the app and see that everything looks good, unless you click on a button and the app crashes.