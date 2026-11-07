The simplest way to implement your own credentials store will be to browse the source code for the existing helpers at GitHub https://github.com/docker/docker-credential-helpers.

Now that we understand what happens when you log into the registry, it's time to make use of it and search for an image.

# Searching for images

To effectively search for a specific image, first we need to understand the Docker image naming schema and tags concept. We have already mentioned it before; let's look at it now in detail.

# Image naming and tags

The full image name has the following syntax:

```
{registry_host}/(_|/{user_or_org_namespace})/{repository}:{tag}
```

Let's explore it in detail, image names start with a namespace and a slash. For example, the image named `hello-world` created and uploaded by user `tutum` (which is an Apache image responding with `hello-world` message on port `80`, by the way) will begin with this part: `tutum/hello-world`.

There can also be images with no namespace prefix-these will be images from the so-called **official repositories**. The official repositories are certified repositories from vendors and contributors to Docker such as Red Hat, Oracle, and Canonical. And of course, you can use them to build your own applications and services. An example of the official images can be the most-starred `ubuntu`, `nginx`, or the official version of `hello-world`-images that we have been using in the previous chapters. Official images are usually supported by the people who have created the application that is containerized in Docker, for example, `nginx` image from the official repository is supported by the Nginx team.

![](bbox=[0.12, 0.78, 0.2, 0.86])

If you decide to use an image from the official repository as the base for your own work, you can be pretty sure you're using a maintained, optimized, and up-to-date image.