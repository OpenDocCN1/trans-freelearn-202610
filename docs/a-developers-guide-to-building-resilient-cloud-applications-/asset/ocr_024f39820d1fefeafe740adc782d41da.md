Once created, we can explore the different settings, as presented in the following screenshot:

![](bbox=[0.169, 0.157, 0.866, 0.594])

Figure 8.25 – Azure Files settings

+   3. If we select **Connect**, we can configure the connection to the Azure file share from Windows, Linux, or macOS. To secure the connection, select **Active Directory** or **Storage account key** for Windows. We can change **Tier** from **Transaction optimized** to **Cold**, for example, or from **Cold** to **Hot**.

Sometimes, it’s not easy to decide when to use Azure file shares instead of blobs or disk shares in your solution. If we compare the different features, we can decide which solution is suitable. If we compare Azure Files to Azure Blob side by side, we see that Azure Files provides an SMB and NFS interface, client libraries, and the REST interface, which allows access from anywhere to store files. Azure Blob provides client libraries and the REST interface, which allows unstructured data with flat namespaces to be stored and accessed at a massive scale in the blobs.