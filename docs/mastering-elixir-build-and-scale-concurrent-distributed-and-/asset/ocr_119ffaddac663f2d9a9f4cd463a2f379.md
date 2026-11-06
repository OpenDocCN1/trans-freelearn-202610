```
bindings:
  # ...
- members:
  - serviceAccount:elixirdrip-prod-sql-client@intense-
talent-188323.iam.gserviceaccount.com
    role: roles/cloudsql.client
  # ...

$ gcloud iam service-accounts keys create secrets/elixirdrip-prod-sql.json
--iam-account elixirdrip-prod-sql-client@intense-
talent-188323.iam.gserviceaccount.com
created key [6fbd...] of type [json] as [secrets/elixirdrip-prod-sql.json]
for [elixirdrip-prod-sql-client@intense-
talent-188323.iam.gserviceaccount.com]
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

![](bbox=[0.12, 0.58, 0.2, 0.68])

The generated key grants anyone the power to update the database, so we have to store it securely. Do not store the key with the rest of your application code!

When our application pod is deployed, the Cloud SQL proxy container will need to use this key to connect to the database. To enable pods to securely access sensitive information such as this, Kubernetes provides a type of object called a **secret**.

Secrets are also isolated per namespace, so let's create the `production` namespace before creating all the secrets.