
We will use our solution in GitHub. You can fork or clone it from this repository: https://github.com/didoureba/healthcaresolution.git. We will deploy our previous healthcare solution, which includes an ASP.NET API and two applications using ASP.NET MVC, and follow the steps illustrated in the following figure to deploy the application to Azure Container Registry. We will use Azure DevOps to ensure CI/CD:

![](img/66aceec30d3e7dead2f71bb68a887688_260_0.png)

Note that we can use Azure Repos instead of a GitHub repository. A Container Registry resource is already created.

## Creating the pipeline

In this section, we will create a pipeline as we need an Azure DevOps organization and a project already created. To do this, follow these steps:

- 1. Sign in to your Azure DevOps organization and navigate to your project: **healthcaresolution**.
- 2. Select **Pipelines** then **Pipelines**, select **Create Pipeline**, then select **New pipeline**, which will be displayed at the bottom, to create a new pipeline, as presented in the following figure:
- 4. After adding your GitHub credentials, select your repository from the list.
- 5. In the Configure tab, select the Docker: Build and push an image to Azure Container Registry task.
- 6. Select the subscription, then click on Continue.
- 7. You will be requested to connect to your Azure account. Once you're authenticated, select your container registry. Enter an image name, then select Validate and configure, as presented in the following figure:

![](img/66aceec30d3e7dead2f71bb68a887688_262_0.png)

Figure 11.4 – Configuring the building and deploying of an image to Azure Container Registry

When Azure Pipelines builds your pipeline, it will create the connection to the Docker Registry service so your pipeline will be able to push images to Azure Container Registry. It will generate, in the end, a YAML file called azure-pipelines.yml that will define your pipeline.

- 8. Review your generated YAML pipeline and select Save and run:

![](img/66aceec30d3e7dead2f71bb68a887688_263_0.png)

Figure 11.5 – The azure-pipelines.yml file

In our YAML file, we have the following:

- - Variables that include the Container Registry connection, such as the image repository, the container registry, and the tag
- - The VM image name, which is the OS environment
- - The stages, which include the jobs for the build and the different settings

In the following figure, we can see the azure-pipeline.yml file and the different settings of stages:

![](img/66aceec30d3e7dead2f71bb68a887688_263_1.png)

Figure 11.6 – Settings of the stages in a YAML file

In the following figure, we show how you can build and push an application using .NET Framework to Azure Container Registry and App Service:

![](img/66aceec30d3e7dead2f71bb68a887688_265_0.png)

Figure 11.8 – Build and push an application to Azure Container Registry and App Service using .NET Framework

## Deploying to Azure Container Registry

We can follow the same steps presented previously to build and push an application to Azure Container Registry.

We can use another method to create the service connection between our project in Azure DevOps and Azure Container Registry using these steps:

- 1. Open your project in Azure DevOps and select **project settings** at the bottom of the page on the left. Under **Pipelines**, select **New Service connection** to define and secure the connection between the Azure subscription and your project in Azure DevOps using a service principal. Click on the **Create service connection** button:

Figure 11.9 – Create a service connection between Azure DevOps and Azure services

2. A new dialog page will be displayed, requesting us to select a service connection from the list to link our Azure account to our Azure DevOps account. Select Azure Resource Manager and click on the Next button to continue the configuration:

![](img/66aceec30d3e7dead2f71bb68a887688_267_0.png)

Figure 11.10 – Configure the new service connection

- 3. On the next dialog page, select **Service principal (automatic)** and click on **Next**. Select the subscription and the resource group, then enter a service connection. Select **Save** to finish the configuration.

> **Important note**
> 
> If we only need to connect our Azure Container Registry with our DevOps account, we can select **New Service connection** to add a new connection link. Then, select **Docker Registry** and **Azure Container Registry**. Fill in all the parameters and select **Save** to confirm the connection.

5. Once the variables are saved, we will update vmImageName in the YAML file. Replace the default value, 'ubuntu-latest', with 'windows-latest'. We will also add buildContext after containerRegistry, as presented in the following screenshot, to make sure that all required application files are being copied over to the image filesystem. If we don't add this line, we will get an error indicating that the path inside of our Dockerfile build can't be found:

![](img/66aceec30d3e7dead2f71bb68a887688_269_0.png)

Figure 11.12 – Pipeline YAML for Container Registry

6. Select Save and run to start your pipeline build.

7. Once your image is pushed to the registry, push your container to App Service. Your App Service instance should be created.

In the next section, we will deploy the application to Azure App Service by creating a new release pipeline.

## Deploying to Azure App Service

To deploy our application to Azure App Service, we will create a new release pipeline.

To create a new release pipeline, follow these steps:

- 1. From the dashboard, select Pipelines and then Releases.
- 2. Select New Pipeline, and then select Empty job, as presented in the following figure:
- 4. Select the subscription and the application name and specify the fully qualified container image name. If you have a multi-container image, provide the names of them all in the Image name input. If you use a docker-compose file, add the path in the Configuration File input and select Save:

![](img/66aceec30d3e7dead2f71bb68a887688_271_0.png)

Figure 11.15 – Configure Azure Web App on Container Deploy

# Important note

You will need to enable the admin user for your Container Registry. Select Access keys under Settings for your Container Registry and then select Enabled for Admin user. This option enables the use of the registry name as the username and the admin user access key as the password to allow Docker to log in to your Container Registry.

![](img/66aceec30d3e7dead2f71bb68a887688_272_0.png)

Figure 11.16 – Enable Admin user in Azure Container Registry

5. On the Pipeline tab, select Add an artifact:

Figure 11.17 – Add an artifact

6. In Add an artifact, select Build for Source type, then select the project, the source, the default version, and select Add:

Figure 11.18 – Build source type

7. Enable Continuous deployment trigger and save all the modifications:

Figure 11.19 – Enable Continuous deployment trigger

8. Select **Create release**, add the stage and update the version for the artifact sources for this release. Then, click on the **Create** button:

![](img/66aceec30d3e7dead2f71bb68a887688_274_0.png)

Figure 11.20 – Configure a release

We are ready to again push our local code to our repository to start the pipelines. We can add approval requests before updating App Service with the latest version.

We will see in the next section how we can integrate Docker Hub with the CI/CD pipeline.

## Integrating Docker Hub with the CI/CD pipeline

In this section, we will set up a CI pipeline for building containerized applications. We will build and publish Docker images to Docker Hub:

![](img/66aceec30d3e7dead2f71bb68a887688_274_1.png)

Figure 11.21 – Scenario: Build and push a Docker image to Docker Hub using Azure DevOps pipelines
