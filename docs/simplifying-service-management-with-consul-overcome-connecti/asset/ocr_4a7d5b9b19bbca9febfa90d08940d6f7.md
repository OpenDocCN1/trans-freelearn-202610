# # Distributed configuration with the key value store

As we've read, one of the fundamental protocols of the Consul system is the gossip protocol, enabling each Consul agent within the system to share information with all others. The information is centrally located and managed by the Consul server cluster, but all agents (who are properly authorized if required) have access to that data. Through a very simple KV interface, Consul provides this ability to distribute intelligence to all entities interacting with the system. Although this provides a very easy way to distribute information, Consul isn't a fully featured distributed data store. Objects within the KV system have a maximum size of 512 KB, and the data only persists to the local data center. However, this doesn't prevent us from using it to do some pretty cool stuff.

To see the benefit of applying the Consul KV system for configuration adjustments, consider the situation of troubleshooting a distributed application. After all, nearly everything we deploy these days consists of a set of services, as opposed to a giant monolithic application. When problems occur, which they always do, we'll find that the log data that we need is only available at elevated levels. From my horrid recollection of doing this in the past, the process usually goes something like this:

+   1. Acquire the IP addresses for each machine/component that needs to be adjusted.
+   2. Open a bunch of terminal windows on my machine.
+   3. SSH into the first machine, usually using some shared PEM key that everybody on the team has.
+   4. Edit the configuration file to change the log level.
+   5. Reload the service and check the logs to make sure that the service is running again with the new log level.
+   6. Log out of the machine and proceed to the next one in the list.

Once the debugging session is over, we have to go back into the machines and revert the changes we've made. Configuration management tools such as Ansible, Puppet, or Chef can help with these changes as well, but those changes often require updates to extensive scripts, which brings in additional risk.