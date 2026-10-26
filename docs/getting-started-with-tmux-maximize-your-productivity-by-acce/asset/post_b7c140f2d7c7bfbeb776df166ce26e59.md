
## Modifying the background color of the status bar

Let's add an item to our configuration to change the color of the status bar. As we saw by jumping in, the default color of the status bar is a shade of green. Note that there happens to be a shade of green in our configuration as well, but it is likely a different color on your terminal unless you are using the same color scheme as we are.

> The authors are using a color scheme called Solarized. It is a color scheme that includes both dark and light variants and is optimized for the terminal and readability. It was developed by Ethan Schoonover and has gained quite a following in the tech community for its precision, symmetry, and because it makes just about any code look downright pretty. For more information, visit http://ethanschoonover.com/solarized.

The color scheme of our current terminal is as shown in the following screenshot:

![](img/5969098f450265ac0c7947efcdbe7889_43_0.png)

Eeek! That's pretty ugly with our current color scheme.

Say, we want it to be a nicer blue color. Jump into your favorite text editor and edit ~/.tmux.conf to have the following line:

```
set-option -g status-bg blue
```

This line specifies that we want the status bar to be blue rather than its default, which on our system happened to be green.

You should see your status bar change to blue immediately (again, depending on the color capabilities of your shell and terminal program).

Notice that all we did was take the same text we put in our configuration file, type it after tmux, and we were good to go.

Recall that any command run in the shell like this is only temporary, so we still want our configuration file; however, this is a nice way to quickly test that a given command is going to work as we'd like.

## Reloading the configuration

Now we have a shiny new configuration file with our new command to change the color of the status bar, but this configuration is useless until we reload the configuration to tell tmux to load with the commands from our new file.

After you save your configuration file, it's ready for action, but look, your status bar is still green! How do we tell tmux that it should go fetch the latest configuration and reload the windows to use that configuration? There are two ways to tell tmux to reload the configuration.

The first is to kill all the sessions entirely and start tmux fresh. This is not a very appealing option because it entails closing all of our tmux sessions, which in turn means closing all the currently open and running programs. We will then have to reinitiate all of our tmux sessions and windows and restart all of our programs to get back to where we were before just to reload the configuration. It works but is a rather messy solution overall. Generally, it is rather nice that tmux loads the new configuration file when it is restarted because this means any subsequent tmux initiation will use our latest configuration, which makes sense. However, it forces us to close all the running applications to load it, which is not ideal.

The second, more common and useful option is to use the tmux source-file command. This command will tell tmux to go fetch the latest configuration and load it immediately. In this case, there is no need to leave tmux!

Get to a command-line prompt within tmux (if you followed along Chapter 1, Jump Right In, you will know that you can just hit <Prefix>, 2 to bring up the command prompt we left there). If you have no window currently open, just open a new window within tmux (<Prefix>, c) and it'll start with a command prompt by default. Now type the following command in the prompt and hit Enter:

```
$ tmux source-file ~/.tmux.conf
```

Similar to the following screenshot, you should see the status bar is now blue!

![](img/5969098f450265ac0c7947efcdbe7889_46_0.png)

## Changing the foreground color of the status bar

Now the blue status bar looks nice! However, the text is a bit difficult to read; there's not much of a contrast between the background and the text. Let's fix that!

Open the .tmux.conf file again, this time adding the second line:

```
set-option -g status-bg blue
set-option -g status-fg white
```

Reload the configuration with the source-file command using the method we discussed previously. Now, as can be seen in the following screenshot, the text on the status bar should be much more readable!

![](img/5969098f450265ac0c7947efcdbe7889_46_1.png)

## Highlighting the active window

Let's make it a bit easier to see which is our currently active window. The asterisk works, but it could be a bit easier since we have these colors.

Open up the .tmux.conf file again, and this time add the third line:

```
set-option -g status-bg blue
set-option -g status-fg white
set-window-option -g window-status-current-bg magenta
```

