# # Configuring PHP

Before you dive into adjusting PHP settings, you need to install a couple of
important extensions. The first is `php-imap`, used for e-mail functionality
and the other is `php-mbstring`, used for double-byte character support.

To install them, execute the following commands:

+   • `yum install php-imap`

+   • `yum install php-mbstring`

Once these extensions are successfully installed, proceed to modify your
PHP configuration.

To modify your PHP configuration perform the following steps:

+   • Navigate to `/etc` and open the file `PHP.INI` using the text editor of
+   your choice.

+   • Search for the `memory_limit` parameter and set its value to at least **64M**
+   (256M would be preferred). If not adjusted, there is a tendency for blank
+   screens and other odd behavior to occur while attempting to use SugarCRM.

+   • Search for `max_execution_time` and set it to **300**. Too low a value usually
+   results in actions that starts, but remain incomplete. For example, exporting
+   a large number of records may fail when exporting a smaller set of the same
+   data succeeds.

+   • Set `max_input_time` to **300**. This allows a 5 minute window for large files
+   to be uploaded. Inadequate values usually result in problems when using the
+   **Upgrade Wizard**, uploading files/attachments, or using the **Module Loader**.

+   • Search for `display_errors`, and set it to **Off**. This will suppress the display
+   of warning messages, which will otherwise disrupt the display.

+   • Search for `post_max_size`, and set it to **25M** to allow large documents to be
+   uploaded. Then search for `upload_max_filesize`, and set it to **25M**. The effect
+   of these two changes will be to allow a document file of 20 megabytes in size
+   to be uploaded to the system.

+   • Search for `session.gc_maxlifetime`. Note that by default it is set to **1440**
+   seconds, which is 24 minutes. This controls the length of time a SugarCRM
+   session can be idle before the session is terminated. A session represents a
+   unique connection to SugarCRM and is automatically initiated when a user
+   accesses the system. If a user's session is terminated, the user will need to
+   login again before they can interact with SugarCRM. A value of 3600, or
+   1 hour, is usually adequate, but feel free to increase or decrease its value.