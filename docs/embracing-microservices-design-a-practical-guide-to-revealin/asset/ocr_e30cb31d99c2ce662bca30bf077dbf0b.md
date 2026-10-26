# Dependencies

In the Twelve-Factor App, the dependencies should be isolated and explicitly declared via a dependency declaration manifest. The application should not depend on the host to have any of its dependencies. The application and all its dependencies are carried together for every deployment. For example, a common way of declaring Node.js application dependencies is package.json, for Java applications, it's pom.xml, and for .NET Core, it's .csproj. For containerized environments, you can bundle the application with its dependencies using Docker for deployment across environments. The following diagram depicts how application dependencies are managed outside the application's repository and included later as part of container packaging:

![](bbox=[0.275, 0.329, 0.734, 0.586])

Figure 1.4 – Application dependency isolation and its inclusion via a Docker file

Containers help in deploying applications to different environments without the need to worry about installing application dependencies. **npm** is a package manager that's responsible for managing packages that are local dependencies for a project. These packages are then included as part of container packaging to build container images.