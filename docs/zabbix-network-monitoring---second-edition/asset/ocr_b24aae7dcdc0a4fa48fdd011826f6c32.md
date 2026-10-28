As with most software, there are some prerequisites that we will need in order to run Zabbix components. These include requirements of hardware and other software that the Zabbix server and agent depend on. For the purpose of our installation, we will settle for running Zabbix on Linux, using a MySQL database. The specific Linux distribution does not matter much — it's best to choose the one you are most familiar with.

# Hardware requirements

Hardware requirements vary wildly depending on the configuration. It is impossible to provide definite requirements, so any production installation should evaluate them individually. For our test environment, though, even as little RAM as 128 MB should be enough. CPU power in general won't play a huge role; Pentium II-class hardware should be perfectly capable of dealing with it, although generating graphs with many elements or other complex views could require more powerful hardware to operate at an acceptable speed. You can take these as a starting point as well when installing on a virtual machine.

Of course, the more resources you give to Zabbix, the snappier and happier it will be.

# Installing from the packages

If you have decided to install Zabbix from the packages, package availability and the procedure will differ based on the distribution. A few distributions will be covered here — read the distribution-specific instructions for others.

# RHEL/CentOS

RedHat Enterprise Linux or CentOS users have two repositories to choose from: the well-known Extra Packages for Enterprise Linux (EPEL) and the Zabbix repository. EPEL might be a safer choice, but it might not always have the latest version.

# EPEL

If EPEL is not set up already, it must be added. For RHEL/CentOS 7, the command is similar to this:

```
# rpm -Uvh http://ftp.colocall.net/pub/epel/7/x86_64/e/epel-release-7-5.noarch.rpm
```

> Check the latest available version at http://download.fedoraproject.org/pub/epel/7/x86_64/repoview/epel-release.html.