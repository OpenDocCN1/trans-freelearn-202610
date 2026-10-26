
# Set the status bar background to blue
set-option -g status-bg blue
# Set the status bar text to white
set-option -g status-fg white

# Widen the status-left a bit to fit more
set-option -g status-left-length 25
# Change status-left to be {username@host}
set-option -g status-left "#{whoami}@#H"

# Set the active window background in the status bar
set-window-option -g window-status-current-bg magenta

# Rebind the prefix key
set-option -g prefix C-t

# Add a key binding for reloading our configuration
bind-key C-r source-file ~/.tmux.conf
# Double tapping the prefix jumps to last window
bind-key C-t last-window

# Set escape time to not break vim
set-option -s escape-time 0

# Set the base-index to 1 rather than 0
set-option -g base-index 1
set-window-option -g pane-base-index 1

# Extend the display time to 2 seconds
set-option -g display-time 2000

# Store more history in the buffer than default
set-option -g history-limit 10000

## Key binding and command reference

This is not intended as an exhaustive reference on tmux key bindings and commands. For that, view the tmux manual page by typing the following in your terminal:

```
$ man tmux
```

You can also view more information on tmux by visiting http://man.cx/tmux.

This is, instead, a chapter-by-chapter reference for the tmux keys and commands mentioned in each chapter along with a small description of what they do in the order in which they are mentioned.

This makes it easy to review and recall these key bindings and commands in the same order that they were covered in the chapter without having to wade through all of the text of that chapter.

## Chapter 1 – Jump Right In

In this chapter, we jumped right in and went on a tour of many of the features of tmux. As a result, the following list of commands is widely varied:

- Launch tmux: $ tmux
- Rename a session: $ tmux rename-session {session name}
- Create a new window: <Prefix>, c
- Switch to the last window: <Prefix>, l
- Switch to a window by its index: <Prefix>, {index of window}
- List all key bindings: <Prefix>, ?
- Initiate a search within the key bindings list: Ctrl + s (or / for vi users)
- Close any dialog opened by tmux, such as the key bindings list: q
- Open the choose window interface: <Prefix>, w
- Switch to the next window (by the window index): <Prefix>, n
- Switch to the previous window (by the window index): <Prefix>, p
- Find an open window with the specified search text: <Prefix>, f
- Detach the current tmux session: <Prefix>, d
- List all active sessions: $ tmux list-sessions
- Start tmux and attach a session by name: $ tmux attach-session -t {session-name}
- A shorter way to start tmux and attach a session: $ tmux attach -t {session-name}
- An even shorter way to start tmux and attach a session (only works when there is a single active session): $ tmux attach
- List all active sessions' aliases: $ tmux ls
- List all tmux commands: $ tmux list-commands
- List all tmux commands' aliases: $ tmux lscm

## Chapter 2 – Configuring tmux

In this chapter, we dealt a lot with configuration. Listed here are the key bindings and commands introduced in *Chapter 2, Configuring tmux*, but see the *Configuration reference* section for information on the new configuration items we discussed:

- Set a tmux option: `$ tmux set-option {option to set} {value}`
- Disable the status bar: `$ tmux set-option status off`
- Enable the status bar: `$ tmux set-option status on`
- See whether you are in the Emacs or vi mode: `$ tmux show-options -g | grep key`
- Reload the tmux configuration: `$ tmux source-file ~/.tmux.conf`
- Show all previously displayed messages: `<Prefix>, ~`

## Chapter 3 – Sessions, Windows, and Panes

In *Chapter 3, Sessions, Windows, and Panes*, we learned all about how to move and organize your content within a terminal window using the three core building blocks of tmux: sessions, windows, and panes. As a result, the key commands we learned throughout this chapter relate to manipulating one of those three items. The key commands covered in this chapter are as follows:

- Launch tmux with a named session: `$ tmux new-session -s {session name}`
- Access the switch session interactive dialog: `<Prefix>, s`
- Switch to the next session: `<Prefix>, )`
- Switch to the previous session: `<Prefix>, (`
- Split a pane into two panes (horizontally): `<Prefix>, %`
- Switch the cursor to the other pane: `<Prefix>, o`
- Move the cursor to the pane to the right, left, down, or up: `<Prefix>, right arrow; <Prefix>, left arrow; <Prefix>, down arrow; <Prefix>, up arrow`
- Kill the current pane: `<Prefix>, x`
- Split a pane into two panes (vertically): `<Prefix>, "`
- Resize the current pane: `<Prefix>, Alt + {arrow key}`

