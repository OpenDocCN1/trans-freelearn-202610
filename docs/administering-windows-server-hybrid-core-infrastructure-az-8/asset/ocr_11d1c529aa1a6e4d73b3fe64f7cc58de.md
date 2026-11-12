+   - Is the app based on .NET Core? You should use the Nano Server base image.
+   - Does your app require a dependency that's missing from Windows Server Core? You should use the Windows base image.
+   - Is GPU acceleration support required? You should use the Windows Server base image.

These base images can be discovered on Docker Hub: https://hub.docker.com/_/microsoft-windows-base-os-images.

A list of all the Microsoft container base images available can be found by running the following PowerShell command:

```
```
docker search Microsoft
```
```

These images can be downloaded from the **Microsoft Container Registry (MCR)**.

To download a base image, you can use the `docker pull` command. The following is an example:

```
```
docker pull mcr.microsoft.com/windows/servercore:ltsc2022
```
```

Once the image(s) have been downloaded, you can run the following command to verify which images are now available locally and display metadata:

```
```
docker image ls
```
```

In this section, we outlined Microsoft-based container creation using Docker. In the next section, we will look at managing Windows Server container images.

# # Managing Windows Server container images

A Windows container is created, run, and managed through Docker. **Docker Engine** provides tools that help automate the creation process. The Docker components that enable this are as follows:

+   - **The Docker text file**, Dockerfile: This file includes instructions for creating a container image. This includes identifying the base image that will be used and the commands that will be run when creating the image and identifying when new instances of the image are deployed.
+   - **The Docker command**, `docker build`: This command calls the Dockerfile, which then triggers the image creation process.

In addition to automation, Docker commands can be run manually. The following are some common commands for reference:

+   - `docker images`: This command lists installed images on the container host
+   - `docker run`: This command creates a container using a container image