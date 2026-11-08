When you are ready to make this judgment, the --to-latest flag allows you to explicitly send all production traffic to your latest revision, effectively promoting that revision to production.

The following sample build step uses this flag:

```
# For demonstration purposes only, to run this build would
require setup beyond the scope of its inclusion
steps:
- id: Send 100% of traffic to latest version
  name: 'gcr.io/google.com/cloudsdktool/cloud-sdk'
  args:
  - 'gcloud'
  - 'run'
  - 'services'
  - 'update-traffic'
  - 'SERVICE_NAME'
  - '--to-latest'
  - '--region'
  - 'us-central1'
```

This model also expands to Cloud Functions 2nd gen. While Cloud Functions 2nd gen abstracts away much of what users need to do with Cloud Run, traffic management still goes through the Cloud Run API.

Using Cloud Build to orchestrate this can enable you to trigger these respective processes manually, or in accordance with changes to your source code repository.

Traffic management is not the only complex deployment pattern that can be utilized with Cloud Run and Cloud Build. Let's now review how you can implement security best practices when deploying to Cloud Run.

# Securing production with Binary Authorization

Given that Cloud Run allows developers, in theory, to deploy any container image they would like, how can administrators prevent developers from deploying arbitrary images, or worse, prevent bad actors from deploying harmful or malicious images?

Similar to what we reviewed for GKE in Chapter 8, Securing Software Delivery to GKE with Cloud Build Binary Authorization is also a feature that is available to Cloud Run developers.

This is the functionality that performs admission control for container images upon deployment, denying container images that violate predefined policies by cloud or security administrators.