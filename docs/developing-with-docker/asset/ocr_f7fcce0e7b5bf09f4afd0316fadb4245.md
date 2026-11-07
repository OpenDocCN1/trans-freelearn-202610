When you run your container and want to interact with it later, updating the restrictions for example, you must tell Docker the image that you want to execute the command for. Images can be identified in the Docker world in a couple of ways. Until now, we have been executing images using long or short UUID identifiers. Let's see some options here.

# Identifying images and containers

As you remember from the previous chapters, images that you want to run can be identified simply by there name, such as Ubuntu. In this case, Docker will pick the image tagged by latest by default. You can also use the tag syntax or the digest syntax:

```
Image[:tag]
Image[@digest]
```

For example, if you want to run Ubuntu tagged as version 14.04, you simply execute the command:

```
docker run ubuntu:14.04
```

The image can contain an identifier called a digest. The digest is calculated as the sha256 checksum of the image manifest, excluding the signature portion. The manifest comprises of the image's name, its tag (latest), architecture, fsLayers (a list of the image's layers, referenced by their digests), history, schema version, and signatures. You can list the digest by specifying the –digests option to the docker images command. A digest can be used with the docker create, docker pull, and dockerrmi commands and with the FROM instruction in a Dockerfile. Also, it can be used in the docker run command, to precisely run the specified image. The digest value is predictable and referenceable and allows of picking the image very precisely.

The following example will run the Ubuntu image that was selected using a digest:

```
docker -it run
ubuntu@sha256:1ffcb21397aa4012a9d88cff8582224ff06aa13ba2de2e9f96c9814bfda95e0 /bin/bash
```