Similar to the following screenshot, you should see the status bar is now blue!

![](bbox=[0.212, 0.173, 0.788, 0.33])

# Changing the foreground color of the status bar

Now the blue status bar looks nice! However, the text is a bit difficult to read; there's not much of a contrast between the background and the text. Let's fix that!

Open the .tmux.conf file again, this time adding the second line:

```
set-option -g status-bg blue
set-option -g status-fg white
```

Reload the configuration with the source-file command using the method we discussed previously. Now, as can be seen in the following screenshot, the text on the status bar should be much more readable!

![](bbox=[0.212, 0.58, 0.788, 0.735])