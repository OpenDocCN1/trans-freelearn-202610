```
public class AddressListFragment extends ListFragment {
private ActivityComs mActivityComs;

private ArrayList<NameAndAddress> mNamesAndAddresses;
```

Now, we need to initialize the member as a reference to MainActivity but cast to the type ActivityComs, and we do this in the overridden onAttach method. Then, we need to use it in the onListItemClick method, and finally, we need to delete the reference by setting it to null in the onDetach method. Add these three methods to the AddressListFragment class, as shown in the next code snippet:

```
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

```
@Override
public void onDetach() {
    super.onDetach();
    mActivityComs = null;
}
```

```
public void onListItemClick(ListView l, View v, int position, long id) {
    // pass the position to MainActivity
    mActivityComs.onListItemSelected(position);
}
```

Now, change the class declaration in MainActivity.java to implement our new interface:

```
public class MainActivity extends ActionBarActivity implements ActivityComs{
```

Now, we need to implement the onListItemClicked method within the