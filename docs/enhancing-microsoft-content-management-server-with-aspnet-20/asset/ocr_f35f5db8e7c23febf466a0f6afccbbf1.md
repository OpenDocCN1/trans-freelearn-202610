+   8. Click **Next** and wait while IIS is installed; you may be prompted to insert your Windows XP CD-ROM.
+   
+   9. Click **Finish** when the **Completing the Windows Component Wizard** page appears, and close **Add or Remove Programs**.

> **When installing on Windows Server 2003 SP1.**
> Install the Application Server Role with ASP.NET (but not FrontPage Server Extensions) using the Manage My Server application. Then use **Add or Remove Programs... Windows Components** to add Active Server Pages and Server Side Includes. IIS Manager cannot be used to enable these; **Add or Remove Programs** must be used or the MCMS installer will not recognize them!
> 
> In addition, create a new virtual website to host the Server Configuration Application. This site should be configured to use Windows Authentication and deny access from hosts other than localhost. Preferably this site hosting the SCA should also use Secure Sockets Layer (SSL). If this is not possible, configure the site to use a port other than 80.

# # Configuring Internet Information Services

We will initially configure IIS to use Windows Authentication. Later in the book we will explore alternative authentication methods.

+   1. From the **Start Menu**, click **Run...**
+   
+   2. In the **Open** textbox, enter **inetmgr** and click **OK**.
+   
+   3. Expand the **Local Computer** node, and the **Websites** folder.
+   
+   4. Right-click **Default Website** and click **Properties**.
+   
+   5. Click the **Directory Security** tab, and within the **Anonymous access and authentication control** frame, click **Edit...**
+   
+   6. Uncheck the **Anonymous Access** checkbox and select the **Integrated Windows Authentication** checkbox, and click **OK**.