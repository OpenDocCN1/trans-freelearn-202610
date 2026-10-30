# Installation

+   - PHP Dependency Manager:
+     - Composer
+   - SSL:
+     - Valid security certificate (required for HTTPS)
+     - **Transport Layer Security (TLS)** requirement
+   - Mail Server:
+     - **Mail Transfer Agent (MTA)** or SMTP server
+   - Optional:
+     - Redis 3.0
+     - Varnish 3.5 or latest stable 4.x version
+     - Memcached
+     - Xdebug
+     - PHPUnit

# Local installation in your computer

In this section, we will review the general process to set up Magento with sample data in your local computer. In the next two sections, we will review the process of choosing the right hosting provider and set up a server to install Magento from scratch.

Taking that into consideration, we will assume that you have PHP 7 and MySQL running in your local machine and all the dependencies listed in the previous section as well.

Besides that, you should have your virtual host for the local instance already created (that is, `http://magento2-playground.dev`), the hosts file with the new entry for the local Magento instance (`127.0.0.1 magento2-playground.dev`), and an empty database.