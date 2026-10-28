To access SVN repositories, specific software—a client—is needed. There are many different SVN clients for various platforms, and you can choose whichever seems most convenient to you. Here, we will use the official command line client. As this client is available on almost all Linux distributions, we may want to use it on our Zabbix test server. But before we start playing with it, we must know that the Zabbix source code repository resides at https://svn.zabbix.com/. In SVN, development is usually split into a trunk and branches. While the trunk represents the most recent development work, branches are usually used for stable version maintenance. Zabbix uses the same schema, and there are branches for stable version maintenance such as 3.0, while the development for the next stable version happens in the development section, the trunk. The changes do not happen in the version branches or trunk right away, though—they are first implemented in the development branches, which are usually located at svn://svn.zabbix.com/branches/dev/ZBX-1, with the correct ZBX or ZBXNEXT issue number at the end.

Let's say we are interested in the latest features and want to retrieve the trunk. To do this, run the following:

```
$ svn checkout svn://svn.zabbix.com/trunk zabbix-trunk
```

This will proceed to retrieve all the files in the trunk and place them in a directory called zabbix-trunk. As of writing this, Zabbix trunk checkout uses approximately 118 MB on disk, but the amount transferred over the network will be smaller than that. Once the process completes, you might be tempted to proceed with compilation, but that won't be easy to do as there is no configuration script at all. There's a convenient script to generate the configuration:

```
$ ./bootstrap.sh
```

After this completes, we should have the configuration script. Now, we can compile this development version of Zabbix, right? Not quite yet. Development repositories hold only a generic database schema and content description, so we will not be able to create the database. We will have to generate the actual schema and data files ourselves. For the Zabbix frontend, specific CSS files have to be generated, too. It is also suggested to create a package, one just like those downloadable from the Zabbix site, so let's do that. Before we can generate the database schema and package, we have to use the configuration script, though. But we can make it slightly faster and require fewer dependencies by omitting any features that are not required. This also enables the creation of a Zabbix package on another machine that does not have all the dependencies for the required functionality, such as SNMP or IPMI monitoring, installed. In the most simple case, run this:

```
$ ./configure
```