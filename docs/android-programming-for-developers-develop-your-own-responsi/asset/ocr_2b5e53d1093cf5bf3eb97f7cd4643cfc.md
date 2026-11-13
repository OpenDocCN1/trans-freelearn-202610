```
// Now the next checkbox
cbTint.setOnCheckedChangeListener(new
    CompoundButton.OnCheckedChangeListener() {
    public void onCheckedChanged(CompoundButton buttonView, boolean
    isChec) {
    if (cbTint.isChecked()) {
    // Checked so set some tint
    imageView.setColorFilter(Color.argb(150, 255, 0, 0));
    } else {
    // No tint required
    imageView.setColorFilter(Color.argb(0, 0, 0, 0));
    }
    }
    });
```

In the anonymous class that handles the **Resize** labeled checkbox, we use the setScaleX method to resize the robot image. When we call setScaleX(2) and setScaleY(2) on imageView, we will double the size of the image, and setScaleX(1) and setScaleY(1) will return it to normal.

Add this code after the previous block of code in onCreate:

```
// And the last check box
cbReSize.setOnCheckedChangeListener
    (new CompoundButton.OnCheckedChangeListener() {
    public void onCheckedChanged(CompoundButton buttonView,
    boolean isChecked) {
    if (cbReSize.isChecked()) {
    // It's checked so make bigger
    imageView.setScaleX(2);
    imageView.setScaleY(2);
    } else {
    // It's not checked make regular size
    imageView.setScaleX(1);
    imageView.setScaleY(1);
    }
    }
    });
```

Now we will handle the three radio buttons. As they are part of RadioGroup, we can handle them much more succinctly than we did the CheckBox objects. Here is how we do it.