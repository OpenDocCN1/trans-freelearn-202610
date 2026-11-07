
```
app.route('/*')
  .get(function (req, res) {
    res.sendFile(path.join(config.root, 'index.html'));
  });
http.createServer(app).listen(config.port, function () {
  console.log('hello-docker server listening on %d, in %s mode',
    config.port, app.get('env'));
});
exports = module.exports = app;
```

As you can see in the preceding section, we include node.js Express library to serve our static context, configure port number, and the root path and Express routes. The only specific configured route is "/*", meaning that index.html from the frontend directory will be sent as a response to every HTTP GET request to http://localhost:8080/ address.

Every other URL, such as http://localhost:8080/hello_world_docker.controller.js or http://localhost:8080/bower_components/angular/angular.js which we include in our index.html will be served as static context. Then we just start the server listening process. This is the last file in our node.js Angular application. The complete structure of files and folders should look similar to the following screenshot:

![](img/0c87321919fa245e22f056c67ec70003_242_0.png)

Again, it needs minimal implementation; just enough to make sure it's running and serving our static content. In the normal application you would probably introduce a more complex routes setup, divide the application into modules, and so on.

Let's run our node.js server, by executing the node command in the root of our project:

```
node backend\app.js
```

Then again, fire up your web browser and enter the following address: http://localhost:8080. You should see our application. This time, it will be served from node.js HTTP server:

![](img/0c87321919fa245e22f056c67ec70003_243_0.png)

As you can see our application works properly, it's time to put it into the Docker container. We need to decide the image that we will use as a base image. As always, using official Docker images is highly recommended. The Docker Hub hosts official node.js repository. We are going to use the long-term support (LTS) version argon of node available. The node:argon image comes with all the node and npm tools installed, configured and ready to use. The decision about picking up the specific base image gives us the foundation for our Dockerfile. Let's create a Dockerfile in the root of the project and include the FROM instruction:

```
FROM node:argon
```

As the base image contains all the tools we need already, all we need to do is copy our application files and trigger the dependency downloading. The current root directory of our project will be a built context for Docker, so let's create a workdir using the following instruction:

```
WORKDIR /usr/hello-docker
```

Now, let's copy the application first. We will use COPY, but we could just also use ADD here. As you remember from Chapter 6, Creating Images, ADD can do more than COPY: it allows <src> to be a URL and if the <src> parameter of ADD is an archive in a recognized compression format, it will be unpacked inside the image. That said COPY is enough in our case:

```
COPY backend /usr/hello-docker/backend
COPY frontend /usr/hello-docker/frontend
COPY .bowerrc bower.json package.json /usr/hello-docker
```

Note that the Docker will automatically create a directory with the COPY instruction and hence mkdir command is not needed. As you remember from the Chapter 3, Understanding Images and Containers and Chapter 6, Creating Images, every separate filesystem command would create another layer in the image. Creating additional, unneeded layers is not the good practice. Skipping mkdir command we can avoid one additional layer.

Now, it's time to run the npm install and bower install commands:

```
RUN npm install
```

The last thing to do would be to expose the proper port. Our application listens on port 8080, so we add the following line to the Dockerfile:

```
EXPOSE 8080
```

The last step will be actually running our node.js server, by adding the CMD instruction:

```
CMD ["node", "backend/app.js"]
```

The final version of the Dockerfile is like:

```
FROM node:argon
WORKDIR /usr/hello-docker
COPY backend /usr/hello-docker/backend
COPY frontend /usr/hello-docker/frontend
COPY .bowerrc bower.json package.json /usr/hello-docker
RUN npm install
EXPOSE 8080
CMD ["node", "/usr/hello-docker/backend/app.js"]
```

That's it, our complete Dockerfile is as tiny and simple as our node.js application, but that should be enough. Let's try and build the image, executing the docker build command, giving the current directory a build context:

```
docker build .
```

After a long listing of log output, downloading node.js dependencies, adding all the layers related to the instructions in the Dockerfile, we will be given the ID of the image we have just built:

```
Successfully built fa19b25b311e
```

Let's run the fresh image, mapping the port on which node.js server listens for connections to our machine port number 8080:

```
docker run -p 8080:8080 fa19b25b311e
```

If you now open a web browser again, pointing to http://localhost:8080, you will see our greeting application, as seen in the following screenshot:

![](img/0c87321919fa245e22f056c67ec70003_245_0.png)

This time, however, it's being served from inside a Docker container. Again, imagine how much hassle you have just saved for your future customer. Operating system: checked. Node.js setup: checked. All the needed frontend and backend dependencies: also checked. Everything is ready to run everywhere a Docker engine can be run.

Those Java and node.js examples should give you an overview of how you can package your own application into a Docker container. You can use Maven and automate most of the things or you can create a Dockerfile manually. The resulting image will be ready to distribute into the remote registry and it will also be portable. In the next chapter we will do some kind of a summary. You will be given some useful and high-quality additional resources, which you can use to extend your knowledge about Docker.

## Summary

In this chapter, we have been using Docker during the build process of the Java applications using Maven. We know that there are a couple of Docker plugins for Maven and we know their functionality and limitations. To give you an idea of how to package your own applications into the container, we have run Java and JavaScript applications using Docker. Of course, this is just the tip of the iceberg, but I hope you were given some inspirations to package your own applications, no matter what language or framework you use. Having the Docker knowledge you have acquired so far, you will be able to pack any application that you wrote into the container and publish it on the Docker Hub. The world of Docker is enormous today, there's a lot to explore. Further in the book, I will point you to other resources that I find very useful; you can use them to further extend your Docker knowledge base.

## More Resources

You have a lot of Docker knowledge at this point. After reading the previous chapters, you can now search and pull the image you need and build your own on top of it. We also know how to package an existing application, let it be a Java web application or a front end Angular.js application working on Node.js. We can do it by hand or use Maven for automating things. There are a lot of places on the Internet where you can extend your knowledge further. I'm going to present the ones I find most useful.

## Official documentation

It's hard not to recommend the official Docker documentation, which is of very high quality. You will find it at https://docs.docker.com. There is a lot of information here, starting from installing Docker on various platforms and going through pulling images, creating Dockerfiles, building images, and publishing them into the remote Registry. It's a great resource for refreshing the Docker knowledge you already have, but you will also find here a lot of information which was not mentioned in this book. This includes, for example:

- Compose is a tool for defining and running multi-container Docker applications. With this tool you can use a single Compose file to configure your application's services. Then, using a single command, you can create and start all the services from your configuration.
- Universal Control Plane is the enterprise-grade (you can install it behind your own firewall) cluster management solution from Docker. It helps you manage your whole cluster from a single interface.
- Docker Trusted Registry is an enterprise-grade image storage solution from Docker–a secure place to store and manage the Docker images you use.
- The Docker Store, which is the place to find the best-trusted commercial and free software distributed as Docker Images.
