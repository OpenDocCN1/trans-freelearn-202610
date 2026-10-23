Debian and Ubuntu provide their own command to control the cluster, pg_ctlcluster(1). The rationale for that is that on a Debian/Ubuntu operating system, every PostgreSQL version is installed in its own directory with separate configuration files, so there is a way to run different versions concurrently and manage them via the operating system. For example, configuration files are under the /etc/postgresql/16/main directory, while the data directory is set by default to /var/lib/postgresql/16/main.

If you want to enable PostgreSQL at boot time, you need to run the following command:

```
$ sudo update-rc.d postgresql enable

In order to start your cluster, you can use the service(1) command as follows:
$ sudo service postgresql start
```

You have thus installed PostgreSQL on GNU/Linux Debian, Ubuntu, and derivatives.

# Installing PostgreSQL on Fedora Linux

Fedora Linux PostgreSQL packages are provided by the PostgreSQL community. In order to allow dnf(8) to find PostgreSQL packages, you need to install the PGDG repository, and then proceed with the installation as a distribution package:

```
$ sudo dnf install -y https://download.postgresql.org/pub/repos/yum/reporpms/F-38-x86_64/pgdg-fedora-repo-latest.noarch.rpm
```

The list of available repositories can be obtained from the PostgreSQL official website on the download page (see the References section).

Packages are named with the postfix of the version number. You can install the PostgreSQL packages using the following command:

```
$ sudo dnf install -y postgresql16-server postgresql16
```

Then you need to configure the system, specifying the PGDATA directory and enabling the option to start the service at boot time. In order to specify the PGDATA directory, you need to use systemd(1) to edit an overriding configuration file for the postgresql-16 service:

```
$ sudo systemctl edit postgresql-16
```