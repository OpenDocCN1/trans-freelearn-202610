This command starts a new container based on the `nginx` image, downloading it if needed, and telling `docker` to forward TCP port 80 to port 80 in the image. Now you can go to `http://localhost` and see a most welcoming website:

![](bbox=[0.256, 0.185, 0.742, 0.379])

Welcoming people to Nginx is all well and good, but obviously, you will want to do more than that. That will be covered in more detail in *Chapter 2, Building Multi-Container Applications with Docker Compose*. For now, the default will be sufficient.

If you run the preceding example, you will notice that the console appears to hang. That's because `docker` starts processes in the foreground. What you are seeing there is `nginx` waiting for a request. If you go to `http://localhost`, then you should see messages from the `nginx` access log printed to the console. Another option is to add `-d` to your `run` command. That will detach the process from the console:

```
$ docker run -d -p 80:80 --name nginx nginx
```

> The `-d` and `--rm` options are mutually exclusive.

There are multiple ways to stop a container. The first way is to end the process running in the container. This will happen automatically for short running processes. When starting a container like `nginx` in the preceding example, pressing `Ctrl + C` in the session will stop `nginx` and the container. The other way is to use `docker stop`. It takes the image ID or name of the container. For example, to stop the container that was started earlier you would run `docker stop nginx`.