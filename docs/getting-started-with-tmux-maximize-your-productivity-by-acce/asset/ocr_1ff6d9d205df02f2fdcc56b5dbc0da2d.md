# What you need for this book

A computer running tmux 1.8+ (Unix, Linux, or OS X) which can be downloaded from the following link:

```
http://tmux.sourceforge.net
```

# Who this book is for

The book is intended for software developers, DevOps engineers, and other professionals who make heavy use of the terminal in their daily workflow. Some familiarity with the terminal is useful but no prior experience with tmux or other terminal multiplexers (such as GNU Screen) is required.

# Conventions

In this book, you will find a number of styles of text that distinguish between different kinds of information. Here are some examples of these styles, and an explanation of their meaning.

When we are describing a key to press, it will appear in italics, like this: "Please press the letter b now" Often we will describe a key combination, which means pressing multiple keys at the same time, it will appear like this: Ctrl + b. That means hold down the Control key and press the letter b. We will also see multiple key combinations, to be pressed in series. They will appear separated by a comma and will appear like this: Ctrl + b, c. That means hold down the Ctrl key and press the letter b, then release both keys and press the letter c.

Code words in text, database table names, folder names, filenames, file extensions, pathnames, dummy URLs, user input, and Twitter handles are shown as follows: "Simply run the tmux command show-options with a flag for which set of options you want to view."

A block of configuration code is set as follows:

```
# Set the status bar background to blue
set-option -g status-bg blue
# Set the status bar text to white
set-option -g status-fg white

# Set the active window background in the status bar
set-window-option -g window-status-current-bg magenta
```