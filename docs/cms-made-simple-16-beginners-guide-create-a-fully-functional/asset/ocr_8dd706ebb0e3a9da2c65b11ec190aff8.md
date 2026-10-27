Failures should be corrected as soon as possible. Warnings can be ignored if you do not have any issues when running CMS Made Simple, but you should correct them if something does not work as expected. In the following table, you will find some required and recommended settings for CMS Made Simple. Required settings are mandatory. Recommended settings can make your system run smoother and solve some issues.

| Setting | Minimum and recommended values |
|---|---|
| **Current PHP Version (phpversion)** | 5.0.4 or greater |
| **GD version (gd_version)** | version 2 |
| **Magic quotes in runtime (magic_quotes_runtime)** | On (True) |
| **PHP Effective Memory Limit (memory_limit)** | minimum 16 MB
recommended 24 MB or greater |
| **Maximum Execution Time (max_execution_time)** | minimum 30
recommend 60 or greater |
| **PHP Safe Mode (safe_mode)** | Off (False) |
| **Session Save Path (session_save_path)** | This path should be writable (green tick) |
| **PHP register_globals (register_globals)** | Off (False) |
| **Maximum Post Size (post_max_size)** | minimum 2M
recommend 10M or greater |
| **Maximum Upload Size (upload_max_filesize)** | minimum 2M
recommend 10M or greater |

The third section on the **System Information** page gives you an overview of your server's configuration. If you are the website administrator, then you should be able to answer at least three questions about your system:

+   - What operating system is used on your server?
+   - What server software (and which version) is installed on your server?
+   - What database (and which version) is used for a CMS Made Simple installation?