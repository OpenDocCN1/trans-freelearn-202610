
3. Next, we will install MySQL. Note that the installer will ask you to set the password for the root user:

```
sudo apt-get install mysql-server
sudo apt-get install php-mysql
```

4. Secure the MySQL installation. To do this, run the following command:

```
mysql_secure_installation
```

It will ask for the MySQL password you configured previously:

- Remove anonymous users: Yes
- Disallow login remotely: Yes
- Remove test database and access to it: Yes
- Reload privilege tables now?: Yes

5. Open the apache2.conf file and set AllowOverride all to grant the right permissions to the document root.

6. Find the following block of code:

```
<Directory /var/www/>
  Options Indexes FollowSymLinks
  AllowOverride none
  Require all granted
</Directory>
```

Replace it with the following code:

```
<Directory /var/www/>
  Options Indexes FollowSymLinks
  AllowOverride all
  Require all granted
</Directory>
```

7. Restart the Apache server to apply the changes:

```
sudo service apache2 restart
```

8. Create a sample phpinfo.php file in the document root:

```
cd /var/www/html
rm index.html
echo "<?php echo phpinfo();" > phpinfo.php
```

9. Then, you can access the document root and see the output for the phpinfo.php file in the browser. For example, <Public IP>/phpinfo.php.

You should see the following page in the browser:

![](img/de3034a92496a32462134266292a51dd_58_0.png)

10. If phpinfo.php is displayed properly, remove the phpinfo.php file and create a custom MySQL user and database to import Magento (create the same username as the old server):

```
mysql -u root -p
CREATE USER '<username>'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON * . * TO '<username>'@'localhost';
FLUSH PRIVILEGES;
CREATE DATABASE <databasename>;
```

The Amazon instance is now ready for Magento 2 and you can proceed with the Setup Wizard or the command-line installation method that we saw in the previous section.

## Summary

In this chapter, we reviewed the system requirements for Magento. We installed an instance in our local machine, and we even set up an Amazon server from scratch for Magento 2. In the next chapter, we will review the process of migrating a store from Magento 1 to Magento 2.

## Code audit

A senior software engineer should review the code base of your current Magento 1.x store and take notes about the current state of the code.

This means that the software engineer will ensure that there are no bad practices in the Magento 1.x store because that would lead to missing features in the Magento 2.x store.

### Modified Magento core

An example of a bad practice in Magento 1.x is directly modifying the Magento core. Some developers do this instead of creating custom modules, which results in changes in the platform that are not visible in the code base through the custom modules. This will prevent those features from being migrated to Magento 2, since they are not visible to the developers when they review the custom modules.

If their core has been modified, then that will be the first step: moving those changes from the core to custom extensions, and restoring the original core files from Magento.

You can download any version of Magento, 1.0 onwards, from the Magento Tech Resources downloads page at `https://magento.com/tech-resources/download`.

The software engineer will be able to compare the current state of the core of your store with the original Magento core.

### Business logic in templates

Another bad programming practice in Magento is placing business logic in templates instead of custom extensions. The right way of adding business logic to templates in Magento is adding that logic in blocks in custom extensions, but some developers include PHP code in frontend templates instead.

The software engineer should review the frontend templates to see if there is any custom business logic that needs to be moved to custom extensions.

### Third-party extensions

I don't recommend having a lot of third-party extensions. You should have just the ones that you need and that you consider essential for your store.

That said, you should review whether the third-party extensions from your store are available for Magento 2. This means that you should go to the website from the module vendor and check whether a new extension has been created for Magento 2.

It's very important to mention that some module vendors are creating a version of Magento 2 for their extensions, but those extensions for Magento 2 don't have all the features from the extension for Magento 1.

This means that you should not only check whether the extension is available, but also whether all the features that you use in Magento 1 for that extension are available in the extension for Magento 2.

If an extension is not available for Magento 2, then you should analyze whether you really need that extension. In case you need it, you can contact the module vendor to see if there is a planned release soon. If the module vendor is not planning to release a new version for Magento 2 soon, then you should create that extension from scratch for Magento 2.

### Custom extensions

If you were looking for a custom feature in Magento 1 but didn't find any extension in Magento Connect, then you had to implement custom code to support that extension. These are custom extensions that are available in Magento 1 but that are not compatible with Magento 2.

Magento 2 is a new platform with a new data structure and new coding practices. You should create a new version from scratch for those extensions with compatible code for Magento 2.

### Unused extensions

Are there unused custom or third-party extensions? If that is the case, you should review the features from those extensions to see if you really need them. By reducing the number of extensions, you reduce the amount of code to migrate and you keep a cleaner Magento 2 instance.

### Custom JavaScript

Custom JavaScript is not migrated to Magento 2 and you need to take note of the custom features that you implemented using JavaScript. Those features should be manually migrated by an experienced Magento developer to Magento 2 to use RequireJS.

### Migration requirements

Now that you have analyzed the code, it's time to review the migration requirements. Sometimes, the original requirement changes after the code audit. That is reasonable since you will have more information about the current state of your store and you will create a plan of action based on that.

After the code audit process, you will have a list including:

- Changes in the core to move to custom extensions
- Business logic in templates to move to custom extensions
- Third-party extensions ready for Magento 2
- Third-party extensions that should be updated for Magento 2
- Custom extensions that should be updated for Magento 2
- Unused extensions that won't be implemented in Magento 2
- Custom Javascript to implement in Magento 2

There is something else to consider–the theme. The theme is not automatically migrated from Magento 1 to Magento 2. Just as you did for the third-party extensions, you should review whether the theme is available for Magento 2 and contact the theme developer, if necessary, to see if there is a planned release coming soon.

If you want to keep the same look and feel for your store, then you should consider that the theme should be created from scratch for Magento 2 if the theme is not available for the new version of Magento.

Now, the plan to move ahead with the migration should be similar to the following:

- Prepare everything from the list that you created before
- Make sure that there are no issues in the code since those issues will persist when you migrate to Magento 2
- Upgrade Magento 1.x to the latest Magento 1.x version
- If you move ahead with all the preceding items, you will be ready for data migration

## Using the Data Migration Tool

The Data Migration Tool has been created by the Magento team to allow store owners to move from Magento 1 to Magento 2.

The tool helps you migrate the database information to Magento 2, such as the products, orders, customers, store configurations, and more.

You can access the code for the Data Migration Tool from the following address in Github:

```
https://github.com/magento/data-migration-tool
```

![](img/de3034a92496a32462134266292a51dd_64_0.png)
