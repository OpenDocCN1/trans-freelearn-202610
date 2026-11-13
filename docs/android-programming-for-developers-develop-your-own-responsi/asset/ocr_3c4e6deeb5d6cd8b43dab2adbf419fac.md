```
@Override
public void onDestroy() {
    // First call the "official" version of this method
    super.onDestroy();

    Toast.makeText(this, "In onDestroy", Toast.LENGTH_SHORT).show();
    Log.i("info", "In onDestroy");
}
```

First of all, let's talk about the code itself. Notice that the method names all correspond to the lifecycle methods and phases we have just discussed. Notice that all of the method declarations are preceded by the @override line of code. Also see that the first line of code inside each method is super.on...

What it is that is going on here is this:

+   - Android calls our methods at the various times we have already discussed.
+   - The @override keyword indicates that these methods replace/override the original version of the method that is provided as part of the Android API. Note we don't see these overridden methods but they are there, and if we didn't override them, these original versions would be called by Android instead of ours.
+   - The super.on... syntax, which is the first line of code within each of the overridden methods, then calls these original versions.

So, we don't simply override these original methods in order to add our own code, we also call them and their code is executed too.

# Tip

For the curious, the keyword super is for super-class. We will explore method overriding and super classes in several chapters as we progress.

Finally, the code that you added will make each of the methods output one Toast message and one Log message. However, the messages that are output vary, as can be seen by the text in between the double quote marks "". The messages that are output will make it plain which method produced them.

Now that we have looked at the code, we can play with our app and learn about the lifecycle from what happens: