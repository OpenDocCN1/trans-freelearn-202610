Create the service account to be used as your user-specified service account for the Cloud Build pipeline:

```
$ gcloud iam service-accounts create ${SA_NAME} \
  --description="Build Dev Service Account" \
  --project ${DEV_PROJECT}
```

The service account needs Service Account User permissions so that it can be used by the Cloud Build service agent in the project that will use the Cloud Build service:

```
$ gcloud projects add-iam-policy-binding ${DEV_PROJECT} \
  --member=serviceAccount:${SA_NAME}@${DEV_PROJECT}.iam.gserviceaccount.com \
  --role=roles/iam.serviceAccountUser
```

The project number for the Google Cloud Project that will leverage the Cloud Build service is required so that service account impersonation can be performed. We set the permission for the Cloud Build service agent to be able to create service account tokens to execute commands on behalf of the service account hosted on our development project:

```
$ export BUILD_PROJECT_NUMBER=$(gcloud projects list \
  --filter="${BUILD_PROJECT}" \
  --format="value(PROJECT_NUMBER)"
)
$ gcloud projects add-iam-policy-binding ${DEV_PROJECT} \
  --member="serviceAccount:service-${BUILD_PROJECT_NUMBER}@gcp-sa-cloudbuild.iam.gserviceaccount.com" \
  --role="roles/iam.serviceAccountTokenCreator"
```

Although the service account is located on the development project, we give the service account permissions to write logs into the build project where the Cloud Build service will be utilized:

```
$ gcloud projects add-iam-policy-binding ${BUILD_PROJECT} \
  --member=serviceAccount:${SA_NAME}@${DEV_PROJECT}.iam.gserviceaccount.com \
  --role=roles/logging.logWriter
```