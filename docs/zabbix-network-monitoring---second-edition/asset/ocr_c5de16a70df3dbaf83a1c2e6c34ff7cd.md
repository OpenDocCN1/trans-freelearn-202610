+   - A special item (usually another user parameter) that adds an `atd` job. This solution is a bit more complicated, but allows us to keep interval management in Zabbix while still allowing the use of long-running scripts for data collection. See http://zabbix.org/wiki/Escaping_timeouts_with_atd for more detail.

> There are reports that `atd` can be crashed in RHEL 5 and 6, and possibly other distributions. If using this method, monitor `atd` as well.

# External checks

All the check categories we explored before cover a very wide range of possible devices, but there's always that one which doesn't play well with standard monitoring protocols, can't have the agent installed, and is buggy in general. A real-life example would be a UPS that provides temperature information on the web interface, but does not provide this data over SNMP. Or maybe we would like to collect some information remotely that Zabbix does not support yet—for example, monitoring how much time an SSL certificate has until it expires.

In Zabbix, such information can be collected with external checks or external scripts. While user parameters are scripts run by the Zabbix agent, external check scripts are run directly by the Zabbix server.

First, we should figure out the command to find out the remaining certificate validity period. We have at least two options here:

+   - Return the time when the certificate expires
+   - Return 0 or 1 to identify that the certificate expires in some period of time

Let's try out both options.

# Finding a certificate expiry time

We could find out the certificate expiry time with an OpenSSL command like this:

```
$ echo | openssl s_client -connect www.zabbix.com:443 2>/dev/null | openssl x509 -noout -enddate
```

> Feel free to use any other domain for testing here and later.