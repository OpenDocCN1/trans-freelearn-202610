Lastly, we create an anonymous class to handle clicks on ListView, and we simply call switchFragment with the appropriate position (whichItem) that was clicked.

Add the highlighted code that we discussed just after the code that we added previously into the onCreate method:

```
@Override
protected void onCreate(Bundle savedInstanceState) {
	super.onCreate(savedInstanceState);
	
	...
	
	// We will finish off this method next
	// From here
	
	// Initialize an array with our titles from strings.xml
	String[] navMenuTitles =
	getResources().getStringArray(R.array.nav_drawer_items);
	// Initialize our ArrayAdapter
	mAdapter = new ArrayAdapter<String>(this,
	android.R.layout.simple_list_item_1, navMenuTitles);
	// Set the adapter to the ListView
	mNavDrawerList.setAdapter(mAdapter);
	
	setupDrawer();
	
	getSupportActionBar().setDisplayHomeAsUpEnabled(true);
	getSupportActionBar().setHomeButtonEnabled(true);
	
	mNavDrawerList.setOnItemClickListener(new
	AdapterView.OnItemClickListener() {
	
	@Override
	public void onItemClick(AdapterView<?> adapter, View view, int
	whichItem, long id) {
	
	switchFragment(whichItem);
	}
	});
	
	switchFragment(0);
}
```