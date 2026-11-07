The CMD instruction should be used in a Dockerfile only once to run the software contained in the image. It's usually the last command in a Dockerfile, but when starting the container from such an image, we can override the CMD instruction simply by providing our own command or parameters as the COMMAND parameter for the docker run. Anything that appears after the image name in the docker run command is passed to the container and treated as CMD arguments. As a result, if the image also specifies an ENTRYPOINT then the CMD or COMMAND gets appended as arguments to the ENTRYPOINT. But guess what, we can override the ENTRYPOINT as well.

# Overriding the ENTRYPOINT

Every Docker image has an entry point. It can be the default one or the one specified by the ENTRYPOINT instruction, it's a default command to be executed at runtime; it specifies what executable to run when the container starts, but it is more difficult to override. The ENTRYPOINT makes a running container more like a complete command to execute, which takes parameters from the COMMAND value. But sometimes when running the container, you will want to run something else, so you can override the default ENTRYPOINT at runtime using a string to specify the new ENTRYPOINT. The syntax for overriding an entry point is as follows:

```
docker run -it --entrypoint /bin/bash Ubuntu
```

Let's take at look of the MongoDB Dockerfile, which has the ENTRYPOINT defined:

```
FROM dockerfile/ubuntu
RUN mkdir -p /data/db /data/configdb \
&&chown -R mongodb:mongodb /data/db /data/configdb
VOLUME /data/db /data/configdb
COPY docker-entrypoint.sh /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
EXPOSE 27017
CMD ["mongod"]
```

As you can see, there is a custom entry point defined; it will be an entrypoint.sh shell script. If you run this container now, the main process of the container will come from this script. We can override this entry point using the command:

```
docker run -it --entrypoint /bin/bash mongo
```