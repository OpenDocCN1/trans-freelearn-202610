From here on, you should be able to log in to the Zabbix frontend as Admin / somepassword—try it out. Feel free to change the password back after that.

There's actually an easier method available. MySQL has a built-in function for calculating MD5 hashes, so all this trickery could be replaced with a simpler approach:

```
mysql> update users set passwd=MD5('somepassword') where alias='Admin';
```

> At this time, Zabbix does not use password salting. While making it simpler to reset the password, it also makes it easier to find the actual password in MD5 tables.

We also mentioned making some user a Zabbix super admin. This change is fairly simple—all we have to do is change a single number:

```
mysql> update users set type=3 where alias='wannabe_admin';
```

And that's it, the user wannabe_admin will become a Zabbix super admin.

# Changing existing data

While once the monitoring data has been gathered you usually won't have a need to change it, there might be some rare cases when that might be required. Back in Chapter 3, Monitoring with Zabbix Agents and Basic Protocols, we created items for network traffic monitoring, and we gathered data in bytes, but in network management, usually bits per second are used instead. While it would often be possible for you to simply reconfigure the items and clear the old data, what if you need to preserve already gathered values? Directly editing the database might be the only solution.

Before doing that, you would have to modify the item in question. If data is coming in bytes but we want bits, what do we do? Right, we configure the multiplier for that item and set the multiplier to be 8. Additionally, change units to b (bits) while performing the change.

When performing the change to the item, take a quick look at a clock.

While this will deal with all future incoming values, it will leave us with inconsistent data before that moment. As we do not want to delete it, we must find some way to fix it. Our problem is twofold:

+   - We have incorrect data in the database
+   - We have both incorrect and correct data in the database (old and new values)