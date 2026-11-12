
You are also able to edit files that represent Custom Objects and fields:

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_479_0.png)

If you choose to combine this development approach with the browser-based approach, the Force.com IDE offers a way to refresh the files from within the desktop tool. Simply right-click on the folder and select the appropriate option. This facility effectively performs a retrieve operation similar to that described in the earlier section via the Ant build.xml script. As such, watch out for similar side-effects in terms of additional files or file changes.

## Hooking up Continuous Integration

So far we have been depending on the developer to ensure that the changes they push up to the GitHub remote repository do not cause regressions elsewhere in the application, by running Apex tests before making their commits. This is always a good practice!

However, this is not always a fully reliable means of determining the quality of the code, as the main code base will likely have moved on due to other developers also pushing their changes. When integrated together, they might cause tests or even code to fail to compile. Continuous Integration monitors changes to the Source Control repository and automatically starts its own build over the fully integrated source code base. Failures are reported back to the developers who last pushed code to the repository.

In this part of the chapter, we are going to explore this process using the popular Jenkins Continuous Integration server that uses the Ant script to perform the build step. Before we get into setting this up, let's take a moment to review the CI process that we are going to use.

### The Continuous Integration process

The following diagram shows the CI process that we will be setting up over the course of the next few sections. Once you have a basic CI setup like this, you can start to add other features such as code documentation generation, static code analysis, security code scanner, and automated regression testing (something we will discuss briefly later on).

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_480_0.png)

The following is the CI process:

- 1. Developers make multiple contributions to Source Control, just as we did earlier in this chapter via the GitHub UI desktop client.
- 2. The CI server is able to poll (configurable) the Source Control server (in our case, the GitHub remote repository) for changes.
- 3. Once changes are detected, it downloads the latest source code into a local workspace folder.
- 4. A part of the configuration of the CI server is to inform it of the location of the Ant `build.xml` script to execute after the workspace is set up.
- 5. A build org is simply a designated developer org set aside solely for the use of the CI process. We will create one of these shortly for this purpose, though, with Salesforce DX a scratch org can be created and deleted dynamically. As with the deploy process, the same Ant script is used to deploy the code to the build org (thus confirming code compilation) and run Apex tests.
- 6. As described earlier, developer orgs are used to develop the changes that are unmanaged environments as opposed to managed. Some of the constraints, such as the ability to change a previously released `global` Apex class, that would normally apply are not enforced. In this step, a verification check-only deployment is performed against the packaging org to confirm that the code would, when needed, still deploy to the packaging org for release. This step can also help validate that code and tests continue to execute in a managed namespace context. This deployment is only a check, no changes are made to the packaging org regardless of the outcome.
- 7. Finally, the CI server will record the log and results of the build for analysis and the number of success versus failed builds and provide reports and notifications on the overall health of the source code in Source Control. If there are failures, there are options to e-mail certain team members as well as specific targets (those who made the most recent changes). Making sure a team stops what they are doing and attends to a broken build is very important. The longer the broken builds exist, the harder they are to solve, especially when further changes occur.

## Updating the Ant build script for CI

Before we can set up our CI server, we need to ensure that our Ant script has an appropriate entry point for it. This is similar to that used by the developer in the steps described earlier. The following is a new target designed as an entry point for a CI server. More complex scripts might involve some reuse between developer usage and CI usage.

The following target will clean the build org (by virtue of the dependency on the `undeploy` target) and then deploy to it. It will then perform a second deploy to perform a check-only deployment to the packaging org. If you are following along, commit the changes to the `build.xml` file so that the Jenkins server can see the new target when it downloads the latest source code from GitHub.

```
<target name="deploy.jenkins" depends="undeploy">
  <sf:deploy
    username="${sf.username}"
    password="${sf.password}"
    serverurl="${sf.server}"
    runAllTests="true"
    deployRoot="${basedir}/src"/>
  <sf:deploy
    username="${sf.package.username}"
    password="${sf.package.password}"
    serverurl="${sf.server}"
    checkOnly="true"
    runAllTests="true"
    deployRoot="${basedir}/src"/>
</target>
```

## Installing, configuring, and testing the Jenkins CI server

Setting up a Jenkins server is a broad topic beyond the scope of this chapter. A good way, however, to get started is to go to http://jenkins-ci.org/ and download one of the prebuilt installations for your operating system. In this case I used the Mac pkg file download. After installation, I was able to navigate to http://localhost:8080 to find Jenkins waiting for me!

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_482_0.png)

Jenkins has a plugin system that permits many custom extensions to its base functionality, by clicking on Manage Jenkins and then Manage Plugins to locate a GitHub Plugin:

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_482_1.png)

Once this plugin is installed, you can create a Jenkins job that points to your GitHub repository and Ant script to execute the `deploy.jenkins` target defined earlier. On the Jenkins main page, click on **New Item** and select the **Build a free-style software project** option. Complete the following steps to create a build project:

- 1. Complete the **Project Name** field with an appropriate name, such as `FormulaForce Build`.
- 2. In the **GitHub project** field, enter the full URL to your GitHub repository, for example `https://github.com/afawcett/formulaforce-chapter13`.
- 3. Under the **Source Control Management** section, select **Git**.
- 4. In the **Repository URL** field, enter your GitHub repository URL, which is different from the preceding URL and takes the format of `https://github.com:githubuser/repositoryname.git`, for example, `https://github.com:afawcett/formulaforce-chapter13.git`.
- 5. If your repository is private and not public, you will need to ensure that you have exchanged deployment keys between Jenkins and your GitHub account. This involves generating a key and copying and pasting it to the **Deploy** keys section in your GitHub repository settings. For the purposes of this book, it is recommended that you use a public GitHub repository.
- 6. Although not recommended for a production Jenkins server, let's set it to poll every minute for changes to the repository. In the **Build Triggers** section, select **Poll SCM** and enter `*/1 * * * *` in the **Schedule** field.
- 7. Scroll further down the project setup page to the **Build** section, and from the **Add build step** drop-down select **Invoke Ant**.
- 8. In the **Targets** field, enter `deploy.jenkins` (as per the name of the Ant target created in the `build.xml` file).
- 9. Click on the **Advanced** button, then click on the black down arrow icon to the right of the **Properties** field to expand it so that you are able to enter multiple properties. The `deploy.jenkins` Ant target that we created earlier requires the `username` and `password` parameters to deploy to the build org and perform the check deploy to the packaging org. Enter the property name-value pairs on separate lines as follows:

```
sf.username=buildorgusername
sf.password=buildorgpasswordbuildorgtoken
sf.package.username=packagingorgusername
sf.package.password=packagingorgpasswordpackagingorgtoken
```

- 10. Click on the **Save** button at the bottom of the page.
