Refer to figure 21.19; if you open either the _s or _r table, you can see the records inserted by SCS in the tables with the columns corresponding to the attributes we specified in Site Publishing Configuration under the Additional Properties (attributes) field.

![](bbox=[0.17, 0.2, 0.83, 0.35])

Figure 21.19: Published records in the test_publish_dev_doc_s table

You can see that the attributes/properties we included in the Site Publishing Configuration are present as columns in the table— title, subject, cust_attrb3, cust_attrb2, object_name, etc.

The records for repeating valued attributes can be found in the corresponding _r table (i.e. test_publish_dev_doc_r).

# 21.6 Unlocking Locked Publishing Operations

There are times when the publishing operation gets locked (almost endlessly!). This could be because of some publishing error or some network/hardware failure or because of multiple publishing operations running concurrently.

Under such scenarios, you can unlock the publishing configuration by one of the following means:

+   1. Stop and start the SCS Source service.
+   2. Execute the unlockConfig.bat script found under the following location on the SCS Source host:
+   %DM_HOME%\webcache\bin
+   Example: c:\Documentum\product\5.2\webcache\bin
+   Execute the unlockConfig.bat script through the command prompt as follows:
+   unlockConfig -u <user> -p <password> [-h <host> | -o <config-id>] <docbase-name>
+     - Unlocking ALL publishing operations on a specific SCS source host for a specific Docbase:
+       unlockConfig -u dummyuser -p userpasswd -h testhost dev_doc
+     - Unlocking a specific publishing operation for a specific Docbase:
+       unlockConfig -u dummyuser -p userpasswd -o 08002232800006d5 dev_doc