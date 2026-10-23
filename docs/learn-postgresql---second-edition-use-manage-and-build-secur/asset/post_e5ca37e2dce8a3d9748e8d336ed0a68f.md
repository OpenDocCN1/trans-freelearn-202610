
The preceding command will open your default text editor with an empty file; you can, therefore, set the PGDATA variable as follows and then save and exit the editor to apply changes:

```
[Service]
Environment=PGDATA=/postgres/16/data
```

Lastly, it is time to initialize the database directory; this can be done with a specific Fedora installation command named postgresql-16-setup, as follows:

```
$ sudo /usr/pgsql-16/bin/postgresql-16-setup initdb
```

In order to enable PostgreSQL to start at boot time and launch the server immediately, you can execute the following commands:

```
$ sudo systemctl enable postgresql-16
$ sudo systemctl start postgresql-16.service
```

If your Fedora installation contains the service(8) command, you can also start the service with the following:

```
$ sudo service postgresql-16 start
```

## Installing PostgreSQL on FreeBSD

PostgreSQL is available on FreeBSD by means of ports and packages. Thanks to the pkg(1) command, it is very easy to install PostgreSQL. First of all, update the package list, and search for the PostgreSQL packages that are named with the major version as the postfix:

```
$ pkg update
$ pkg search postgresql16
```

You can then install packages by executing pkg(1) and specify the set of packages you need. Of course, the installation must be executed as a user with administrative privileges, as follows:

```
$ sudo pkg install postgresql16-server-16.0 \
    postgresql16-client-16.0 \
    postgresql16-contrib-16.0 \
    postgresql16-docs-16.0
```

In order to start the cluster, you need to initialize the directory to serve the database and enable the server startup at the machine boot. The minimal parameters to set are postgresql_enable and postgresql_data.

For example, to edit (as an administrative user) the /etc/rc.conf file, add the options as follows:

```
# to enable PostgreSQL at boot time
postgresql_enable="YES"

# PGDATA to use
postgresql_data="/postgres/16/data"
```

Now you can initialize the data directory with the following command:

```
$ sudo /usr/local/etc/rc.d/postgresql initdb
```

Now that everything is in place, you can start the PostgreSQL instance with the following command:

```
$ sudo service postgresql start
```

## Installing PostgreSQL from sources

Installing PostgreSQL from sources requires downloading a tarball, which is a compressed package with all the source code files, and starting the compilation. Usually, this takes several minutes, depending on the power of the machine and the I/O bandwidth. In order to compile PostgreSQL from source, you will need different tools and libraries and mainly a C compiler compliant with the C99 standard (or higher). Usually, you already have these tools on a Linux or Unix system; otherwise, please refer to your operating system documentation on how to install these tools.

Once you have all the dependencies installed, follow the steps given here to compile and install PostgreSQL:

1. The very first step is to download the PostgreSQL tarball related to the version you want to install, verifying that it is correct. For instance, to download version 16.0, you can do the following:
```
$ wget https://ftp.postgresql.org/pub/source/v16.0/postgresql-16.0.tar.bz2
...
$ wget https://ftp.postgresql.org/pub/source/v16.0/postgresql-16.0.tar.bz2.md5
```
2. Before starting the compilation, check that the downloaded tarball is intact:
```
$ md5sum --check postgresql-16.0.tar.bz2.md5
postgresql-16.0.tar.bz2: OK
```
3. Once you are sure that the downloaded tarball is not corrupt, you can extract its content and start the compilation (please consider that the extracted archive will take around 200 MB of disk space, and the compilation will take up some extra space):

```
$ tar xjvf postgresql-16.0.tar.bz2
$ cd postgresql-16.0
$ ./configure --prefix=/usr/local
$ make && sudo make install
```

If you want or need the systemd(1) service file, add the --with-systemd option to the configure line.

4. Once the database has been installed, you need to create a user to run the database with, usually named postgres, and initialize the database directory:

```
$ sudo useradd postgres
$ sudo mkdir -p /postgres/16/data
$ sudo chown -R postgres:postgres /postgres/16
$ /usr/local/bin/initdb -D /postgres/16/data
```

## Installing PostgreSQL via pgenv

pgenv is a nice and small tool that allows you to download and manage several instances of different versions of PostgreSQL on the same machine. The idea behind pgenv is to let you explore different PostgreSQL versions—for instance, to test your application against different major versions. pgenv does not aim to be an enterprise-class tool to manage in-production instances; rather, it is a tool to let developers and DBAs experiment with different versions of PostgreSQL and keep them under control easily.

Of course, being an external tool, pgenv must be installed before it can be used. The installation, however, is very simple, since the application is made by a single Bash script.

The fastest way to get pgenv installed is to clone the GitHub repository and set the PATH environment variable to point to the executable directory, as follows:

```
$ git clone https://github.com/theory/pgenv
$ export PATH=$PATH:./pgenv/bin
```

Now, the pgenv command is at your fingertips, and you can run the command to get a help prompt and see the available commands.

The idea behind pgenv is pretty simple: it is a tool to automate the “boring” stuff—that is, downloading, compiling, installing, and starting/stopping a cluster. In order to let pgenv manage a specific instance, you have to “use” it. When you use an instance, pgenv detects whether the instance has been initialized or not, and in the latter case, it does the initialization for you.

In order to install versions 16.0 and 15.1 of PostgreSQL, you simply have to run the following commands:

```bash
$ pgenv build 16.0

$ pgenv build 15.1
```

The preceding commands will download and compile the two versions of PostgreSQL, and the time required for the operations to complete depends on the power and speed of the machine you are running on. After that, you can decide which instance to start with the use command:

```bash
$ pgenv use 16.0
```

pgenv is smart enough to see whether the instance you are starting has already been initialized, or it will initialize it (only the first time) for you.

If you need to stop and change the PostgreSQL version to use, you can issue a stop command followed by a use command with the targeted version. For instance, to stop running the 16.0 instance and start a 15.1 instance, you can use the following:

```bash
$ pgenv stop

$ pgenv use 15.1
```

The pgenv tool provides a lot of other commands to get information about which PostgreSQL versions are installed, what is executing (if any), and so on.

If you are searching for a quick way to test and run different PostgreSQL versions on the same machine, pgenv is a good tool.

## Summary

This chapter has introduced you to PostgreSQL, the project, and its main features. You have learned about PostgreSQL terminology, as well as how to install a cluster on Unix-like operating systems, including in containers, as well as installing the cluster from various sources.

Having installed PostgreSQL and having learned its terminology allows you to proceed to the next chapters, where you will learn how to use, connect, and store data in a database.

## References

- PostgreSQL release notes: https://www.postgresql.org/docs/16/release-16.html
- Upgrading documentation: https://www.postgresql.org/docs/current/upgrading.html
- PostgreSQL version policy: https://www.postgresql.org/support/versioning/
- PostgreSQL initdb official documentation: https://www.postgresql.org/docs/current/app-initdb.html
- PostgreSQL pg_ctl official documentation: https://www.postgresql.org/docs/current/app-pg-ctl.html
- pgenv GitHub repository and documentation: https://github.com/theory/pgenv

## Learn more on Discord

To join the Discord community for this book – where you can share feedback, ask questions to the author, and learn about new releases – follow the QR code below:

https://discord.gg/jYWCJF6Tku

![](img/bbe5644feba3d6a2ab994dc7e6cecbf1_52_0.png)
