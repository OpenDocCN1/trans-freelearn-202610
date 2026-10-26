Storage classes, represented by the `StorageClass` Kubernetes Kind resource, are used to reference each type of volume. When Cloud Pak for Data services are provisioned, they include PVC declarations, which are then processed by Kubernetes to provision and bind PVs.

For example, the user-home volume is requested by defining a `PersistentVolumeClaim` that resembles the following:

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: "user-home-pvc"
spec:
  storageClassName: {{ <user-selected-storage-class> }}
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: {{ default "100Gi" or <user-selected-size> }}
```

You will also notice that the PVC definition identifies parameters, such as the storage class to use and the storage size that's been requested. These are introduced via the installation options for the Cloud Pak for Data services.

The declarations in the *claim*, along with the class of storage and other attributes (such as `size` and `accessModes`), represent a *request* to Kubernetes to provision a volume for use by the Cloud Pak for Data service. Once the storage volume has been provisioned, Kubernetes then satisfies that claim by *binding* the provisioned PV to the PVC. This binding metadata then enables Kubernetes to always mount the same storage volume in the Pods, as well as for services to use the abstraction of the PVC to always refer to the volume, without needing to know the specifics of where the volume exists physically or the driver/storage technology behind it.

> **Note**
> 
> You will not need to explicitly define the PVC yourself. The PVC will be declared during the Cloud Pak for Data service installation process.