To run the test, a new job needs to be added to .gitlab-ci.yml. The following snippet shows the job. Once .gitlab-ci.yml is updated in the repository, future commits will run the tests:

```
test-image:
  stage: test
  tags:
    - docker
  script:
    - docker pull $DOCKER_CI_IMAGE
    - docker run -d -P --name $CI_BUILD_ID $DOCKER_CI_IMAGE
    - docker run --rm --network container:$CI_BUILD_ID --name $CI_BUILD_ID-test
      --entrypoint /tests/run-tests.sh $DOCKER_CI_IMAGE
    - docker stop $CI_BUILD_ID; docker rm -f $CI_BUILD_ID
```

The stage option is set so that the job will run in the test stage. This job will not run if the build stage fails. It also includes the docker tag so that GitLab will use a runner that is also tagged with docker.

The script section lists the commands that are run to test the image. The first two commands download the image and run it in the background. The container name is set to $CI_BUILD_ID, which is the unique build ID assigned to the build by GitLab. It is important to set the name because it will be needed when the test is run.

The third entry in the script block runs the actual test. It starts a new container that shares the target container's network. This allows the tests to connect to the web server on localhost. The test container runs the test script which was shown earlier. The grep in the script will fail if the pattern is not matched and the test will return a non-zero exit code, which GitLab interprets as a failure. If the pattern in the script is found, the test succeeds and the process moves on.

The last line stops the target container and removes it. The container running the tests is removed automatically when the test is done because --rm was used when it was started.

# Cleanup jobs

The preceding test job works great when the tests succeed, but when it fails, it can leave the test container running. This is not an issue when using the shared runner on GitLab.com because a new VM is started for every test and removed immediately after the build process has been completed. However, it could be a problem when using a local runner. In that case, a job may be needed for cleanup after the test.