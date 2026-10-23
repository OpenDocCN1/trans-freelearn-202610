
- 2. Again, from that same **Document Class Properties** dialogue, select the **Property Definitions** tab. Highlight the row for **Document Title** and click **Edit**. On the **General** tab, you will see a similar checkbox for **CBR Enabled**, this time applying only to this specific property. Select the checkbox to enable CBR indexing, and click **OK** to apply the settings and close the panels. (Again, this step may take some time after you click **OK**.)

This completes the CSE installation and configuration. The CE and the CSE are loosely coupled and some indexing is done in batches for performance reasons, so it can take a few minutes for new content to show up in the text index. By default, the CE dispatcher background thread runs every three minutes. Some additional time may be needed for indexing if the CSE has to first extract the text from a word processing document or similar file.

You can test that your CE and CSE servers are configured correctly by adding some documents to Object Store One (for test purposes, plain text documents are the simplest), waiting at least three minutes, and then doing a simple keyword search for those documents from the K2 Dashboard. To do that, open the K2 Dashboard with your web browser and navigate to **System View | K2 Servers | wjc-search-server**. On the page that opens, click the **Test Search** link. Type a search term that you know is present in the document in the **Search for** field and click **Search** button. Your test document should be found.

![](img/2295191c450a0c86b2d82df6cec04526_161_0.png)

Now let's turn to another major component, the Process Engine.

## Process Engine server (PE server)

### Installing Other Components

The PE is not currently supported on Linux (though it is supported on various other flavors of Unix, and it will be supported on Linux in the P8 5.0.0 release), so we will be installing it on a Microsoft Windows server. We can, however, still use the Linux-based DB2 instance that we already installed on wjc-rhel.example.net. In a production environment, the databases would likely be installed on their own set of servers, and we would want the PE database close—in geography and network topology—to the PE servers. For our all-in-one environment, we're simply reusing the already-installed DB2 instance for our own convenience.

At a high level, the following are the steps in installing the PE server:

- 1. Configure operating system accounts on both Linux and Microsoft Windows. There is also an LDAP directory user and group.
- 2. Configure database for PE use.
- 3. Install database client software on Microsoft Windows.
- 4. Install and configure PE server software on Microsoft Windows.
- 5. Configure PE items in CE.

#### Users and groups

As we have done in other examples, the user and group accounts we're using here are the same as the figurative names used in the PPG and the installation worksheet. You can choose different names if you'd like. If you use different names, then watch for the note about aliases when we get to the PE installation program.

On the Microsoft Windows server (where the PE will be installed), create the following. Our environment is not part of a Windows domain, so all of our users and groups are local.

- Group fnop: Members of this group are PE operators.
- Group fnusr: Members of this group are PE users but not operators.
- Group fnadmin: Members of this group are PE administrators.
- User fnsw: Add this user to groups fnop, fnadmin, and fnusr. If there is a Microsoft Windows group Administrators, also add this user to that group. This user account is used to start and stop PE software. We will also use it for installing the PE server software.

On the Linux server (where DB2 is installed), create the following local operating system accounts:

- User f_sw: This is the PE's primary runtime user ID for database activity
- User f_maint: Used by the PE for database maintenance

In the Tivoli (TDS) directory, create the following. If you need help with creating TDS users and groups, refer to the section for adding CE users and groups in Chapter 2.

- Group pe_admin_group: Members of this group automatically get certain administrator privileges in PE.
- User pe_service_user: The PE uses this account when contacting the CE. This user must be a member of the LDAP group pe_admin_group.

#### Database configuration

The database steps consist of creating tablespaces (Linux), configuring user account permissions (Linux), and installing the DB2 client software (Windows).

##### DB2 database and tablespaces

Although we could create the PE-related tablespaces within one of the CE databases, it's easier to see what's going on if we keep things completely distinct. We'll use the same DB2 instance, but we'll create a database for PE use. We'll use the conventional name for this (VWDB), but you could use a different name.

