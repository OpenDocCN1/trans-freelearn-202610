![](bbox=[0.115, 0.085, 0.855, 0.435])

Figure 4.2 – Google-published container images for gcr.io/cloud-builders/docker

If you would like to customize these container images, you can view the provided source code (https://github.com/GoogleCloudPlatform/cloud-builders) and build your own variations of these images. For each builder, you can find a Dockerfile defining the container image for the cloud builder, along with a cloudbuild.yaml file to build your own version of the cloud builder.

If your use case for an individual build step is not covered by one of the provided cloud builder images, you can also turn to builders that are provided by the Cloud Build community (https://github.com/GoogleCloudPlatform/cloud-builders-community).

These are far greater in number and have varying cadences around the frequency of updates or required maintenance, as they are community owned. Some of the more commonly used community-contributed builders include the following:

+   - Container and Kubernetes builders, such as `helm`, `kustomize`, `ko`, and `pack`
+   - Infrastructure-as-code builders, such as `terraform`, `pulumi`, and `terragrunt`
+   - Mobile development builders, such as `firebase` and `flutter`

Similar to the official cloud builders, each of these community-contributed builders provides a Dockerfile, defining the builder container image, and a cloudbuild.yaml configuration file, to actually use Cloud Build to build the builder container image.