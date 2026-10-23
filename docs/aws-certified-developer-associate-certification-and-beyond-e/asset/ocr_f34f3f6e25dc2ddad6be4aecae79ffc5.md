10. Create a Docker (container) image using the Dockerfile provided:

+   I. While still connected to the instance using Session Manager and in the same folder as the Dockerfile, type in the docker build -t <image uri> command (note the full stop after the image URI). Use the image URI you noted at the end of project task 1. For example, the command would look similar to the following: docker build -t 441639934710.dkr.ecr.us-east-1.amazonaws.com/todo-plus.

+   II. Your Docker image will now be created using the commands in the Docker file to create various image layers. This may take a few minutes to complete. You should get an output similar to the following screenshot:

![](bbox=[0.14, 0.31, 0.87, 0.48])

+   III. To check your image, type in the docker images command and press Enter, and you should get an output similar to the following screenshot:

![](bbox=[0.14, 0.57, 0.87, 0.65])

In the next project task, you will push the image into the ECR repository you created in project task 1.

# Project Task 11.4 – Push the Docker Image to ECR

In this task, you will push the Docker image you created into the ECR repository:

+   1. Ensure you are still logged into the AWS Development account as the IAM user Alice.

+   2. Ensure you are still remotely connected to the instance and in the exact location where you completed project task 3.