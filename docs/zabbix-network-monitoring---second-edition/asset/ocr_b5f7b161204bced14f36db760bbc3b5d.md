# The upgrade process

Read the upgrade notes.

What was that? Yes, before performing any upgrades, take a little time, go to the Zabbix manual, and read the upgrade notes. If you are jumping over a few major versions, do read all the upgrade notes in between. Even if you have followed Zabbix development a bit, you might have missed some change that could cause problems—removed or added configuration parameters, memory requirement changes, API changes...upgrade notes should list all significant changes.

It is also highly suggested to read the pages on new features and improvements, called **What's new**. While it's much less risky to miss some of those changes, knowing about them could help you use Zabbix in a more efficient way.

Let's talk about the upgrade process itself now. This process and compatibility will differ depending on the version change you are performing:

+   - A minor version upgrade inside the same major version is simple and easy to undo
+   - A major version upgrade is more complicated and hard or impossible to undo

# Minor version upgrade

This is the simplest case. For example, upgrading from 3.0.0 to 3.0.1 or from 3.0.1 to 3.0.5 would be considered a minor version upgrade.

> Zabbix uses the third number to denote a minor version.

When performing a minor version upgrade, we may upgrade any combination of components—server, agents, proxies, Java gateway, and so on. While it is suggested to keep the main components of the same version to reduce confusion, a 3.0.0 server will happily work with a 3.0.1 frontend, 3.0.2 proxies, and 3.0.3 agents. Inside one major version, all components are compatible with each other.

It is also perfectly fine to skip minor versions when upgrading—as mentioned, going from 3.0.1 directly to 3.0.5 is perfectly fine.

While minor versions won't have upgrade notes often, do make sure to check for them. And read those **What's new** pages.