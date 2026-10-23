6. Reset permissions on the new Git repository for Apache:

```
# chown -R apache:apache /var/www/html/git/
```

7. Finally, create a configuration file which requires authentication for the Git repository. The following example can be copied to /etc/httpd/conf.d/git.conf:

```
<Directory /var/www/html/git/>
    DAV on

    AuthName "Git login:"
    AuthType Basic
    AuthUserFile /var/www/htpasswd

    # Allow anonymous clone, but require auth for push.
    <LimitExcept GET PROPFIND>
        Require valid-user
    </LimitExcept>
</Directory>
```

8. Create a user account for us to use for the repository:

```
# htpasswd -c /var/www/htpasswd git
```

9. Last, restart the httpd server:

```
# systemctl restart httpd
```

10. You should now be able to clone the Git repository using the following command:

```
# git clone http://git@localhost/git/openstack
```

11. You may be prompted for the password that you entered when creating the user account and you should receive the following message:

```
warning: You appear to have cloned an empty repository.
```