First, we make sure that they are clear to start with by calling clearCheck() on radioGroup. Then, we create our anonymous class of the OnCheckedChangedListener type and override the onCheckedChanged method.

This method will be called when any radio button from RadioGroup is clicked. All we need to do is get the id of the RadioButton widget that was clicked on and respond accordingly using a switch statement with the three possible cases, one for each RadioButton widget.

Remember when we first talked about RadioButton that the ID supplied in the checkedId parameter of onCheckedChanged is int? This is why we must first create a new RadioButton object from checkedId:

```
RadioButton rb = (RadioButton) group.findViewById(checkedId);
```

Then, we can call getId on the new RadioButton widget as the condition for switch:

```
switch (rb.getId())
```

Then, in each case, we use the setTimeZone method with the appropriate Android time zone code as an argument.

# Tip

You can see all the Android time zone codes here at https://gist.github.com/arpit/1035596.

Add this next code that incorporates everything we just discussed and includes the previous two lines of code. Add it in onCreate after the last code that we entered to handle the checkboxes:

```
// Now for the radio buttons
// Uncheck all buttons
radioGroup.clearCheck();

radioGroup.setOnCheckedChangeListener(new
RadioGroup.OnCheckedChangeListener() {
@Override
public void onCheckedChanged(RadioGroup group, int checkedId) {
RadioButton rb = (RadioButton) group.findViewById
(checkedId).
```