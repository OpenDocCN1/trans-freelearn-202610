
## Creating a multi-container deployment

Multi-container deployments require that the deployment be defined in YAML or JSON. The file specifies the name of the deployment, the number of replicas, and the pod specification. This same format can also be used for single container deployments:

```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: multi-deploy
spec:
  replicas: 2
  template:
    metadata:
      labels:
        app: web-deploy
    spec:
      containers:
        - name: nginx
          image: nginx:1.10.4
        - name: mysql
          image: mysql:5.5
          env:
            - name: MYSQL_ROOT_PASSWORD
              value: password
```

The example creates two pods with two containers—nginx and mysql. The pod is defined in the template section of the deployment definition. It works just like the pods already covered in this chapter. The biggest difference is that the pod name is omitted. Pod names will be based on the deployment name.

Take note of apiVersion. Deployments are a new feature and still, officially, in beta. The specification may change in later versions.

## Scaling a deployment

Kubernetes makes it easy to increase or decrease the number of replicas in a deployment with the scale command. The following command scales the deployment named web to three replicas:

```
$ kubectl scale --replicas=3 deployment/web
deployment "web" scaled
```

A deployment can be scaled up or down as much as you need. The scheduler will ensure that the required pods are started across the cluster.

## Rolling updates

Deployments in Kubernetes have a feature that allows all of the pods in a deployment to be updated automatically. Updates are triggered whenever a part of the pod specification in the template section of a deployment is changed.

### Updating a deployment

Consider the following deployment which has two containers. The first, named web, is an nginx:1.10.1 image. The second, named db, is a mysql:5.5 image:

```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: update-deploy
spec:
  replicas: 2
  template:
    metadata:
      labels:
        app: update-deploy
    spec:
      containers:
        - name: web
          image: nginx:1.10.1
        - name: db
          image: mysql:5.5
          env:
            - name: MYSQL_ROOT_PASSWORD
              value: password
```

Both containers are running older versions of their images. Let's start by upgrading the web container:

```
$ kubectl set image deployment/update-deploy web=nginx:1.11.4
deployment "update-deploy" image updated
```

This commands sets the image attribute of the web container in the update-deploy deployment to nginx:1.11.4. Immediately after that command is run, Kubernetes starts a new pod with the updated image. When the new pod is ready, one of the existing pods is stopped. This process is repeated until all of the replicas are updated.

A word of warning. Make sure that your container can run multiple times safely before updating a deployment. By default, Kubernetes uses the same mechanism to perform updates as it does to scale pods. If your application cannot run multiple copies, you can set the update strategy type to Recreate in the deployment definition. Kubernetes will then stop the existing pods before starting new ones. The following excerpt shows how to set the deployment strategy:

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

### Getting the rollout history

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

### Rolling back an update

You may find that, after the update, something has gone horribly wrong, and that the update needs to be rolled back. Fortunately, there is a very convenient `undo` command:

```
$ kubectl rollout undo deploy/update-deploy
deployment "update-deploy" rolled back
```

Without options, the command will revert the named deployment to the previous one in the history. The `--to-revision=N` flag will revert the deployment to revision `N` from the history.

### Deleting a deployment

Remember that the job of a deployment is to ensure that the required number of pods are running. Trying to delete a deployment by deleting the pods is like Sisyphus trying to roll a rock up a hill. Kubernetes will keep recreating the pods as fast as they are deleted. When a deployment is no longer needed, it can be deleted with `kubectl delete deployment`. Make sure that you want to proceed because this will delete all running pods which are part of the deployment:

```
$ kubectl delete deployment web
deployment "web" deleted
```

It is possible to delete a deployment while leaving the pods running, using the `--cascade=false` flag. The leftover pods will have to be deleted manually.

## Running pods on every node with DaemonSets

A **DaemonSet** is a type of resource that ensures a pod or set of pods runs on every node in the cluster. The format of the resource is very similar to that of a deployment. The `template` section is a pod specification just like a deployment:

```
apiVersion: extensions/v1beta1
kind: DaemonSet
metadata:
  name: daemon
spec:
  template:
    ...
```

DaemonSets are useful for running services such as logging or monitoring. For example, you might have a service such as collectd which sends performance data to a monitoring solution.

It is also possible to limit a DaemonSet to only run on a set of nodes that match a given selector. For example, your local Kubernetes cluster may have a mix of server hardware from HP and Dell. You can create a DaemonSet that runs the Dell OpenManage tools on nodes that have a hardware label set to dell:

```
apiVersion: extensions/v1beta1
kind: DaemonSet
metadata:
  name: omsa
spec:
  template:
    spec:
      nodeSelector:
        hardware: dell
...
```

## Introducing scheduled jobs

Kubernetes 1.4 introduced scheduled jobs. These are sets of containers that run on a schedule. They are similar to Cron jobs in Linux and even use the Cron time format in the specification. Scheduled jobs are still considered alpha and are not available unless the `--runtime-config=batch/v2alpha1` flag is added to the API server on startup. For more details refer to http://kubernetes.io/docs/user-guide/jobs/.

## Using labels

Nearly every resource in Kubernetes can be assigned labels. Labels are a way to organize your resources but do not change how they operate. They are completely arbitrary and should be chosen to fit your environment. For example, you may want to group resources by application, label pods that are currently in testing, or signify that certain pods should only run on frontend nodes.

Labels are set in the `metadata` section of a resource. They are a series of key/value pairs. In the following example, the pod has four labels set. The `app` label is used to show that this pod is part of the `wordpress` application. The `tier` label signifies that this pod should run on backend nodes. This pod is still in testing so the `env` label is set appropriately. Finally, the `hero` label exists in this example to show how arbitrary labels are:

```
apiVersion: v1
kind: Pod
metadata:
  name: mysql
  labels:
    app: wordpress
    tier: backend
    env: testing
    hero: darkwing-duck
...
```

By themselves, labels do nothing. When a resource is created, Kubernetes notes the labels and goes about scheduling the resource somewhere in the cluster.

### Using selectors to find resources

All of the tools that list resources can take a selector with the `-l` flag to limit the list to resources with labels that match. This was used earlier in the chapter to see the list of pods started by a deployment:

```
$ kubectl get pods -l 'app=update-deploy'
NAME READY STATUS RESTARTS AGE
update-deploy-1680774371-9x8qd 2/2 Running 0 23h
update-deploy-1680774371-y0tbf 2/2 Running 0 23h
```

This example lists all of the pods with the `app` key and the `update-deploy` value. There are several different comparison operators that can be used. Wrap the selector in single quotes to ensure that the shell handles spaces and special characters properly:

| Operator | Description | Example |
|---|---|---|
| =, == | The value of the key must match. | `env = production` |
| != | The value of the key must not match. | `env != production` |
| in | The value is in a set of values. | `env in (testing, development)` |
