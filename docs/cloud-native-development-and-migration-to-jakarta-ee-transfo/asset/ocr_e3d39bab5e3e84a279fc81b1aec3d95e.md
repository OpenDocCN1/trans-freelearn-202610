# Introduction to Containers and Docker

In the previous chapter, you learned how to test the Cargo Tracker application. At this point, the Cargo Tracker application is ready to be deployed to different environments, machines, and the cloud.

However, how do you ensure the environment is ready for the Cargo Tracker and that it can run there? In this chapter, you will look at how to solve that problem using containers and Docker. We will start with what containers are, what they can do, and how to create them. When you have the specifications for your container ready, you will install Docker and create and run the container.

In this chapter, we will cover the following main topics:

+   - What are containers?
+   - A brief introduction to Docker
+   - Installing Docker
+   - Running a Docker container

By the end of the chapter, you will have a clear understanding of what containers are, and how to specify and create them. You will also create a container for the Cargo Tracker application that you can run on every machine with Docker.

# Technical requirements

For this chapter, you need to have a Cargo Tracker application on your system and need the result of the Cargo Tracker application of either Chapter 3, 4, or 5. To edit the Docker files, you can use any text editor of your choice. We will install Docker on a Linux machine. If you want to follow those instructions, you will also need a Linux machine or virtual machine. You can also follow the instructions on the Docker website: https://docs.docker.com/engine/install/ubuntu/.