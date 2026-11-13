Android Studio thinks it might know what we want to type, it will pop up some options. It is a great time-saving habit to get into.

Add this line of code using auto-complete:

```
numMessages = 10;
```

Now that we have introduced the really handy code completion feature we will add a larger chunk of code. Immediately after the previous line of code and before the closing } of oncreate, add the following code:

```
// Output the value of numMessages
Log.i("numMessages = ", "" + numMessages);

numMessages++;
numMessages = numMessages + 1;
Log.i("numMessages = ", "" + numMessages);

// Now a boolean (just true or false)
boolean isFriend = true;
Log.i("isFriend = ", "" + isFriend);

// A contact and an important message
String contact = "James Gosling";
String message = "Dear reader, I invented Java.";

// Now let's play with those String variables
Toast.makeText(this, "Message from" + contact,
Toast.LENGTH_SHORT).show();

Toast.makeText(this, "Message is:" + message,
Toast.LENGTH_SHORT).show();
```

Run the app and we can examine the output and then the code. In the logcat window, you will see the following output:

```
numMessages =: 10
numMessages =: 12
isFriend =: true
```

On the screen, you will see two pop-up Toast messages. The first says Message from James Gosling. The second says Message is: Dear Reader, I invented Java. and is shown in the next screenshot: