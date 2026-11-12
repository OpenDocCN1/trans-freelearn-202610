the update strategy type to Recreate in the deployment definition. Kubernetes will then stop the existing pods before starting new ones. The following excerpt shows how to set the deployment strategy:

```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: update-deploy
spec:
  strategy:
    type: "Recreate"
...
```

# Getting the rollout history

Kubernetes remembers the changes that have been made to the deployment. The revision history is available by running the `kubectl rollout history deploy/<name>` command, where `<name>` is the name of the deployment:

```
$ kubectl rollout history deployment/update-deploy
deployments "update-deploy"
REVISION          CHANGE-CAUSE
1                 <none>
2                 <none>
```

Full details of the deployment are available by adding `--revision=N` to the history command, where N is revision number. In the following output, you can see that the image for the `web` container was updated to `nginx:1.11.4`:

```
$ kubectl rollout history deployment/update-deploy --revision=2 deployments
"update-deploy" with revision #2
Labels:         app=update-deploy
                pod-template-hash=1680774371
Containers:
web:
  Image:        nginx:1.11.4
  Port:
  Volume Mounts: <none>
  Environment Variables: <none>
db:
  Image:        mysql:5.5
  Port:
  Volume Mounts: <none>
  Environment Variables:
  MYSQL_ROOT_PASSWORD:  password
No volumes.
```