Here we see our first window option. The first two options were global. This means that in all windows the status bar will always have a blue background.

This new line is saying that, for each window, if it is the current window, set its background to magenta. Reload the configuration again with tmux source-file ~/.tmux.conf to see that in action, as shown in the following screenshot:

![](img/5969098f450265ac0c7947efcdbe7889_47_0.png)

Change to the other window and watch the magenta highlight follow.

You may be noticing that each time, we're typing a lot to reload this configuration, going through the whole tmux source-file ~/.tmux.conf command. Surely, there must be a better way!

The bad news is that there is not a key combination out of the box in tmux to reload the configuration. The good news is that we can bind our own key to make this task easier!

Thankfully, by looking through the list of key bindings, it appears that Ctrl + r is not currently bound to any tmux command. So it's fair game. All the keys that start with C- in the key binding screen mean that you need to press the Ctrl key. So, C-r is used in the same way as Ctrl + r.

![](img/5969098f450265ac0c7947efcdbe7889_49_0.png)

This will be rather convenient as the default prefix key also uses the Ctrl key. So assuming the current default prefix key of Ctrl + b, we will be able to hold down Ctrl and tap B then R to reload the configuration.

## Rebinding Ctrl to Caps Lock

![](img/5969098f450265ac0c7947efcdbe7889_49_1.png)

One of my favorite programming productivity tips of all time is to rebind Caps Lock to Ctrl on a system-wide scale. This is easily done on most computers at the operating system level and even on some more capable keyboards.

The majority of computer users seldom use Caps Lock, yet its place on the home row makes it a convenient key to press. Since Ctrl is used often by command-line programmers and its place on the keyboard is rather difficult to press, swapping the two can be very helpful. By swapping them, in the unlikely event that you ever do need to use Caps Lock, it is still accessible by the old Ctrl key on your keyboard.

The author is a huge fan of Emacs that makes heavy use of the Ctrl key, and tmux is another program that does as well.

This is by no means a requirement, but if you find yourself using the Ctrl key regularly, consider making use of this trick!

On a Mac, it is very easy and requires no additional software. It's a bit trickier on some other systems, but possible everywhere.

For detailed tutorials visit http://vq.io/rebindcaps.

> Notice how we also added some spaces between some logical groupings here as well. Thankfully, the tmux configuration ignores whitespace so we can space them apart however we'd like.

## Binding a new prefix key

We can also rebind the prefix key to make it something more convenient or set it up in a way that it doesn't get into a conflict with other programs.

Some of you might be from a background where you use GNU screen, another terminal multiplexer that uses Ctrl + a as its prefix key. So instead of learning the tmux key, you might want to simply rebind Ctrl + a as your tmux prefix key.

Alternatively, you might be a frequent user of Emacs, nano, or another program that makes extensive use of Ctrl + b to move the point back one character. In that case, Ctrl + b is not a convenient key to have rebound globally.

Users in this category may choose to rebind the prefix to something less used or less important in Emacs or other programs, such as Ctrl + t. This also conveniently helps to provide guidance on the key (t for tmux!).

Whatever your motives, it is possible to rebind the prefix key. So, as an exercise, let's rebind it to Ctrl + t. If this makes you uncomfortable, feel free to ignore it in your own configuration. However, rebinding the prefix key is a bit different than rebinding any other key. You might think that the following in your .tmux.conf file would work, but that is not the case:

```
bind-key C-t prefix
```

Recall that all of the key bindings within tmux first require the prefix key to work. So this code snippet, in plain English, is essentially saying, "After you have pressed the prefix key, press Ctrl and t as the prefix key." It is not saying, "Here is the new prefix key, please use it."

So in order to set the prefix key, add the following lines to our ever-growing .tmux.conf:

```
# Set the status bar background to blue
set-option -g status-bg blue
# Set the status bar text to white
set-option -g status-fg white
```
