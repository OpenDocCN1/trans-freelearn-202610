Now any command will be targeted at the remote host, so running a simple ps will show the container running on the remote daemon:

```
docker ps
CONTAINER ID      IMAGE                     COMMAND
CREATED           STATUS                    PORTS
NAMES
556224b9435e      hellonico/withdocker      "java -jar
/app/withd"      9 minutes ago             Up 9 minutes
0.0.0.0:8080->8080/tcp    withdocker
```

So now, even building an image will be done on the remote daemon:

```
docker build . -t hellonico/withdocker
```

This is so cool. That means deploying code from a different branch of a code repository to a remote environment can now be achieved in just a few seconds:

```
lein uberjar && docker build . -t hellonico/withdocker
```

As an exercise left to the reader, update the withdocker/handler.clj to something funnier, build the container again, and then run the Docker image with the following:

```
docker run -d -p 8081:8080 --name withdockerremote
hellonico/withdocker
```

The new image has been created, and is now running off port 8081:

![](bbox=[0.06, 0.76, 0.65, 0.91])