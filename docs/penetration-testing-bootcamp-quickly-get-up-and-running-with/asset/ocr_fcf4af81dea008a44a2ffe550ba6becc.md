# LUKS Nuke blowing up partition

So far, we talked about options to hide our tracks by shredding, removing lines within the files, or deleting files themselves. We also talked about overwriting partitions. These all have their pros and cons, but what happens if you want to accomplish this at a specific time with a specific keyword? Well, then things can get quite tricky. This is where LUKS Nuke comes in. This allows you the ability to send over a signal to a remote host to let it know to nuke itself, sort of a remote kill signal.

LUKS Nuke accomplishes this by working with LVM and disk encryption. When the machine boots, you need to enter a password. If you enter the nuke password, the machine will delete all the saved keys that will basically render all the data inaccessible. This has been implemented into Kali since version 1.06. This is an easy way to make sure your data is protected and lets you nuke without having to fully log in. This is a great safeguard for users, especially those that may have sensitive information on them, such as penetration testers.

![](bbox=[0.12, 0.48, 0.2, 0.58])

For some more great links explaining LUKS Nuke in much more detail, check out the following links:

```
https://www.kali.org/tutorials/nuke-kali-linux-luks/
http://www.zdnet.com/article/developers-mull-adding-data-nuke-to-kali-linux/
```

LUKS Nuke is a great little addition to help protect your personal data as well as any client data you may have.

# Destroying equipment

Sometimes equipment must be destroyed after an engagement. This could be because of a policy that requires it, or it could be just a precaution since sensitive data may reside on some of the systems. Either way, sometimes the need for destroying equipment may come up and be part of the negotiations during the stakeholder meeting.

# Stakeholder-sponsored destruction

Many organizations, especially if they are security conscious, may require any physical equipment that may leave their environment to be physically destroyed. This was most likely talked about during the stakeholder meetings. You, as a responsible penetration tester, need to make sure that this task is followed out to the stakeholders' exact requirement. Chain of custody may be brought up at this point as well.