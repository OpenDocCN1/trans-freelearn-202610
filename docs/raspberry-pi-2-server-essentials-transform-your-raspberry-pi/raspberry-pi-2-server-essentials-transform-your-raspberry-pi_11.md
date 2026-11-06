## 11 运行自己的 ownCloud

This chapter covers how to install the open source ownCloud onto the Raspberry Pi 2, which is a great, free, self-hosted alternative to services such as DropBox, Google Drive, or Live Drive.

If you feel like you would like full control over the data, how you share it, and who can access it, ownCloud is a great solution. It is a self-hosted file sync and share server, which provides access through a web interface and the sync clients of WebDAV.

There are clients available for most operating systems, but even when you don't have a client, you can always use a modern browser to get straight into your data.

### 安装

As of writing this chapter, ownCloud version 8 is the latest version available for installation.

You may already have a few things that you require installed if you have followed the previous chapters. In case you need a few extra packages, it is best to go through the entire installation process from the beginning as described here.

### 要求

ownCloud requires a database storage engine, PHP, a web server, and data. As usual, I recommend to use nginx as it is lightweight and powerful. You can learn how to install nginx in Chapter 4, Using Fast Web Servers and Databases. It is also recommended by the developers of ownCloud to use nginx as the web server.

To be able to access ownCloud from the World Wide Web (WWW), refer to the dynamic DNS section in Chapter 2, Preparing a Network.

### MySQL

Update your repositories and install MySQL:

```
sudo apt-get update
```

```
sudo apt-get install mysql-server
```

During installation, you will be asked to set a root mysql password on a blue screen.

> Use a strong password and save it into a program such as Keepass so that you can quickly recover it for the next time you need it.

After installation, we will enter into the MySQL database console and create a database for ownCloud. You should replace the password in the script with your own strong password:

```
mysql -u root -p
```

```
CREATE DATABASE owncloud;
CREATE USER owncloud@localhost IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON owncloud.* TO owncloud@localhost;
FLUSH PRIVILEGES;
```

```
exit
```

Remember the username, password, and database name if you have changed it, as it will be required in the First Configuration section.

### nginx 和 PHP

We will install a variety of packages for PHP followed by nginx as follows:

- 1. Run the following command:

```
sudo apt-get install php5-mysql openssl ssl-cert php5-cli php5-common php5-cgi php-pear php-apc curl libapr1 libtool php5-curl libcurl4-openssl-dev php-xml-parser php5-dev php5-gd memcached php5-memcache
```

```
sudo apt-get install php5-fpm nginx
```



- 2. 安装完所有必需的软件包后，您可以创建一个新的网页文件夹，下载 ownCloud 的最新版本，并将其解压到名为 owncloud 的新文件夹中：
```
sudo mkdir -p /var/www/owncloud
cd /var/www
sudo wget https://download.owncloud.org/community/owncloud-8.1.3.tar.bz2
sudo tar -xvf owncloud-8.1.3.tar.bz2
sudo chown -R www-data:www-data /var/www
```
- 3. 我们将生成一个自签名证书，但您可以从 https://letsencrypt.org 获取免费证书。自签名证书会在浏览器中显示错误消息，但在您继续后，您的所有流量仍将被加密：
```
sudo mkdir -p /etc/nginx/ssl
sudo openssl req -x509 -nodes -days 36500 -newkey rsa:2048 -keyout /etc/nginx/ssl/owncloud.key -out /etc/nginx/ssl/owncloud.crt
```
- 4. 在证书生成过程中，您将被要求输入各种其他信息。您可以将国家名称、州、地区名称、组织等留空，但对于通用名称，您必须输入您的动态 DNS 地址。
- 5. 我们现在将在 /etc/nginx/sites-available/owncloud 创建并编辑 nginx 配置文件。
- 6. 如果您决定为 WWW 访问使用域名，则需要相应地调整 server_name 属性。您的 Raspberry Pi 的 DNS 名称和私有 IP 如下：
```
server_name {domain.name.com},{pi's IP};
```
- 7. 为了简单起见，我只使用我的 Pi 的 IP 地址，而不是 DNS 的 IP 地址。

第一个 server 块会强制将所有非 SSL 请求永久重定向到 SSL 配置的 server 块。

SSL server 块具有相当大的配置区域。您可以在代码包中找到文本版本。

