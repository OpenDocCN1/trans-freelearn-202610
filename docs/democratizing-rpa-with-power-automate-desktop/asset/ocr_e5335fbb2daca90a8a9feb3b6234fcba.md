also has a parameter to define whether to just shut down, restart, hibernate, or set the workstation to sleep.

+   - **Print: Print document/Get default printer/Set default printer:** These work with a text field for the document name, getting or setting the printer.
+   - **Screen: Get screen resolution/Set screen resolution/Control screen saver:** These actions need a connected console session to work with. This session is available when a user is logged in to the workstation and PAD is executed by this user. That means that a user is logged in and has started the PAD console. In unattended scenarios, these actions don’t have any effect. Setting the screen resolution can help to run scenarios in a standardized way, which works with screen coordinates, for example, mouse movements (see next section).
+   - **Miscellaneous other actions: Show desktop/Play sound/Empty recycle bin/Take screenshot:** It is worth mentioning here that **Show desktop** can both show the desktop and restore all windows. Furthermore, **Take screenshot** is useful for collecting and documenting information about a system for the entire desktop or foreground window.

Before we look at another example, let’s first look at the actions for the keyboard and mouse.

# Mouse, keyboard, and clipboard

The actions in this group complete the possibilities from this chapter in terms of simulating user sessions and provide all the actions that a real user can perform, starting from specific mouse sequences to sending shortcuts and predefined key combinations. Here are the details:

+   - **Block Input**: This action has a switch to block or unblock any keystroke or mouse click made by the user. This is useful so that a flow is not disturbed by additional keystrokes or mouse clicks from the user during execution. But be careful. On the one hand, please do not forget to insert an `Unblock` action at the end of the flow in any case. Also,