# ## 4. Push the Docker image to Docker Hub

After tagging the image, the last step is to push the tagged image to Docker Hub.

For this purpose, we will execute the following command:

```
docker push docker.io/<dockerhub login>/demobook:v1
```

The following screenshot shows the execution of the preceding command:

![](bbox=[0.133, 0.233, 0.867, 0.337])

We can see from this execution that the image is uploaded to Docker Hub.

To view the pushed image in Docker Hub, we connect to the Docker Hub web portal at https://hub.docker.com/ and see that the image is present, as shown in the following screenshot:

![](bbox=[0.133, 0.486, 0.867, 0.761])

By default, the image pushed to Docker Hub is in public mode—everybody can view it in the explorer and use it.

We can access this image in Docker Hub in the Docker Hub search engine, as shown in the following screenshot: