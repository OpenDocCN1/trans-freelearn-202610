# # Saving a new photo from the capture fragment

Open up the CaptureFragment.java file in the editor.

We need to add a new member variable for the CaptureFragment class to hold our new DataManager class instance.

Add the new highlighted member variable to CaptureFragment, which we have just discussed:

```
```java
public class CaptureFragment extends Fragment{
    private static final int CAMERA_REQUEST = 123;
    private ImageView mImageView;

    // Where the captured image is stored
    private Uri mImageUri = Uri.EMPTY;

    // Where to save the image
    String mCurrentPhotoPath;

    // A reference to our database
    private DataManager mDataManager;

    @Override
    public void onCreate(Bundle savedInstanceState){
        super.onCreate(savedInstanceState);
    }
}
```
```

Now, we can initialize the DataManager instance by adding this highlighted line of code to the onCreate method in CaptureFragment:

```
```java
@Override
public void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);

    mDataManager =
        new DataManager(getActivity()
                .getApplicationContext());
}
```
```