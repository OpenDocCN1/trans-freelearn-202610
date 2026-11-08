Once you have a project, IAM, and a billing account sorted out, you will then need to enable the Cloud Build and Service Networking APIs in your Project.

For this and all following examples within this book, we will be utilizing Cloud Shell, a free and ephemeral developer workstation provided by Google Cloud. It runs a Debian machine with multiple tools already installed, including gcloud, which is the **command-line interface (CLI)** for utilizing Google Cloud services.

Open Cloud Shell (https://shell.cloud.google.com/) and authenticate with gcloud by running the following command and following the instructions in the response:

```
$ gcloud auth login
```

Once authenticated, run the following command to enable the required APIs to use Cloud Build and configure networking for private pool workers:

```
$ gcloud services enable cloudbuild.googleapis.com
servicenetworking.googleapis.com
```

With your Google Cloud project set up, let's now review the various ways worker pools can be configured!

# Using the default pool

At its initial **General Availability (GA)** launch in 2017, Cloud Build was designed to provide users with a simple interface and fully managed experience in Google Cloud when building container images and other artifacts such as Go executables or Java archives.

> **Note**
> Cloud Build was initially launched under the name **Container Builder** in 2017, underscoring its simple and focused purpose to provide end users with a way to automate builds.

From its inception, Cloud Build has aimed to ensure the following:

+   - Users could create builds and triggers for builds by interacting with Google's API or **user interface (UI)**.
+   - Users would not need to create, patch, scale, or manage the worker machines that run builds.
+   - Users would have pre-built integration with logging and monitoring to introspect builds.

Cloud Build first accomplished this experience by providing users with the first mode for workers in Cloud Build: **the default pool.**

The default pool is naturally named as it is the default method for Cloud Build to provision workers to execute your builds. When submitting a build to Cloud Build, if you do not specify which type of