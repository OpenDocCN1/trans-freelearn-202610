Actual deployment of XT is mostly a standard WAS EAR deployment done via the WAS console:

+   - Log on to the WAS console for profile AppSrv01 and navigate to **Applications | Application Types | WebSphere enterprise applications.** Click **Install** to add a new application.
+   - On the panel that opens, you're prompted for the location of the XT EAR file, on either the local or remote filesystem. "Local" means the machine where you are running the browser, and "remote" means the machine where WAS is running. In whichever case is appropriate for you, enter the filename **/opt/ibm/FileNet/WebClient/deploy/web_client.ear**, and click **Next**.
+   - Work through the remaining WAS panels for installing a new EAR. We suggest you take the WAS **Fast Path** and accept the defaults offered. Some of the steps may take a bit of time because the files being manipulated are quite large. Be sure to **Save** your changes on the final panel.

You'll be brought back to the **Enterprise Applications** list. You'll see WorkplaceXT in that list, but it will not yet be started. Before starting it, we have to make a few important configuration changes, described in the following subsections.

# Classloader configuration

Click on **WorkplaceXT**, and then click **Class loading and update detection**. Select the radio button to choose **Classes loaded with local class loader first (parent last)**. Click **OK**. (This classloader policy was called "application first" in earlier versions of WAS.)

That should bring you back to the WorkplaceXT configuration page. Click **Manage Modules**, and select **WorkplaceXT**. (The seeming redundancy is because this is the WAR file within the EAR file.) From the drop-down list, choose **Classes loaded with local class loader first (parent last)**. Click **OK**.

**Save** the changes.