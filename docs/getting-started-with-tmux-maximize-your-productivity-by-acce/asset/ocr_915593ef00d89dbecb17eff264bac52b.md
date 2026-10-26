![](bbox=[0.21, 0.15, 0.79, 0.3])

Pick window 0 to open it.

# Detaching and attaching

Now press <Prefix>, d.

Uh oh! Looks like tmux is gone! The familiar status bar is no more available. The <Prefix> key set does nothing anymore.

![](bbox=[0.22, 0.47, 0.78, 0.62])

You may think we the authors have led you astray, causing you to lose your work. What will you do without that detailed document you were writing in nano?

Fear not explorer, we are simply demonstrating another very powerful feature of tmux. <Prefix>, d will simply detach the currently active session, but it will keep running happily in the background!

Yes, although it looks like it's gone, our session is alive and well.

How can we get back to it? First, let's view the active sessions. In your terminal, run the following command:

```
$ tmux list-sessions
```