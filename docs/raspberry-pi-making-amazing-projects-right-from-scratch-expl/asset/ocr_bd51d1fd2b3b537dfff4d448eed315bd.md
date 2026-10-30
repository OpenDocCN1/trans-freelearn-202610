When you hit Enter, you'll see the following screen:

![](bbox=[0.174, 0.172, 0.825, 0.491])

You can also choose to overclock your device. This is a way for you to get higher performance from your system. However, there is a risk that you can end up with a system that has reliability problems.

Once you are done and are back at the main configuration menu, hit the Tab key until you are positioned over the <Finish> selection, then hit Enter. Then, hit Enter again so that you can reboot your Raspberry Pi. This time, when you log in, you will not see any configuration selections. However, if you ever want to change your configuration choices, you can run the configuration tool by typing in raspi-config at the command prompt.

# Adding a remote graphical user interface

For some steps in your robot build, you will need a graphical look at your system. You can get this on your Raspberry Pi using an application called vncserver. You'll need to install a version of this on your Raspberry Pi by typing in sudo apt-get install tightvncserver in a terminal window on your Raspberry Pi.