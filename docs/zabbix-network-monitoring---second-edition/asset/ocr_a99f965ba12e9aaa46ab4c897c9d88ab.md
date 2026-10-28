# Acting upon Monitored
Conditions

Now that we know more about triggers, let's see what we can do when they fire. Just seeing some problem on the frontend is not enough; we probably want to send notifications using e-mail or SMS, or maybe even attempt to remedy the problem automatically.

Actions make sure something is done about a trigger firing. Let's try to send notifications and automatically execute commands.

In this chapter, we will:

+   - Learn how to limit conditions when alerts are sent
+   - Send notifications
+   - Escalate once a threshold is reached
+   - Use scripts as media
+   - Integrate with issue manager
+   - Understand global scripts

# Actions

The trigger list would be fine to look at, way better than looking at individual items, but that would still be an awful lot of manual work. That's where actions come in, providing notifications and other methods to react upon condition change.