# # The importance of using CI

It is worth keeping in mind how a Docker application is defined. At its most basic level, a Docker application is really no different from any software project. It is simply a `Dockerfile` and the resources needed for the image are most likely stored in version control. It is software.

Treating Docker applications like code means that an administrator or team of administrators can benefit from all of the advances in software development methodologies. One of those is the automated building and testing of a project. This process is called CI. Any time a change is made to an application, whether it is a small change to a configuration file or the upgrade of the actual application, the image can be automatically rebuilt and tested to ensure that the image will work as intended when it is deployed.

I often joke that my job as a system administrator is to get other people or things to do my work for me. The reality is that the more I can empower other people in my organization, the more effective we can all be. For example, imagine a development team that is working on a web application. In a traditional model, these developers must wait for an administrator to deploy any changes into the testing and production environments and hope that nothing breaks in the process. Using a proper CI workflow means that whenever the developers are ready to deploy, the CI tools can safely deploy the application without waiting for an administrator to get around to it. The end result is faster, more consistent, and more reliable deployments.

# # Using GitLab for CI

GitLab is a wonderful tool for CI and deployment for Docker images and services. It combines Git with issue tracking, a Docker Registry, and CI to provide a unified experience. However, this process is not unique to GitLab. The lessons learned in this section should be applicable regardless of the systems used.