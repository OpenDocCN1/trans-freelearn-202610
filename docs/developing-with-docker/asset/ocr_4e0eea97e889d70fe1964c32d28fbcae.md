# FROM

The syntax for the FROM instruction is very simple. It can have three forms. It can be as simple as that:

```
FROM <image>
```

By default, Docker will use the image tagged latest. If you need to build upon different tags, you will need to add the tag name after the name of the image. The FROM instruction will take this form then:

```
FROM <image>:<tag>
```

Last but not least, you can even more precisely pick the image using its digest. As you remember from the previous chapters, images with the same tag can change in time, if their author decides so. If you like to lock your base image to the fixed version, you can specify its digest as the image name. This way your own image will be always built using the specific version of the base image, regardless of tags. To specify base image using digest, use the following syntax of the FROM instruction:

```
FROM <image>@<digest>
```

The FROM instruction tells Docker what will be the base for the image being created. Here you can use your skills of finding images on the Docker Hub, learned in Chapter 5, Finding Images. Now this is important–as it will be starting point–choose wisely. Of course you can pick whatever you want to be base for your own image, it's up to you–but my recommendation would be to always prefer the official repositories. By choosing the official image, you can be pretty sure it will be of high quality, tested, supported, and maintained.

![](bbox=[0.13, 0.64, 0.2, 0.7])

You can use your own images in the FROM instruction.

If a FROM image is not found on your Docker host (on your local machine, for example), Docker will try to find and download it from the Docker image index. Again, the Docker Hub will be picked by default.