如果您运行了其他站点，则应注意 root 指令。如果您将 `/var/www/` 用作 root，那么只需在 URL 中添加 `/owncloud` 即可访问站点。如果您希望仅将 Raspberry Pi 用于 ownCloud，您可以将 root 更改为 `/var/www/owncloud/`，然后您就可以使用 IP 或 DNS 访问 ownCloud，而无需在 URL 中添加子文件夹。我已按多租户配置的推荐方式保留，这样您需要在 URL 中添加 `/owncloud`。您也可以选择设置一个锁定到 `/owncloud` 目录的子域名，这是一个不错的替代方案。您应在此决定一次，因为稍后更改可能会破坏 ownCloud：
```
server {
    listen 80;
    server_name 192.168.1.21;
    return 301 https://$server_name$request_uri;  # enforce https
}

server {
    listen 443 ssl;
    server_name 192.168.1.21;
    access_log /var/log/nginx/sitename.access.log;
    error_log /var/log/nginx/sitename.error.log;

    ssl_certificate /etc/nginx/ssl/owncloud.crt;
    ssl_certificate_key /etc/nginx/ssl/owncloud.key;

    # Path to the root of your installation
    root /var/www/;
    client_max_body_size 10G; # set max upload size
    fastcgi_buffers 64 4K;

    # Some rewrite rules, more to come later
    rewrite ^/owncloud/caldav((/|$).*) $ /owncloud/remote.php/caldav$1
    last;
    rewrite ^/owncloud/carddav((/|$).*) $ /owncloud/remote.php/carddav$1
    last;
    rewrite ^/owncloud/webdav((/|$).*) $ /owncloud/remote.php/webdav$1
    last;

    # Protecting sensitive files from the evil outside world
    location ~ ^/owncloud/(data|config|\.ht|db_structure.xml|README) {
        deny all;
    }
    # Configure the root location with proper rewrite rules
    location /owncloud/ {
rewrite ^/owncloud/.well-known/host-meta /public.php?service=host-meta last;
rewrite ^/owncloud/.well-known/host-meta.json /public.php?service=host-meta-json last;
rewrite ^/owncloud/.well-known/carddav /remote.php/carddav/ redirect;
rewrite ^/owncloud/.well-known/caldav /remote.php/caldav/ redirect;
rewrite ^/owncloud/apps/calendar/caldav.php /remote.php/caldav/ last;
rewrite ^/owncloud/apps/contacts/carddav.php /remote.php/carddav/ last;
rewrite ^/owncloud/apps/([^\*/])(/.*\.(css|php))$ /index.php?app=$1&getfile=$2 last;
rewrite ^(/owncloud/core/doc[^\*/]+)$ $1/index.html;
try_files $uri $uri/ index.php;
}
location ~ \.php(?:$|/) {
    fastcgi_split_path_info ^(.+\.php)(/.+)$;
    include fastcgi_params;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    fastcgi_param PATH_INFO $fastcgi_path_info;
    fastcgi_param HTTPS on;
    fastcgi_pass unix:/var/run/php5-fpm.sock;
}
### Optional: set long EXPIRES header on static assets
location ~* \.(?:jpg|jpeg|gif|bmp|ico|png|css|js|swf)$ {
    expires 30d;
    # Optional: Don't log access to assets
    access_log off;
}
}
```

现在您已启用 ownCloud，如果您刚刚安装了 nginx，则已禁用默认配置：
```
sudo ln -s /etc/nginx/sites-available/owncloud /etc/nginx/sites-enabled/owncloud
```
```
sudo unlink /etc/nginx/sites-enabled/default
```

最后，我们需要重启所有服务以使新配置生效：
```
sudo service nginx restart
```
```
sudo service php5-fpm restart
```

## 权限

ownCloud 会将所有文件作为普通文件存储在目录中。它使用数据库对文件进行索引以提高查询速度。如果由于某种原因，您的 ownCloud 停止正常运行，您仍然可以直接访问所有文件，而无需任何复杂的恢复程序。

ownCloud 在 www-data 下运行。为了解决文件权限问题，最简单的解决方案是将 www-data 添加到 pi 组中，然后创建一个应用了 www-data 所有者的子文件夹：
```
sudo usermod -a -G pi www-data
```

以下两个命令要求您根据文件系统挂载的位置和方法添加目录目标。我们将创建一个新的子目录并将其所有权分配给 www-data。我们还需要为上传的文件创建一个临时目录：
```
sudo mkdir -p /mnt/{your mount point}/owncloud
sudo chown -R www-data:www-data /mnt/{your mount point}/owncloud
```
```
sudo mkdir -p /mnt/{your mount point}/owncloud/tmp
sudo chown -R www-data:www-data /mnt/{your mount point}/owncloud/tmp
```

## 首次配置

到目前为止，ownCloud 应用程序已全部预配置。最后一步，仅需执行一次，即使用浏览器运行逐步配置向导：

- 1. 打开浏览器并输入您的 Raspberry Pi 的 IP 地址或 DNS 名称。
- 2. 您将看到证书错误。根据您使用的浏览器，您可以将其证书添加到受信任列表，或单击按钮继续：

![](img/a8753cacc9d454dfc09eda1ed5012bd3_149_0.png)

- 3. 如果一切按计划进行，您将看到 ownCloud 的初始配置屏幕。
- 4. 输入管理员用户名和密码（请使用强密码，因为我们不应经常使用此账户）。这是能够创建新用户、更改配置等的用户。创建管理员后，我们将创建一个普通用户，您应从此开始使用。
- 5. 下一个输入会询问您的 data 文件夹。这是您输入我们之前创建并设置权限的目录的地方。
- 6. 在最后一步，我们需要输入数据库设置，其中包括我们之前创建的用户名、密码和数据库。用户名是 owncloud（不是 owncloud@localhost），后面跟您输入的密码，以及 owncloud 的表名。保持 localhost 不变。
- 7. 最后，我们可以单击 Finish Setup。然后，ownCloud 将配置数据库并测试可能需要解决的任何问题。

