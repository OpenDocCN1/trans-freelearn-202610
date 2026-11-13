
if(mImageUri != null){
    if(!mImageUri.equals(Uri.EMPTY)) {
    // We have a photo to save

    Photo photo = new Photo();
    photo.setTitle(mEditTextTitle.getText().toString());
    photo.setStorageLocation(mImageUri);

    // What is in the tags
    String tag1 = mEditTextTag1.getText().toString();
    String tag2 = mEditTextTag2.getText().toString();
    String tag3 = mEditTextTag3.getText().toString();

    // Assign the strings to the Photo object
    photo.setTag1(tag1);
    photo.setTag2(tag2);
    photo.setTag3(tag3);

    // Send the new object to our DataManager
    mDataManager.addPhoto(photo);
    Toast.makeText(getActivity(), "Saved",
    Toast.LENGTH_LONG).show();
    }else {
    // No image
    Toast.makeText(getActivity(), "No image to save",
    Toast.LENGTH_LONG).show();
    }
} else {
    // Uri not initialized
    Log.e("Error ", "uri is null");
}

});

return view;

}// End onCreateView

We could run the app at this point and actually begin to save photos to our database. The problem, of course, is that we don't yet have any way to see the list of available tags, photo titles, or any of the photos themselves.

Before TitlesFragment can respond to clicks and show a photo, we need a fragment to show those photos. So, let's code a fragment to view photos.

## Displaying a photo from the database

Let's start by building a simple layout that our soon-to-be-built fragment can use.

### Preparing the View layout

Create a new layout by right-clicking on the `layout` folder and choosing **New** | **Resource layout file**, naming it `fragment_view`, and left-clicking **OK**.

This image is what we are aiming for, but there is also an unseen `ImageView` covering the entire layout:

![](img/35d01831c19688a44e50bce7c489007a_786_0.png)

To achieve this layout, add the following XML code to the `fragment_view.xml` file. I have highlighted a few points worth noting that we will discuss:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout
xmlns:android="http://schemas.android.com/apk/res/android"
android:orientation="vertical"
android:layout_width="match_parent"
android:layout_height="match_parent">

<ImageView
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:id="@+id/imageView"
    android:layout_gravity="center_horizontal" />

<Button
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="@string/show_map"
    android:id="@+id/buttonShowLocation"
    android:layout_alignParentBottom="true"
    android:layout_centerHorizontal="true"
    android:layout_marginBottom="35dp" >

<TextView
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:textAppearance="?android:attr/textAppearanceLarge"
    android:text="Large Text"
    android:id="@+id/textView"
    android:layout_alignParentTop="true"
    android:layout_centerHorizontal="true"
    android:layout_marginTop="33dp" >

</RelativeLayout>
```

Notice that the **ImageView** widget's layout_width and layout_height properties are set to match_parent to fill the screen.

Notice there is a button (buttonShowLocation) that uses a string resource to set its text. This button is ready for [Chapter 26, Upgrading SQLite – Adding Locations and Maps, after we have learned to handle GPS locations and upgrade a database.](Chapter 26, Upgrading SQLite – Adding Locations and Maps, after we have learned to handle GPS locations and upgrade a database.)

Also notice the id properties of the three widgets in order to better follow what happens when we code our new Fragment class.

### Coding the ViewFragment class

Create a new class called ViewFragment. Now, we need to add some members and have our new class extend the Fragment class. We have a Cursor member ready to use the data that we will grab from the database. We also have an **ImageView** member, which, unsurprisingly, will be used to get a reference to the **ImageView** widget in our layout.

Amend the ViewFragment class to match the next code as we have just discussed:

```java
public class ViewFragment extends Fragment {
    private Cursor mCursor;
    private ImageView mImageView;
}
```

Now, we continue by adding the overridden onCreate method. First, we declare and initialize a new int variable called position from the Bundle passed into onCreate. From the getInt method, we can see that we are getting some data that was loaded as a key-value pair with a key of Position. Of course, we haven't written any code to actually send this data yet, but we will later in the chapter.

Next, we declare and instantiate a DataManager object so we can talk to the database, and then we initialize our Cursor member by calling the DataManager method getPhoto and passing in the position variable, whose value we extracted from Bundle a moment ago.

So now that we have cursor with Photo in it, we can see how to display it, and then we can see how we pass in the position.

Code the onCreate method in the ViewFragment class as we have just discussed:

```java
@Override
public void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);

    // Where is the photo object we want to show?
    int position = getArguments().getInt("Position");

    // Load the appropriate photo from db
    DataManager d = new DataManager(getActivity().getApplicationContext());
    mCursor = d.getPhoto(position);

}
```

As usual, when we want to initialize the view of a regular Fragment, we do so in the onCreateView method. Here is how the next block of code works.

We inflate the view as we have done several times before. Then, we get a reference to our TextView, Button, and ImageView widgets. We will not, however, be doing anything with Button just yet.

Then, we see how we access the data in cursor to set TextView with the title of the photo and ImageView with the image.

Notice that we can identify the correct column in the returned data (in cursor) by using the public static strings from our DataManager class. The DataManager.TABLE_ROW_TITLE string is the column name for the photo's title, and the DataManager.TABLE_ROW_URI string is, of course, the column name that holds the URI.

We can also see that Cursor has some useful methods that allow us to use the names of the columns to extract the data. They are getString and getColumnIndex.

Add the onCreateView method we have just been talking about into the ViewFragment class:

```java
public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {

    View view = inflater.inflate(R.layout.fragment_view, container,
            false);
    TextView textView = (TextView) view.findViewById(R.id.textView);
    Button buttonShowLocation = (Button)
            view.findViewById(R.id.buttonShowLocation);
    // Set the text from the title column of the data.
    textView.setText(mCursor.getString(mCursor.getColumnIndex(DataManager.TABLE_ROW_TITLE)));

    mImageView = (ImageView) view.findViewById(R.id.imageView);
    // Load the image into the TextView via the URI
    mImageView.setImageURI(Uri.parse(mCursor.getString(mCursor.getColumnIndex(DataManager.TABLE_ROW_URI))));
    return view;
}
```

Finally, for the ViewFragment class, we need to override the onDestroy method. We do so to make sure that as the user views multiple images (which could be quite large with high-quality cameras on devices these days), we call recycle on the image we loaded with uri and set the source of ImageView to null. This has the effect of freeing up RAM memory and prevents the device from eventually running out and crashing.

### Tip

Actually, the device probably won't run out of memory, but the amount of memory the OS is prepared to allocate for our app probably will.

Add the onDestroy method we have just discussed to the ViewFragment class:

```java
public void onDestroy(){
    super.onDestroy();
    // Make sure we don't run out of memory
    BitmapDrawable bd = (BitmapDrawable) mImageView.getDrawable();
    bd.getBitmap().recycle();
    mImageView.setImageBitmap(null);
}
```

Now, the app has somewhere to show the photo connected to any titles that are clicked on by the user. It will also need a way of communicating with our new ViewFragment before it can do so. So, let's create an interface to do just that.
