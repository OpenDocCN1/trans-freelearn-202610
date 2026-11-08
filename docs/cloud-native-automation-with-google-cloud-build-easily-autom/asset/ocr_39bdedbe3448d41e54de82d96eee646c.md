```
metadata:
  name: staging
description: staging cluster
gke:
  cluster: projects/PROJECT_ID/locations/us-central1/clusters/
  staging
---
apiVersion: deploy.cloud.google.com/v1
kind: Target
metadata:
  name: prod
description: prod cluster
gke:
  cluster: projects/PROJECT_ID/locations/us-central1/clusters/
  prod
```

Now that users have defined their respective targets, they can then begin to use their DeliveryPipeline template to create an individual Release – which references that DeliveryPipeline template and also intakes a skaffold.yaml configuration file.

The following command shows how users can kick off by deploying the new version of their application to their desired targets using the Release resource:

```
$ gcloud deploy releases create release-01 \
  --project=PROJECT_ID \
  --region=us-central1 \
  --delivery-pipeline=my-demo-app-1 \
  --images=app-01=gcr.io/PROJECT_ID/image:imagetag
```

This resource compiles down into a Rollout for each Target.

This is where we see a stark difference with doing this via Cloud Build alone. Cloud Deploy enables you to promote an artifact through each Rollout, while also providing easy roll-back mechanisms when deploying this artifact to each Target environment: