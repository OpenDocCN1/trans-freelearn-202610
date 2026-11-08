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
```