经过几秒钟，您将看到主仪表板，显示欢迎使用 ownCloud 页面，提供多个可下载的应用程序以及连接 ownCloud 的方式。

## 管理员配置

如果您刚刚以管理员身份登录到仪表板，强烈建议您创建一个用户，用于日常云任务：

- 1. 在右上角，您将看到您的管理员用户名和一个下拉菜单。单击它，然后单击 **用户**。
- 2. 在此页面创建用户之前，单击 groups 并单击 + **添加组** 以创建新的用户组，例如 **用户**。
- 3. 现在，您只需输入用户名和密码；确保已勾选新组，然后单击 **创建**：

![](img/a8753cacc9d454dfc09eda1ed5012bd3_150_0.png)

通过创建新组，您现在可以更好地管理限制或选项，以便以后对该组或其他新组下的任何人使用。

例如，在此页面上，您可以为每个组设置配额。

## 服务器管理员

在仍以管理员身份登录的情况下，单击右上角的管理员用户名，然后单击 **管理员**。默认情况下，您将进入共享选项页面。

在此页面上，您可能会注意到一些安全和设置警告。如果看到任何关键问题，您可以通过阅读在线文档来尝试解决这些问题。

作为标准，这里没有什么关键的内容需要更改，但阅读可用选项时，您可能会发现喜欢或不喜欢的内容并相应地进行调整。

第一个允许您为组进行调整的选项是 **限制用户仅与所在组的用户共享**。



当发布更新时，您将重新访问此页面以启用更新模式，并创建配置备份。

您可以使用左侧的菜单跳转到页面的各个部分。

## 安装应用

通过 WebGUI 以管理员身份登录时，您可以点击左上角的链接，该链接位于 ownCloud 图标旁边。下拉菜单将会出现，您会看到一个添加应用的按钮：

![](img/a8753cacc9d454dfc09eda1ed5012bd3_151_0.png)

免费版提供基本的文件支持和一些不错的附加功能。企业版提供对 **Calendar**、联系人管理和同步的官方支持。

在此页面上，您将看到已启用的官方应用列表，例如图片或 PDF 查看器。还有一些应用处于禁用状态；要查看它们，请点击右侧的 **Not Enabled** 菜单项。

不过无需担心应用数量不足，因为 ownCloud 设计为可扩展的。您可以在 https://apps.owncloud.com 免费安装大量用户制作的应用。

在应用页面，点击左下角的齿轮图标，并启用 **Experimental Apps** 以允许安装第三方应用。

如果您在查看或启动下载的应用时遇到任何问题，可能需要将您创建的文件夹的所有者设置为 **www-data**。

## 日历

要在 ownCloud 上启用日历功能，我们将安装一款评分很高的应用，名为 Calendar Plus：

- 1. 在 `https://apps.owncloud.com` 网站上搜索此应用并下载它。
- 2. 将 zip 文件解压到名为 calendar plus 的目录中。
- 3. 要安装应用，我们需要将解压后的目录上传到位于 `/var/www/owncloud/apps` 的 `/apps` 目录。使用 Filezilla 通过 SFTP 连接到您的 Raspberry Pi，并将目录上传到 apps 目录。
- 4. 在 WebGUI 的 **Apps** 页面上，点击 **Not Enabled** 并在页面底部找到 **Calendar+**。点击 **Enable**：

![](img/a8753cacc9d454dfc09eda1ed5012bd3_152_0.png)

## 其他应用

`https://apps.owncloud.com` 网站拥有大量各种应用可供选择，您可以以相同的方式安装它们，除非作者在应用描述中提供了额外步骤。

## 日常功能

如果您已查看配置页面，您可以注销管理员账号，并使用 Web GUI 以您为自己创建的用户身份登录。

对于在本地网络上运行的计算机和设备，您可以将客户端应用程序安装到桌面或移动计算机上，并使用内部 IP 或 Pi 的 DNS。

## 全球访问

如果您决定使用动态 DNS 服务在 WWW 上使用 ownCloud，第一件事是检查其是否配置正确。一种简单的方法是将客户端应用程序下载到您的智能手机或支持 3G 的平板电脑上。

安装应用程序，然后关闭 Wi-Fi 以接入移动网络，例如 3G。并非所有路由器都支持回环 DNS 查找，这意味着尝试从您的 IP 访问您的 IP 可能会失败，但使用外部网络可以验证您的端口是否已正确转发。

启动应用程序并在登录屏幕上输入您的详细信息。客户端应用程序将进行快速连接测试并告知您状态。

如果您已登录，可以尝试重新打开 Wi-Fi，看看客户端是否会正常继续运行。

## 摘要

Raspberry Pi 2 现在可以成为您实现云存储独立的解决方案。只要在存储介质上设置了一些冗余，您就可以基本摆脱大型企业巨头，重新完全控制您的数据。

在下一章中，我们将研究使用一些服务器端应用程序，例如数据库和 nginx，来处理存储 GPIO 数据以监测天气。
