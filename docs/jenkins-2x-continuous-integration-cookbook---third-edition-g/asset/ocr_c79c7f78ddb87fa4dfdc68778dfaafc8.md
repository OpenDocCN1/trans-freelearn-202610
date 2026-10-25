![](bbox=[0.117, 0.062, 0.883, 0.354])

If you get an error while setting the password field when creating a generic user account in `phpldapadmin`, open `/usr/share/phpldapadmin/lib/TemplateRender.php` in edit mode and change `password_hash` to `password_hash_custom` on line 2469: