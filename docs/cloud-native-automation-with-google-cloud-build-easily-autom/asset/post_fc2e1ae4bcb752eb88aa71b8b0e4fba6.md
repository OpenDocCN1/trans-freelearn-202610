
```
serviceAccounts/${GCP_SA_NAME_01}@${PROJECT_ID}.iam.gserviceaccount.com
$ gcloud beta builds triggers create github \
  --name=team-a-deploy \
  --region=$REGION \
  --repo-name=$GH_A \
  --repo-owner=$GH_USERNAME \
  --branch-pattern=master --build-config=deploy-a.yaml \
  --service-account=projects/$PROJECT_ID/serviceAccounts/${GCP_SA_NAME_01}@${PROJECT_ID}.iam.gserviceaccount.com \
  --require-approval
```

Let's review some of the differences between the two triggers.

In the first trigger created, we are utilizing Cloud Build's support for triggering on PRs by using the --pull-request-pattern parameter. By specifying main, this means that builds will run upon PR being opened against the master branch.

In the second trigger created, we specify --branch-pattern instead to trigger on any changes to the master branch.

Now, create triggers for Team B, which will follow the same pattern as the triggers for Team A, as follows:

```
$ gcloud beta builds triggers create github \
  --name=team-b-build \
  --region=$REGION \
  --repo-name=$GH_B \
  --repo-owner=$GH_USERNAME \
  --pull-request-pattern=master --build-config=build-b.yaml \
  --service-account=projects/$PROJECT_ID/serviceAccounts/${GCP_SA_NAME_02}@${PROJECT_ID}.iam.gserviceaccount.com
$ gcloud beta builds triggers create github \
  --name=team-b-deploy \
  --region=$REGION \
  --repo-name=$GH_B\
  --repo-owner=$GH_USERNAME \
  --branch-pattern=master --build-config=deploy-b.yaml \
  --service-account=projects/$PROJECT_ID/serviceAccounts/${GCP_SA_NAME_02}@${PROJECT_ID}.iam.gserviceaccount.com \
  --require-approval
```

With the triggers now created, we can now review how we can gate these builds, ensuring that there can be additional human review before the builds actually run.

## Gating builds with manual approvals

This brings us to the last difference you may have noticed between each of the two triggers for each team—the addition of the -require-approval flag, which introduces this method of explicit and manual approval for builds.

This provides a basis in which an administrator with the distinct IAM permissions of Cloud Build Approver can approve builds that require approval. Without this approval, the builds sit and wait and do not execute.

For this example workflow, we place manual approvals in front of the second trigger for both Team A and Team B, allowing for a gate prior to simulating a rollout to production.

If you visit the Cloud Build console, you will see the following options for a build awaiting approval:

![](img/2fe9576240c146c49e3a21d5e7f6af00_170_0.png)

Figure 8.10 – Viewing individual build approval requests

When actually approving the build, not only does this action get captured in Cloud Audit Logging to understand who performed which rollout and when, but it also enables the Cloud Build Approver to provide reasons and metadata that defend the decision to approve the build, as illustrated in the following screenshot:

## Approve Build

Approving will cause this build to run. Learn more

This trigger has 8 older builds awaiting approval. This is the latest.

Add an optional message to be displayed with the build's approval 0 / 1000

SHOW ADDITIONAL OPTIONS

CANCEL   APPROVE

Figure 8.11 – Viewing build approval dialog from the Cloud Build console

This allows for multiple gates, both implicit and explicit, that allow for human review to decide whether or not code and artifacts get promoted into different environments, including production.

Now, let’s run through this example from end to end by triggering all four of the builds we described for Team A and Team B respectively.

## Executing builds via build triggers

Begin by creating Cloud Build configuration files for each of these triggers.

As a reminder, if you have lost context since working on the previous section, run the following commands to navigate to the right directory and source the proper environment variables.

Replace <add-project-id>, shown here, with the project you are working with:

```
$ gcloud config set project <add-project-id>
```

Navigate to the directory you are working in, as follows:

```
$ cd ~/cloudbuild-gke/08
```

Source the required environment variables, like so:

```
$ source bin/variables.sh
```

Start by creating build files for building container images for Team A and Team B. These builds will be responsible for building a container image and pushing it to the Team A and Team B repositories in Artifact Registry respectively. Here's the code you need to execute:

