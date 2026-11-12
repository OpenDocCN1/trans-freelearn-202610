The following topics will be covered in this chapter:

+   - Building an image from a `Dockerfile` with Docker Engine
+   - Installing Docker Compose
+   - Using Docker Compose to build single and multi-container applications
+   - Using environment files to change how your application runs
+   - Extending Docker Compose files
+   - Controlling the container start order
+   - Using named networks for data isolation
+   - Using volumes for data persistence

# # Building an image with Docker Engine

They say the longest journey begins with a single step. In this case, an application begins with a single image. More often than not, existing images will not be perfect fits. This will certainly be the case if you are deploying your own services.

It all starts with a `Dockerfile`. This file provides detailed instructions to Docker for building an image. There are a lot of things that can be done in a `Dockerfile`. Specific operating system packages or language libraries can be installed; configuration files can be put into place; and a default command can be set. It is best to base a new image for an application off an official image, when possible. For example, an application written in Ruby should be based off of the official `ruby` image. A full reference of the `Dockerfile` directives can be found at https://docs.docker.com/engine/reference/builder/.

# # Building from a Dockerfile

Back in Chapter 1, Getting Started with Docker Orchestration, we started with a container that ran Nginx and displayed the default welcome page. This time, let us add a custom website to the container. Start by creating an empty directory somewhere. In that directory, download the examples from the GitLab repository for this book:

```
$ git clone https://gitlab.com/perlstalker/docker-orchestration-examples
$ cd docker-orchestration-examples/ch02
```

Then, create a file named `Dockerfile` with the following content:

```
FROM nginx:1.10
COPY sample-website /usr/share/nginx/html
```