# CheckBox

We used CheckBox in our form layout in Chapter 5, Real-World Layouts. As
with EditText, however, we didn't explore how to actually get data from it. With
CheckBox, we simply detect its state (checked or unchecked) at a given moment,
perhaps, when a form's submit button is clicked. This next code gives us a
glimpse of how this might happen, again, using an inner class that acts as a
listener:

```
myCheckBox.setOnCheckedChangeListener(
    new CompoundButton.OnCheckedChangeListener() {

    public void onCheckedChanged(
    CompoundButton buttonView, boolean isChecked) {

    if (myCheckBox.isChecked()) {
    // It's checked so do something
    } else {
    // It's not checked do something else
    }
    }

    }
);
```

In the previous code, we assume that myCheckBox has been declared and
initialized, and we then use exactly the same type of anonymous class as we did
for Switch to detect and respond to clicks.