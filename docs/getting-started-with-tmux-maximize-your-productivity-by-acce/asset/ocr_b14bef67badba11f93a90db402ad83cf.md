Sure enough, now window 1 is active and everything is present, just as we left it. Now we typed some silly commands here, but it could just as well have been an active running process here, such as unit tests, code linting, or top. Any such process would run in the background in tmux without an issue.

![](bbox=[0.23, 0.29, 0.28, 0.35])

> This is one of the most powerful features of tmux.
> 
> In the traditional world, to have a long-running process in a terminal window and get some stuff done in a terminal, you would need two different terminal windows open; if you accidentally close one, the work done in that window will be gone.
> 
> tmux allows you to keep just one terminal window open, and this window can have a multitude of different windows within it, closing all the different running processes. Closing this terminal window won't terminate the running processes; tmux will continue humming along in the background with all of the programs running behind the scenes.

# Help on key bindings

Now a keen observer may notice that the trick of entering the window number will only work for the first 10 windows. This is because once you get into double digits, tmux won't be able to tell when you're done entering the number. If this trick of using the prefix key plus the number only works for the first 10 windows (windows 0 to 9), how will we select a window beyond 10?

Thankfully, tmux gives us many powerful ways to move between windows. One of my favorites is the choose window interface.

However, oh gee! This is embarrassing. Your author seems to have entirely forgotten the key combination to access the choose window interface. Don't fear though; tmux has a nice built-in way to access all of the key bindings. So let's use it!

Press <Prefix>, ? to see your screen change to show a list with bind-key to the left, the key binding in the middle, and the command it runs to the right. You can use your arrow keys to scroll up and down, but there are a lot of entries there!

Thankfully, there is a quicker way to get to the item you want without scrolling forever.