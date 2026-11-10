+   5. Specify the primary DocBroker host machine and port number (default shown) so that the Servers projecting to the specified DocBroker can be connected to by DAB.
+   6. Click the Next button.

![](bbox=[0.23, 0.205, 0.77, 0.475])

Figure 8.6: Specifying DocBroker host and port

Behind the scenes, Documentum creates the dmc1.ini file under C:\WINNT or C:\WINDOWS (depending on the OS) on the desktop machine and adds the following entries:

```
[DOCBROKER_PRIMARY]
host =content-server_host
port =1489
```

Once DAB has been installed, if someone mistakenly changes the name of the specified DocBroker host to an invalid value (say content-server_host222) in dmc1.ini, then the following error message is seen on opening DAB:

```
[DM_DOCBROKER_E_HOST_NAME]error: "Unable to find host with name: content-server_host222. Network specific error: Host not found. Network error: invalid host name."
```