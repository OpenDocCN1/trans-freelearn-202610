![](bbox=[0.294, 0.148, 0.678, 0.383])

+   1. From **System View**, select **K2 Index Servers | Add a K2 Index Server**. Type **wjc-index-server** as the **Service Alias** and use **Port 9960**. Click **Next** and change the access type to **Authorized Administrator**. Click **Finish**.
+   
+   2. From **System View**, select **K2 Brokers | Add a K2 Broker**. Type **wjc-broker** as the **Service Alias** and use **Port 9990**. Click **Finish**.
+   
+   3. From **System View**, select **K2 Servers | Add a K2 Server**. Type **wjc-search-server** as the **Service Alias** and use **Port 9920**. Click **Next** twice, which will bring you to the screen for K2 brokers. From the drop-down, select **wjc-broker** that we just created above. Accept the defaults for various types of worker threads, and click **Finish**.
+   
+   4. From **System View**, select **Collections | Manage Style Sets**. If you see a notice about the style sheet editor, you can ignore it (we don't use that feature). Click **Import**. Type **FileNet_FileSystem_PushAPI** as the **Style Set Alias** and type **/opt/ibm/FileNet/ContentEngine/verity/data/stylesets/FileNet_FileSystem_PushAPI** as the **Source Path**. The style sheet subdirectory will be found in that directory relative to the CSE software installation location. Click **Import**.
+   
+   5. From **System View**, select **K2 Ticket Servers | Add a K2 Ticket Server**. Type **wjc-ticket-server** as the **Service Alias** and use **Port 9910**. Click **Next** and make sure that **UNIX** is selected as the **Login Module** type. Click **Next** and make sure that **File** and **Memory** is selected as the **Persistent Store Module** type. Click **Finish**.