
## Deploying a WAR file from Jenkins to Tomcat

For application deployment, we can utilize multiple methods to deploy an application to a web server or application server. We can use a batch script or shell script to copy the package file created after the Continuous Integration process, or we can use the Jenkins plugin to deploy an application.

## Getting ready

Go to Manage Jenkins | Manage Plugins and install Deploy to container Plugin:

| Install ↓ | Name | Version |
|---|---|---|
| Deploy to container Plugin | 1.10 |
| Deploy to Websphere container Plugin | 1.0 |

Filter: Deploy to  
Tabs: Updates, Available, Installed, Advanced  
Buttons: Install without restart, Download now and install after restart, Check now  
Update information obtained: 1 day 2 hr ago

Wait until the plugin has installed successfully.

5. Give the path to the WAR file for deployment using the Jenkins plugin. Select **Deploy war/ear to a container** from Post-build Actions. Click on **Add Container** and select the latest version of Tomcat. Give the Tomcat URL. Give the username and password we have defined in `tomcat-users.xml`:

6. Execute the build by clicking **Build now**. Verify the logs for application deployment:

```
Results :

Tests run: 59, Failures: 0, Errors: 0, Skipped: 0

[INFO]
[INFO] --- maven-war-plugin:2.3.1:war (default-war) @ spring-petclinic ---
[INFO] Packaging webapp
[INFO] Assembling webapp [spring-petclinic] in [d:\jenkins\workspace\PetClinic-Test\target\spring-petclinic-4.2.5-SNAPSHOT]
[INFO] Processing war project
[INFO] Copying webapp resources [d:\jenkins\workspace\PetClinic-Test\src\main\webapp]
[INFO] Webapp assembled in [1669 msecs]
[INFO] Building war: d:\jenkins\workspace\PetClinic-Test\target\spring-petclinic-4.2.5-SNAPSHOT.war
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 28.772 s
[INFO] Finished at: 2016-07-06T22:59:37+05:30
[INFO] Final Memory: 29M/261M
[INFO] ------------------------------------------------------------------------
Deploying d:\jenkins\workspace\PetClinic-Test\target\spring-petclinic-4.2.5-SNAPSHOT.war to container Tomcat 7.x Remote
[d:\jenkins\workspace\PetClinic-Test\target\spring-petclinic-4.2.5-SNAPSHOT.war] is not deployed.
Doing a fresh deployment.
  Deploying [d:\jenkins\workspace\PetClinic-Test\target\spring-petclinic-4.2.5-SNAPSHOT.war]
Finished: SUCCESS
```

7. Go to your browser and visit the application with the Tomcat URL and the context of the application:

![](img/2a4d2e09d9af2c678ea8f43ea344059d_410_0.png)

8. In the next section, we will deploy the PetClinic application to Tomcat, which resides in the AWS EC2 instance.

## Deploying a WAR file from Jenkins to AWS Beanstalk

AWS Elastic Beanstalk is a **Platform as a Service (PaaS)**. We will use it to deploy the PetClinic application. These are the steps you need to follow to deploy an application on AWS Elastic Beanstalk:

![](img/2a4d2e09d9af2c678ea8f43ea344059d_411_0.png)

## Getting ready

Let's create a sample application in Elastic Beanstalk to understand how Elastic Beanstalk works. Then, we will use the Jenkins plugin to deploy an application into it.

Click on **Services** in the AWS management console and select **AWS Elastic Beanstalk**. Create a new application named `petclinic`. Select **Tomcat** as a Platform and select the **Sample application** radio button:

![](img/2a4d2e09d9af2c678ea8f43ea344059d_412_0.png)

Verify the sequence of events for the creation of a sample application:

![](img/2a4d2e09d9af2c678ea8f43ea344059d_412_1.png)

It will take some time and, once the environment has been created, it will be highlighted in green:

![](img/2a4d2e09d9af2c678ea8f43ea344059d_413_0.png)

Click on the petclinic environment and verify Health and Running Version in the dashboard:

![](img/2a4d2e09d9af2c678ea8f43ea344059d_413_1.png)

Verify the environment ID and URL. Click on the URL and verify the default page:

