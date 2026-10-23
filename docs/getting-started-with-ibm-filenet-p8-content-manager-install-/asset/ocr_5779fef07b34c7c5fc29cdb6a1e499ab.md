The purpose of recording changes, including the order in which you made the changes and things like reboot/restart steps, is two-fold. First, you want to be able to reproduce the change in exactly the same way in multiple environments. Second, if things don't go the way you expect them to go, it's a lot easier to solve the resulting puzzles if you know exactly what you did.

(By the way, we are terrible at following this advice, which is why we are in development instead of operations.)

# Snapshots

If you can arrange to test your changes in a virtual machine environment, then your virtual machine manager will surely have the capability to make at least one snapshot. A snapshot for a virtual machine is a file-based capture of the entire virtual machine state at a given point in time. Some platforms will allow a single snapshot while others will allow arbitrarily many in a branched, hierarchical topology.

The idea of a snapshot is that you can discard all changes back to the point of the snapshot. All mistakes are conveniently erased. That's good and bad. You get a chance to start over, but you should first capture as much information as you can about what went wrong so that you don't waste too much time re-making the mistakes.

# Backups

We're not going to say too much about backups because the benefit is pretty obvious. If your configuration management system doesn't include doing a backup of configuration data before a change is made, you should definitely do it manually in any environment that is important enough that you don't want to lose it.

Backup of business data is often a decision made based on a risk assessment if there is significant downtime involved. The sheer volume of data and complex relationships can make planning for business data backup a complicated matter. For many good suggestions and best practices, there is an IBM Redbooks publication called *Disaster Recovery and Backup Solutions for IBM FileNet P8 Version 4.5.1 Systems*, available at http://www.redbooks.ibm.com/abstracts/sg247744.html.