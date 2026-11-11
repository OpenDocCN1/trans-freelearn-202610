
## Installing Visual Studio 2005

Visual Studio 2005 can be installed side by side with Visual Studio.NET 2003. Once we have completed the upgrade, we can remove Visual Studio.NET 2003 if we wish to only develop MCMS applications using SP2 and ASP.NET 2.0.

- 1. Insert the Visual Studio 2005 DVD, and on the splash screen, click **Install Visual Studio 2005**.
- 2. On the **Welcome to the Microsoft Visual Studio 2005 installation wizard** page, click **Next**.
- 3. On the **Start Page**, select the **I accept the terms of the License Agreement** checkbox, enter your **Product Key** and **Name**, and click **Next**.
- 4. On the **Options Page**, select the **Custom** radio button, enter your desired **Product install path**, and click **Next**.

![](img/18a0aa8d36427a4c069b30410f998b8e_20_0.png)

- 5. On the second Options page, select the Visual C# and Visual Web Developer checkboxes within the Language Tools section, and the Tools checkbox within the .NET Framework SDK section. Click Install.

![](img/18a0aa8d36427a4c069b30410f998b8e_21_0.png)

Feel free to install any additional features you may wish to use. The above selections are all that’s required to follow the examples in this book.

- 6. Wait (or take a coffee break) while Visual Studio 2005 is installed. When the **Finish Page** appears, click **Finish**.

![](img/18a0aa8d36427a4c069b30410f998b8e_22_0.png)

- 7. From the **Visual Studio 2005 Setup** dialog, you can choose to install the Product Documentation (MSDN Library) if desired.
- 8. From the **Visual Studio 2005 Setup** dialog, click **Check for Visual Studio Service Releases** to install any updates that may be available.
- 9. Click Exit.

## Installing MCMS 2002 Service Pack 2

Next, we will install MCMS Service Pack 2.

- 1. From the **Start Menu**, click **Run...**
- 2. In the **Open** textbox, enter **IISRESET /STOP** and click **OK**. Wait while the IIS Services are stopped.
- 3. Double-click the SP2 installation package.
- 4. On the **Welcome to Microsoft Content Management Server 2002 SP2 Installation Wizard** page, click **Next**.

![](img/18a0aa8d36427a4c069b30410f998b8e_23_0.png)

### Installation

- 5. Select the **I accept the terms of this license agreement** radio button, and click **Next**.

![](img/18a0aa8d36427a4c069b30410f998b8e_24_0.png)

- 6. On the **ready to begin the installation** page, click **Next**.

![](img/18a0aa8d36427a4c069b30410f998b8e_24_1.png)

Wait while Service Pack 2 is installed.

- 7. During installation you may be prompted for the MCMS 2002 SP1a CD-ROM.
- 8. Once **The Installation Wizard has completed** page, click **Finish**.
- 9. If prompted, click **Yes** on the dialog to restart your computer, which will complete the installation.
- 10. Otherwise, from the **Start Menu**, click **Run...**
- 11. In the **Open** textbox, enter **IISRESET /START** and click **OK** to restart the IIS services.

> Stopping IIS prior to the installation of SP2 avoids potential problems with replacing locked files during the installation, and can prevent the requirement to reboot.

## Configuring the Development Environment

Before continuing, a few additional steps are required to configure the development environment. We will:

- Configure the shortcut that opens **Site Manager** to bypass the **Connect To** dialog.
- Install the MCMS website and item templates in Visual Studio.

## Site Manager Shortcut

During the installation of SP2 the **Site Manager** Start-menu shortcut will be overwritten. To configure **Site Manager** to bypass the **Connect To** dialog, take the following steps:

- 1. Select **Start | All Programs | Microsoft Content Management Server**.
- 2. Right-click the **Site Manager** shortcut and click **Properties**.
- 3. In the **Target** textbox, replace "C:\Program Files\Microsoft Content Management Server\Client\NRClient.exe" http://NR/System/ClientUI/login.asp with "C:\Program Files\Microsoft Content Management Server\Client\NRClient.exe" http://localhost/NR/System/ClientUI/login.asp.
- 4. Click **OK**.

## Visual Studio Templates

The installation of MCMS Service Pack 2 automatically registers the MCMS developer tools such as MCMS Template Explorer in Visual Studio 2005. However, before we can create MCMS applications with Visual Studio, we need to make the website and item templates available.

- 1. Select Start | All Programs | Microsoft Visual Studio 2005 | Visual Studio Tools | Visual Studio 2005 Command Prompt.
- 2. Execute the following commands, replacing MCMS_INSTALL_PATH with the install location of MCMS (usually c:\Program Files\Microsoft Content Management Server) and PATH_TO_MY_DOCUMENTS_FOLDER with the location of your My Documents folder:
  xcopy "MCMS_INSTALL_PATH\DevTools\NewProjectWizards80\Visual Web Developer" "PATH_TO_MY_DOCUMENTS_FOLDER\Visual Studio 2005\Templates\ProjectTemplates\Visual Web Developer" /E
  xcopy "MCMS_INSTALL_PATH\DevTools\NewItemWizards80\Visual Web Developer" "PATH_TO_MY_DOCUMENTS_FOLDER\Visual Studio 2005\Templates\ItemTemplates\Visual Web Developer" /E
- 3. Execute the following command to register the templates with Visual Studio 2005:
  devenv /setup
- 4. Close the command prompt.

This completes the steps to upgrade to SP2, and our environment is now ready for development! We can test our installation by viewing the version number in the SCA, connecting with Site Manager, or by using the Web Author. Of course, any existing MCMS web applications will at this time still be hosted by .NET Framework v1.1.

![](img/18a0aa8d36427a4c069b30410f998b8e_26_0.png)

## Preparing the MCMS Database for SQL Server 2005

Before upgrading our SQL Server installation to SQL Server 2005, we need to prepare the MCMS database so that it is compatible with SQL Server 2005.

- 1. Request the following MCMS hotfix from Microsoft: http://support.microsoft.com/?kbid=913401.
- 2. Run the hotfix executable to extract the files to a local folder, e.g. c:\913401.
- 3. Copy both of the files (_dca.ini and _sp1aTop2upgrade.sql) to the MCMS SQL install folder (typically c:\Program Files\Microsoft Content Management Server\Server\Setup Files\SQL Install). Overwrite the existing files.
- 4. Delete the temporary folder.

At this stage you can also choose to upgrade SQL Server or move forward to Chapter 2.

Additionally it is unnecessary to configure IIS to use ASP.NET 2.0 using the Internet Information Services Snap-In, as Visual Studio 2005 automatically sets this option on each MCMS website application created.
However, if you are installing on Windows Server 2003, you must configure the Virtual Website root and the MCMS Virtual Directory to use ASP.NET 2.0, as it is not possible to use two versions of ASP.NET within the same Application Pool.

The ActiveX controls that are part of HtmlPlaceholderControl are updated with SP2. Therefore you will be prompted to install this control when first switching to edit mode.
If you have pre-installed the controls using regsvr32 or Group Policy as detailed at http://download.microsoft.com/download/4/2/5/4250f79a-c3a1-4003-9272-2404e92bb76a/MCMS+2002-+(complete)+FAQ.htm#51C0CE4B-FC57-45C4-BAAE-12C09421B57B, you might also be prompted, and you will need to update your distribution for the controls.

- 5. Select Start | Microsoft Content Management Server | Data Configuration Application.
- 6. On the splash screen, click Next.
- 7. In the Stop Service? dialog, click Yes.
- 8. On the Select MCMS Database page, click Next.
- 9. In the Upgrade Required dialog, click Yes.
- 10. On the Upgrade Database page, click Next.
- 11. In the Add an Administrator dialog, click No.
- 12. On the Database Configuration Application page, uncheck the Launch the SCA Now checkbox and click Finish.

## Upgrading SQL Server 2005

Before proceeding, be sure to have the details of your existing SQL Server Service Account, which you will need during the upgrade process.

- 1. Insert the SQL Server 2005 CD-ROM or DVD.
- 2. On the splash screen, click the Server components, tools, Books Online, and samples link within the Install section.
- 3. On the End User License Agreement dialog, select the I accept the licensing terms and conditions checkbox and click Next.
- 4. On the Installing Prerequisites dialog, click Install and wait while the prerequisites are installed.
