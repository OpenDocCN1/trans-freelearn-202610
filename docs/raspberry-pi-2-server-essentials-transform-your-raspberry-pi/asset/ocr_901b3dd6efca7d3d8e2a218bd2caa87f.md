You will be presented with blue-background screens asking you for the root user password. You should write these down and keep them in a secure location, but it is more convenient and secure to store passwords in an application such as `KeePass`.

> It is good practice to never use the root user with MySQL for web applications, especially the production ones available on the Internet.

# Installing phpMyAdmin

A very popular SQL management tool called `phpMyAdmin` can be installed and used to further manage your databases and even transact against them.

The `phpMyAdmin` is the only application that needs to run with root settings so that it can fully manage MySQL. The best practice for `phpMyAdmin` is to only allow local network connections for administration or via SSH. This is because it will be used to create or delete databases, users, and passwords, and apply permission access to these databases. You can create less privileged users to log in via the Internet if you need too:

```
```bash
sudo apt-get install phpMyAdmin
```
```

During installation, you will get another blue screen asking for the web server that should be automatically configured. Regrettably, `nginx` is *not* on the list; do not check any box and click on **OK**. After a short while, a final screen will ask about `dbconfig-common`; click on **NO**.

We need to manually configure phpMyAdmin just like any other website for nginx. PhpMyAdmin places its files in the shared location `/usr/share/phpmyadmin`, and anyone with access to the system can use this shared website to access only their database.

In `sites-available`, create a new file called `phpmyadmin`; now use the following configuration. Remember to create a symbolic link in `sites-enabled` and then reload nginx:

```
```nginx
server {
    location /phpmyadmin {
        root /usr/share/;
        index index.php;
    }
    location ~ ^/phpmyadmin/(.+\.php)$ {
        try_files $uri =404;
        root /usr/share/;
        fastcgi_pass unix:/var/run/php5-fpm.sock;
        fastcgi_index index.php;
    }
}
```
```