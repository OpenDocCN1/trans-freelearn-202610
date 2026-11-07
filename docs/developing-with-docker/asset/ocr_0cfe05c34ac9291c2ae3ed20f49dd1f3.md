Again, it needs minimal implementation; just enough to make sure it's running and serving our static content. In the normal application you would probably introduce a more complex routes setup, divide the application into modules, and so on.

Let's run our node.js server, by executing the node command in the root of our project:

```
node backend\app.js
```

Then again, fire up your web browser and enter the following address: http://localhost:8080. You should see our application. This time, it will be served from node.js HTTP server:

![](bbox=[0.1, 0.31, 0.9, 0.52])

As you can see our application works properly, it's time to put it into the Docker container. We need to decide the image that we will use as a base image. As always, using official Docker images is highly recommended. The Docker Hub hosts official node.js repository. We are going to use the long-term support (LTS) version arg on of node available. The node:argon image comes with all the node and npm tools installed, configured and ready to use. The decision about picking up the specific base image gives us the foundation for our Dockerfile. Let's create a Dockerfile in the root of the project and include the FROM instruction:

```
FROM node:argon
```

As the base image contains all the tools we need already, all we need to do is copy our application files and trigger the dependency downloading. The current root directory of our project will be a built context for Docker, so let's create a workdir using the following instruction:

```
WORKDIR /usr/hello-docker
```