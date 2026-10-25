
## Unlock Jenkins

To ensure Jenkins is securely set up by the administrator, a password has been written to the log (not sure where to find it?) and this file on the server:
/var/lib/jenkins/secrets/initialAdminPassword
Please copy the password from either location and paste it below.
Administrator password

29. In the PuTTY window, execute the cat command to get details of the file mentioned for the Administrator password.
30. Replace that password in the Administrator password box and click on Continue:

```
mitesh51@Jenkins:~$ sudo cat /var/lib/jenkins/secrets/initialAdminPassword
e0a0e477a2234e7bb45061173512eeff
mitesh51@Jenkins:~$
```

31. Click on Install suggested plugins:

## Customize Jenkins

Plugins extend Jenkins with additional features to support many different needs.

### Install suggested plugins
Install plugins the Jenkins community finds most useful.

### Select plugins to install
Select and install plugins most suitable for your needs.

Jenkins 2.60.1

32. Once all plugins are installed successfully, Create First Admin User. Click Save and Finish, as demonstrated in this next screenshot:

## Create First Admin User

Username: mitesh51
Password: ······
Confirm password: ······
Full name: Mitesh P
E-mail address:

Jenkins 2.60.1 Continue as admin Save and Finish

33. Now, the Jenkins setup is completed. Click on Start using Jenkins:

[24]

![](img/2a4d2e09d9af2c678ea8f43ea344059d_54_0.png)

34. Finally, we are at the dashboard screen:

## Welcome to Jenkins!

