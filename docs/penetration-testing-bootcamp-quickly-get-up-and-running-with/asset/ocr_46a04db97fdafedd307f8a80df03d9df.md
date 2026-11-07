This option works, definitely not as elegant as shred, but sometimes this has to be done. However, what if you don't want to deal with this again, or you want to limit what is kept in the .bash_history file? Well, let me show you how to change this. From the CLI, you can echo $HISTSIZE to see what your current history size is set for. Mine is currently set for 500 as seen in the following command:

```
```
root@pi-kali:~# echo $HISTSIZE 500
```
```

If I want to change this, I can by updating this value. If I set it to 0, then no history will ever be kept. It's a good idea to change this while running your various tests so that you don't forget to change this later. However, remember to change back after you are finished. To change, just run the export command with the appropriate value. You will need to log out and log back in for this value to take effect:

```
```
root@pi-kali:~# export HISTSIZE=0
root@pi-kali:~# echo $HISTSIZE 0
root@pi-kali:~# more .bash_history
root@pi-kali:~#
```
```

Now, any commands that I run will not show up in that .bash_history file. Perfect for hiding your tracks on that machine!

You also have the option to just edit the log files and remove any lines that you don't want others to see. This can be accomplished using your favorite text editor of choice (vi , nano, vim, and so on). This is basic but effective in removing anything you don't want others to see.

# ClearLogs for Windows

ClearLogs is an event logs cleaner for Microsoft Windows-based systems. It will clear all types of event logs, including security, system, or application. This utility makes it extremely easy for a penetration tester to clear their tracks and saves them a lot of time in the end. To accomplish this task manually would definitely involve some tedious cleaning.