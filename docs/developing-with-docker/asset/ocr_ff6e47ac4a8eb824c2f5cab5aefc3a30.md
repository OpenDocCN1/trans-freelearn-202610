Next, click on the CREATE button next to the hello-world image. Kitematic (docker-machine to be precise) will download the image from the Docker Hub and present you with the output:

![](bbox=[0.1, 0.18, 0.9, 0.59])

From now on, Docker's command-line tools are also at your disposal. Opening the Terminal from Kitematic will automatically set all the required environment variables and connect you to the default machine. But if you would like to be able to work from your own Terminal session, you may get a message saying that Docker client cannot connect to the Docker daemon. The reason is that the client doesn't know what machine it should control. To attach your client to the specific machine, type the following command in your Terminal session:

```
eval "$(docker-machine env default)"
```

The eval command sets environment variables to dictate that docker should run a command against a particular machine, which is the default machine in our example.

To see the list of Docker-related environment variables, use the env command:

```
env | grep DOCKER
```