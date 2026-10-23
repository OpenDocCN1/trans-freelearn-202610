Launch the *DB2 Configuration Assistant* from the Windows Start menu by navigating **Start | All Programs | IBM DB2 | DB2COPY1 (Default) | Set-up Tools | Configuration Assistant**. (Your DB2 instance name might be different from **DB2COPY1**.) The *DB2 Configuration Assistant* provides a convenient graphical interface for configuring DB2 client software connectivity to a remote database. Locate and configure a connection for database VWDB in DB2 instance db2inst1 on host **wjc-rhel.example.net**. The tool provides many helpers for browsing or discovering these components. Give the database an alias that is the same as the database name (VWDB).

If you find yourself manually configuring the DB2 connectivity, you can find or confirm the port number by running the *DB2 Control Center* for instance db2inst1 on the DB2 server (our Linux server). Right-click the node for instance **db2inst1** and select **Setup Communications**. From the panel that appears, click the **Properties** button for **TCP/IP**. The connectivity parameters will be displayed, including the port number. In our case, it was port 3801. Also make sure that the port is not blocked by a firewall so that **wjc-w2k3.example.net** can connect to it.

![](bbox=[0.308, 0.426, 0.663, 0.595])

After you have configured the local connectivity to the remote database, the **DB2 Configuration Assistant** gives you the opportunity to test the connection. You should do so, testing with both the **f_sw** and the **f_maint** userid. Assuming that all went as expected, you can close the **DB2 Configuration Assistant**.

# PE server installation and configuration

Now that DB2 client software and its connectivity to the DB2 server have been taken care of, it’s time to install and configure FileNet software on the Windows server.