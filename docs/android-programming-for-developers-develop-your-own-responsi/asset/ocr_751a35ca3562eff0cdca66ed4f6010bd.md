# Coding the fragment communications in MainActivity

We just need to implement the ActivityComs interface and add an instance of our DataManager class. Then, we'll handle the two methods of the interface, and we are good to go.

Implement the interface and add an instance of DataManager as shown highlighted in the next code:

```
public class MainActivity extends ActionBarActivity implements ActivityComs{
    private ListView mNavDrawerList;
    private DrawerLayout mDrawerLayout;
    private ArrayAdapter<String> mAdapter;
    private ActionBarDrawerToggle mDrawerToggle;
    private String mActivityTitle;

    public DataManager dataManager;
```

Initialize the DataManager instance by adding the highlighted line of code in onCreate:

```
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);

    dataManager = new DataManager(getApplicationContext());

    // We will come back here in a minute!
    mNavDrawerList = (ListView)findViewById(R.id.navList);
    mDrawerLayout = (DrawerLayout)findViewById(R.id.drawerLayout);
    mActivityTitle = getTitle().toString();

    ...
```

Now, we will code the onTagsListItemSelected method. Here, we put the passed in string that represents a tag into Bundle and pass it in to the new instance of TitlesFragment. We have seen already how TitlesFragment deals with this string.