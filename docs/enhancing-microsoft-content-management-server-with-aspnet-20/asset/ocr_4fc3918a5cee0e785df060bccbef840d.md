+   - **Developer tools for VS.NET 2003 disappear after installing MCMS 2002 Service Pack 2.** A hotfix for this has been released and can be requested from Microsoft Support as KB 914195.
+   - **Developer Tools for VS.NET 2003 do not work with some regional settings.** A hotfix for this has been released and can be requested from Microsoft Support as KB 914195.

In addition if you have performed a fresh installation of MCMS SP2 and wish to move an existing database to your new installation, you need to obtain the KB913401 hotfix and follow the steps detailed in the *Prepare the MCMS Database for SQL Server 2005* section under our discussion on upgrading to MCMS 2002 Service Pack 2 towards the beginning of this chapter.

# Installation Tips

The following section provides two tips for installations on different editions of Windows or across separate machines.

# Installing MCMS SP2 on Windows Server 2003 X64 Edition

MCMS Service Pack 2 introduces support for 64-bit versions of Windows Server 2003. However MCMS requires IIS to be configured to run in 32-bit compatibility mode. This means that all IIS applications (not just MCMS ones) cannot take advantage of the additional addressable memory on the X64 platform. In order to install MCMS SP2 on this platform, the following extra steps are necessary:

+   - ASP.NET 2.0 must be manually registered with IIS by executing the `aspnet_regiis` utility.
+   - IIS must be configured to run in the 32-bit compatibility mode, using the following command from the `c:\Inetpub\adminscripts` folder:
+     `cscript adsutil.vbs set w3svc/AppPools/Enable32bitAppOnWin64 1`

# Installing the Required SQL Server 2005 Components

When making use of SQL Server on separate machine from the one running MCMS SP2, there is a prerequisite for SQLDMO, also known as SQL Server Client Tools, on the MCMS machine. This can be installed from your SQL Server 2005 distribution by: