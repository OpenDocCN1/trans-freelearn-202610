
![](img/18a0aa8d36427a4c069b30410f998b8e_29_0.png)

5. Once complete, click Next, and wait while the System Configuration Check completes.
6. On the Welcome to the Microsoft SQL Server Installation Wizard page, click Next.
7. On the System Configuration Check page, all Actions should report a Status of Success.

![](img/18a0aa8d36427a4c069b30410f998b8e_30_0.png)

8. Click **Next** and wait while Setup prepares the installation.

9. On the **Registration Information** page, personalize your installation details, enter your product key, and click **Next**.

![](img/18a0aa8d36427a4c069b30410f998b8e_30_1.png)

10. On the Components to Install page, select the SQL Server Database Services and Workstation components, Books Online and development tools checkboxes, and click Next.

![](img/18a0aa8d36427a4c069b30410f998b8e_31_0.png)

11. On the Instance Name page, select the Default instance radio button, and click Next.

![](img/18a0aa8d36427a4c069b30410f998b8e_31_1.png)

## Installation

12. On the Existing components page, check the SQL Server Database Services 8.00.2039 checkbox, and click Next.

![](img/18a0aa8d36427a4c069b30410f998b8e_32_0.png)

13. On the Upgrade Logon Information page, select the Windows Authentication Mode radio button, and click Next.

![](img/18a0aa8d36427a4c069b30410f998b8e_32_1.png)

Wait while the upgrade is analyzed.

14. On the Service Account page, select SQL Browser from the Service combo box, enter your SQL Server Service Account details, and click Next.

![](img/18a0aa8d36427a4c069b30410f998b8e_33_0.png)

15. On the Error and Usage Report Settings page, choose if you would like to submit error and usage reports to Microsoft, and click Next.

![](img/18a0aa8d36427a4c069b30410f998b8e_33_1.png)

16. On the Ready to Install page, click Install.

![](img/18a0aa8d36427a4c069b30410f998b8e_34_0.png)

17. Wait while SQL Server is upgraded, and click Next.

![](img/18a0aa8d36427a4c069b30410f998b8e_34_1.png)

18. On the Completing Microsoft SQL Server 2005 Setup page, click Finish.

![](img/18a0aa8d36427a4c069b30410f998b8e_35_0.png)

To avoid possible ODBC errors while saving in the Web Author following an upgrade to SQL Server 2005, perform an **IISRESET** on the MCMS server machine. Note that for a production system, we would stop IIS before upgrading the SQL Server instance.

## Installing SQL Server 2005 Service Pack 1

We will now install Service Pack 1 for SQL Server 2005, which can be downloaded from `http://www.microsoft.com/downloads/details.aspx?FamilyID=cb6c71ea-d649-47ff-9176-e7cac58fd4bc`.

1. Double-click the installer file, and wait while the files are extracted.
2. On the **Welcome** page, click **Next**.
3. On the **End User License Agreement** page, check the **I accept the licensing terms and conditions** checkbox, and click **Next**.
4. On the **Feature Selection** page, click **Next**.
5. On the **Authentication Mode** page, click **Next**.
6. On the **Ready to Install** page, click **Install**, and wait while the Service Pack is installed. During the installation, you may be prompted regarding **Pending Reboot Files**. If you are, click **Yes**.
7. When the **Locked Files Found** dialog appears, click **Continue**.
8. In the **Computer Reboot Required** dialog, click **OK**.
9. Click **Next**, and on the **Installation Complete** page, click **Finish**.
10. Restart your computer.

## Modifying the CreateBackgroundProcessingJob Stored Procedure

Before proceeding, you need to modify one of the MCMS Stored Procedures to ensure its compatibility with SQL Server 2005.

1. Select **Start | All Programs | Microsoft SQL Server 2005 | SQL Server Management Studio**.
2. In the **Connect To** dialog, click **Connect**.
3. In the **Object Explorer**, expand the **Databases** folder, right-click the MCMS database, and click **New Query**.
4. Open Internet Explorer and navigate to `http://support.microsoft.com/default.aspx?id=906145#XSLTH3161121120121120120`.
5. Copy and paste the SQL Script in the grey textbox to the **Query Window** in **SQL Server Management Studio**.
6. On the **SQL Editor** toolbar, click the **Execute** button.
7. Close **SQL Server Management Studio**.

This completes the steps to upgrade SQL Server 2005. You can now move ahead to Chapter 2.

## Fresh Installation of Microsoft Content Management Server 2002 Service Pack 2

This section details the steps required to perform a fresh installation of SP2 using SQL Server 2005. All components are installed on a single machine and to their default locations. The following installation order provides all necessary prerequisites and is the optimal installation order from a time perspective. We start with a base installation of Windows XP Professional SP2, with all Windows updates installed. The outline process for the installation is as follows:

- Configure Local Security Policy.
- Create Service Accounts.
- Install Internet Information Services.
- Configure Internet Information Services.
- Install SQL Server 2005.
- Install SQL Server 2005 SP1.
- Configure SQL Server 2005.
- Bypass installation of Visual Studio.NET 2003.
- Install MCMS 2002 prerequisites.
- Install MCMS 2002 SP1a.
- Remove temporary items.
- Install Visual Studio 2005.
- Install MCMS 2002 SP2.
- Configure MCMS using the Database Configuration Application.
- Configure the development environment.

We will perform all steps while logged on as a local machine administrator.

## Configuring Local Security Policy

In order to authenticate Service Account identities, we need to modify the Windows XP Local Security policy. Note that this section is not required if your machine is a member of a domain.

1. Select Start | Control Panel.
2. Double-click Administrative Tools, and double-click Local Security Policy.
   (If you do not see Administrative Tools, click the Switch to Classic View link.)
3. Expand the Local Policies node, and click the Security Options node.
4. In the Policy pane, scroll to locate the Network access: Sharing and security model for local accounts item and then double-click it.
5. Set the combo box to **Classic – local users authenticate as themselves** and click **OK**.

![](img/18a0aa8d36427a4c069b30410f998b8e_38_0.png)

6. Close **Local Security Settings** and **Administrative Tools**.

## Creating Service Accounts

We will create two Service Accounts, one for use by SQL Server and one for the MCMS System Account.

1. From the **Desktop**, right-click **My Computer** and select **Manage**.
2. In **Computer Management**, expand the **Local Users and Groups** node.
3. Right-click on the **Users** folder and click **New User...**
4. In the **New User** dialog, enter **mssql** in the **User name** textbox.
5. Provide a **Full Name** and **Description**.
6. Provide a **Password**.
7. Uncheck the **User must change password at next logon** checkbox.
8. Check the **User cannot change password** and **Password never expires** checkboxes and click **Create**.

![](img/18a0aa8d36427a4c069b30410f998b8e_39_0.png)

9. In the **New User** dialog, enter **mcmssys** in the **User name** textbox.
10. Provide a **Full Name** and **Description**.
11. Provide a **Password**.
12. Uncheck the **User must change password at next logon** checkbox.
13. Check the User cannot change password and Password never expires checkboxes and click Create.

![](img/18a0aa8d36427a4c069b30410f998b8e_40_0.png)

14. Close the New User dialog and close Computer Management.

## Installing Internet Information Services

The following steps detail the installation of the minimal required components of Internet Information Services (IIS). Any differences to the procedure when installing on Windows Server 2003 SP1 are noted.

1. Select Start | Control Panel.
2. Double-click Add or Remove Programs.
3. Click Add/Remove Windows Components.
4. In the Windows Components Wizard, click (note: not check) Internet Information Services (IIS) followed by Details...
5. In the Internet Information Services (IIS) dialog, click (note: not check) World Wide Web Service followed by Details...
6. In the World Wide Web Service dialog, check the World Wide Web Service item and click OK.
