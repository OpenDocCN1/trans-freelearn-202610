Once the repository has been set up, you may install the packages:

```
```bash
# yum install zabbix22-agent zabbix22-dbfiles-mysql zabbix22-server-mysql
zabbix22-web-mysql
```
```

# The Zabbix repository

First, the package that will define the Zabbix repository should be installed:

```
```bash
# rpm -ivh http://repo.zabbix.com/zabbix/3.0/rhel/7/x86_64/zabbix-
release-3.0-1.el7.noarch.rpm
```
```

Once the repository has been set up, you may install the packages:

```
```bash
# yum install zabbix-server-mysql zabbix-web-mysql zabbix-agent
```
```

# OpenSUSE

For OpenSUSE, Zabbix is available in the `server:monitoring` repository. First, the repository should be added and its package list downloaded (you might have to change the distribution version):

```
```bash
# zypper addrepo http://download.opensuse.org/repositories/
server:monitoring/openSUSE_Leap_42.1/server:monitoring.repo

# zypper refresh
```
```

Once the repository has been set up, you may install the packages:

```
```bash
# zypper install zabbix-server-mysql zabbix-agent zabbix-phpfrontend
```
```

# Installing from source

If you have decided to install Zabbix from source, you will need to obtain the source, configure it, and compile it. After the daemons are put in place, the frontend will have to be set up manually as well.

# The server and agent

At first, we will only set up the Zabbix server and agent, both running on the same system. We will set up additional components later during the course of this book.