The output of this command is shown in the following screenshot:

![](bbox=[0.176, 0.173, 0.824, 0.403])

Let's switch back to our other session with <Prefix>, s and then highlight and select the other session we want, as shown here:

![](bbox=[0.202, 0.467, 0.798, 0.597])

Jump onto the other session (Advanced, as shown in the preceding screenshot). Now if we do the same thing again, using <Prefix>, s and switch back to the Another session, we will arrive back in Another, in window 2 that is running top. This is because tmux will take us back to the last window we were viewing in an active session.

However, what if we wanted to move directly from one session to a specific window in another? Surely, we should not have to switch to the session and then select the window. Well, we can directly choose a window within a session, and tmux actually makes this a breeze.