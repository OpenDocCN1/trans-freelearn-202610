# # Writing our first Java code

So we now know the code that will provide an output to logcat or the user's screen. But, where do we put it? For now, we just need to understand that the oncreate method is executed as the app prepares to be shown to the user. So if we put our code at the end of this method, it will run just as the user sees the app. Sounds good.

# ## Tip

We know that to execute the code in a method, we need to call it. We have wired our buttons up to call a couple of methods: topClick and bottomClick. Soon, we will write these methods. But who or what is calling onCreate!? The answer to this mystery is that Android itself calls oncreate in response to the user clicking on the app icon to run the app. In [Chapter 6, The Life and Times of an Android App], we will explore the Android lifecycle and it will be really clear exactly which code is executed and when. You don't need to completely comprehend this now. I just wanted to give you an overview of what was going on.

Let's quickly try this out. We know that the onCreate method is called just before the app starts for real. Let's copy and paste the following lines of code into the onCreate method of our Hello Android app and see what happens when we run it:

+   1. Identify the closing curly brace } of the onCreate method and add the highlighted code, as shown in the next snippet:

```
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.my_layout);

    // Let's take a look at Toast and Log in action
    Toast.makeText(this, "Can you see me",
    Toast.LENGTH_SHORT).show();

    Log.i("info", "Done creating the app");
}
```

+   2. Note that the two instances of the word Toast and the word Log are