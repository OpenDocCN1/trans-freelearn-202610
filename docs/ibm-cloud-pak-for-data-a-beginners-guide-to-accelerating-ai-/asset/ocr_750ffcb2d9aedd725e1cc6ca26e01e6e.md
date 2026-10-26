Looking at one of the deployments (such as `zen-core`), you will notice how the Deployment indicates where the volume should be mounted:

```
```yaml
kubectl get deployment zen-core -o yaml
:
  - name: user-home-mount
    persistentVolumeClaim:
      claimName: user-home-pvc
:
volumeMounts:
  - mountPath: /user-home
    name: user-home-mount
```
```

In this example, you will notice that `user-home-pvc` gets mounted as `/user-home` in the `zen-core` Pods.

Cloud Pak for Data also uses StatefulSets, where each member replica is typically associated with its own dedicated (non-shared) persistent volume. Here's an example:

```
```shell
kubectl get statefulsets
```
```

This identifies a couple of StatefulSets present in the Cloud Pak for Data control plane:

| NAME | READY | AGE |
|---|---|---|
| dsx-influxdb | 1/1 | 33d |
| zen-metastoredb | 3/3 | 33d |

Each StatefulSet spawns one or more member Pods:

```
```shell
kubectl get pods -lcomponent=zen-metastoredb
```
```

The preceding snippet shows the *members* associated with that metastoredb StatefulSet. In this case, there are three of them and they are numbered ordinally:

| NAME | READY | STATUS | RESTARTS | AGE |
|---|---|---|---|---|
| zen-metastoredb-0 | 1/1 | Running | 11 | 12d |
| zen-metastoredb-1 | 1/1 | Running | 11 | 12d |
| zen-metastoredb-2 | 1/1 | Running | 10 | 12d |