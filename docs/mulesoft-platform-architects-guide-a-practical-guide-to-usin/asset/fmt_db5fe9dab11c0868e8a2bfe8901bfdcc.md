
In *Chapter 11*, we discussed Quorum management and how to use it to ensure high availability and a fault tolerance infrastructure for Runtime Fabric. We discussed the minimum master and control nodes required for production and non-production environments to ensure the infrastructure’s high availability.

# High Availability and Fault Tolerance with Runtime Fabric (self-managed Kubernetes)

Both the application and infrastructure layers of Runtime Fabric can achieve high availability. Setting up multiple worker nodes can help you achieve infrastructure-level high availability. Self-managed Kubernetes provides managed Kubernetes master nodes, which eliminates the need to manage master nodes. AWS oversees the EKS master nodes. Once again, managing the availability of the master nodes is the responsibility of the cloud providers (AWS, in the case of EKS) and is not the duty of MuleSoft.

To achieve application-level high availability for Runtime Fabric on self-managed Kubernetes and VMs/bare-metal servers, you can deploy multiple replicas for your application. You can cluster multiple replicas to ensure that data and information are distributed among the replicas. Just like an on-premises cluster, Runtime Fabric supports the cluster.

We discussed Anypoint clustering in detail in [*Chapter 11*](#B19922_11.xhtml#_idTextAnchor263).

# Scalability

The capacity of a system to adapt its performance and cost to changes in application and system processing demands is known as scalability. Examples include the way an operating system functions on various hardware classes, the way a database handles an increasing volume of queries, or the way a hardware system handles an increase in user count.

There are two types of scaling:

*   **Horizontal scaling**: Horizontal scaling means adding more machines to your pool of resources. Horizontal scaling is divided into two types:
    *   **Scaling up**: Adding more machines to your pool of resources
    *   **Scaling down**: Removing a machine from your pool of resources
*   **Vertical scaling**: Vertical scaling means adding more power or compute resources, such as memory and CPU, to existing servers:
    *   **Scaling out**: Adding more power to your existing machines
    *   **Scaling in**: Reducing the power of your existing machines

## Scalability with CloudHub 1.0

CloudHub 1.0 is a managed service where we don’t have to oversee its infrastructure, and it is completely managed by MuleSoft. We are only responsible for choosing the correct worker and worker size required to run the Mule applications or proxies.

MuleSoft will automatically configure infrastructure, depending on the worker and worker size we have selected for the application. An application can be horizontally scaled by increasing or decreasing the number of workers and vertically scaled by increasing or decreasing the worker size.

CloudHub 1.0 supports auto-scaling features, as discussed in [*Chapter 9*](#B19922_09.xhtml#_idTextAnchor179), which allows an application to auto-scale depending on an increase or decrease of its traffic.

## Scalability with CloudHub 2.0

Like CloudHub 1.0, CloudHub 2.0 is a managed service that doesn’t have to oversee its infrastructure, and it is completely managed by MuleSoft. We are only responsible for choosing the correct replica and replica size required to run the Mule applications or proxies.

MuleSoft will automatically configure infrastructure, depending on the replica and replica size we have selected for the application. An application can be horizontally scaled by increasing or decreasing the number of replicas and vertically scaled by increasing or decreasing the replica size. CloudHub 2.0 doesn’t support the auto-scaling feature.

We discussed scalability with CloudHub 2.0 in detail in [*Chapter 10*](#B19922_10.xhtml#_idTextAnchor228).

## Scalability with Runtime Fabric (bare-metal servers/VMs)

The Runtime Fabric infrastructure can be scaled horizontally by adding more worker or master nodes to the cluster and scaled vertically by increasing or decreasing computing resources, such as CPU and memory. It is the customer’s responsibility to perform the changes for implementing scalability at the infrastructure level.

An application can be scaled horizontally by adding more replicas of the application and vertically scaled by increasing the replica size (the reserved CPU, CPU limit, or memory).

Runtime Fabric is a service provided by MuleSoft on top of Kubernetes for running Mule applications. This abstracts away the complexities of administering and managing the Kubernetes cluster, allowing developers to focus on application development and deployments.

However, Runtime Fabric doesn’t directly provide capabilities for scaling applications. In such cases, Kubernetes comes into play to provide robust scaling.

Kubernetes is an open source platform for automating containerized application deployment, scaling, and administration.

Kubernetes provides two different ways of auto-scaling application instances:

*   A `Horizontal Pod Autoscaler` (`HPA`)
*   A `Vertical Pod Autoscaler` (`VPA`)

The HPA adds more `Pods` in the event of more traffic. In the event of load decreases, the number of `Pods` will scale down to a minimum.

The HPA is used as a controller and resource for the Kubernetes API. The controller’s behavior is dictated by the resource. The horizontal directory autoscaling controller, which is housed in the Kubernetes control plane, uses observed metrics, such as average CPU usage, average memory usage, or any other custom metric you specify, to determine the appropriate scale of its object (such as a `Deployment`) regularly.

The HPA works on the principle of targets. It adjusts the number of `Pods` in a `Deployment` or replica that have been set to match a predefined target usage level for a given resource (such as CPU or memory).

To create an HPA, you need to use the `kubectl autoscale` command:

```
kubectl autoscale deployment mule-test-app --cpu-percent=80 --min=2 --max=6
```

Let’s break down the preceding `kubectl` command:

*   Here, `mule-test-app` is the name of the application.
*   Then, we have `--cpu-percent=80`, which means Kubernetes will add more replicas when the average CPU utilization of all `Pods` goes above 80%
*   Finally, `--min=2` is the minimum number of replicas that will always run, and `--max=6` is the maximum number of replicas that Kubernetes can create if there is high CPU utilization

The `kubectl autoscale` command is a simple command that activates the HPA. However, defining the HPA with a manifest file is more flexible as it allows multiple metrics to enable scaling.

The following manifest file allows you to scale horizontally if the average CPU utilization of a node reaches 80% or higher. Create a `mulesoft-test-app-hpa.yaml` manifest file. Then, execute the `kubectl apply` command to enable the HPA:

```
kubectl apply -f mulesoft-test-app-hpa.yaml
```

Here’s the output:

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: mulesoft-test-app
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: mule-test-app
  minReplicas: 2
  maxReplicas: 6
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 80
```

Let’s take a closer look at the attributes that have been defined in this YAML file:

*   Here, `scaleTargetRef` is the MuleSoft deployment that this HPA will manage.
