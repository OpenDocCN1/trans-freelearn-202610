```
mImageView = (ImageView) view.findViewById(R.id.imageView);
// Load the image into the TextView via the URI
mImageView.setImageURI(Uri.parse(mCursor.getString(mCursor.getColumnIndex(DataManager.TABLE_ROW_URI))));
return view;
}
```

Finally, for the ViewFragment class, we need to override the onDestroy method. We do so to make sure that as the user views multiple images (which could be quite large with high-quality cameras on devices these days), we call recycle on the image we loaded with uri and set the source of Imageview to null. This has the effect of freeing up RAM memory and prevents the device from eventually running out and crashing.

# Tip

Actually, the device probably won't run out of memory, but the amount of memory the OS is prepared to allocate for our app probably will.

Add the onDestroy method we have just discussed to the ViewFragment class:

```
public void onDestroy(){
    super.onDestroy();
    // Make sure we don't run out of memory
    BitmapDrawable bd = (BitmapDrawable) mImageView.getDrawable();
    bd.getBitmap().recycle();
    mImageView.setImageBitmap(null);
}
```

Now, the app has somewhere to show the photo connected to any titles that are clicked on by the user. It will also need a way of communicating with our new ViewFragment before it can do so. So, let's create an interface to do just that.