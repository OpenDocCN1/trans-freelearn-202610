# Installing MySQL

In this section, we'll learn how to install MySQL on our development machine. The instructions for installing MySQL on your computer depend on your operating system, but here, we'll focus on the instructions for Ubuntu.

MySQL is a popular database management system that is also useful for local development since it is simple to install and configure.

The installation process is straightforward; simply update your system's package index, install the mysql-server package, and then execute the accompanying security script.

MySQL is probably already installed on your development machine. If that's the case, you may skip this step.

> **Important Note**
> The steps below are exclusively for setting up MySQL on your local machine for development purposes. In production, you must follow the appropriate guides, especially when it comes to securing your database from attacks. You can easily achieve this with cloud services, which provide a managed database that you don't have to manage or secure yourself.

Now, let's get started by running the instructions to install MySQL Server. Open a new command-line interface and run the following commands:

```
sudo apt-get update
```

```
sudo apt-get upgrade -y
```

These instructions will update your system's package index to the most recent version.

Then, to install MySQL Server, use the following command:

```
sudo apt-get install mysql-server
```

Now that we've installed MySQL Server, let's learn how to configure it.

# Configuring MySQL Server

After installing MySQL Server, you must execute a security script. Return to the command-line interface and execute the following command:

```
sudo mysql_secure_installation
```

You'll be prompted for your root user password; enter it and press Enter.