First, let's ensure that drivers cannot be deleted if they are associated with a Contestant record (meaning they have or are taking part in a race). Edit the Driver field on the Contestant object and enable Don't allow deletion of the lookup record that's part of a lookup relationship. This is similar to expressing a referential integrity rule in other database platforms.

![](bbox=[0.118, 0.162, 0.907, 0.309])

Now if you try to delete Lewis Hamilton, you should receive a message as shown in the following screenshot. This also applies if you attempt to delete via Apex DML or the Salesforce API.

![](bbox=[0.118, 0.412, 0.91, 0.573])

You've just saved yourself some Apex Trigger code and used a platform feature.

Let's take a look at another lookup feature, known as lookup filters. Formula 1 drivers come and go all the time; let's ensure when selecting drivers to be in races via the Driver lookup on the Contestant record, that only the active drivers are shown and are permitted when entered directly.

Perform the following steps in the packaging org to try out the process:

+   1. Create a Driver record, for example, Rubens Barrichello and ensure that the Active field is unchecked.