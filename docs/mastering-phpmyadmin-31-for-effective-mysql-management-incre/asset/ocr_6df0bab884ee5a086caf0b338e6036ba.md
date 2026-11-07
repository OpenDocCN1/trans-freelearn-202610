# The config.inc.php file

This file contains valid PHP code, defining the majority of the parameters (expressed by PHP variables) that we can change to tune phpMyAdmin to our own needs. There are also normal PHP comments in it, and we can comment our changes.

> Be careful not to add any blank line at the beginning or end of the file; this would hamper the execution of phpMyAdmin.

Note that phpMyAdmin looks for this file in the first level directory – the same one where index.php is located.

In versions before 2.8.0, a generic config.inc.php file was included in the downloaded kit. Since 2.8.0, this file is no longer present in the directory structure. Since version 2.9.0, a config.sample.inc.php file is included, which can be copied over to config.inc.php to act as a starting point. However, it is recommended that you use the web-based setup script (explained in this chapter) instead, for a more comfortable configuration interface.

There is another file – layout.inc.php – containing some configuration information. As phpMyAdmin offers theme management, this file contains the theme-specific colors and settings. There is one layout.inc.php per theme, located in themes/themename, for example, themes/original. We will cover modifying some of those parameters in Chapter 4, under the First steps section.

# Permissions on config.inc.php

In its normal behavior, phpMyAdmin verifies that the permissions on this file do not allow everyone to modify it. This means that the file should not be writable by the world. Also, it displays a warning if the permissions are not correct. However, in some situations (for example a NTFS file system mounted on a non-Windows server), the permission detection fails. In these cases, you should set this parameter to false:

```
$cfg['CheckConfigurationPermissions'] = false;
```