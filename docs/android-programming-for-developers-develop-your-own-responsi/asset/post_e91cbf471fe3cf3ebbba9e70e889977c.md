
Making all these variables into members/fields means that they have a class scope, and we can access them from anywhere within the MainActivity class. This will be essential for this project because we will need to use them all in onCreate and in our new onClick method.

Enter this next code that we just discussed after the opening curly brace, {, of the MainActivity class and before the onCreate method:

```
// An int variable to hold a value
private int value = 0;

// A bunch of Buttons and a TextView
private Button btnAdd;
private Button btnTake;
private TextView txtValue;
private Button btnGrow;
private Button btnShrink;
private Button btnReset;
private Button btnHide;
```

### Tip

Remember to use the Alt + Enter keyboard combination to import new classes.

Next, we want to prepare all our variables ready for action. The best place for this to happen is in the onCreate method because we know that will be called by Android just before the app is shown to the user. This code uses the findViewById method to associate each of our Java objects with a widget from our UI.

It does so by returning a reference to the object that is associated with the UI widget on the heap. It knows which one we are after because we use the appropriate id as an argument. For example, ...(R.id.btnAdd) will return Button with the ADD text that we created in our layout.

As a reminder, we use the odd-looking = (Button) syntax because the method is polymorphic and could potentially return any object type that is a subclass of the View class. This is called casting.

Enter the following code just after the call to setContentView in the onCreate method:

```
// Get a reference to all the buttons in our UI
// Match them up to all our Button objects we declared earlier
btnAdd = (Button) findViewById(R.id.btnAdd);
btnTake = (Button) findViewById(R.id.btnTake);
txtValue = (TextView) findViewById(R.id.txtValue);
btnGrow = (Button) findViewById(R.id.btnGrow);
btnShrink = (Button) findViewById(R.id.btnShrink);
btnReset = (Button) findViewById(R.id.btnReset);
btnHide = (Button) findViewById(R.id.btnHide);
```

Now that we have a reference to all of our Button objects and TextView, we can start using their methods. In the code that follows, we use the setOnClickListener method on each of the Button references to make Android pass any clicks from the user onto our onClick method.

This works because when we implemented the View.OnClickListener interface, our MainActivity class effectively became an OnClickListener.

So, all we have to do is call setOnClickListener on each button in turn. As a reminder, the this argument is a reference to MainActivity. So, the method call says, "hey Android, I want an onClickListener and I want it to be the MainActivity class". Android now knows which class to call onClick on. This next code wouldn't work if we hadn't implemented the interface first. Also, we must set up these listeners before the Activity starts, which is why we do it in onCreate.

We will add code to onClick to actually handle what happens soon.

Add this code after the previous code inside the onCreate method:

```
// Listen for all the button clicks
btnAdd.setOnClickListener(this);
btnTake.setOnClickListener(this);
txtValue.setOnClickListener(this);
btnGrow.setOnClickListener(this);
btnShrink.setOnClickListener(this);
btnReset.setOnClickListener(this);
btnHide.setOnClickListener(this);
```

Now, scroll down to the onClick method that Android Studio added for us after we implemented the OnClickListener interface. Add the float size; variable declaration and an empty switch block inside it so that it looks like this next code. The new code to be added is highlighted here:

```
public void onClick(View v)

// A local variable to use later
float size;

switch(v.getId()){ }
}
```

Remember that switch will check for case to match the condition inside the switch statement.

In the previous code, the switch condition is v.getId(). Let's step through and understand this. The v variable is a reference to an object of the type View, which was passed into the onClick method by Android:

```
public void onClick(View v)
```

View is the parent class for Button, TextView, and more. So, perhaps, as we might expect, calling v.getId() will return the ID property of the UI widget that has been clicked, the widget that triggered the call to onClick in the first place.

All we need to do then is provide a case statement (and appropriate action) for each of the Button references we want to respond to.

We will see that this next code has the first three case statements. They handle R.id.btnAdd, R.id.btnTake, and R.id.btnReset.

The code in the R.id.btnAdd case simply increments the value variable, and then it does something new. It calls the setText method on the txtValue object. Here is the argument ("" + value). This argument uses an empty string and adds (concatenates) whatever value is stored in value to it. This has the effect of causing our TextView txtValue widget to display whatever value is stored in value.

The TAKE button (R.id.btnTake) does exactly the same, but subtracts 1 from value instead of adding 1.

The third case statement handles the RESET button, sets value to 0, and again updates the text property of txtValue.

Then, at the end of each case, there is a break statement. At this point, the switch block exits, the onClick method returns, and life returns to normal— until the user's next click.

Enter this code that we just discussed inside the switch block after the opening curly brace, {:

```
case R.id.btnAdd:
    value ++;
    txtValue.setText(""+ value);

    break;

case R.id.btnTake:
    value--;
    txtValue.setText(""+ value);

    break;

case R.id.btnReset:
    value = 0;
    txtValue.setText(""+ value);

    break;
```

The next two case statements handle the **SHRINK** and **GROW** buttons from our UI. We can confirm this from the R.id.btnGrow and R.id.btnShrink IDs. What is new and more interesting are the two new methods that are used.

The getTextScaleX method returns the horizontal scale of the text within the object it is used on. We can see that the object it is used on is our TextView txtValue. The size = at the start of the line of code assigns the returned value to our float variable, size.

The next line of code in each case statement changes the horizontal scale of the text using setTextScaleX. When the **GROW** button is pressed, the scale is set to size + 1, and when the **SHRINK** button is pressed, the scale is set to size - 1.

The overall effect is to allow these two buttons to grow and shrink the text in txtValue by a scale of 1 on each click.

Enter the next two case statements that we just discussed below the previous code:

```
case R.id.btnGrow:
    size = txtValue.getTextScaleX();
    txtValue.setTextScaleX(size + 1);

    break;

case R.id.btnShrink:
    size = txtValue.getTextScaleX();
    txtValue.setTextScaleX(size - 1);

    break;
```

In our last case statement, which we will code next, we have an if-else block. The condition takes a little bit of explaining. Here it goes.

The condition to be evaluated is txtValue.getVisibility() == View.VISIBLE.
The first part of this condition before the == operator returns the visibility
property of txtValue TextView. The return value will be one of the three
possible constant values defined in the View class. They are View.VISIBLE,
View.INVISIBLE, and View.GONE.

If TextView is visible to the user on the UI, the method returns View.VISIBLE,
the condition is evaluated as true, and the if block is executed.

Inside the if block, we use the setVisibility method on txtValue and make it
invisible to the user with the View.INVISIBLE argument.

In addition to this, we change the text on btnHide to SHOW using the setText
method.

After the if block is executed, txtValue becomes invisible, and we have a
button on our UI that says SHOW. When the user clicks on it in this state, the if
statement will be false and the else block will be executed. In the else block,
we reverse the situation. We set txtValue back to View.VISIBLE and the text
property on btnHide back to HIDE.

If this is in anyway unclear, just enter the code, run the app, and revisit this last
code and explanation once you have seen it in action:
