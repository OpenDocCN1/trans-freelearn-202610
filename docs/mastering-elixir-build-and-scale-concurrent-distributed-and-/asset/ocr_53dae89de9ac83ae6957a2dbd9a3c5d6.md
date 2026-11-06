In the previous snippet, we are telling Travis to do different things based on the build phase it is in. In the `before_install` phase, it updates Docker and installs Docker Compose. Then, in the `script` phase, it spins up the `docker-compose.test.yml` compose file to build the image release. If the previous step succeeds, it installs the needed `gcloud` and `kubectl` tools in the `before_deploy` phase. It's in the `deploy` phase that Travis will run the `deploy/travis_deploy.sh` script that actually deploys the application to Kubernetes. The last `after_script` step stops and removes the containers that were started in the previous `script` phase.

![](bbox=[0.12, 0.28, 0.2, 0.36])

Because we're using the `sudo: required` option, Travis spins up an Ubuntu machine for the build process. The `before_install` and `before_deploy` phases were omitted for brevity, but you can find the full `.travis.yml` file in this book's code.

When Travis detects a new commit in the `master` Git branch, it will start a temporary virtual machine, fetch the specific code version from the GitHub repository, and start the previously described steps. In the `.travis.yml` file, we are setting the `PROJECT_ID` environment variable, but Travis also sets the `TRAVIS_COMMIT` variable (among others) for us, which we'll use to tag the release image.

Remember the `app` service in the `docker-compose.test.yml` compose file tagging the resulting Docker image as `gcr.io/${PROJECT_ID}/elixir-drip-prod:${BUILD_TAG}`? This is why we need to set the `PROJECT_ID` and `BUILD_TAG` environment variables before starting the compose file (we use the Git commit as the `BUILD_TAG` value).

Before we delve into the `deploy/travis_deploy.sh` script, we need to create a Google Cloud service account, which will be used by Travis CI to push the new release image and apply the Kubernetes migrations job and deployment configuration files:

```
$ gcloud iam service-accounts create travis-ci-k8s-deployment-prod --display-name "Travis CI K8s Prod Account"
Created service account [travis-ci-k8s-deployment-prod].
```

```
$ gcloud projects add-iam-policy-binding intense-talent-188323 --member serviceAccount:travis-ci-k8s-deployment-prod@intense-talent-188323.iam.gserviceaccount.com --role roles/container.developer
```

```
$ gcloud projects add-iam-policy-binding intense-talent-188323 --member serviceAccount:travis-ci-k8s-deployment-prod@intense-talent-188323.iam.gserviceaccount.com --role roles/cloudbuild.builds.builder
  # ...
```