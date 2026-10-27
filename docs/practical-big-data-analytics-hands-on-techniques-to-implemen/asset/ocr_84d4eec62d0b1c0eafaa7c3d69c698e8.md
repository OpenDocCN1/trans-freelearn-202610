The exercise is however not dependent on the platform on which you install MongoDB. Once the installation has been completed, you can execute the commands indicated in this chapter on any other supported platform. If you have access to a separate Linux machine, you can use that as well.

We will visit some of the common semantics of MongoDB and also download two datasets to compute the highest number of Nobel Prizes grouped by continent. The complete dump of the Nobel Prize data on Nobel Laureates is available from nobelprize.org. The data contains all the primary attributes of Laureates. We wish to integrate this data with demographic information on the respective countries to extract more interesting analytical information:

+   1. **Download MongoDB**: MongoDB can be downloaded from https://www.mongodb.com/download-center#community.

To determine which version is applicable for us, we checked the version of Linux installed on the CDH VM:

```
(cloudera@quickstart ~)$ lsb_release -a
LSB Version:     :base-4.0-amd64:base-4.0-noarch:core-4.0-amd64:core-4.0-noarch
Distributor ID:  CentOS
Description:     CentOS release 6.7 (Final)
Release:  6.7
Codename: Final
```

+   2. Based on the information, we have to use the CentOS version of MongoDB, and accordingly, following the instructions at https://docs.mongodb.com/manual/tutorial/install-mongodb-on-red-hat/, we installed the software, shown as follows:

```
The first step involved adding the repo as follows. Type in sudo nano /etc/yum.repos.d/mongodb-org-3.4.repo on the command line and enter the text as shown.
```

```
(root@quickstart cloudera)# sudo nano /etc/yum.repos.d/mongodb-org-3.4.repo
```

```
### Type in the information shown below and press CTRL-X
### When prompted to save buffer, type in yes
```

```
(mongodb-org-3.4)
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org
```