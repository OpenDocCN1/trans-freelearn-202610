
It's also important to note that all these Kubernetes objects, such as pods, services, and deployments, are all represented using similar YAML configuration files. This is a crucial aspect, because it effectively enables you to have the infrastructure stored as code in your code repository. Also, all your interactions with Kubernetes clusters, including applying infrastructure changes, can be done with the `kubectl` tool. Using a single, common interface for everything Kubernetes-related significantly simplifies the user experience.

![](img/c636f4cfb07f25f1db535995e50706ed_476_0.png)

After creating your account on Google Cloud Platform, install and run the Google Cloud SDK tool (`gcloud init`) to log in and create a new project. You can now create a cluster with `gcloud container clusters create <cluster-name>`. If you don't have `kubectl` installed, install it with `gcloud` (that is, `gcloud components install kubectl`) and it will be automatically configured to access the cluster. If you already have `kubectl` installed, run `gcloud container clusters get-credentials <cluster-name>` to update your `kubectl` configuration with the cluster credentials.

At this point, you should have a Google Cloud Platform project created and a Kubernetes cluster up and running:

```
$ gcloud projects list
PROJECT_ID NAME PROJECT_NUMBER
intense-talent-188323 My First Project 5338223468

$ gcloud container clusters list
NAME LOCATION MASTER_VERSION MASTER_IP MACHINE_TYPE NODE_VERSION NUM_NODES STATUS
elixir-drip-prod-cluster europe-west1-b 1.8.8-gke.0 35.205.44.153 n1-standard-1 1.8.8-gke.0 1 RUNNING

$ kubectl cluster-info
Kubernetes master is running at https://35.205.44.153
GLBCDefaultBackend is running at
https://35.205.44.153/api/v1/namespaces/kube-system/services/default-http-backend:http/proxy
Heapster is running at
https://35.205.44.153/api/v1/namespaces/kube-system/services/heapster/proxy
KubeDNS is running at
https://35.205.44.153/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
kubernetes-dashboard is running at
https://35.205.44.153/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy
```

Most of the Google Cloud Platform services that we'll use need to be associated with a billing account. Go to https://console.cloud.google.com, choose Billing on the left-hand side, and then New billing account to create and enable a billing account. After creating the account, use its billing account ID to link the current project to it:

```
$ gcloud beta billing projects link intense-talent-188323 --billing-account=012345-6789AB-CDEF01
billingAccountName: billingAccounts/012345-6789AB-CDEF01
billingEnabled: true
name: projects/intense-talent-188323/billingInfo
projectId: intense-talent-188323

$ gcloud beta billing projects list --billing-account=012345-6789AB-CDEF01
PROJECT_ID BILLING_ACCOUNT_ID BILLING_ENABLED
intense-talent-188323 012345-6789AB-CDEF01 True
```

We will also need to activate the following Google APIs on our project: the Kubernetes Engine API (container.googleapis.com), the Container Registry API (containerregistry.googleapis.com), Cloud SQL (sql-component.googleapis.com), the Cloud SQL API (sqladmin.googleapis.com), Cloud Storage (storage-component.googleapis.com), and the Cloud Storage JSON API (storage-api.googleapis.com). To activate these services, run the gcloud services enable <API> command for each API URL, as we do here:

```
$ gcloud services enable container.googleapis.com
Waiting for async operation operations/tmo-acf.b1897824-... to complete...
Operation finished successfully.
```

![](img/c636f4cfb07f25f1db535995e50706ed_477_0.png)

Because of the credit you have, you won't be billed during the free trial. Nonetheless, remember to stop all the Google Cloud Platform services and delete all the Kubernetes objects after you've finished, to avoid spending all the free credit with just idle computation. A quick way to stop everything is to close the billing account associated with the project.

Let's now configure the database that will be used by our application when deployed on the cloud.

## Configuring the cloud database

In the local development environment, we were spinning up a Postgres database every time we started our application with docker-compose up. This won't be our approach when we deploy the application to the Kubernetes cluster because we'll use the Cloud SQL database solution provided by Google.

Use gcloud sql to create a new database instance (it will take a few minutes):

