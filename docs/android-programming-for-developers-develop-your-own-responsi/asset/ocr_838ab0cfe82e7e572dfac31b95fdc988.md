# # Radio button and group

RadioButton is used when there are two or more mutually exclusive options for the user to choose from. That is, if you choose one, the other choices cannot be chosen, such as an old-fashioned radio. Take a look at a simple radiogroup with a few RadioButton  options in this next image:

![](bbox=[0.117, 0.242, 0.198, 0.309])

As the user selects an option, the other options will automatically be deselected. We control RadioButton widgets by placing them within RadioGroup in our UI layout. We can, of course, use the visual designer to simply drag a bunch of RadioButtons onto RadioGroup. When we do so, the XML layout looks something like this:

```
<RadioGroup
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:layout_alignParentTop="true"
    android:layout_alignParentLeft="true"
    android:layout_alignParentStart="true"
    android:id="@+id/radioGroup">
```

```
<RadioButton
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Option 1"
    android:id="@+id/radioButton1"
    android:checked="true" >
```

```
<RadioButton
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Option 2"
    android:id="@+idradioButton2"
    android:checked="false" >
```

```
<RadioButton
```