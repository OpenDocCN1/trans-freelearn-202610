# Multiple panes

Now let's break this window into two panes. Type <Prefix>, % (split the window vertically) and see how the window is now divided down the middle by vertical bars; we now have another command prompt. This is the second pane, as can be seen in the following screenshot:

![](bbox=[0.19, 0.25, 0.803, 0.4])

Now we have two sessions, three windows, and four panes. One session, one window, and one pane make up the play session (which is no longer visible but humming along in the background), while one session, two windows, and three panes make up the work session we saw in the previous screenshot.

Let's play around with panes a bit. First, try typing in some text. You'll see the cursor is in the right pane. To get it to the left pane, simply press <Prefix>, o (o for other). Now your cursor is in the other pane. Type some stuff to verify which pane the cursor is in.

There is actually a better way to tell which pane has the cursor than typing in random stuff. It may be tough to tell from the previous screenshot; however, when a window is split into two vertically, the top half of the vertical bars that split the two windows will be colored when the cursor is in the left pane, and the bottom half will be colored when the cursor is in the right pane. In the previous screenshot, the bottom half is colored in green to indicate that the cursor is in the right pane. When there are more than two panes, tmux tries different schemes to let us know which pane is active, as we soon shall see.

You can also use <Prefix> and the right arrow key, <Prefix> and the left arrow key, <Prefix> and the down arrow key, or <Prefix> and the up arrow key to move the cursor to the pane to the right, left, down, or up, which is a bit easier to remember and also more useful when we have a more complex pane structure.