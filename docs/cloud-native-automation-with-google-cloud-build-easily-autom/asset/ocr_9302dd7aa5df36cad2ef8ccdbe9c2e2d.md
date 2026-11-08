```
images: [us-central1-docker.pkg.dev/${PROJECT_ID}/my-docker-repo/myimage']
options:
  logging: CLOUD_LOGGING_ONLY

EOF
```

> **Note**
> When working with container images, it is best practice to specify the `images` field with the image you are creating. This enables functionality such as build provenance, which we will discuss in *Chapter 6, Managing Environment Security*.
> 
> For simple builds that are okay with pushing the container image at the end of the build's run, you can omit a distinct build step to build this image and just specify the `images` field instead. Doing so will prompt Cloud Build to push the image in the field at the end of the build's run.
> 
> However, in this example, because we are creating a VM that runs our newly built container image, we still maintain a distinct build step.

Create the Dockerfile for the container image we will build in this build:

```
$ cat > Dockerfile <<EOF
FROM alpine
CMD ["echo", "Hello World!"]
EOF
```

Earlier in the book, we briefly noted that builds utilize a **Service Account** from **Identity and Access Management (IAM)** to perform certain actions that access Google Cloud APIs. For this chapter, we will create a temporary service account:

```
$ gcloud iam service-accounts create packt-cloudbuild-chp4-sa \
--description="Temporary SA for chp 4 exercises" \
--display-name="packt-cloudbuild-chp4-sa"
```