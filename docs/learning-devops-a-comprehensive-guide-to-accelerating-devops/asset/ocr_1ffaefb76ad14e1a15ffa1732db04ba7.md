We can check if Docker Compose is correctly installed by running the following command:

```
`docker-compose version`
```

The following screenshot shows the result of this command:

![](bbox=[0.1367, 0.2188, 0.8672, 0.282])

Figure 9.36 – docker-compose version command

This command displays the version of the installed `docker-compose` binary.

Now that Docker Compose is installed, we will write the Docker Compose configuration YAML file.

# # Writing the Docker Compose configuration file

For deploying containers using Docker Compose, we will write a configuration file to run an nginx container coupled with a `mysql` container.

For this, we will create a new file named `docker-compose.yml` with the following content that is in two blocks of code.

The first code snippet, in YAML, is for creating an nginx container, as follows:

```
`version: '3' #version of the Docker Compose YAML schema
services:
  nginx:
    image: nginx:latest
    container_name: nginx-container
    ports:
      - 8080:80`
```

In the preceding code snippet, we start with the `services` property, which contains a list of services (or Docker applications) to run in Docker. The first service is the nginx service. We configure the `nginx` `docker` image to use, the name of the container, and the exposed port, which is `8080`, for the local access of the nginx service.