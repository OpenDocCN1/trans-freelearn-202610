
### Tab completion

There is less complexity to the long commands than it may seem because most of them can be tab-completed. Try going to your command prompt and typing the following:

```
$ tmux list-se
```

Next, hit the Tab key. You should see it fill out to this:

```
$ tmux list-sessions
```

So thankfully, due to tab completion, there is little need to remember these long commands.

Note that tab completion will only work in certain shells with certain configurations, so if the tab completion trick doesn't work, you may want to search the Web and find a way to enable tab completion for tmux.

### Aliases

Most of the commands have an alias, which is a shorter form of each command that can be used. For example, the alias of list-sessions is ls. The alias of new-session is new.

You can see them all readily by running the tmux command list-commands (alias lscm), as used in the following code snippet:

```
$ tmux list-commands
```

This will show you a list of all the tmux commands along with their aliases in parenthesis after the full name.

Throughout this text, we will always use the full form for clarity, but you could just as easily use the alias (or just tab complete of course).

One thing you'll most likely notice is that only the last few lines are visible in your terminal. If you go for your mouse and try to scroll up, that won't work either! How can you view the text that is placed above? We will need to move into something called the Copy mode that has its own chapter later in this text (See Chapter 4, Manipulating Text).

### Renaming windows

Let's say you want to give a more descriptive name to a window. If you had three different windows, each with the nano editor open, seeing nano for each window wouldn't be all that helpful. Thankfully, it's very easy to rename a window. Just switch to the window you'd like to rename. Then <Prefix>, will prompt you for a new name. Let's rename the nano window to masterpiece.

See how the status line has been updated and now shows window 0 with the masterpiece title as shown in the following screenshot. Thankfully, tmux is not smart enough to check the contents of your window; otherwise, we're not sure whether the masterpiece title would make it through.

![](img/5969098f450265ac0c7947efcdbe7889_36_0.png)

### Killing windows

As the last stop on our virtual tour, let's kill a window we no longer need. Switch to window 1 with our find-window trick by entering <Prefix>, f, cat test, Enter or of course we could use the less exciting <Prefix>, l command to move to the last opened window.

Now let's say goodbye to this window. Press <Prefix>, & to kill it. You will receive a prompt to which you have to confirm that you want to kill it.

This is a destructive process, unlike detaching, so be sure anything you care about has been saved.

Once you confirm it, window 1 will be gone. Poor window 1! You will see that now there are only window 0 and window 2 left:

![](img/5969098f450265ac0c7947efcdbe7889_36_1.png)

You will also see that now <Prefix>,f, cat test, Enter no longer loads window 1 but rather says **No windows matching: cat test**. So, window 1 is really no longer with us.

Whenever we create a new window, it will take the lowest available index, which in this case will be 1. So window 1 can rise again, but this time as a new and different window with little memory of its past. We can also renumber windows as we'll see later, so if window 1 being missing is offensive to your sense of aesthetics, fear not, it can be remedied!

### Summary

In this chapter, we got to jump right in and get a whirlwind tour of some of the coolest features in tmux.

Here is a quick summary of the features we covered in this chapter:

- Starting tmux
- Naming and renaming sessions
- The window string and what each chunk means
- Creating new windows
- The prefix key
- Multiple ways to switch back and forth between windows
- Accessing the help documents for available key bindings
- Detaching and attaching sessions
- Renaming and killing windows

In the next chapter, we will look into configuration and many of the ways tmux can be customized to fit your tastes and workflow.

For a reference of all the commands learned in this chapter and every following chapter, you can refer to the *Key binding and command reference* section in *Appendix.*

- Binding a new prefix key
- Binding keys without the prefix key
- Unbinding keys
- Revising the status bar
- Option types
- Handy configuration tips

By the end of this chapter, we will also have built up a sample configuration file that you can use as a starting point for your own personalized configuration.

### Using the set-option command

The main way in which options are set and configured within tmux is through the appropriately named `set-option` command.

This command can be called on its own to set an option temporarily. However, most of the time, we will use it to set options within a tmux configuration file so each time we launch tmux, the options are set the way we would like.

For a taste of using the `set-option` command to set an option temporarily, open a terminal window and launch tmux; alternatively, if you've been following along through Chapter 1, Jump Right In, just type the following command in the window:

```
$ tmux set-option status off
```

Once you do this, the status bar will disappear! This setting is only temporary, so if you leave it as is, end the session, and restart tmux or create a new session, it will not load this setting. In other words, a status bar will appear.

What you have done is that you've told tmux to set the option `status` to a value of `off`. You can use this command to set any tmux option on the fly in this manner. Having no status bar can be a bit unsettling, so let's turn it back on:

```
$ tmux set-option status on
```

If we want these configuration options to persist through tmux restarts and sessions, we need some way to tell tmux what their values should be. We will do that by creating a tmux configuration file.

### Creating a tmux configuration file

In order to keep the settings around after restarting tmux, we will create a configuration file that will include all the information about what options we want to set and to what values.

Like many other Unix-based utilities, configuring tmux is as simple as creating a file with the name .tmux.conf in your home directory.

However, unlike many other configuration files, a tmux configuration file does not contain some kind of specific configuration language for the program, rather it consists of a series of tmux commands that are run, in order, when tmux starts up. Most of the commands we'll see are simple set-option commands similar to the commands we played with in the previous examples. Let's create and start building our configuration file:

```
$ cd ~
$ touch .tmux.conf
```

Open this newly created file in your favorite text editor and we're ready to rock!

> Like with most Unix-based utilities, this file in your personal directory contains the configuration for your user only.
> It is also possible to set the global tmux configuration that will apply to all the users in the system. It follows the same format as that of a personal configuration file but generally lives in /etc/tmux.conf.
> However, since it is likely that not all the users of a system want the same global configuration, it's probably best to just modify your own personal configuration.

### Emacs or vi mode

As we discovered in Chapter 1, Jump Right In, tmux tries to help developers by providing two sets of key bindings, one each for the two most widespread text editors. If you swing one way or the other, this is likely to be one of the first things you'll want to change to make tmux feel right for you.

The default for tmux is to use the Emacs mode keys, so if you are an Emacs user, you may be all set. tmux also tries to help out and might, based on environment variables, switch to one group or the other by default. To check which mode tmux is currently in, run the following command:

```
$ tmux show-options -g | grep key
```
