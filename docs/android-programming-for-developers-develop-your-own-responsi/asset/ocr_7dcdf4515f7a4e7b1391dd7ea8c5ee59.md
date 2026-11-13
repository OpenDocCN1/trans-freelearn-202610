```
`Log.i("info","our message here");`
```

Exactly why this works will become clearer in Chapter 9, Object-Oriented Programming, but for now, we just need to know that whatever we put between the two sets of quote marks will be the output of the logcat window. We will see where to put this type of code shortly.

Here is the code we need to write in order to send a message to the user's screen:

```
`Toast.makeText(this, "our message", Toast.LENGTH_SHORT).show();`
```

This is a very convoluted-looking line of code, and exactly how it works, again, will not become clear until Chapter 9, Object-Oriented Programming. The important thing here is that whatever we put between the quote marks will appear in a pop-up message to our users.

Let's put some code just like this into a real app.