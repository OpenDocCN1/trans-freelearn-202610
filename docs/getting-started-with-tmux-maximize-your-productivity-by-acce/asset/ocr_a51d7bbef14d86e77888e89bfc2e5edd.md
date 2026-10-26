You should see a nice list that has your session name, number of windows, and date of creation and dimensions. If you had more than one session, you'd see them here too.

![](bbox=[0.19, 0.2, 0.8, 0.36])

To re attach the detached session to your session, simply run the following command:

```
$ tmux attach-session -t tutorial
```

This tells tmux to attach a session and the session to attach it to as the target (hence -t). In this case, we want to attach the session named tutorial. Sure enough, you should be back in your tmux session, with the now familiar status bar along the bottom and your nano masterpiece back in view.

Note that this is the most verbose version of this command. You can actually omit the target if there is only one running session, as is in our scenario. This shortens the command to tmux attach-session. It can be further shortened because attach-session has a shorter alias, attach. So, we could accomplish the same thing with just tmux attach.

Throughout this text, we will generally use the more verbose version, as they tend to be more descriptive, and leave shorter analogues as exercises for the reader.

# Explaining tmux commands

Now you may notice that attach-session sounds like a pretty long command. It's the same as list-sessions, and there are many others in the lexicon of tmux commands that seem rather verbose.