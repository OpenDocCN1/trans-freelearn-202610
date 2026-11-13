```
try {
    mImageView.setImageURI(Uri.parse(mImageUri.toString()));
}catch(Exception e){
    Log.e("Error","Uri not set");
}

}else{
    mImageUri = Uri.EMPTY;
}
}
```

We also need to override onDestroy so that we can clean up after ourselves. This next code gets Bitmap from the drawable, and by calling its chained getBitmap().recycle() methods, we can prevent the possibility of the app running out of memory:

```
public void onDestroy(){
    super.onDestroy();

    // Make sure we don't run out of memory
    BitmapDrawable bd = (BitmapDrawable) mImageView.getDrawable();
    bd.getBitmap().recycle();
    mImageView.setImageBitmap(null);
}
```

Now, we can test the image capturing functionality.