[Log In to create new jobs.](https://mjjjenkins.centralindia.cloudapp.azure.com/login?from=%2F)

+   1 Idle
+   2 Idle

Page generated: Jul 18, 2017 6:06:58 PM UTC [REST API](/api/) Jenkins ver. 2.60.1

## How it works...

Behind the scene, the template creates all the resources that are required to host Jenkins, including virtual network, network security group, virtual machine, and so on, based on the best practices; and then Jenkins is installed and configured in a secured environment in Microsoft Azure.

## Installing Jenkins as a Service in Windows

Installing Jenkins as a Windows service allows you to start Jenkins as soon as the machine starts, and regardless of who is interactively using Jenkins.

## Getting ready

Install and configure Jenkins on Windows.

## How to do it...

Follow these steps to install Jenkins as a service in Windows:

+   1. Go to the Jenkins dashboard.
+   2. Click on Manage Jenkins.
+   3. Click on Install as Windows Service, as shown in the following diagram:

![](img/2a4d2e09d9af2c678ea8f43ea344059d_59_0.png)

+   4. Keep Installation Directory as the default and click on Install:

![](img/2a4d2e09d9af2c678ea8f43ea344059d_59_1.png)

+   5. Once Windows as a Service installation is successfully completed, click on Yes:

![](img/2a4d2e09d9af2c678ea8f43ea344059d_59_2.png)

+   6. Now Jenkins will be available as a service in the Windows system.

## How it works...

Go to Control Panel and search for Services.

Click on Services (Local) to view:

![](img/2a4d2e09d9af2c678ea8f43ea344059d_60_0.png)

Once the Jenkins service is available, you can right-click on it and start, stop, or restart services based on your requirements.

## Installing plugins in Jenkins

Jenkins is an open source automation server that is widely used and extensible due to more than 400+ plugins. Plugins make Jenkins' integration with other tools very easy. You can create your own plugins also.

## Getting ready

There are various categories of plugins available, such as Source Code Management, Slave launchers and controllers, Build triggers, Build tools, Build notifies, Build reports, Other Post- build Actions, External site/tool integrations, UI plugins, Authentication and user management, Android development, iOS development, .NET development, Ruby development, Library plugins, and so on.

## How to do it...

+   1. Go to the Jenkins dashboard. Click on Manage Jenkins and then Manage Plugins:

## Manage Jenkins

+   - Configure System
+   Configure global settings and paths.
+   - Configure Global Security
+   Secure Jenkins: define who is allowed to access/use the system.
+   - Configure Credentials
+   Configure the credential providers and types
+   - Global Tool Configuration
+   Configure tools, their locations and automatic installers.
+   - Reload Configuration from Disk
+   Discard all the loaded data in memory and reload everything from file system. Useful when you modified config files directly on disk.
+   - Manage Plugins
+   Add, remove, disable or enable plugins that can extend the functionality of Jenkins.
+   - System Information
+   Displays various environmental information to assist trouble-shooting.
+   - System Log
+   System log captures output from `java.util.logging` output related to Jenkins.
+   - Load Statistics
+   Check your resource utilization and see if you need more computers for your builds.

2. Go to Available tab and select any plugin to install. Click on Install without restart:

## There's more...

Jenkins defines interfaces or abstract classes that model a facet of a build system. Interfaces or abstract classes define agreement on what needs to be implemented; and Jenkins uses plugins to extend those implementations.

## Uploading plugins in Jenkins

There will be instances where you have all the plugins downloaded from [https://updates.jenkins-ci.org/download/plugins/](https://updates.jenkins-ci.org/download/plugins/), or you may create your own plugin and you want to utilize that in Jenkins. This recipe will explain how to upload these plugins in Jenkins.

## Getting ready

Download the required plugin https://updates.jenkins-ci.org/download/plugins/ or create a plugin using the Plugin tutorial, https://wiki.jenkins.io/display/JENKINS/Plugin+tutorial.

## How to do it...

For this recipe, we will download a plugin and upload it from the Jenkins dashboard:

+   1. Download the copyartifact plugin (`.hpi`) file:

![](img/2a4d2e09d9af2c678ea8f43ea344059d_71_0.png)

+   2. Download the latest version to your system:

![](img/2a4d2e09d9af2c678ea8f43ea344059d_71_1.png)

+   3. Go to the Jenkins dashboard.
+   4. Click on Manage Jenkins.
+   5. Click on Manage plugins.
+   6. Go to the Advanced tab. Click on Choose File in the Upload Plugin section:

## How it works...

Once a plugin is uploaded successfully, we can utilize extensibility provided by the plugin that is uploaded into Jenkins.

## Configuring proxy in Jenkins

Jenkins is often installed in organizations behind the proxy server. In such scenarios, it is important to configure a proxy in the Jenkins dashboard so plugin updates can be installed successfully.

## Getting ready

Get all the details related to the server name, port number, and credentials that are allowed to access the proxy server.

## How to do it...

+   1. Go to the Jenkins dashboard.
+   2. Click on Manage Jenkins.
+   3. Click on Manage plugins.
+   4. Go to the Advanced tab.
+   5. Provide Server, Port, User name, Password, and No Proxy Host:

![](img/2a4d2e09d9af2c678ea8f43ea344059d_77_0.png)

+   6. Click on Submit.

## Configuring global settings in Jenkins

Configure system is the place where initial settings can be done, and it is useful. You can configure SMTP server details, SonarQube server details, environment variables, and so on.

## Getting ready

To configure global settings and paths, we need to go to Configure System in the Jenkins dashboard.

## How to do it...

+   1. Go to the Jenkins dashboard.
+   2. Click on Manage Jenkins.
+   3. Click on Configure System.
+   4. Verify the Home directory available:

5. In the Configure System page, we can define Environment variables too so that it can be used during the execution of build jobs.

6. For example, we can configure the ANDROID SDK path in Environment variables in order to set Continuous Integration for Android Apps.

# Global properties

Environment variables

List of variables

Name PROP_LOC

Value <Path of the Property File>

Delete

Add

7. Another important configuration available on the same page is Jenkins Location. You can specify the HTTP address of the Jenkins installation, as Jenkins cannot reliably detect such a Jenkins URL from within itself:

# Jenkins Location

Jenkins URL http://10.0.0.5:8080/

System Admin e-mail address address not configured yet <nobody@nowhere>
