This is a simple file but there are two important pieces here. First is the FROM command, which tells Docker that your container extends the nginx image that was used before. Basing your custom images on the existing images can save a lot of time and hassle. They may not be perfect out of the box but often it only takes a few changes to get what you need.

Second, there is the ADD command. The nginx container uses /usr/share/nginx/html within the container as the document root. The ADD command places the content of the website directory in /usr/share/nginx/html when the container is built.

The ADD command can add single files, directories, or tarballs. The TAR files will be unpacked in the directory that you specify. The TAR file can be compressed with gzip, bzip2, or xz. The ADD command can also take a URL. The file pointed to by ADD will be downloaded and placed in the given directory in your image. A tarball added via URL will not be unpacked.

In addition to the ADD command, there is a command called COPY. It simply adds the given files or directories with any of the automated unpacking that ADD does. The COPY command is preferred when adding simple files or directories to an image.

Now, it is time to build the container. Change to the directory with your Dockerfile and website directory and run docker build. When the build completes, you will have an image named website ready to use with docker run:

```
$ docker build -t website .
```

# Tagging the image

The -t flag allows you to give your image a name. The name may include a : followed by a tag. The most common way of using this is as a version number. It might also be a date, Git tag, or something else that makes sense in your environment. For example, the official ubuntu image has tags for releases such as ubuntu:trusty or ubuntu:xenial, which refer to the appropriate Ubuntu release:

```
$ docker build -t website:0.1 .
```

Image versioning will be important going forward. At some point, you will be deploying an updated version of your service. It is possible to reuse a name and version, which may be fine while you develop the application, but different versions will make it easy to roll back to a previous, known good image in the event of a failure. It also allows you or your QA team to test an image without overwriting the image your production services are started from.