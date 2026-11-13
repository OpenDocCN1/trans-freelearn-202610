For examplie, what napppens when tne user exits tne app by pressing tne home button? When we open two apps, one after the other, and then use the back button to switch to the previous app: will that destroy or just stop the app? What happens when the user has a dozen apps in his task manager and the operating system needs to destroy some apps that were previously only stopped: will our app be one of the "victims"?

You can of course test out all of these scenarios on the emulator, but the results will only be true for the one time you test it. It is not guaranteed that the same behavior will be exhibited every time, and certainly not on every different Android device.

At last some good news! The solution to all this complexity is to follow a few simple rules:

+   - Set up your app ready to run in the onCreate method
+   - Load your user's data in the onResume method
+   - Save your user's data in the onPause method
+   - Tidy up your app and make it a good Android citizen in the onDestroy method
+   - Watch out throughout the book for a couple of occasions when we might like to use onStart and onStop

If we do these things, and we will see how to over the course of the book, we can just stop worrying about all this lifecycle stuff and let Android handle it!

There are a few more methods we can override as well. So let's take a look at them.