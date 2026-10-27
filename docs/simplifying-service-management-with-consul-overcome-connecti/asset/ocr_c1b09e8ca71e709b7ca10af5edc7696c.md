# # Automate me!

So, we've established how great it is to be able to discover the services available on our network without cumbersome static addressing, and how to utilize that to ensure the secure delivery of messages among our services. Wouldn't it be great to share this dynamic information with other components in the network? Well, why would we want to do that? To understand how this functionality can improve the lives of so many people, let's jump into the way-back machine and once again recall what we used to do, and often still do, when making adjustments to the network:

+   1. Yeah! We have our application deployed into the network and I can communicate with it. However, I can't reach the service my application needs to hit. Better call the network team.
+   
+   2. The network team told me that the problem must be on my end because the destination service is *alive* and responding to their monitoring tools.
+   
+   3. After a few days of troubleshooting, we learn that there is a firewall in the path that is protecting that service from nefarious creatures.
+   
+   4. So, next, we submit a ticket to the firewall team and request a rule be configured to allow traffic from, you guessed it, my IP address.
+   
+   5. Of course, the firewall team is not only getting hammered with requests, but they know that one wrong move on the list of firewall rules and the number of tickets will be the least of their problems, so extreme care must be taken.
+   
+   6. Eventually, the team is able to safely add and test the new rule, and we're off and running.

Now after all of that, let's hope that we didn't forget to include any important information in that ticket (such as an additional port). Not to mention that when the address changes (not if), we'll need to enter the process again. This process continues today, even with cloud automation, for a number of different reasons. In my personal experience, these changes are the ones that tend to create the most confusion and delay in any project, and in a dynamic world, the changes aren't slowing down.