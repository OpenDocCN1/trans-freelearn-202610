```
ports:
  - port: 80
    targetPort: 4000
```

We are routing requests arriving through port 80 of the load-balancer to port 4000 of a pod with the app=elixir-drip and env=production labels. Creating a LoadBalancer service like ours exposes the service using a load balancer from the cloud provider. This means a TCP load balancer, outside of Kubernetes and with its own IP, will be running and routing the requests to our pods.

![](bbox=[0.12, 0.33, 0.2, 0.42])

When using a LoadBalancer service, Kubernetes will automatically create the objects needed to route the requests to the pods, namely NodePort and ClusterIP. How the load-balancer works depends on the cloud provider, but each deployed service will always have its own load balancer, hence relying on just load balancers may be costly. A better alternative is to use a Kubernetes Ingress, which is able to do SSL termination, HTTP routing, and so on, and can be shared between services. In this book, we will just use the LoadBalancer service.

If we now check the existing services, this is what we'll see (we're using the --watch option, hence kubectl will update its output whenever one of the objects is updated):

```
$ kubectl create -f deploy/elixir-drip-loadbalancer-prod.yml
service "elixir-drip-loadbalancer" created

$ kubectl -n production get service --watch
NAME TYPE CLUSTER-IP EXTERNAL-IP PORT(S) AGE
elixir-drip-loadbalancer LoadBalancer 10.43.254.164 <pending> 80:32153/TCP 9s
elixir-drip-loadbalancer LoadBalancer 10.43.254.164 35.205.152.228 80:32153/TCP 1m
```

In the beginning, the load balancer IP was pending (probably because it wasn't provisioned yet), but after a minute, we finally got an external IP. We can now reach our application from the outside as shown here:

```
$ curl http://35.205.152.228
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <meta name="description" content="">
    <meta name="author" content="">
```