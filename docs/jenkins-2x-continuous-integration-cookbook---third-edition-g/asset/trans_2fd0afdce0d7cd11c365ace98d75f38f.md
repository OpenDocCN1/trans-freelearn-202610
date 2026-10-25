
15. 确认 LDAP 目录中 admin 条目的管理员密码：

16. 在“是否省略 OpenLDAP 服务器配置？”中选择“否”：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_218_0.png)

21. 在“是否在清除 slapd 软件包时移除数据库？”中选择“否”：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_218_1.png)

22. 在“移动旧数据库”对话框中选择“是”：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_218_2.png)

23. 在“是否允许 LDAPv2 协议？”中选择“否”：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_219_0.png)

24. 完成所有配置后，安装过程即告完成。

```
Creating initial configuration... done.
Creating LDAP directory... done.
* Starting OpenLDAP slapd [ OK ]
Setting up ldap-utils (2.4.31-1+nmu2ubuntu8.4) ...
Processing triggers for libc-bin (2.19-0ubuntu6.13) ...
Processing triggers for ufw (0.34~rc-0ubuntu2) ...
Processing triggers for ureadahead (0.100.0-16) ...
root@myopenldap:/home/mitesh51#
root@myopenldap:/home/mitesh51# sudo dpkg-reconfigure slapd
* Stopping OpenLDAP slapd [ OK ]
Moving old database directory to /var/backups:
- directory unknown... done.
Creating initial configuration... done.
Creating LDAP directory... done.
* Starting OpenLDAP slapd [ OK ]
Processing triggers for libc-bin (2.19-0ubuntu6.13) ...
root@myopenldap:/home/mitesh51#
```

你将安装 PHPldapadmin 来管理 OpenLDAP。在 PuTTY 控制台中使用命令 sudo apt-get install phpldapadmin 进行安装：

```
root@myopenldap:/home/mitesh51# sudo apt-get install phpldapadmin
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following extra packages will be installed:
apache2-bin apache2-data libapache2-mod-php5 libapr1 libaprutil1
libaprutil1-dbd-sqlite3 libaprutil1-ldap php5-cli php5-common php5-json
php5-ldap php5-readline ssl-cert
Suggested packages:
apache2-doc apache2-suexec-pristine apache2-suexec-custom apache2-utils
php-pear php5-user-cache openssl-blacklist
The following new packages will be installed:
apache2-bin apache2-data libapache2-mod-php5 libapr1 libaprutil1
libaprutil1-dbd-sqlite3 libaprutil1-ldap php5-cli php5-common php5-json
php5-ldap php5-readline phpldapadmin ssl-cert
0 upgraded, 15 newly installed, 0 to remove and 0 not upgraded.
Need to get 6,881 kB of archives.
After this operation, 30.9 MB of additional disk space will be used.
Do you want to continue? [Y/n] Y
Get:1 http://azure.archive.ubuntu.com/ubuntu/ trusty/main libapr1 amd64 1.5.0-1 [85.1 kB]
Get:2 http://azure.archive.ubuntu.com/ubuntu/ trusty/main libaprutil1 amd64 1.5.3-1 [76.4 kB]
Get:3 http://azure.archive.ubuntu.com/ubuntu/ trusty/main php5-json amd64 1.3.2-2build1 [34.4 kB]
Get:4 http://azure.archive.ubuntu.com/ubuntu/ trusty-updates/main php5-common amd64 5.5.9+dfsg-1ubuntu4.22 [449 kB]
Get:5 http://azure.archive.ubuntu.com/ubuntu/ trusty-updates/main php5-ldap amd64 5.5.9+dfsg-1ubuntu4.22 [19.1 kB]
Get:6 http://azure.archive.ubuntu.com/ubuntu/ trusty-updates/main php5-cli amd64 5.5.9+dfsg-1ubuntu4.22 [2,154 kB]
Get:7 http://azure.archive.ubuntu.com/ubuntu/ trusty-updates/main php5-readline amd64 5.5.9+dfsg-1ubuntu4.22 [12.1 kB]
Get:8 http://azure.archive.ubuntu.com/ubuntu/ trusty/main libaprutil1-dbd-sqlite3 amd64 1.5.3-1 [10.5 kB]
Get:9 http://azure.archive.ubuntu.com/ubuntu/ trusty/main libaprutil1-ldap amd64 1.5.3-1 [8,634 B]
```

