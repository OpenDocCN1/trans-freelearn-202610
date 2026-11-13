```
```java
public View getView(int whichItem, View view, ViewGroup
viewGroup) {
	if (view == null) {
		LayoutInflater inflater = (LayoutInflater)
		getActivity().getLayoutInflater();
		view = inflater.inflate(R.layout.list_item, null);
	}
	// We also have this super-handy getItem method
	NameAndAddress tempNameAndAddress = getItem(whichItem);
	TextView txtName = (TextView)
	view.findViewById(R.id.txtName);
	txtName.setText(tempNameAndAddress.getName());
	return view;
}
}
```
```

Now, we need to create a layout for our list item called list_item.xml. Create a new layout resource file called list_item in the layout folder in the usual way, drag a single **Plain TextView** widget onto the layout, and give the id property the value txtName.

While dealing with layouts, we need to modify our activity_main and activity_main_land layout files. The former so that it contains a single Fragment and the latter so that it contains two Fragments side by side. We will now set the IDs that we will use to place the Fragments.

In activity_main.xml, delete the **portrait TextView** widget and add FrameLayout from the Containers category of the pallette. Set its id property to listFragmentHolder. Here is the code that you should now have in activity_main.xml. Check whether you have the same code as the following that we just discussed:

```
```xml
<RelativeLayout
xmlns:android="http://schemas.android.com/apk/res/android"
xmlns:tools="http://schemas.android.com/tools"
android:layout_width="match_parent"
android:layout_height="match_parent"
android:paddingLeft="@dimen/activity_horizontal_margin"
android:paddingRight="@dimen/activity_horizontal_margin"
```
```