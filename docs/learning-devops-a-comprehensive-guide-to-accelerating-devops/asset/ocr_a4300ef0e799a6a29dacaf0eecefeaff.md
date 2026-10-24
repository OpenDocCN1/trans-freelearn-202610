# # Pushing an image to Docker Hub

The goal of creating a Docker image that contains an application is to be able to use it on
servers that contain Docker and host the company's applications, just as with a VM.

In order for an image to be downloaded to another computer, it must be saved in a Docker
image registry. As already mentioned in this chapter, there are several Docker registries that
can be installed on-premises, which is the case for JFrog Artifactory and Nexus Repository.

If you want to create a public image, you can push (or upload) it to Docker Hub, which
is Docker's public (and free, depending on your license) registry. We will now see how to
upload the image we created in the previous section to Docker Hub. To do this, you need
to have an account on Docker Hub, which we created prior to installing Docker Desktop.

To push a Docker image to Docker Hub, perform the following steps:

+   1. **Sign in to Docker Hub**: Log in to Docker Hub using the following command:
+   
+   ```
+   docker login -u <your dockerhub login>
+   ```

When executing the command, you will be asked to enter your Docker Hub
password and indicate that you are connected to the Docker registry, as shown in
the following screenshot:

![](bbox=[0.114, 0.5, 0.851, 0.578])

+   2. **Retrieve the image ID**: The next step consists of retrieving the ID of the image that
+   has been created. To do so, we will execute the `docker images` command to
+   display a list of images with their ID.

![](bbox=[0.114, 0.65, 0.851, 0.739])

+   3. **Tag the image for Docker Hub**: With the ID of the image we retrieved, we will now
+   tag the image for Docker Hub. To do so, the following command is executed:
+   
+   ```
+   docker tag <image ID> <dockerhub login>/demobook:v1
+   ```

The following screenshot shows the execution of this command on the created image:

![](bbox=[0.114, 0.854, 0.851, 0.908])