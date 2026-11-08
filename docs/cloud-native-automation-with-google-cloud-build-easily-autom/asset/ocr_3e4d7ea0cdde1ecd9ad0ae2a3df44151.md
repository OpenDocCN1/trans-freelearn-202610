# Defining your build step container image

Let's start with the top-level field, `steps`. This field encompasses all the build steps that will make up a build. All three build steps in the example are defined by stanzas underneath the `steps` field.

> **Note**
> At the time of writing, the maximum number of build steps you can have in a single build is 100 build steps. You can find the most up-to-date limitations at https://cloud.google.com/build/quotas#resource_limits.

In any of these stanzas, we can see the fields that a user must define to specify a single build step. At a bare minimum, we need to define the following two fields:

+   - `name`
+   - `args`

Here, `name` is a field that references the specific image path to your builder image that will run your build step. When you specify your container image, your Cloud Build worker will pull this container image and run an instance of it using `docker run`.

In the preceding example, we can see that the first and second steps both utilize the `gcr.io/cloud-builders/docker` image and that the third step utilizes the `gcr.io/google.com/cloudsdktool/cloud-sdk` image.

Note that to run this build in the prior chapter, you actually did not have to build these images yourself. Rather, these images are a part of the pre-built container images provided and hosted by Google that contain common toolings such as `docker` or `gcloud`. These Google-provided publicly available images are called **cloud builders** (https://cloud.google.com/build/docs/cloud-builders).

At the time of writing, cloud builders provide pre-built images for the following tools:

+   - General purpose builders, such as `wget`, `curl`, and `git`
+   - Language-specific builders, such as `dotnet`, `go`, `mvn`, `javac`, `npm`, and `yarn`
+   - Google Cloud-specific builders, such as `gcloud`, `gsutil`, `gcs-fetcher`, and `gke-deploy`
+   - Container- and Kubernetes-specific builders, such as `docker` and `kubectl`

Because these cloud builders are made available publicly by Google, you can actually navigate to the image path and see all of the versions of the image that Google has published. For example, navigate to `gcr.io/cloud-builders/docker` in your browser: