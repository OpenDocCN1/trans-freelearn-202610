# Add a user to the database

This database user will be used in configuring the datasources within the J2EE application server. Select the database GCD and then navigate to User and Group Objects | DB Users. Add user cedbuser by selecting it from the dropdown menu. This is the same user we added at the operating system level earlier; we’re just giving it database privileges now. This user must be given the authority to:

+   - Connect to database, Create tables, and Create schemas implicitly (all set on the Database tab)
+   - Use the User temporary (CEUSRTEMP) and Large (CEDATA) tablespaces that we created above (set via the Table Space tab)
+   - SELECT for tables SYSIBM.SYSTABLES and SYSIBM.SYSCOLUMNS (set via the Table tab), and view SYSCAT.DATATYPES (set via the View tab)

![](bbox=[0.26, 0.35, 0.71, 0.75])