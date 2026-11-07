# Command and control server SSH setup

Setup on the command and control host for the SSH is not a complicated process as well and is very similar to our on-premises box. The only real difference is the fact that the Kali image for the Raspberry Pi has some different defaults actions/settings compared to the default Kali image:

+   1. Similar to the Raspberry Pi setup, we need to copy old keys and create new ones. Unlike the Raspberry Pi, the keys are not shared and should already be unique; however, I like to recreate them just in case (force of habit and a best practice for security):

![](bbox=[0.16, 0.33, 0.84, 0.58])