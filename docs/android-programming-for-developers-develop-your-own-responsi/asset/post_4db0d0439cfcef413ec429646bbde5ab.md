
```java
public View getView(int whichItem, View view, ViewGroup viewGroup) {
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

Now, we need to create a layout for our list item called list_item.xml. Create a new layout resource file called list_item in the layout folder in the usual way, drag a single Plain TextView widget onto the layout, and give the id property the value txtName.

While dealing with layouts, we need to modify our activity_main and activity_main_land layout files. The former so that it contains a single Fragment and the latter so that it contains two Fragments side by side. We will now set the IDs that we will use to place the Fragments.

In activity_main.xml, delete the portrait TextView widget and add FrameLayout from the Containers category of the palette. Set its id property to listFragmentHolder. Here is the code that you should now have in activity_main.xml. Check whether you have the same code as the following that we just discussed:

```xml
<RelativeLayout
xmlns:android="http://schemas.android.com/apk/res/android"
xmlns:tools="http://schemas.android.com/tools"
android:layout_width="match_parent"
android:layout_height="match_parent"
android:paddingLeft="@dimen/activity_horizontal_margin"
android:paddingRight="@dimen/activity_horizontal_margin"
android:paddingTop="@dimen/activity_vertical_margin"
android:paddingBottom="@dimen/activity_vertical_margin"
tools:context=".MainActivity"
android:id="@+id/layout">
<FrameLayout
android:layout_width="match_parent"
android:layout_height="match_parent"
android:layout_alignParentTop="true"
android:layout_alignParentLeft="true"
android:layout_alignParentStart="true"
android:id="@+id/listFragmentHolder"/></FrameLayout>
</RelativeLayout>
```

Switch to the activity_main_land.xml file and delete the landscape TextView widget.

Change the existing LinearLayout to set the orientation property to horizontal. Drag two FrameLayouts to the layout and set both their width properties to unset and both their layout_weight properties to .5. Set the id property of the left-hand side FrameLayout to listFragmentHolder and the id property of the right-hand side FrameLayout to detailFragmentHolder.

To be absolutely certain that you have the correct code for activity_main_land.xml, you can compare it to this listing. Just copy and paste the code if necessary:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
xmlns:android="http://schemas.android.com/apk/res/android"
android:orientation="horizontal"
android:layout_width="match_parent"
android:layout_height="match_parent">
<FrameLayout
android:layout_width="wrap_content"
android:layout_height="match_parent"
android:id="@+id/listFragmentHolder"
android:layout_weight=".5"/>
<FrameLayout
android:layout_width="wrap_content"
android:layout_height="match_parent"
android:id="@+id/detailFragmentHolder"
android:layout_weight=".5"/>
</LinearLayout>
```

The activity_main_land layout should now look like as shown in the next screenshot in the designer. Remember that you can switch between the landscape and portrait view in the designer, but that whichever way you view the layout in the designer, it will still work properly in the app. The designer is unaware of what our Java code or aliases will do:

![](img/35d01831c19688a44e50bce7c489007a_645_0.png)

We are making good progress. At this point, we can run the app, and our slightly short and somewhat unlikely list of names from our address book will be displayed in both the portrait and landscape orientation.

The landscape view might not be exactly what we expect because the list side expands to fill the entire screen as the detail side is currently empty. But our two different layouts both use the same AddressListFragment class, which doesn't need a layout file itself because ListView is supplied with ListFragment.

Now, we need to manage what happens when a list item is clicked. We already know how to handle list item clicks, but we have a few small problems:

- If a list item is clicked on in the landscape view, we need to show the appropriate details in the detail pane (on the right-hand side)
- If a list item is clicked on in the portrait view, we need a whole new Activity to show the detail (albeit still in an instance from the same Fragment)

Whichever of the preceding is the case, the detail view is a different class. So, we need to somehow communicate with that class. Before we look at the solution, which we have already discussed in the Inter-Fragment communications – interfaces revisited section earlier in this chapter, it is worth pointing out that we haven't created this class yet.

It is worth dealing with the communication issue first, however. We need an efficient way for AddressListFragment to communicate with MainActivity so that MainActivity can populate the detail pane (if in the landscape orientation) or start a new Activity to show a detail pane (if in the portrait orientation).

We will first code a solution to the communication issue. Then, we will create AddressDetailFragment that can be used in both our dualFragment landscape layout as well as a new Activity that we will call PortraitDetailActivity, which will be for the devices that are held in the portrait orientation.

## Creating the communication interface

This code will be surprisingly straightforward and nearly identical to the one we discussed in the *Inter-Fragment communications – interfaces revisited* section earlier in this chapter. The only difference is that we will see exactly where and how to implement the interface, and the onListItemClicked method will be fully coded to see how it either starts a new Activity with the Fragment detail in it or repopulates/renews the existing Fragment detail when it is in the landscape orientation.

I will show you the code and then we will discuss where and how to use the code again to try and reinforce what we are doing:

```java
public interface ActivityComs {
    void onListItemSelected(int pos);
}
```

We will create an interface (the one we just saw) with a single method. We will then make MainActivity implement this interface, and it will then be forced to override the onListItemSelected method.

The AddressListFragment class can then call the method when a list item is clicked on and pass the appropriate NameAndAddress reference as well as its position in ArrayList. We will see why both the pieces of data are used.

MainActivity can then handle either starting a new Activity (when in the portrait orientation) or just passing the data onto DetailFragment directly (when in the landscape orientation).

Right-click on the folder that contains all our .java files and navigate to New | Class. Type ActivityComs in the Name field and select Interface from the drop-down list named Kind. Then, click on OK and code the new interface exactly as shown in the previous block of code.

Next, we need to create an instance of this interface in the AddressListFragment class as a member. Add this member just after the class declaration, as highlighted in the next snippet:

```java
public class AddressListFragment extends ListFragment {
private ActivityComs mActivityComs;

private ArrayList<NameAndAddress> mNamesAndAddresses;
```

Now, we need to initialize the member as a reference to MainActivity but cast to the type ActivityComs, and we do this in the overridden onAttach method. Then, we need to use it in the onListItemClick method, and finally, we need to delete the reference by setting it to null in the onDetach method. Add these three methods to the AddressListFragment class, as shown in the next code snippet:

```java
@Override
public void onAttach(Activity activity) {
    super.onAttach(activity);

    /*
    activity is a ActivityComs but must
    still be explicitly cast to the type
    */
    mActivityComs = (ActivityComs)activity;
}
```

```java
@Override
public void onDetach() {
    super.onDetach();
    mActivityComs = null;
}
```

```java
public void onListItemClick(ListView l, View v, int position, long id) {
    // pass the position to MainActivity
    mActivityComs.onListItemSelected(position);
}
```

Now, change the class declaration in MainActivity.java to implement our new interface:

```java
public class MainActivity extends ActionBarActivity implements ActivityComs{
```

Now, we need to implement the onListItemClicked method within the
