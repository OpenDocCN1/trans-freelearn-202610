Executing a manual builds submit of the preceding configuration will install gcloud components and validate the terraform version available:

```
$ gcloud builds submit --region us-west1
```

The following is a sample output of which gcloud components are installed locally for the image and the version of terraform:

```
Step #0 - "See what gcloud components are installed": Your
current Google Cloud CLI version is: 381.0.0
Step #0 - "See what gcloud components are installed": kubectl
Step #0 - "See what gcloud components are installed": bq
Step #0 - "See what gcloud components are installed": core
Step #0 - "See what gcloud components are installed": kustomize
...
Step #1 - "Check terraform version": Terraform v1.1.0
Step #1 - "Check terraform version": on linux_amd64
...
```

The team that owns the image may differ depending on your organization, but ideally, it is maintained by a single team. In an end-to-end build, requirements, suggestions, and feedback may come from different teams within the organization. Versioning becomes critical as well when certain builds depend on specific versions and combinations of tools that need to be considered when maintaining a custom builder image. It comes down to what works for the organization but the flexibility of the requirements of different teams can lead to optimal outcomes.

# Managing the principle of least privilege for builds

In the previous chapter, we covered the underlying principle of least privilege for our build pipelines. While the example in the first section of this chapter leveraged the Cloud Build default service account, it was convenient, but depending on the type of pipeline or automation we want to run, we may not want to provide a service account that has the ability to manipulate both the network and compute. One way to achieve this is by separating our Cloud Build pipeline configurations – we can also minimize the impact of mistakes or the attack surface.

If you haven’t cloned the repo, go ahead and clone it (https://github.com/PacktPublishing/Cloud-Native-Automation-With-Google-Cloud-Build):

```
$ git clone https://github.com/PacktPublishing/Cloud-Native-Automation-With-Google-Cloud-Build
```