
The most versatile of the bunch is a key binding that will essentially cycle through to a different preset pane layout each time you press a key combination. This key combination is <Prefix>, Space bar (cycle through pane layouts).

Try it out; press <Prefix>, Space bar, and you will see your panes move around. The content stays in them, so any programs in them would keep running fine, but they get rearranged and resized in different ways each time you use that key combination.

If you look at the status bar, you will see the name of the layout. There are five different preset layouts that you can cycle through; these are even-horizontal, even-vertical, main-horizontal, main-vertical, and tiled.

Until you know what you like for a given use case, cycling through them is a pretty good way to see what the possibilities are.

Once you are comfortable enough with these layouts, you can switch to them directly, without cycling through all of them, by pressing <Prefix>, then Meta and the index of the preset layout in the previous list. So, for example, you can use <Prefix>, Meta + 1 to switch to the even-horizontal view, <Prefix>, Meta + 2 to switch to the even-vertical view, and so on.

## Other pane operations

There are far too many pane operations, so we'll not be able to cover all of them exhaustively in this book; for most that we'll not discuss, we implore you to dig into the tmux man page:

```
$ man tmux
```

Alternatively, you can refer to them on the Web at http://man.cx/tmux.

Now that you understand panes, the man page should be very manageable and understandable on the topic of panes.

You can do things like breaking one pane out into its own window; rotating the panes; swapping one pane with another; moving a pane to another window; and arranging all the panes horizontally, vertically, tiled, and much more.

There are many more commands to resize panes, many of which don't even have keyboard shortcuts because they're not used all that much. Of course, you could add your own, and if you read Chapter 2, Configuring tmux, you should have all the tools in your toolbox to do so.

## Summary

In this chapter, we learned a lot about sessions, windows, and panes in tmux. We learned how each of these fit into the tmux hierarchy, and we played around with them to solidify some of the concepts. We created multiple sessions, split windows into multiple panes, moved them around, and learned different ways to switch the cursor from one pane to another.

In the next chapter, we will move on to text manipulation and learn about how we can scroll back through the window history, how we can copy text from the window history into a paste buffer, and how we can paste that content.

## Manipulating Text

There are two important components of tmux that we are yet to discuss in more detail, namely, **Copy mode** and **paste buffers**.

Copy mode is a mode in tmux that we can switch to; it allows us to select the text that already appears on the screen and copy it. Also, it allows us to move our cursor anywhere on the screen, even to places that have moved off the screen.

When an item is copied from Copy mode, it ends up in the paste buffer. As its name implies, this is a buffer that exists to hold anything that is copied so it can be pasted later.

Here is a quick summary of the features we will cover in this chapter:

- Window history and how tmux handles text that has moved off screen
- Explaining the two tmux modes
  - Scrolling up through the Window history
  - Jumping by search or line
  - Copying text into the paste buffer
- Interacting with the paste buffer
  - Pasting the last copied item
  - Viewing the whole paste buffer stack
  - Choosing an item to paste interactively

By the end of this chapter, we will know all about working with text from within tmux.

## Explaining the Window history

One thing you may have noticed in the previous chapters is that commands with too much text output appear to get cut off. You can see the tail end, but the rest seems to go above the window. If you try scrolling up, it will not work. What is going on here?

The start of a command is not lost forever. It still exists; it has simply scrolled off the screen.

In order to work its magic and keep everything in a single terminal window, tmux has to hide all of the text that won't fit in the currently viewed pane. It keeps all of this text stored in something called **Window history**.

Think of it like pages in a physical book. A book contains far more text than you can see at any given point in time, but to make it manageable, all of that text is on pages that aren't visible at the same time. The one page that is visible is the one you have opened.

Similarly, tmux maintains all of the text that didn't fit on the pane you are currently viewing and tucks it away in the Window history.

Now, of course, tmux doesn't need to stack its pages in a way that one is on top of the other, like with a traditional book. So, they're more laid out, one above the next, for as long as the history is configured.