## Congratulations

Your first AWS Elastic Beanstalk Application is now running on your own dedicated environment in the AWS Cloud

### What's Next?

- Learn how to build, deploy and manage your own applications using AWS Elastic Beanstalk
- AWS Elastic Beanstalk concepts
- Learn how to create new application versions
- Learn how to manage your application environments

### Download the AWS Reference Application

- Explore a fully-featured reference application using the AWS SDK for Java

### AWS Toolkit for Eclipse

- Developers may build and deploy AWS Elastic Beanstalk applications directly from Eclipse
- Get started with Eclipse and AWS Elastic Beanstalk by watching this video
- View all AWS Elastic Beanstalk documentation

## How to do it...

1. Install the AWS Elastic Beanstalk Publisher plugin. For more details, visit https://wiki.jenkins-ci.org/display/JENKINS/AWS+Beanstalk+Publisher+Plugin:

2. A new section will come up in Post-build Actions for Elastic Beanstalk.

3. Click on the Jenkins dashboard and select Credentials; add your AWS Credentials.

4. Go to your Jenkins build and select AWS Credentials, which is set in the global configuration.

5. Select AWS Region from the list and click on **Get Available Applications**. As we have created a sample application, it will show up like this.

6. In **EnvironmentLookup**, provide an environment ID in the **Get Environments By Name** box and click on **Get Available Environments**:

![](img/2a4d2e09d9af2c678ea8f43ea344059d_415_0.png)

### Elastic Beanstalk Application

AWS Credentials

AWS : AKI

AWS Credentials lookup by name

Get Credentials Names

AWS Region

us-east-1

AWS Region Text

Application Name

petclinic

petclinic

Get Available Applications

EnvironmentLookup

#### Get Environments By Name

Environment Names

e-y2fmvwri3n

petclinic

Get Available Environments

7. Save the configuration and click on **Build now**.

8. Now, let's go to the AWS management console to check whether the WAR file is being copied in Amazon S3 or not:

9. Go to S3 Services and check the available buckets:

![](img/2a4d2e09d9af2c678ea8f43ea344059d_416_0.png)

10. Since the WAR file is large, it will take a while to upload to Amazon S3. Once it is uploaded, it will be available in the Amazon S3 bucket.

11. Check the build job's execution status in Jenkins. Some sections of the expected output are the following:

- The test case execution and WAR file creation were successful
- The build was successful

12. Now, check the AWS management console:

![](img/2a4d2e09d9af2c678ea8f43ea344059d_418_0.png)

16. Once the application deployment is successful, terminate the environment. Go to Specific environment and click on **Actions** to **Terminate** the environment. We have successfully deployed our application on Elastic Beanstalk.

## Deploying a WAR file from Jenkins to Azure App Services

Microsoft Azure App services is a PaaS. In this section, we will look at the Azure web app and how we can deploy our PetClinic application.

## Getting ready

We need to have a Microsoft Azure subscription. Go to App Services and click **+ Add**:

![](img/2a4d2e09d9af2c678ea8f43ea344059d_420_0.png)

Then, click on **Create**:

### Web App

Microsoft

Create and deploy web sites in seconds, as powerful as you need them.

Leverage your existing tools to create and deploy applications without the hassle of managing infrastructure. Microsoft Azure Web Sites offers secure and flexible development, deployment, and scaling options for any sized web application. Use frameworks and templates to create web sites in seconds. Choose from source control options like TFS, GitHub, and BitBucket. Use any tool or OS to develop your site with .NET, PHP, Node.js or Python.

- Fastest way to build for the cloud
- Provision and deploy fast
- Secure platform that scales automatically
- Great experience for Visual Studio developers
- Open and flexible for everyone
- Monitor, alert, and auto scale (preview)

![](img/2a4d2e09d9af2c678ea8f43ea344059d_421_0.png)

![](img/2a4d2e09d9af2c678ea8f43ea344059d_421_1.png)

Create

Go to the Microsoft Azure portal at https://portal.azure.com. Click on App Services and then on **+ Add**. Provide values for App name, Subscription, Resource Group, and App Service plan/Location. Then, click on **Create**.
