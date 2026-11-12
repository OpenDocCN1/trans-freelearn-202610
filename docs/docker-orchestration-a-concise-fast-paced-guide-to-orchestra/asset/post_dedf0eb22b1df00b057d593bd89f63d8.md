
The fact that GitLab does not automatically tag the image with the `latest` tag is a feature. The idea is for the CI plan in `.gitlab-ci.yml` to explicitly tag the image. This is done by creating a new job. A snippet of `.gitlab-ci.yml` is shown here:

```yaml
variables:
  DOCKER_CI_IMAGE: registry.gitlab.com/perlstalker/ci-demo:${CI_BUILD_REF_NAME}
  DOCKER_RELEASE_IMAGE: registry.gitlab.com/perlstalker/ci-demo:latest
...
release-image:
  stage: release
  tags:
  - docker
  script:
  - docker pull $DOCKER_CI_IMAGE
  - docker tag $DOCKER_CI_IMAGE $DOCKER_RELEASE_IMAGE
  - docker push $DOCKER_RELEASE_IMAGE
  only:
  - master
```

The first difference is that a new variable, `$DOCKER_RELEASE_IMAGE`, was added with the name of the release image.

![](img/02bead9c8ff4515f78dac0eb33163961_238_0.png)

Note that the image tag is `latest`.

As always, the work is done in the `script` block. The first step is to pull the latest copy of the image for this branch. Next, `docker tag` tags that image with the release image name. Specifically, it is given the `latest` tag. Finally, the `docker push` command pushes the newly tagged image back into the registry.

The `only` block is important. It tells GitLab that this job must only be run when building one of the listed branches. In this case, `master`. Without the `only` block, any newly built image from any branch would replace the `latest` image.

The nice thing about this configuration is that new images can be worked on in different branches and images for those branches will be created. Those images can be individually run, tested, and debugged without modifying the released image. When the changes are ready, the development branch can be merged back into the `master` branch and a new `latest` image is created.

## Deploying the image

Let's take a moment and see where things stand. At this point, new Docker images for a project are being automatically built any time changes are pushed to the Git repository in GitLab. A test suite is being run against the new image to ensure that it will function as expected. If the changes are pushed to the `master` branch, a new release image is created. The only thing left to do is to deploy the image to the cluster.

How the image is deployed will depend on the orchestration suite chosen. In most cases, all that is needed is to run the command that performs a rolling update of the service. A snippet of an automated deployment job is as follows:

```yaml
deploy-to-dev:
  stage: deploy
  tags:
    - docker
  environment: development
  script:
    - ./deploy.sh $DOCKER_CI_IMAGE development
  only:
    - master
```

There are a couple of things to note here. First, the `stage` is set to `deploy`. The job will only run if the `build`, `test`, and `release` stages are completed successfully; `environment` is optional. The **Environments** page in the **Pipelines** section in the project makes it easy to track deployments to different environments:

![](img/02bead9c8ff4515f78dac0eb33163961_240_0.png)

The `deploy.sh` script in the snippet is a placeholder. The script can contain whatever is needed to deploy the image in your environment. The script could also be included directly in the `script` block. Later in the chapter, it will be shown how to deploy an image to a Kubernetes cluster.

> **TIP** Consider using the `tags` keyword instead of a branch name in the `only` section for deployments. This will limit deployments only to commits that have been tagged. It will ensure that all images that are deployed use the tag. It will be easier to tell which version of the image a container is running based on the tag on the image.

## Deploying the image manually

Automatically deploying an image to the cluster is exciting. Perhaps too exciting. Not everyone wants services to be updated automatically. This does not mean that GitLab cannot be used for deployments. GitLab provides a way of configuring a job to run manually. The build process will run as it normally does, but when it gets to a manual job, it will wait until someone tells GitLab that it is okay to proceed. This provides the consistency of an automated deployment while still allowing a human to decide whether the deployment happens or not.

The following .gitlab-ci.yml snippet shows how to create a job with a manual trigger. It is basically the same as the preceding automated deployment. The only functional change is the when tag:

```yaml
deploy-to-prod:
  stage: deploy
  tags:
    - docker
  environment: production
  script:
    - ./deploy.sh $DOCKER_CI_IMAGE production
  only:
    - master
  when: manual
```

Setting when to manual will pause the build and deployment process until someone goes to Pipelines, clicks on the build, and clicks on the deployment. The following screenshot shows the build pipeline:

![](img/02bead9c8ff4515f78dac0eb33163961_241_0.png)

Note that all of the jobs have succeeded. Only the `deploy-to-prod` job has not. The triangle play button shows that the `deploy-to-prod` job is awaiting `manual` action. When that button is pushed, the deployment will begin.

Any number of deployment jobs may be defined to fit a variety of needs. For example, a fully automatic job could deploy new images to a development environment. A manual job could then trigger the deployment to a QA or canary environment. Finally, another manual job could deploy the image fully into production. This could be done with parallel jobs as shown here or as separate build stages. Using separate stages ensures that each previous deployment is done before finally being pushed into production.

## Deploying to Kubernetes

The preceding examples used a dummy deploy script. Let's take a look at how one might use GitLab CI to deploy a new image to Kubernetes. In Chapter 5, Deploying and Managing Services with Kubernetes we covered managing deployments in Kubernetes.

The following .gitlab-ci.yml snippet makes a couple of assumptions:

- First, kubectl is installed on the runner machine and that the gitlab-runner user has permissions to make changes to the Kubernetes cluster
- Second, a deployment has already been created which will be updated by the build process:

```yaml
variables:
  DOCKER_CI_IMAGE: registry.gitlab.com/perlstalker/ci-demo:$CI_BUILD_REF_NAME
  DOCKER_RELEASE_IMAGE: registry.gitlab.com/perlstalker/ci-demo:latest
DEPLOYMENT: ci-demo
CONTAINER: web
...
deploy-to-kube:
  stage: deploy
  tags:
    - kubectl
  environment: production
  script:
    - kubectl set image deployment/$DEPLOYMENT
      $CONTAINER=$DOCKER_CI_IMAGE
  only:
    - tags
  when: manual
```

Two new variables, DEPLOYMENT and CONTAINER are added to the configuration. They contain the name of the deployment and the container within that deployment that will be updated, respectively.

As an example, the kubectl tag is set on the job. The expectation is that a runner with the kubectl tag is available, which has kubectl configured to manage the cluster. This makes it easy to create a runner that is only used for deployments and is not used for other build jobs.

The script contains only the command to update an image in the deployment. It is not necessary to do anything with the image in the runner. The Kubernetes nodes that are running the deployment will perform the update on the container based on the deployment strategy of the pod.

Finally, the only block tells GitLab to only run this job if the build has a git tag set. This will ensure that the built image has a different tag than what was running previously. If the image name is the same as what the deployment currently uses, Kubernetes will not know that the image needs to be updated.

## Building DAB files

Docker Swarm is moving to Distributed Application Bundles (DAB) to define stacks for deployment. GitLab CI can be used to automatically build DAB files for a project. The following snippet assumes that docker-compose.yml is in the root directory of the project:

```yaml
build-dab:
  stage: release
  tags:
    - docker
  script:
    - docker-compose bundle
  artifacts:
    paths:
      - *.dab
    name: "${PROJECT_NAME}-${CI_BUILD_REF_NAME}"
```

The script block runs docker-compose bundle which will generate the .dab file. Additional options can be added to the command, if needed. The artifacts block tells GitLab that the build process generates files that are important. In this example, the .dab files will be saved if the build is successful. All of the files matching the entries in the path list will be added to the artifacts archive file.
