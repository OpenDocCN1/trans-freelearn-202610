In a nutshell, we first navigate to the HTML folder, take ownership of the folder, remove all the pre-existing files, and then fetch the WordPress archive. Next, we extract the contents of the archive and place them in the HTML folder. We do that with the following commands:

```
$ tar xzf latest.tar.gz
$ mv wordpress/*
$ rm -rf wordpress latest.tar.gz
```

Now, if you run the ls command, you will see the following output:

![](bbox=[0.26, 0.31, 0.74, 0.63])

# Configuring the WordPress installation

Once the files are properly placed in the right locations, to activate the installation and start using the WordPress website, we first need to set up a MySQL database that WordPress can use. To set up the database, execute the following command:

```
$ mysql -uroot -p
```