Restart the agent daemon and observe monitoring resuming from the Zabbix server. Now, let's make the server uses its certificate. We'll place the certificate in a place where the Zabbix server can use it. In the directory where the Zabbix server configuration file is located, create a new directory called zabbix_server_certs. Restrict access to it, like this:

```
# chown zabbix zabbix_server_certs
# chmod 500 zabbix_server_certs
```

> If using packages that run Zabbix server with a different username such as zabbixs or zabbixsrv, replace the username with the proper one in the two commands.

From the directory where we generated the certificates, copy the certificates over to the new directory:

```
# cp zabbix_ca.crt /path/to/zabbix_server_certs/
# cp zabbix_server.crt /path/to/zabbix_server_certs/
# cp zabbix_server.key /path/to/zabbix_server_certs/
```

Edit zabbix_server.conf, and modify these parameters:

```
TLSCAFile=/path/to/zabbix_server_certs/zabbix_ca.crt
TLSCertFile=/path/to/zabbix_server_certs/zabbix_server.crt
TLSKeyFile=/path/to/zabbix_server_certs/zabbix_server.key
```

Now, restart Zabbix server. Although we have specified the certificates on both agents and the server, passive items still work in unencrypted mode. Let's proceed with making them encrypted. In the Zabbix frontend, navigate to Configuration | Hosts, click on A test host, and switch to the Encryption tab. In the Connections to host selection, choose Certificate, and then click on the Update button. After the server configuration cache has been updated, it will switch to using certificate-based encryption for this host.

> We are changing the configuration for A test host, not Encrypted host.

Going back to our scenario where we slowly rolled out certificate-based configuration to our agents and added it to the server later, we can now disable unencrypted connections on the agent side. Change zabbix_agentd.conf:

```
TLSAccept=cert
```