```
Get:10 http://azure.archive.ubuntu.com/ubuntu/ trusty-updates/main apache2-bin amd64 2.4.7-1ubuntu4.17 [845 kB]
Get:11 http://azure.archive.ubuntu.com/ubuntu/ trusty-updates/main apache2-data all 2.4.7-1ubuntu4.17 [160 kB]
Get:12 http://azure.archive.ubuntu.com/ubuntu/ trusty-updates/main apache2 amd64 2.4.7-1ubuntu4.17 [87.4 kB]
Get:13 http://azure.archive.ubuntu.com/ubuntu/ trusty-updates/main libapache2-mod-php5 amd64 5.5.9+dfsg-1ubuntu4.22 [2,194 kB]
Get:14 http://azure.archive.ubuntu.com/ubuntu/ trusty/main ssl-cert all 1.0.33 [16.6 kB]
Get:15 http://azure.archive.ubuntu.com/ubuntu/ trusty-updates/universe phpldapadmin all 1.2.2-5ubuntu1.1 [730 kB]
Fetched 6,881 kB in 5s (1,305 kB/s)
Preconfiguring packages ...
Selecting previously unselected package libapr1:amd64.
(Reading database ... 29206 files and directories currently installed.)
Preparing to unpack .../libapr1_1.5.0-1_amd64.deb ...
Unpacking libapr1:amd64 (1.5.0-1) ...
...
Processing triggers for man-db (2.6.7.1-1ubuntu2) ...
Processing triggers for ureadahead (0.100.0-16) ...
Processing triggers for ufw (0.34~rc-0ubuntu2) ...
Setting up libapr1:amd64 (1.5.0-1) ...
Setting up libaprutil1:amd64 (1.5.3-1) ...
...
Setting up apache2 (2.4.7-1ubuntu4.17) ...
Enabling module mpm_event.
.
.
Enabling site 000-default.
* Starting web server apache2
AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 10.0.0.4. Set the 'ServerName' directive globally to suppress this message
*
Setting up ssl-cert (1.0.33)
sent invalidate(group) request, exiting
.
Setting up php5-common (5.5.9+dfsg-1ubuntu4.22) ...

Creating config file /etc/php5/mods-available/pdo.ini with new version
php5_invoke: Enable module pdo for apache2 SAPI
php5_invoke: Enable module pdo for cli SAPI
.
.
Enabling module mpm_prefork.
apache2_switch_mpm Switch to prefork
* Restarting web server apache2
AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 10.0.0.4. Set the 'ServerName' directive globally to suppress this message
[ OK ]
apache2_invoke: Enable module php5
* Restarting web server apache2
AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 10.0.0.4. Set the 'ServerName' directive globally to suppress this message
[ OK ]
Processing triggers for libc-bin (2.19-0ubuntu6.13) ...
root@myopenldap:/home/mitesh51#
```

以编辑模式打开 `/etc/phpldapadmin/config.php`，并更新以下内容：

- `$servers->setValue('server','host','52.172.200.249')` 设置为所创建虚拟机的 IP 地址
- `$servers->setValue('server','base',array('dc=jenkinscookbook,dc=net'));`
- `$servers->setValue('login','bind_id','cn=admin,dc=jenkinscookbook,dc=net');`
- `$config->custom->appearance['hide_template_warning'] = true;`

转到创建虚拟机时所在的资源组。找到网络安全组。

单击“添加入站安全规则”，然后添加 HTTP 规则：

# 添加入站安全规则

Microsoft Azure myopenladap-nsg - 入站安全规则 > 添加入站安全规则

myopenladap-nsg

高级

* 名称
HTTP
* 优先级
1010
* 源
任何 CIDR 块 标记
服务
HTTP
* 协议
任意 TCP UDP
* 端口范围
80
* 操作
拒绝 允许
确定

同时添加 Ping 命令，以便我们可以测试虚拟机的 IP 地址是否可访问：

这是 phpLDAPadmin 的 Web 界面：

单击域组件以查看你正在使用的登录账号：

现在你需要添加组织单位、组和用户。单击“在此创建新条目”。选择“Generic: Organizational Unit”模板：

输入 groups 并单击“创建对象”：

单击“提交”：

选择新创建的组织单位并验证内容：

[195]

同样，在组织单位中创建用户：

在 phpldapadmin Web 界面中提交并验证对象：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_226_0.png)

现在，你需要在 groups 中创建具体的组。选择 groups 并单击“创建子条目”：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_227_0.png)

选择“Generic: Posix Group”：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_227_1.png)

输入组名并单击“创建对象”：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_228_0.png)

提交更改：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_228_1.png)

同样，创建 jenkinsusers 组：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_228_2.png)

在 phpldapadmin Web 界面中验证这两个组：

[199]