- View the contents of a paste buffer by index: `show-buffer -b {index}`
- Delete the last copied item from the paste buffer: `<Prefix>,-`
- Delete items from the paste buffer by index: `delete-buffer -b {index}`
- Clear the tmux history for the current pane: `clear-history`
- Clear the tmux history for a pane by index: `clear-history -t {index}`
- Move the window from one session to another: `move-window` or `<Prefix>,.`
- Link a window between two sessions: `link-window -t {target session}`
- Unlink the window from the current session: `unlink-window`
- Break the current pane from the current window: `<Prefix>,!`
- Break a pane into its own window: `break-pane -s {session}:{window}.{pane}`
- Join the current pane to a target window: `join-pane -t {session}:{window}`
- Join a pane to a target window: `join-pane -s {session}:{window}.{pane} -t {session}:{window}`

## Index

### A

active window
  highlighting 33
advanced paste buffer usage
  about 74
  copied text, deleting from paste buffer 79
  pane contents, capturing in paste buffer 78
  paste buffer history, clearing 79, 80
  paste buffer, loading from file 76, 77
  paste buffer, saving to file 75, 76
  paste buffer, setting directly 77
advanced session, and window usage
  window, linking between sessions 83, 84
  window, jumping in sessions 80-82
  window, moving 82, 83
aggressive-resize setting 98
aliases 21

### B

background color
  modifying, of status bar 29, 30
binding keys
  about 34
  comments, in configuration file 37
  current bindings, viewing 34-36
  multiple commands, chaining to
    single keys 37
  prefix key, binding 38, 39
  URL, for tutorials 35
  without prefix keys 39
break-pane command 85

### C

capture-pane command 78
choose window interface 15
clear-history command 80
Clock mode 64
Command mode 63, 74
command reference 121
commented lines, tmuxinator 109-111
configuration reference 120
Control mode 64
copied text
  deleting, from paste buffer 79
Copy mode
  about 61-63
  cursor, moving 66
  Emacs-style key bindings 66
  entering 64, 65
  jump by search or line 68
  leaving 68
  reference 68
  vi-style key bindings 66
  workflow 64
Ctrl to Caps Lock
  rebinding 35

### D

Default mode 63
default windows
  sessions, launching with 86, 87
delete-buffer command 79

### E

**Emacs mode** 16, 27, 28  
**Emacs-style key bindings, Copy mode** 66

### F

**file**  
  paste buffer, loading from 76, 77  
  paste buffer, saving to 75, 76  
**foreground color**  
  modifying, of status bar 32

### G

**grouped sessions**  
  used, for pairing 100, 101

### H

**handy configuration tips**  
  about 43  
  base index, starting at 1 46  
  display time, lengthening 45, 46  
  double tapping of prefix key, binding to  
    last-window 43  
  escape time, modifying 44  
  history limit, lengthening 44  
**Homebrew**  
  URL 104

### I

**installation, tmuxinator** 107  
**items**  
  selecting, from paste buffer 71

### K

**key binding**  
  about 121  
  help page 15–17  
**keys**  
  unbinding 40

### L

**link-window command** 84  
**load-buffer [path] command** 76

### M

**man page**  
  accessing 47  
  reference, for example 47, 66  
**Mirror mode, wemux** 113  
**modes, tmux**  
  Clock mode 64  
  Command mode 63  
  Control mode 64  
  Copy mode 63  
  Default mode 63  
**modes, wemux**  
  Mirror 113  
  Pair 113  
  Rogue 113  
**mouse modes**  
  enabling 28  
**multiple panes**  
  working with 54–56  
**multiple sessions**  
  working with 53

### O

**option types** 42, 43  
**OS X Pasteboard**  
  tmux, using with 103–105

### P

**Pair mode, wemux** 113  
**pair programming, tmux**  
  about 96  
  connecting to same session, locally 96, 97  
  grouped sessions, using 100, 101  
  Vagrant Cloud, for better security pair  
    programming 98–100  
**pane contents**  
  capturing, in paste buffer 78  
**pane layouts**  
  cycling through 58, 59  
**panes**  
  benefits 52  
  breaking 85  
  joining 85, 86  
  operations 59  
  overview 51
