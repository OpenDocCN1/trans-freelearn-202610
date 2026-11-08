It is highly recommended that you run this chapter's examples in a sandbox or test Google Cloud project, as you will have full control not only to run the example but also to easily clean up resources created.

You will begin by initiating a session in Cloud Shell (shell.cloud.google.com), the ephemeral Linux workstation provided by Google Cloud.

Clone the example repository from GitHub using the following command:

```
$ cd ~ && git clone https://github.com/agmsb/cloudbuild-gke.git && cd cloudbuild-gke/08
```

Run the gcloud command to configure gcloud to use the appropriate Google Cloud project as follows, replacing <add-project-id> with the Google Cloud project you want to use:

```
$ gcloud config set project <add-project-id>
```

Given that it is recommended that this example be run in a sandbox Google Cloud project, it is best completed with the role of a project owner assigned to your user in Identity and Access Management (IAM).

With the project owner role, you can enable the services required for this project, using the following command:

```
$ gcloud services enable container.googleapis.com cloudbuild.googleapis.com artifactregistry.googleapis.com binaryauthorization.googleapis.com servicenetworking.googleapis.com containeranalysis.googleapis.com
```

This command will enable the following services in your project:

+   - GKE
+   - Cloud Build
+   - Artifact Registry
+   - Binary Authorization
+   - Service Networking API
+   - Container Analysis API

With these services enabled, you can now begin setting up the resources for this chapter's example.

# Setting up the VPC networking for your environment

We will run numerous commands that will require names for various resources; for your convenience, default names are provided in a file accessible via the GitHub repository.