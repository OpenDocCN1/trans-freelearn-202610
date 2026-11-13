# # Defining a simple layout

This layout is really basic. When we add the picture capture functionality into Where it's snap later this chapter, we will spend a bit more time and effort laying out the capture Fragment:

```
<LinearLayout
xmlns:android="http://schemas.android.com/apk/res/android"
android:orientation="vertical"
android:layout_width="fill_parent"
android:layout_height="fill_parent">

<Button android:id="@+id/button1"
android:layout_width="wrap_content"
android:layout_height="wrap_content"
android:text="Take picture">

<ImageView android:id="@+idimageView1"
android:layout_height="wrap_content"
android:layout_width="wrap_content">

</LinearLayout>
```

The preceding layout has Button with an id property of button1 and ImageView,
to place the newly captured image, with an id property of imageView1.

Now, we can move on to coding Java in MainActivity.