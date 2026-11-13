
be rotated into the landscape mode by pressing the Ctrl + F11 keyboard combination on PC or Ctrl + fn + F11 on Mac.

## Tip

At the time of writing this, the emulators seem to crash when their orientation is changed using Ctrl + F11. So if you have any trouble getting your emulator displayed in the landscape mode, just reconfigure it to startup in the landscape mode. You can do this by clicking on the AVD icon from the toolbar, then clicking on the edit (pencil icon) next to the Nexus 5 AVD, and selecting Landscape. Click on Finish and you're ready to run the app.

Try checking the radio buttons to see the time zone change on the clock. Enter different values into EditText and then click on the button to see it grab the text and display it on itself.

![](img/35d01831c19688a44e50bce7c489007a_394_0.png)

Change what the image looks like with different combinations of checked and unchecked checkboxes and hide and show the web page using the switch widget.

Let's move on to look at another way in which we can significantly enhance our UIs.

## Summary

In this chapter, we explored a plethora of widgets, saw how to implement widgets in Java code without any XML layout, used our first anonymous classes to handle clicks on a widget, and put all our new widget prowess into a mini app.

In the next chapter, we will see a totally new UI element that we can't just drag and drop from the palette, but we will still have plenty of help from the Android API. Next up are dialog boxes. We will also make a start on our most significant app so far—the Note To Self, memo, to-do, and personal note app.

## Chapter 12. Having a Dialogue with the User

In this chapter, we will see how to present a user with a pop-up dialog window. We can then put all we know into the first phase of our first app, Note To Self. You will learn about new Android and Java features in this chapter and the next five chapters as well, and we will use your newly acquired knowledge to enhance the Note To Self app each time.

In each chapter, we will also build a selection of mini apps that are separate from this main app. So, this chapter holds the following in store for you:

- How to implement a simple app with a pop-up dialog box
- How to add DialogFragments to start the Note To Self app
- How to add resources such as Strings and graphics to a project
- How to use Android naming conventions for the first time to make our code more readable
- How to implement more complex dialog boxes to capture input from the user

Let's get started.

### Dialog windows

Often, we will want to show the user some information or perhaps ask for confirmation of an action in a pop-up window. This is known as a **dialog** window. If you quickly scan the palette, you might be surprised to see no mention of dialogs whatsoever.

Dialogs in Android are more advanced than a simple widget or even a layout. They are classes that can contain layouts and widgets of their own.

The best way to create a dialog window in Android is to use the `Fragment` class. Fragments are a wide and vital topic in Android, and we will spend much of the second half of this book exploring and using them.

Creating a neat pop-up dialog for our user to interact with, however, is a great introduction to fragments and not too complicated at all.

### The dialog demo mini project – introducing chaining

We previously mentioned that the best way to create a dialog in Android is with the Fragment class. In Android, there is another way to create dialogs that is arguably a little bit simpler. The problem with this slightly simpler Dialog class is that it is not as well supported in the Activity lifecycle. It is even possible that Dialog could accidentally crash the app.

If you were writing an app with one fixed orientation layout that required one simple pop-up dialog, it could be argued that the simpler Dialog class should be used. However, as we are aiming to build modern, professional apps with advanced features, we will benefit from ignoring this class. More specifically, we will be using the DialogFragment class.

To start the mini project, create a new blank project in Android Studio and call it Dialog Demo. As you have come to expect, the completed code for this project is in the Chapter 12/Dialog Demo folder of the download bundle.

Create a new class by right-clicking on the folder with the name of your package that contains the MainActivity.java file. Select Java class under New and name it MyDialog. Click on OK to create the class.

The first thing to do is to change the class declaration in order to extend DialogFragment. When you have done so, your new class will look like this:

```java
public class MyDialog extends DialogFragment {
...
}
```

Now, let's add code to this class, a bit at a time, and see what is happening at each step.

First, we override the onCreateDialog method that will be called when we later show the dialog window via code from MainActivity.

Then, inside the onCreateDialog method, we get our hands on a new class. We declare and initialize an object of the AlertDialog.Builder type that needs a reference to MainActivity that is passed in its constructor. This is why we use getActivity() as the argument. The getActivity method is part of the Fragment class (and therefore, DialogFragment too), and it returns a reference to the Activity that created DialogFragment. In this case, it is our MainActivity class. Add this code to get the builder reference variable ready for use, and then we will see what we can do with it:

```java
@Override
public Dialog onCreateDialog(Bundle savedInstanceState) {
    // Use the Builder class because this dialog has a simple UI
    AlertDialog.Builder builder =
    new AlertDialog.Builder(getActivity());
}
```

Now, we can use our builder object to do the rest of the work. There is something slightly odd in the next three blocks of code. If you quickly scan the next three blocks, you will note that there is a distinct lack of semicolons, ;. This indicates that these three blocks of code are in fact just one line to the compiler.

What is going on here is something that we have actually seen before in a less pronounced situation. When we create a Toast message and we add a .show() method to the end of it, we are **chaining**, that is, we are calling more than one method, in sequence, on the same object. This is equivalent to writing multiple lines of code; it is just clearer and shorter this way.

In the first of the three blocks that uses chaining, we call builder.setMessage that sets the main message the user will see in the dialog box. Note also that it is fine to have comments in between parts of the chained method calls, as these are ignored entirely by the compiler.

Then, we add a button to our dialog with the .setPositiveButton method, and the first argument sets the text on it to OK. The second argument is an anonymous DialogInterface.OnClickListener class that handles clicks on the button. Note that we are not going to add any code to the onClick method.

Add this code, without any semicolons, of course:

```java
// Dialog will have "Make a selection" as the title
builder.setMessage("Make a selection")
```

```java
// An OK button that does nothing
.setPositiveButton("OK", new DialogInterface.OnClickListener() {
    public void onClick(DialogInterface dialog, int id) {
        // Nothing happening here
    }
})
```

Next, we call yet another method on the same builder object. This time, it's the setNegativeButton method. Again, the two arguments set Cancel as the text for the button and an anonymous class to listen for clicks. For the purpose of this demo, we are not taking any action in the overridden onClick method.

After the call to setNegativeButton, we finally see a semicolon marking the end of the line of code. We then code return builder.create(). This has the effect of returning to MainActivity (which called onCreateDialog in the first place), which is our new, fully configured, dialog window. We will see this calling code quite soon.

Add the following code directly after the previous block of code and within the onCreateDialog method:

```java
// A "Cancel" button that does nothing
.setNegativeButton("Cancel", new DialogInterface.OnClickListener() {
    public void onClick(DialogInterface dialog, int id) {
        // Nothing happening here either
    }
});
// Create the object and return it
return builder.create();
};// End onCreateDialog
```

Now, we have our MyDialog class that extends DialogFragment; all we have to
