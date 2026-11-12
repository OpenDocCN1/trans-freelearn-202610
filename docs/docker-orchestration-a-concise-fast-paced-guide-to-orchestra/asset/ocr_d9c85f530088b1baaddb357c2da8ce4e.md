It is possible for a claim to match multiple volumes. If that is the case, the volume that Kubernetes determines is the closest match provided. If you are trying to connect to a specific volume, use a unique label on the persistent volume that the claim can match.

# Using a PersistentVolumeClaim as a volume

When a pod wants to use a persistent volume, a claim is listed in the volumes section of the pod specification. Following is the nginx pod from before. This time, it is using PersistentVolumeClaim to access the volume:

```
apiVersion: v1
kind: Pod
metadata:
  name: web-pvc
spec:
  volumes:
    - name: test-pd
      persistentVolumeClaim:
        claimName: test-pd-pvc
  containers:
    - name: nginx
      image: nginx:1.11
      volumeMounts:
        - mountPath: /mnt
          name: test-pd
```

The volume type is a persistentVolumeClaim. The claimName attribute must match the PersistentVolumeClaim created earlier. Like the plain volume before, the volume name in the pod, test-pd in this case, is used in the container definition to mount the storage in the container.

![](bbox=[0.135, 0.655, 0.205, 0.725])

> There are number of other things that can be done with PersistentVolumes including dynamic provisioning of volumes for pods. For more details, refer to: http://kubernetes.io/docs/user-guide/persistent-volumes/.