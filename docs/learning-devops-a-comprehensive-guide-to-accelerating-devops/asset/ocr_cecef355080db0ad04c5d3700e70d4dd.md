# # Installing the Kubernetes dashboard

In order to install the Kubernetes dashboard, which is a pre-packaged containerized web application that will be deployed in our cluster, we will run the following command in a Terminal:

```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.4.0/aio/deploy/recommended.yaml
```

Its execution is shown in the following screenshot:

![](bbox=[0.1436, 0.2773, 0.8643, 0.4277])

Figure 10.4 – Kubernetes dashboard installation

From the preceding screenshot, we can see that different artifacts are created, which are outlined as follows: secrets, two web applications, **role-based access control (RBAC)** roles, permissions, and services.

> > **Note**
> >
> > Note that the **Uniform Resource Locator (URL)** mentioned in the parameters
> of the command that installs the dashboard may change depending on the
> versions of the dashboard. To find out the last valid URL to date, consult
> the official documentation by visiting `https://kubernetes.io/
> docs/tasks/access-application-cluster/web-ui-
> dashboard/`.

Now that we have installed this Kubernetes dashboard, we will connect to it and configure it.