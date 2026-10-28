# Deploying an ASP.NET Core application to Azure Service Fabric

Right-click on the application, then select **Publish**. In the target profile, we will select `PublishProfiles\Cloud.xml`. We will select the account related to the Azure portal. In the connection endpoint, we can select an already created Azure Service Fabric cluster or you can create your cluster using Visual Studio. To do that, we will click on **Add Custom Endpoint**.

![](bbox=[0.143, 0.229, 0.844, 0.686])

Figure 5.26 – Add Custom Endpoint

Next, we will add value to the endpoint. After following the previous steps to create the cluster, we will click on **Create a new Cluster**. The following figure shows the window for creating a Service Fabric cluster: