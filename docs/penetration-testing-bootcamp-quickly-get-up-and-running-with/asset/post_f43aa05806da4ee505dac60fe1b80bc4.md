
Running **ClearLogs** on the desktop allows for a quick and easy deletion process. Note though that this command needs to be run as administrator in order to work correctly. Typically, I right-click on the icon and select **Run as administrator** as shown in the following screenshot:

![](img/095df2cd1930baecbcecaa51ee3417aa_220_0.png)

In the next screenshot, you can see that the event logs are full of information:

![](img/095df2cd1930baecbcecaa51ee3417aa_220_1.png)

The following screen appears once the application loads. Notice there is not much to do here, except the **Clear All System Logs** or **Cancel** buttons. In our case, we want to clear all logs, so hit the **Clear All System Logs** button:

![](img/095df2cd1930baecbcecaa51ee3417aa_221_0.png)

You will start to see a play-by-play status of all logs that are being deleted:

![](img/095df2cd1930baecbcecaa51ee3417aa_221_1.png)

In the following screenshot, you can see the event logs before and after using ClearLogs. It has removed all the relevant information quickly:

![](img/095df2cd1930baecbcecaa51ee3417aa_222_0.png)

## Using DD and mkfs to clear drives

The one important thing to note is that deleting a file doesn't actually delete a file. We talked about this earlier with shred. So, if you want to truly delete and need another option to shred, DD is your answer. This utility allows us to copy files to a destination of our choice. However, we can use this utility to write random data as well to any partition. In the following command, I will write random characters to the partition of my choice. This will make sure that the data that was there was actually overwritten as opposed to just having the pointers removed:

```
dd if=/dev/urandom of=/dev/sda2
```

The main difference between the preceding command and the command we used to copy partitions is that the input file is an actual file as opposed to /dev/urandom. Along the same lines as dd, we can also just reformat the partition with a new filesystem as well using the mkfs.ext4. To accomplish this on the preceding partition, the similar command will be as follows:

```
mkfs.ext4 /dev/sda2
```

There are many options to hide your tracks using the command line. Make sure you understand the pros and cons of each to help you make an informed decision.

## LUKS Nuke blowing up partition

So far, we talked about options to hide our tracks by shredding, removing lines within the files, or deleting files themselves. We also talked about overwriting partitions. These all have their pros and cons, but what happens if you want to accomplish this at a specific time with a specific keyword? Well, then things can get quite tricky. This is where LUKS Nuke comes in. This allows you the ability to send over a signal to a remote host to let it know to nuke itself, sort of a remote kill signal.

LUKS Nuke accomplishes this by working with LVM and disk encryption. When the machine boots, you need to enter a password. If you enter the nuke password, the machine will delete all the saved keys that will basically render all the data inaccessible. This has been implemented into Kali since version 1.06. This is an easy way to make sure your data is protected and lets you nuke without having to fully log in. This is a great safeguard for users, especially those that may have sensitive information on them, such as penetration testers.

![](img/095df2cd1930baecbcecaa51ee3417aa_223_0.png)

For some more great links explaining LUKS Nuke in much more detail, check out the following links:

```
https://www.kali.org/tutorials/nuke-kali-linux-luks/
http://www.zdnet.com/article/developers-mull-adding-data-nuke-to-kali-linux/
```

LUKS Nuke is a great little addition to help protect your personal data as well as any client data you may have.

## Destroying equipment

Sometimes equipment must be destroyed after an engagement. This could be because of a policy that requires it, or it could be just a precaution since sensitive data may reside on some of the systems. Either way, sometimes the need for destroying equipment may come up and be part of the negotiations during the stakeholder meeting.

## Stakeholder-sponsored destruction

Many organizations, especially if they are security conscious, may require any physical equipment that may leave their environment to be physically destroyed. This was most likely talked about during the stakeholder meetings. You, as a responsible penetration tester, need to make sure that this task is followed out to the stakeholders' exact requirement. Chain of custody may be brought up at this point as well.

A chain of custody is a series of documents that show secure possession and secure transfer of any IT hardware in its most simple form. Any time a piece of hardware is taken offsite for destruction, the whole process from point A to point B needs to be fully documented. Some destruction companies will even record the whole process to make sure that the company can also see the entire process.

If you are working for a company that requires this type of activity to happen, it's not a bad idea to create a form to present to them during the initial meetings. This way, they will understand the process and see that you are taking this sensitive data very seriously.

The following is an example of a chain of custody form for hardware destruction. Feel free to use and modify anything that pertains to specific customer requirements:

![](img/095df2cd1930baecbcecaa51ee3417aa_224_0.png)

## Destruction by the penetration tester

Even though the company you are performing the penetration test against may not have a specific policy, it may be in your best interest to destroy any equipment that was onsite at a customer location just to be safe. We talked about a lot of software ways to destroy the data but physical may be an option as well.

The most important items to think about destroying would be the places where data may reside. This would be any non-volatile storage such as a flash disk, SSD, and hard drive. If these devices get into the wrong hands, the data that is contained on these devices may be stolen if the devices are not destroyed correctly.

How we destroy the device will ultimately depend on the type of device that it is. For any device, you can have them shredded. This will make them into a big pile of shavings similar to the following image. This will work with pretty much any device:

![](img/095df2cd1930baecbcecaa51ee3417aa_225_0.png)

You can also drill holes into the drive. The following is a hard drive from my lab that I destroyed by drilling holes into it. Shredding it would have been much more fun, but I didn't have access to the right equipment:

![](img/095df2cd1930baecbcecaa51ee3417aa_226_0.png)

Finally, since I have a couple of Raspberry Pis that I use for various functions, I tend to physically destroy the microSD cards after each customer engagement. This way, I can assure the client that I do not have any of their data and I am not taking it with me. Here is one of my microSD card that I shattered into many pieces prior to me leaving one of my customer engagements:

![](img/095df2cd1930baecbcecaa51ee3417aa_226_1.png)

## Summary

We are now done with the chapter, so let us recap. Since we completed all the penetration tests that were required for our engagement, we need to make sure that we clean up any mess we made, just like our mothers have taught us. It is important as professionals to make sure that we leave the environment the same as it was before we started and leave no tracks behind. To accomplish this, we talked about various items that we should be certain get done, basically, a cleaning-up list. To accomplish this, we talked about utilities to hide our tracks, for example, shred, dd, ClearLogs, and cleanenv. They all have various specialties on where and when to use and all of them should be in the penetration tester's toolbox.

Next, since physical destruction may be necessary depending on the stakeholders' requirement and/or data sensitivity level, we talked about the various ways we can physically destroy our hardware. However, in some cases we may need to engage a third-party hardware destruction company, so we talked about the chain of custody and I even included a form that can be used for making sure everything is completed as required by the appropriate parties.

In the next chapter, I will talk about the last topic in this book: writing up the penetration testing report. This will be the culmination of all the chapters and tests that we ran. All information that we have collected will be put into a very detailed report and sent over to the stakeholders that have funded the penetration test.

I will go over the various items to include in the report as well as my preferred syntax and layout, to name just a few. This report will be your deliverable, so it needs to be the best it can be to reflect how great a job you did. We definitely want to make sure it contains great information so that we can show our value as penetration testers to the stakeholders.
