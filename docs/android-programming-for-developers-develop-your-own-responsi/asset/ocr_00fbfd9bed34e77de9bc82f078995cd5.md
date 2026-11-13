Implement the required onTagsListItemSelected method of the interface as discussed:

```
public void onTagsListItemSelected(String clickedTag){
    // We have just received a String for the TitlesFragment

    // Prepare a new Bundle
    Bundle args = new Bundle();

    // Pack the string into the Bundle
    args.putString("Tag", clickedTag);

    Create a new instance of TitlesFragment
    TitlesFragment fragment = new TitlesFragment();

    // Load the Bundle into the Fragment
    fragment.setArguments(args);

    // Start the fragment
    FragmentManager fragmentManager = getFragmentManager();
    fragmentManager.beginTransaction().replace(R.id.fragmentHolder,
    fragment, "TAGS").commit();

    // update selected item and title, then close the drawer
    mNavDrawerList.setItemChecked(1, true);
    mNavDrawerList.setSelection(1);
    mDrawerLayout.closeDrawer(mNavDrawerList);
}
```

In the onTitlesListItemSelected method, we have received int, which represents the _id of a row in the wis_table_photos table that needs to be shown to the user in the ViewFragment.

The code adds int to Bundle and then adds Bundle to Fragment before starting Fragment.

Implement the onTitlesListItemSelected method of the interface:

```
// Open ViewFragment with the photo indicated by position
public void onTitlesListItemSelected(int position) {
    // Load up the bundle with the row _id
    Bundle args = new Bundle();
    args.putInt("Position", position);
```