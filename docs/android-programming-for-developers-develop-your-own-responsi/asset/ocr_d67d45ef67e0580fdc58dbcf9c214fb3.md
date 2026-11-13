```
<selector>
```

As we already said, the order is very important. We want to show the pressed state when it is pressed; we do not care if it is focused or not, so that is the first case. Then we want to show if it is focused. Finally we have a default case with no parameters that covers "everything else".

The backgrounds have been created using GIMP and the base icons are free ones that we found using an icon search website such as IconFinder (https://www.iconfinder.com). There are many icon search websites. Try several and see which one you like best.

Note that the naming convention is to use a name for the state list and then the same name followed by pressed, selected, or normal for each state.

# State lists colors

We can use selectors to define colors as well as images. This can be used on the text that goes into a button or the background of a view. It works the same way as for drawables, but uses the color keyword instead of drawable. The files that define colors must be placed under the res/colors directory.

An example of a color state list that we will reference later is btn_background.xml.

```
<?xml version="1.0" encoding="utf-8"?>
<selector
xmlns:android="http://schemas.android.com/apk/res/android"
	<item
	android:color="@color/btn_pressed"
	android:state_pressed="true">
	<item
	android:color="@colorbtn_focused"
	android:state_selected="true">
	<item
	android:color="@colorbtn_normal">
</selector>
```

And of course we need to define these colors as well. Note that these colors are not state lists, and therefore should be defined in a file under the res/values directory. We normally call this file colors.xml as a convention, but any name