Using the DB2 Command Center for instance db2inst1 (see Chapter 2 if you need a refresher on how to launch it), perform the following actions:

- 1. Create the new database, VWDB, with automatic maintenance.
- 2. All PE-related tablespaces need a minimum page size of 8 KB. The default buffer pool probably has a page size of 4 KB. Within database VWDB, create a new buffer pool with a page size of 8 KB. As a mnemonic, we'll call it VWDB8K. We'll use that buffer pool with all the tablespaces we create in this database.
- 3. Create a new system temporary tablespace.
- 4. Create a new user temporary tablespace.
- 5. Create three large tablespaces with automatic storage and a minimum size of 200 MB. Use these names: VWDATA_TS, VWINDEX_TS, and VWBLOB_TS.
- 6. You can delete the default tablespace that was created with the database.

##### DB2 user permissions

In a previous step, we created a couple of Linux operating system user accounts, f_sw and f_maint, that PE will use for its database interactions with DB2. Now it's time to give database permissions to those accounts. Because we are using Linux user accounts for a PE server running elsewhere, we will use DB2 server authentication. We'll just summarize the necessary permissions here. If you need a refresher on how to do this in DB2, see the DB2 configuration steps for CE in Chapter 2.

For user f_sw:

- 1. Connect to database.
- 2. Create tables.
- 3. Use privilege for each of the "VW" tablespaces that we just created.

For user f_maint:

- 1. Determine the Linux operating system group that has SYSADM rights. You can do that by running the command db2 get dbm cfg and examining the output. In our configuration, it's the group db2iadm1 that was automatically created during DB2 installation.
- 2. Determine the primary group of the DB2 instance owner db2inst1. It's most likely the same group db2iadm1 just mentioned.
- 3. Make the f_maint user a member of the groups in the previous two bullets.

##### DB2 client software

The PE running on the Windows server needs DB2 client software to be able to talk to the DB2 server running on the Linux server. Installation of the DB2 client software is straightforward, and we don't cover it in detail here. We'll just mention a few specific points:

- From the DB2 installation program, you need only install the IBM Data Server Client package. Unless you are really tight on disk space, we suggest you do the "typical" installation option.
- For simplicity, we uncheck the box labeled Enable operating system security.
- The default installation location is C:\Program Files\IBM\SQLLIB\.

Launch the *DB2 Configuration Assistant* from the Windows Start menu by navigating **Start | All Programs | IBM DB2 | DB2COPY1 (Default) | Set-up Tools | Configuration Assistant**. (Your DB2 instance name might be different from **DB2COPY1**.) The *DB2 Configuration Assistant* provides a convenient graphical interface for configuring DB2 client software connectivity to a remote database. Locate and configure a connection for database VWDB in DB2 instance db2inst1 on host **wjc-rhel.example.net**. The tool provides many helpers for browsing or discovering these components. Give the database an alias that is the same as the database name (VWDB).

If you find yourself manually configuring the DB2 connectivity, you can find or confirm the port number by running the *DB2 Control Center* for instance db2inst1 on the DB2 server (our Linux server). Right-click the node for instance **db2inst1** and select **Setup Communications**. From the panel that appears, click the **Properties** button for **TCP/IP**. The connectivity parameters will be displayed, including the port number. In our case, it was port 3801. Also make sure that the port is not blocked by a firewall so that **wjc-w2k3.example.net** can connect to it.

![](img/2295191c450a0c86b2d82df6cec04526_165_0.png)

After you have configured the local connectivity to the remote database, the **DB2 Configuration Assistant** gives you the opportunity to test the connection. You should do so, testing with both the **f_sw** and the **f_maint** userid. Assuming that all went as expected, you can close the **DB2 Configuration Assistant**.

#### PE server installation and configuration

Now that DB2 client software and its connectivity to the DB2 server have been taken care of, it’s time to install and configure FileNet software on the Windows server.
