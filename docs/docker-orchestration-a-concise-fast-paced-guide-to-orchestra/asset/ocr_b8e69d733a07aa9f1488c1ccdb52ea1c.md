# # Automating image builds with Docker Hub

Docker Hub has a feature that will automatically build a new image based on updates to a Git branch hosted on GitHub or Bitbucket. To create an automated build, select **Create Automated Build** from the **Create** menu. You will be prompted to link to an account on GitHub or Bitbucket, if you have not already done so.

Note that a linked GitHub or Bitbucket account may only be linked to a single Docker Hub account.

The wizard will walk you through the creation of a new repository. You will be asked for a name and description for the repository. Once the repository is created, go to **Build Settings** to configure the automated build. Docker Hub can track multiple branches from your Git repository and build a new tagged image every time that branch is updated.

You can also configure Docker Hub to rebuild your image if an upstream container is updated. For example, let’s suppose that your container is based on the `ubuntu` image, in other words, you used `From ubuntu` at the top of your `Dockerfile`. Whenever the `ubuntu` image is rebuilt, it will trigger a rebuild of your image as well. This is one way to ensure that your images always have the latest security updates from the upstream image.

Automated builds are a great way to get started with **Continuous Integration (CI)** and **Continuous Deployment (CD)**. It does not have the image testing capabilities that **GitLab** or **Jenkins** have, but you could still build an automated testing process. For example, when your developers are ready to test their application, they could push their changes to a testing branch in GitHub. That push will trigger a build on Docker Hub that builds a new image. A process could run on one of your servers that regularly pulls the testing branch, runs a test suite against it, and sends your team a build report. If the tests were successful, the team could then push their code to the `master` branch and trigger a build for production.

CI/CD tools are one way to ensure stability and reliability in your deployment process. The *Chapter 9, Using Continuous Integration to Build, Test, and Deploy Containers,* will show you how you can use the built-in Docker Registry in GitLab to automate testing and deployment.