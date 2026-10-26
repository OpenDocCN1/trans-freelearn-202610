# Multiple sessions

Now let's see what it's like working with more than one session. Let's first detach the current session. Recall that in order to do so, we'll press <Prefix>, d (d for detach).

Now that the work session is detached, we are back at a command line, and your screen should be similar to the next screenshot. Instead of reattaching the work session, let's create a new session and call this session play:

```
$ tmux new-session -s play
```

![](bbox=[0.18, 0.3, 0.82, 0.47])

Now it looks like we have an entirely different session. We have a different status bar with the session name [play] in the bottom-left corner rather than [work].

Now to switch back to the work session, we could detach the play session, arrive back at a tmux-less terminal and then reattach the session, this time with the work session as the target; however, there is actually an easier way.

Simply press <Prefix>, s (select the session interactively); this will bring up a dialog where we can take our pick about which session to attach. So, we didn't even need to leave the session we were in to switch to the other one. Use the arrow keys to highlight the session you want, or just tap the number associated with that session to switch.

Try switching to the other session, then back again, but wait, there is an even easier way. Simply use <Prefix>, ) (next session) and <Prefix>, ( (previous session) to cycle around between the sessions. You'll see it's very easy to switch between sessions. We'll skip switching between multiple windows since you already know how to do that from the first chapter; let's move right on to multiple panes.