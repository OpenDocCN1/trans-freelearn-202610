+   - Developers in an enterprise may need to be prepared to have their code easily migrate to a different platform, such as Kubernetes.
+   - Developers may be deploying the code to multiple locations in need of a unifying packaging standard for deploying that code.

For these developers, they may require the need to build their own containers first, and then bring them to a “serverless” platform in which operations are minimal. Cloud Run provides such a platform, allowing developers to use containers as the deployment mechanism while still taking advantage of Cloud Run managing the following tasks:

+   - Creating the infrastructure to run their code
+   - Managing infrastructure to run and scale their code
+   - Setting up connectivity to instances of their code
+   - Configuring logging and monitoring for their running code

Cloud Run also allows developers to make a distinction between “services” and “jobs.” Cloud Run services allow for long-running services that are accessible via an HTTPS endpoint, while Cloud Run jobs allow for run-to-completion services that can run instances to perform work in parallel, assigning each instance an index that developers can use to distribute work.

Finally, Cloud Run allows developers to have services adhere to the Knative API.

Knative is an open source project that enables operators to host their own, self-managed serverless platform atop a Kubernetes cluster. Because Cloud Run is adherent to the Knative API specification, when users deploy a Cloud Run service, they also get a configuration that can be used to deploy the same container to a Kubernetes cluster running anywhere.

This makes Cloud Run not only a serverless container platform, but one that can extend to environments external to Google Cloud, requiring minimal to no configuration changes.

# Cloud Functions 2nd gen

There is a newer version of Cloud Functions, called Cloud Functions 2nd gen (https://cloud.google.com/functions/docs/2nd-gen/overview), released in 2022. This will be the primary version of Cloud Functions that we will interact with in the remainder of this chapter. One of the critical changes with Cloud Functions 2nd gen is that it is built atop Cloud Run.

This means that Cloud Functions provides an abstraction layer atop Cloud Run that satisfies the requirement to build a container and deploy it to Cloud Run, allowing users not familiar with containers to still use the platform. It maintains the developer contract that the previous version of Cloud Functions established: write code and let the platform handle the rest.