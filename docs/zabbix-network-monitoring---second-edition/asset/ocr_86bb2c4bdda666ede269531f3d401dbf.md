# # Authentication methods

Before we look at more detailed user configuration, it might be helpful to know that Zabbix supports three authentication methods. Navigate to **Administration | Authentication** to take a look at authentication configuration:

![](bbox=[0.182, 0.189, 0.819, 0.577])

As can be seen here, these are the three authentication methods:

+   - **HTTP:** Users are authenticated with web server HTTP authentication mechanisms. Support for HTTP authentication basically allows the use of any of the authentication methods for Zabbix that the web server supports, and in the case of the Apache HTTPD daemon, there are many.
+   - **LDAP:** Users are authenticated using an LDAP server. This can be handy if all enterprise users that need access to Zabbix are already defined in an LDAP structure. Only user passwords are verified; group membership and other properties are not used. A Zabbix user account must also exist for the login to be successful.
+   - **Internal:** With this method, users are authenticated using Zabbix's internal store of users and passwords. We will use this method.