```
$ cat << EOF > bin/$GH_A/build-a.yaml
steps:
  - name: gcr.io/cloud-builders/docker
    id: Build container image
    args: ['build', '-t', '${REGION}-docker.pkg.dev/${PROJECT_ID}/${REPOSITORY_A}/team-a-app:v1',  '.']
images: [${REGION}-docker.pkg.dev/${PROJECT_ID}/${REPOSITORY_A}/team-a-app:v1]
serviceAccount: 'projects/${PROJECT_ID}/serviceAccounts/${GCP_SA_NAME_01}@${PROJECT_ID}.iam.gserviceaccount.com'
options:
  requestedVerifyOption: VERIFIED
  workerPool: 'projects/${PROJECT_NUM}/locations/${REGION}/workerPools/${PRIVATE_POOL_NAME}'
  logging: CLOUD_LOGGING_ONLY
images: [${REGION}-docker.pkg.dev/${PROJECT_ID}/${REPOSITORY_A}/team-a-app:v1]
EOF

$ cat << EOF > bin/build-b.yaml
steps:
  - name: gcr.io/cloud-builders/docker
    id: Build container image
    args: ['build', '-t', '${REGION}-docker.pkg.dev/${PROJECT_ID}/${REPOSITORY_B}/team-b-app:v1',  '.']
images: [${REGION}-docker.pkg.dev/${PROJECT_ID}/${REPOSITORY_B}/team-b-app:v1]
serviceAccount: 'projects/${PROJECT_ID}/serviceAccounts/${GCP_SA_NAME_02}@${PROJECT_ID}.iam.gserviceaccount.com'
options:
  requestedVerifyOption: VERIFIED
  workerPool: 'projects/$PROJECT_NUM/locations/$REGION/workerPools/$PRIVATE_POOL_NAME'
  logging: CLOUD_LOGGING_ONLY
images: [${REGION}-docker.pkg.dev/$PROJECT_ID/$REPOSITORY_B/team-b-app:v1]
EOF
```

Next, create build files to deploy Team A and Team B's applications to their respective namespaces in the GKE cluster, as follows:

```
$ cat << EOF > bin/$GH_A/deploy-a.yaml
steps:
- name: gcr.io/cloud-builders/gke-deploy
  id: Prep k8s manifests
  args:
  - 'prepare'
  - '--filename=k8s.yaml'
  - '--image=${REGION}-docker.pkg.dev/$PROJECT_ID/$REPOSITORY_A/team-a-app:v1'
  - '--version=v1'
- name: gcr.io/google.com/cloudsdktool/cloud-sdk
  id: Get kubeconfig and apply manifests
  entrypoint: sh
  args:
  - '-c'
  - |
    gcloud container clusters get-credentials $CLUSTER_NAME --region $REGION --project $PROJECT_ID
    kubectl apply -f output/expanded/aggregated-resources.yaml -n $NAMESPACE_01
serviceAccount: 'projects/${PROJECT_ID}/serviceAccounts/${GCP_SA_NAME_01}@${PROJECT_ID}.iam.gserviceaccount.com'
options:
  workerPool: 'projects/$PROJECT_NUM/locations/$REGION/workerPools/$PRIVATE_POOL_NAME'
  logging: CLOUD_LOGGING_ONLY
EOF

$ cat << EOF > bin/$GH_B/deploy-b.yaml
steps:
  - name: gcr.io/cloud-builders/gke-deploy
    id: Prep k8s manifests
    args:
      - 'prepare'
      - '--filename=k8s.yaml'
      - '--image=${REGION}-docker.pkg.dev/${PROJECT_ID}/${REPOSITORY_A}/team-b-app:v1'
      - '--version=v1'
  - name: gcr.io/google.com/cloudsdktool/cloud-sdk
    id: Get kubeconfig and apply manifests
    entrypoint: sh
    args:
      - '-c'
      - |
        gcloud container clusters get-credentials $CLUSTER_NAME --region $REGION --project $PROJECT_ID
        kubectl apply -f output/expanded/aggregated-resources.yaml -n $NAMESPACE_02
serviceAccount: 'projects/${PROJECT_ID}/serviceAccounts/${GCP_SA_NAME_02}@${PROJECT_ID}.iam.gserviceaccount.com'
options:
  workerPool: 'projects/$PROJECT_NUM/locations/$REGION/workerPools/$PRIVATE_POOL_NAME'
  logging: CLOUD_LOGGING_ONLY
EOF
```

With the build files in place for each team's repository, let's now get those changes upstream. We'll create a test branch and push those to our remote repository on GitHub.com, like so:

```
$ cd bin/$GH_A
$ git branch test
$ git checkout test
$ git add .
$ git commit -m "Add Cloud Build YAML files."
$ git push --set-upstream origin HEAD
$ cd ../$GH_B
$ git branch test
```
