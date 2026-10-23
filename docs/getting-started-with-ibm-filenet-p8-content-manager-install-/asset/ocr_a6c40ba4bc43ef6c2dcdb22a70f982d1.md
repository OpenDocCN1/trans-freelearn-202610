Because we haven't changed any fundamental configuration items for our CE server installation, we have only a couple of real steps to perform in applying the fix pack. If you have not kept the original configuration profile, you will have to consult the product documentation mentioned in the fix pack instructions and do a more detailed procedure. We'll assume you were either smart or lucky, and we'll just describe the short procedure:

+   - **Bootstrapping**. Double-click the **Configure Bootstrap Properties** task in the configuration profile tree view. Change the **Bootstrap Operation** to **Upgrade**. Right-click the **Configure Bootstrap Properties** task and select **Run Task**. This will modify the fix pack CE EAR file to include the bootstrap information from the initial installation.

![](bbox=[0.153, 0.346, 0.816, 0.534])

+   - **Deployment**: Right-click the **Deploy Application** task and select **Run Task**. This will deploy the bootstrapped CE EAR file into the application server. If all went well, you are done.

> We are often unlucky in the deployment step and have experienced failures for a variety of reasons. If that happens to you, you can easily deploy the CE EAR with your application server's tools. From the WAS console, navigate to **Applications | Application Types | WebSphere enterprise applications**. Click the **Install** button, navigate to the bootstrapped CE EAR file (using the **Remote file system** option unless you are running your browser on the CE server machine), and follow the additional steps to deploy the CE EAR.

+   - From the WAS console, check to see if the FileNetEngine application is running. If not, start it.