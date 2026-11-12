# Using Continuous Integration to Build, Test, and Deploy Containers

One of the most important parts of running containers is building the images those containers are based on. After all, without an image, there is no container. One of the challenges is to ensure that the Docker images actually, not only build, but also work *before* being deployed.

This chapter will demonstrate how to use GitLab to implement **Continuous Integration** (CI) for Docker images. It will show how to automatically build an image, run a test suite against it, push the new image into a Docker Registry, and even deploy the new image to a Docker cluster.

The following topics will be covered in the chapter:

+   • The importance of using CI
+   • Setting up a project in GitLab for CI
+   • Automatically building a new image
+   • Running tests against an image
+   • Releasing a tested image
+   • Automatically deploying the image
+   • Using GitLab CI to create DAB files
+   • Using Docker Cloud for image testing