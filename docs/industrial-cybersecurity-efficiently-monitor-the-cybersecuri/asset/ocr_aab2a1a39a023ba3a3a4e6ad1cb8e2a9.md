A tool that can help with collecting these configured users and user groups is a tool that's native to Windows called `net.exe`. In the following example, we are running the `net` tool to enumerate all the users on a Windows computer:

```
```
PS C:\tools> net user
User accounts for \\WORKSTATION1



Administrator      DefaultAccount     Guest
pac                RsWebDav           theAttacker

The command completed successfully.
```
```

We can also enumerate security groups, as shown in the following example:

```
```
PS C:\tools> net localgroup
Aliases for \\WORKSTATION1




------------------------------------------------
----
*Access Control Assistance Operators
*Administrators
*Backup Operators
*Cryptographic Operators
*Distributed COM Users
*Event Log Readers
*Guests
*Hyper-V Administrators
*IIS_IUSRS
*Network Configuration Operators
*Performance Log Users
*Performance Monitor Users
*Power Users
*Remote Desktop Users
*Remote Management Users
*Replicator
*SQLServer2005SQLBrowserUser$DESKTOP-JNASSB6
*SQLServerMSSQLServerADHelperUser$DESKTOP-JNASSB6
*SQLServerMSSQLUser$DESKTOP-JNASSB6$FTVIEWX64TAGDB
```
```