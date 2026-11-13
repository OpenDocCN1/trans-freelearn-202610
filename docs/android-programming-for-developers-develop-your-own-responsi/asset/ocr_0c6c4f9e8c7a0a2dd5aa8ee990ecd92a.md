# # The dialog demo mini project – introducing chaining

We previously mentioned that the best way to create a dialog in Android is with the Fragment class. In Android, there is another way to create dialogs that is arguably a little bit simpler. The problem with this slightly simpler Dialog class is that it is not as well supported in the Activity lifecycle. It is even possible that Dialog could accidentally crash the app.

If you were writing an app with one fixed orientation layout that required one simple pop-up dialog, it could be argued that the simpler Dialog class should be used. However, as we are aiming to build modern, professional apps with advanced features, we will benefit from ignoring this class. More specifically, we will be using the DialogFragment class.

To start the mini project, create a new blank project in Android Studio and call it Dialog Demo. As you have come to expect, the completed code for this project is in the Chapter 12/Dialog Demo folder of the download bundle.

Create a new class by right-clicking on the folder with the name of your package that contains the MainActivity.java file. Select Java class under New and name it MyDialog. Click on OK to create the class.

The first thing to do is to change the class declaration in order to extend DialogFragment. When you have done so, your new class will look like this:

```
```java
public class MyDialog extends DialogFragment {
...
}
```
```

Now, let's add code to this class, a bit at a time, and see what is happening at each step.

First, we override the onCreateDialog method that will be called when we later show the dialog window via code from MainActivity.

Then, inside the onCreateDialog method, we get our hands on a new class. We