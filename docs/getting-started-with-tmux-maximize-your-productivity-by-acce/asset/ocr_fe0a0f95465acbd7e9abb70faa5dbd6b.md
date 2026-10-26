+   7. Now open another terminal window (simulating another user on a remote machine) and run the command it suggested. In my case, the command used was:
+   ```
+   $ vagrant connect --ssh greedy-lamb-6478
+   ```
+   However, the machine name will be different for you since it's randomized. After entering the SSH key password, you'll be connected via SSH to the server! This could be a person on the other side of the world.

+   8. Now run tmux and attach the session of interest:
+   ```
+   $ tmux attach-session -t MyServer
+   ```
+   And you're ready to rock.

Of course, the other person connecting will also have to sign up for Vagrant Cloud in order to connect. This is an incredibly easy way to come up with a relatively safe pair programming environment so two (or more) people can collaborate in real time over the Web without fear of compromising one of the collaborator's environments.

If the worst happens and one of the pair is nefarious, the worst they can do is make a mess of this virtual machine, which was created with only a handful of commands.

# Using grouped sessions for pairing

You may have seen the previous pairing and thought, "Well that's great and seeing what the other person sees in real time is cool, but what if you want each person to be able to have different windows?"

This is where grouped sessions come in. You can have one person create a session and the next to join create a new session, but specify the grouped session as well. This way, each person has their own control over the windows in a session, but anything they do in each session is shared. This allows one person to be in one window and the other in another.

They can switch to each other's sessions at will, but otherwise don't need every command and keystroke to enact on both windows. To create a grouped session, one person must initiate the session as we discussed previously. If you still have the MyServer session running, you can use that.