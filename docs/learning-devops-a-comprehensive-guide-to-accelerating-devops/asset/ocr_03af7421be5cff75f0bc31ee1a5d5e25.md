Then, to configure the build agent to use in the **Agent job** options, we use the Azure
Pipelines agent-hosted Ubuntu 16.04, as shown in the following screenshot:

![](bbox=[0.1338, 0.1528, 0.8662, 0.4069])

Figure 9.29 – Agent job parameters

Finally, the last configuration is the trigger configuration on the **Triggers** tab, which
enables the CI with the trigger of this build at each commit, as shown in the following
screenshot:

![](bbox=[0.1338, 0.5153, 0.8662, 0.7514])

Figure 9.30 – Enabled CI

That completes the configuration of the CI/CD pipeline in Azure Pipelines.

After we trigger this build, we should be able to see a new version of the Docker image at
the end of its execution, which corresponds to the number of the build that pushed the
Docker image into Docker Hub, as illustrated in the following screenshot: