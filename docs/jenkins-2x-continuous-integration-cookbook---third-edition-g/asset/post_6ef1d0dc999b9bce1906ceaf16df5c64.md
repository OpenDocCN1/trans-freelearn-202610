
## Getting ready

Make sure that you have all the build jobs available that you created in earlier chapters, such as the deployment of WAR files, functional testing, and load testing.

from the PetClinic-FuncTest job, you will configure PetClinic-LoadTest as a downstream job in Post-build Actions:

![](img/2a4d2e09d9af2c678ea8f43ea344059d_497_0.png)

Once load testing is completed, you should deploy the application in the Prod environment, so from the PetClinic-LoadTest job, you will configure PetClinic-Prod as a downstream job in Post-build Actions:

![](img/2a4d2e09d9af2c678ea8f43ea344059d_497_1.png)

## Configuring a build pipeline

Now you have configured all upstream and downstream jobs. You know the sequence so you can execute the first build and the rest of the builds will follow until any job fails in the sequence. You can configure to move ahead with unstable builds as well.

However, it is more useful if you have the visualization of the flow of execution you have configured as upstream and downstream jobs.

The build pipeline plugin provides visualization of the execution flow based on the upstream and downstream jobs configuration.

## Getting ready

Install the plugin from Manage Jenkins | Manage Plugins:

![](img/2a4d2e09d9af2c678ea8f43ea344059d_499_0.png)

Verify the successful installation of the Build Pipeline Plugin:

# Installing Plugins/Upgrades

Preparation

- Checking internet connectivity
- Checking update center connectivity
- Success

- TestNG Results Plugin  Success
- promoted builds plugin  Success
- Run Condition Plugin  Success
- Conditional BuildStep  Success
- Parameterized Trigger plugin  Success
- Build Pipeline Plugin  Success

Go back to the top page
(you can start using the installed plugins right away)

Restart Jenkins when installation is complete and no jobs are running

Now, you are ready to create a build pipeline and configure it with an initial job.

## How to do it...

- 1. Go to the Jenkins dashboard and click on the + sign on the tabs available.
- 2. Provide the View name and select Build Pipeline View.
- 3. Click on Save:

## Jenkins

Open Blue Ocean

search

admin | log out

- New Item
- People
- Build History
- Project Relationship
- Check File Fingerprint
- Manage Jenkins
- My Views
- Disk Usage
- Credentials

View name

PetClinic

Build Pipeline View
Shows the jobs in a build pipeline view. The complete pipeline of jobs that a version propagates through are shown as a row in the view.

Dashboard
Customizable view that contains various portlets containing information about your job(s)

List View
Shows items in a simple list format. You can choose which jobs are to be displayed in which view.

My View
This view automatically displays all the jobs that the current user has an access to.

OK

Build Queue
No builds in the queue.

Build Executor Status

4. Verify that the Layout is configured as Based on upstream/downstream relationship.

5. You want to execute PetClinic-code as a first job and you want to see the visualization from that job, so select it in the Select Initial Job:

![](img/2a4d2e09d9af2c678ea8f43ea344059d_502_0.png)

- 6. Select the rest of the configuration as per your requirements.
- 7. Change the No Of Displayed Builds from 1 to 3 so it will display the latest three build pipelines executed.
- 8. Click on OK:

![](img/2a4d2e09d9af2c678ea8f43ea344059d_502_1.png)

9. Verify the Build Pipeline view in the Jenkins dashboard:

![](img/2a4d2e09d9af2c678ea8f43ea344059d_503_0.png)

Build Pipeline in Initial Phase

10. What if you want to continue the execution of the pipeline even if some build is unstable? In such cases, you need to select Trigger even if the build is unstable option in Post-build Actions, as seen in the following screenshot:

![](img/2a4d2e09d9af2c678ea8f43ea344059d_503_1.png)

11. Click on the run in Build Pipeline view. Wait till the complete pipeline is executed:

![](img/2a4d2e09d9af2c678ea8f43ea344059d_504_0.png)

## Successful Build Pipeline Execution

12. Done. This is how you can create a sequence of execution for different build jobs and achieve end-to-end automation for the application life cycle management. In the next recipe, you will achieve similar things using a the pipeline as a code feature.

## Creating a pipeline job

In a pipeline, you model all related tasks to decide the sequence of execution. You will perform
the same tasks you performed with the build pipeline, but with code.

## Getting ready

Open the Jenkins dashboard.

## How to do it...

- 1. Click on New Item. Enter an item name and select Pipeline.
- 2. Click OK:

## Enter an item name

FirstPipeline

- **Freestyle project** — This is the central feature of Jenkins. Jenkins will build your project, combining any SCM with any build system, and this can be even used for something other than software build.
- **Maven project** — Build a maven project. Jenkins takes advantage of your POM files and drastically reduces the configuration.
- **Pipeline** — Orchestrates long-running activities that can span multiple build slaves. Suitable for building pipelines (formerly known as workflows) and/or organizing complex activities that do not easily fit in free-style job type.
- **External Job** — This type of job allows you to record the execution of a process run outside Jenkins, even on a remote machine. This is designed so that you can use Jenkins as a dashboard of your existing automation system.
- **Multi-configuration project** — Suitable for projects that need a large number of different configurations, such as testing on multiple environments, platform-specific builds, etc.

![](img/2a4d2e09d9af2c678ea8f43ea344059d_508_0.png)

## Using a sample pipeline for execution

Jenkins provides sample syntax to get a feel for how to configure a pipeline.

## Getting ready

With Jenkins 2, the pipeline and other features are installed with the default installation with the suggested plugins.

## How to do it...

- 1. Go to the Jenkins dashboard | New item | Pipeline.
- 2. It will open the configuration of a newly created pipeline job.
- 3. Go to the Pipeline section:

![](img/2a4d2e09d9af2c678ea8f43ea344059d_511_0.png)

4. In the try sample Pipeline... dropdown, select GitHub + Maven. It will automatically generate the syntax for the sample code. Make sure that mvnHome has the proper value as per the path given for Maven in your system:

## Pipeline

Definition

Pipeline script

Script

```
node {
    def mvnHome
    stage('Preparation') { // for display purposes
        // Get some code from a GitHub repository
        git 'https://github.com/jglick/simple-maven-project-with-tests.git'
        // Get the Maven tool
        // ** NOTE: This 'M3' Maven tool must be configured
        // **       in the global configuration.
        mvnHome = tool 'M3'
    }
    stage('Build') {
        // Run the maven build
        if (isUnix()) {
            sh "${mvnHome}/bin/mvn" -Dmaven.test.failure.ignore clean package"
        } else {
            bat("/${mvnHome}/bin/mvn" -Dmaven.test.failure.ignore clean package/)
        }
    }
```

GitHub + Maven

Use Groovy Sandbox

Pipeline Syntax

Save

Apply

- 5. Click on Save and execute the build to verify it.
- 6. However, you will create your own pipeline with the same sequence you tried with the Build Pipeline plugin in the next recipe.

## Configuring a pipeline job for end-to-end automation

In this recipe, you will create script to create a pipeline similar to what you configured with the `Build Pipeline` plugin.

## Getting ready

Click on Pipeline Syntax to generate the syntax for specific tasks you want to execute.

You can select the steps and configure the required things, and then click on Generate Pipeline Script to get the syntax that you can directly utilize in your pipeline:
