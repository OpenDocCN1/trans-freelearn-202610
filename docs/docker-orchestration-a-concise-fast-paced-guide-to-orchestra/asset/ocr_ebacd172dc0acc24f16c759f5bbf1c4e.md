It is also possible to tell `kubectl create` to read all of the files in a directory by adding the `-R` flag and setting `-f` to a directory name. This makes it easy to put all of the resources for a specific application in a directory and load them all at once.

The Kubernetes project has created a tool called Helm which can be used to package pre-configured resources. For more information see https://github.com/kubernetes/helm.

# Deleting resources

Resources are deleted with the `kubectl delete` command. At its simplest, `kubectl delete` is called with the type and name of the resource to delete:

```
$ kubectl delete service wp-service
service "wp-service" deleted
```

Like the `create` command, `delete` can use the `-f` flag to pass in a file. Every resource defined in the file will be deleted. The `-R` flag can also be used to delete every resource defined in a directory.

# Running pods

Kubernetes organizes containers into pods. Each pod is a collection of one or more containers. A pod can be replicated, scaled, and updated independently of other pods running in the cluster. Each pod is assigned an IP address, which is used to connect to the exposed ports of containers running in the pod. Care must be taken to ensure that services within a pod do not try to use the same port, but that is much easier to coordinate than managing port usage across a Docker Swarm.

# Defining a pod

**Pods** define one or more containers as well as all of the volumes that the containers need. The containers operate as a single unit. They are all started and stopped together. They can also communicate with each other on localhost. The pod is defined using YAML or JSON and passed to `kubectl create`. Following is a simple pod definition:

```
apiVersion: v1
kind: Pod
metadata:
 name: simple
spec:
```