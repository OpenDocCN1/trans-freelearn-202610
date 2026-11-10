# What is the Transfer User specified in a Site Publishing Configuration?

The Transfer User specified in a Site Publishing Configuration is the user account that SCS uses in order to connect to the target server (web server machine).

# What are the various available mechanisms for unlocking locked SCS publishing operations?

The following methods can be used in order to unlock locked SCS publishing operations: Stopping and starting SCS Source service, executing the unlockConfig.bat script found on SCS Source host, or nullifying the holder attribute in webc_lock object.

# What is the simplest means to monitor the status of SCS Source publish operations, such as the total number of publish operations, break-down in terms of full refreshes and incremental publish operations, etc.?

The easiest means to monitor the status of SCS Source publish operations is by invoking the following URL from your browser:

```
http://<SCS Source host name>:<SCS Source port number>/webcache/state
```

This will return a flat XML file displaying the various publishing operation details you require.

# What is the Web View feature available in Web Publisher?

The Web View feature allows you to preview the content files on your web servers for testing purposes before finally publishing over to the production website.

# How can you access DFC Java interfaces if you are working on a Microsoft COM (Component Object Model) environment?

DFC includes a Documentum Java-COM bridge (DJCB) for accessing DFC Java interfaces in case you are working in a Microsoft COM (Component Object Model) environment.

# What is the inheritance hierarchy of the IDfDocument interface in DFC?

In DFC, IDfDocument interface extends from IDfSysObject interface, which extends from IDfPersistentObject interface, which in turn extends from IDfTypedObject interface.

# What is the use of the com.documentum.fc.client package in DFC?

The com.documentum.fc.client package in DFC contains classes and interfaces used for creating/managing Docbase sessions and for common data manipulation in Docbase.

# What purpose does the IDfClient interface serve in DFC?

The IDfClient interface in DFC serves as a factory for IDfSessionManager objects, which help in creating sessions for establishing connection with servers.

# Mention a few methods available in the IDfSysObject interface in DFC.

A few of the methods available in the IDfSysObject interface in DFC are: checkout() for checking-out and placing a lock on an object, getACL() for fetching the ACL associated with an object, getTitle() for retrieving the title attribute of an object, and setObjectName() for setting the object_name attribute of an object.