The following illustration helps visualize it:

![](img/5969098f450265ac0c7947efcdbe7889_76_0.png)

## Manipulating Text

- **Clock mode:** This mode shows the current time and is more of a novelty / utility than an actual mode, like the rest. It can be accessed by `<Prefix>, t`.
- **Control mode:** This mode allows third-party applications to interact with tmux through a text-only protocol.

In addition, this is where we start to get to the text manipulation goodies within tmux. When we drop into Copy mode, we can scroll back to look back through the Window history, and we can also access some cool text manipulation tools.

Let's walk through a sample workflow with Copy mode.

## A sample workflow with Copy mode workflow

If you have followed us from the beginning of the chapter, you should have a tmux session started. Let's first rerun the long-running command we used previously to list the available key bindings:

```
```bash
$ tmux list-keys
```
```

Just as we saw the last time we ran this command, only the last few lines are made visible in our current pane. However, now we are armed with a bit of knowledge about the Window history and can likely surmise that the remaining lines are there but have scrolled off the screen. How can we scroll up and see what that text was? Copy mode!

## Entering Copy mode

Let's enter Copy mode so we can scroll back and see the rest of it, which is stored in our Window history. Press `<Prefix>, [` to switch back to Copy mode.

The first thing you'll notice is a new interface element added to our screen. There will be a new box in the upper-right corner of the terminal screen that shows **[0/76]**, as shown in the following screenshot:

![](img/5969098f450265ac0c7947efcdbe7889_78_0.png)

This shows us both the number of lines in the history (76) and the current line our cursor is currently on (0).

Note that these lines are in reverse chronological order, so 0 stands for our currently viewed pane, 1 is a line above it, 2 is a line above line 1, and 76 is way up at the top of our Window history. This helps give context as to where in the history we are as we scroll up through it.

Going back to our Window history diagram, the lines can be thought of as shown in the following diagram:

![](img/5969098f450265ac0c7947efcdbe7889_79_0.png)

The first number in the [0/76] bit will show you the index of the lowest visible line on the current viewpoint. This can be a bit confusing at times as it seems, when you scroll up, that the first number doesn't start going up until you scroll your cursor past the current pane; however, it makes a bit more sense when you scroll down and bring the lines into the current view one by one.

## Moving the cursor around

Now that you're in Copy mode, you can move the cursor up, down, left, or right, one character at a time. This is, rather intuitively, the up arrow, down arrow, left arrow, or right arrow keys or Ctrl + p, Ctrl + n, Ctrl + b, Ctrl + f (k, j, h, l). Try moving the cursor around a bit in Copy mode.

## Emacs or vi style key bindings for Copy mode

Actually, there are two main ways to bind keys in tmux for Copy mode: Emacs-style key bindings or vi-style key bindings.

This is configurable using the mode-keys option. We will be primarily covering the default (Emacs-style) key bindings and will include the vi key binding in parenthesis following it. However, note that as we saw in Chapter 1, Jump Right In, tmux is smart and may alter the defaults based on your environment variables. So, while Emacs is the usual default, you may, without any configuration, be set to the vi mode with tmux out of the box, based on another configuration.

If you are a vi user, know that you can set up vi-style mode keys, and if you do, then the keys to browse Copy mode are very much like browsing vi's normal mode.

The Emacs key bindings for Copy mode can be viewed with the following command:

```
tmux list-keys -t emacs-copy
```

The vi key bindings for Copy mode can be viewed with the following command:

```
tmux list-keys -t vi-copy
```

Alternatively, you can look at the man pages under mode-keys for more details on configuring and which key does what (http://man.cx/tmux or man tmux from a terminal).

![](img/5969098f450265ac0c7947efcdbe7889_80_0.png)

Now let's scroll up and see what's left behind.

## Scrolling through the Window history

To scroll up a page, you can use Page Up or Meta + v (Ctrl + b) and you'll see that each time you press it, the number in the upper-right corner changes from [0/76] to match wherever you currently are.
