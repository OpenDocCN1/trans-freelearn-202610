# Some other overridden methods

Almost certainly you have noticed that there are two other auto-generated methods in the code of all our projects so far. They are onCreateOptionsMenu and onOptionsItemSelected. Most Android apps have a pop-up menu, so Android Studio generates one by default; including the basic code to make it work.

You can see the XML that describes the menu in menu/menu_main.xml from the project explorer. The key line of XML code is this:

```
<item android:id="@+id/action_settings"
android:title="@string/action_settings"
android:orderInCategory="100" app:showAsAction="never" />
```

This describes a **menu item** with the text, **Settings**. If you run any of the apps we have created so far you can see the button, as shown next:

![](bbox=[0.114, 0.453, 0.464, 0.513])

If you tap the button you can see it in action, as shown next:

![](bbox=[0.114, 0.613, 0.464, 0.673])

So, how do the onCreateOptionsMenu and onOptionsItemSelected methods produce these results?

The onCreateOptionsMenu method loads the menu from the menu_main.xml file with this line of code:

```
getMenuInflater().inflate(R.menu.menu_main, menu);
```