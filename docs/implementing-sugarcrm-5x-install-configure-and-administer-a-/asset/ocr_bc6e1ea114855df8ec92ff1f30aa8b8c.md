![](bbox=[0.156, 0.061, 0.789, 0.429])

Proceed to complete the installation process and restart Apache. To verify that PHP is operational, create a file named `test.php` with the following content:

```php
<?php
Phpinfo();
?>
```

Place the file at the following location:

`C:\Program Files\Apache Software Foundation\Apache2.2\htdocs` and attempt to open it through your web browser by navigating to `http://localhost/test.php` (you may need to adjust your URL to include a custom port number). If everything is working properly, you should see a screen with the PHP version number and other pertinent information.

Now that you have a working Apache and PHP installation, you need to install the database component.