# Configure Artifactory in Jenkins:

![](bbox=[0.126, 0.096, 0.873, 0.438])

When you use Maven as a build tool, it creates the `.m2` directory. This keeps all the dependencies in the repository directory when it downloads dependencies from Maven repositories available on the internet.

For the sake of verification, change the directory name in the `.m2` directory to `repository1`:

![](bbox=[0.117, 0.55, 0.611, 0.663])

The moment Jenkins build execution starts, it will fetch dependencies from Artifactory and create a repository directory in `.m2`.