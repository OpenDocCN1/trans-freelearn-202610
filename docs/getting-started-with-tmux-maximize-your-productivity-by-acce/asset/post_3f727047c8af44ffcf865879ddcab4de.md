
Notice the -n option? This allows you to press the bound key without first pressing the prefix key. So you could simply press F1 with no prefix and immediately switch to window 1!

This sounds like a really cool trick (and it generally is!), until you find even a single program that you want to run within tmux that expects the key F1. Then, since you've captured F1 globally for tmux, it's not possible to use that keystroke within that or any other program because tmux captures it, uses it for the purpose you've specified, and never passes it along to the program. Again, you can use a trick such as binding `<Prefix>, F1` to send F1 through to the underlying windows; however, for most users, it's probably not a great practice to get in the habit of globally overriding keys.

So, the most likely outcome here is that you can create some cool global shortcuts for something like that so they work for a while until you find an exception. You will then unbind them and end up having to convince your muscle memory to forget what it learned.

## Unbinding keys

Now let's say there is a key binding that you want to remove for some reason. This is very easy; just add an unbind directive to your `.tmux.conf` file.

Say we wanted to unbind 0 from its default action of selecting window 0 for some reason. To do this, simply add the following code snippet to your configuration:

```
unbind 0
```

Note that there is no need to explicitly unbind a key before binding it to something else; every key can only have one binding. So, we find it rather rare to use this bit of utility, but it's nice to know it's there when you need it.

## Status bar revisited

Last time we touched the status bar, we altered its colors, but we did not do anything to change its content. tmux allows us to change a lot of different aspects of the content of the status bar. We won't have an in-depth look at every possible configuration, but will explain the concept and show one example.

So, the status bar has three chunks basically:

- **status-left**: This represents the stuff on the status bar on the left-hand side, including the current session

So let's say we wanted to change the value of status-left so it looks similar to {username@host} where the username and host are filled in automatically; we can easily do that. Simply add the following lines to `.tmux.conf` and reload the configuration (note that we also widened the status because the default is 10 and it was getting cut off):

```
set-option -g status-left-length 25
set-option -g status-left "#(whoami)@#H"
```

This will fill in the status bar with {victor@minerva.local} for me because it executes the whoami command, puts the output of that command into the string before @, and fills in the hostname after it.

## Option types

Thus far, all of the options we have set had the -g flag. This means that the option applies globally. However, there are three types of options: server options, session options, and window options.

There are also multiple flags that indicate how a given option should affect or be affected. This sounds complex, but it's rather simple.

A server option will apply to any client that connects to tmux. Under the hood, when you type and run the tmux command the first time, tmux creates a server; then, it creates your client and connects to the server it started. This is the magic that allows you to detach your session, reattach it later, and have everything running as you left it because the tmux server keeps chugging in the background. So an option specified with the -s flag will apply to the server, meaning it will affect any clients that attach to it.

A session option will apply to the current session. This means that two clients could connect to the same server and each could have slightly different session options. This is in contrast with the server option, where every client has that same option whether they want it or not. Session options do not have a flag; they are set by leaving off the flag, as they are the default.

Then, there are window options, which apply only to one window. To set a window option, specify it with the -w flag.

When it comes to configuring options, they cascade down. So there are global options specified with -g that will serve as the master set. Then, any option further down the chain will inherit from the global settings or may override the global setting with its own.

To do this, simply add a bind-key entry to your `.tmux.conf` file and bind the same key combination as your prefix key to the last-window command. For example, if you followed the preceding steps and set the prefix key to Ctrl + t, the following line in your `.tmux.conf` file is appropriate:

```
# Double tapping the prefix jumps to last window
bind-key C-t last-window
```

Now moving on to the last window is as easy as holding down Ctrl and tapping t twice.

## Changing the escape time

I have heard from some vim users that tmux adds a short, almost imperceptible delay between the commands that can cause funny behavior when running vim inside tmux.

In tmux terms, this delay is called escape-time. By default, it's set to 500 ms. To fix things with vim, set the escape-time to 0 by adding the following line to your configuration file:

```
set-option -s escape-time 0
```

Note that this option obviously exists for a reason, so tread carefully when doing this as you may come across unexpected behavior. However, we've heard from a lot of vi users that make this change and they never look back. Also note, as discussed previously, this has the -s flag so it is a session-specific option, which means that if someone else connects to the same server, they could have a different value for escape-time.

## Lengthening the history limit

tmux only has a limited amount of scrollback history. The scrollback history is the number of lines it retains in memory that have scrolled off the screen and which are accessible in Copy mode (see Chapter 4, Manipulating Text).

However, the default history-limit for tmux is 2,000, which can be pretty low for many use cases. In order to crank it up, add the following line to your configuration file:

```
set-option -g history-limit 10000
```

Note that by increasing this history limit, you are also increasing the memory footprint of tmux. So some tweaking may be necessary if it's set too high, particularly if you tend to have a lot of open windows and panes.

## Lengthening the display time

Some commands will cause tmux to display a message to you, replacing the status bar with the text it displays for you. The time a message is displayed is called display-time.

Out of the box, the value of display-time for tmux is rather low. Try doing a search for text in another window with some text that you know doesn't exist. For example, try `<Prefix>, f`, then type any search string that you know doesn't exist in any other window, for example, potato.

You will see the text `No windows matching: potato` appear in place of the status bar and then disappear in the blink of an eye:

![](img/5969098f450265ac0c7947efcdbe7889_59_0.png)

It will go in a flash because it only stays on the screen for the duration of the current display-time, which is only 750 ms by default. It's probably a good idea to crank the value of display-time up a bit so messages stay visible a bit longer, at least until you are more comfortable with tmux. To do so, add the following line to your configuration file, changing the duration to 2 seconds:

```
set-option -g display-time 2000
```

For a 2-second duration, we set display-time to 2000 because the display time is set in milliseconds.

## Summary

So let's put it all together. Refer to the *Appendix* to see the final product, the `.tmux.conf` file we have built throughout this chapter.

Throughout this chapter, we learned a lot about configuring tmux. Starting with a blank `.tmux.conf` configuration file, we built up a much larger configuration changing many aspects of our tmux experience.

We altered the look and feel by modifying the status bar, and we learned how to bind keys, including the prefix key. We showed you how to bind multiple commands to a single key binding and how to unbind keys. We learned how to keep the file manageable and well documented by adding comments, and how to bind keys without needing the prefix key. We learned about the different types of options that can be set in the configuration file and some handy configuration tips that probably wouldn't be readily obvious to most readers.

By the end of the chapter, we have built up a very rich sample configuration file, which can be a great starting point for your own personal customization. We learned a bit about how to browse the man page for an extensive list of the available options.

We hope you treat this `.tmux.conf` file as a living document and update it with other neat things you may find and remove or change any bits that exist in it now. Personalize it to suit your needs; after all that's what it's for!

Now that we've learned all about configuration, in the next chapter, we will move on to discuss one of the major fundamentals of tmux: the concept of sessions, windows, and panes.