```
$ gcloud sql instances create elixir-drip-production --database-version=POSTGRES_9_6
# ...

$ gcloud sql instances list
NAME DATABASE_VERSION LOCATION TIER ADDRESS STATUS
elixir-drip-production POSTGRES_9_6 europe-west1-b db-custom-1-3840 35.195.47.137 RUNNABLE
```

Let's now create the postgres database user that our application will use:

```
$ gcloud sql users create postgres % --instance=elixir-drip-production --password=a-really-c0mplex-password
Creating Cloud SQL user...done.
Created user [exampleuser].

$ gcloud sql users list --instance=elixir-drip-production
NAME HOST
postgres
```

Because of the way Cloud SQL works, no application should be able to directly connect to the database. Instead, the application should connect itself to a Cloud SQL proxy, which will then securely handle the database connection. To cater for this, we will deploy pods with two containers, one with our application and the other running the SQL proxy.

Every time an external application needs to access Google Cloud resources, we need to create a service account for it. We can then restrict the external application access by defining the roles assigned to the service account. In this case, the service account will need to access the database, so we will assign it roles/cloudsql.client:

```
$ gcloud iam service-accounts create elixirdrip-prod-sql-client --display-name "ElixirDrip Prod SQL client"
Created service account [elixirdrip-prod-sql-client].

$ gcloud projects add-iam-policy-binding intense-talent-188323 --member serviceAccount:elixirdrip-prod-sql-client@intense-talent-188323.iam.gserviceaccount.com --role roles/cloudsql.client
```

```
bindings:
  # ...
- members:
  - serviceAccount:elixirdrip-prod-sql-client@intense-talent-188323.iam.gserviceaccount.com
    role: roles/cloudsql.client
  # ...

$ gcloud iam service-accounts keys create secrets/elixirdrip-prod-sql.json --iam-account elixirdrip-prod-sql-client@intense-talent-188323.iam.gserviceaccount.com
created key [6fbd...] of type [json] as [secrets/elixirdrip-prod-sql.json]
for [elixirdrip-prod-sql-client@intense-talent-188323.iam.gserviceaccount.com]
$ cat secrets/elixirdrip-prod-sql.json
{
  "type": "service_account",
  "project_id": "intense-talent-188323",
  "private_key_id": "6fbd...",
  "private_key": "abcdef..."
  # ...
}
```

In the previous snippet, we've created the `elixirdrip-prod-sql-client` service account, assigned it the `cloudsql.client` role, and then created a service account key that will be used by the Cloud SQL proxy to connect to the database.

![](img/c636f4cfb07f25f1db535995e50706ed_479_0.png)

The generated key grants anyone the power to update the database, so we have to store it securely. Do not store the key with the rest of your application code!

When our application pod is deployed, the Cloud SQL proxy container will need to use this key to connect to the database. To enable pods to securely access sensitive information such as this, Kubernetes provides a type of object called a **secret**.

Secrets are also isolated per namespace, so let's create the `production` namespace before creating all the secrets.

## Creating a namespace

Since we're using a single cluster, we will create two namespaces, `staging` and `production`, to isolate the resources dedicated to each environment. In a real scenario, you would instead have two separate clusters, to mitigate the risk of a staging hiccup affecting the production application:

```
$ kubectl create namespace production
namespace "production" created

$ kubectl create namespace staging
namespace "staging" created

$ kubectl get namespaces
NAME STATUS AGE
default Active 31d
kube-public Active 31d
kube-system Active 31d
production Active 45s
staging Active 52s
```

This time, we only used the `kubectl` command-line syntax to create the namespaces, because the only thing we had to configure was the namespace name. Nonetheless, we could have done the same thing by running `kubectl apply -f deploy/elixir-drip-namespace-prod.yml`. As you can see here, the equivalent YAML configuration file simply declares a resource of the Namespace kind with a `production` name:

```
$ cat deploy/elixir-drip-namespace-prod.yml
apiVersion: v1
kind: Namespace
metadata:
    name: production
```

In the next section, we'll create all the application secrets. Notice how we're always passing the `-n production` option to `kubectl`, hence creating every secret in the `production` namespace.
