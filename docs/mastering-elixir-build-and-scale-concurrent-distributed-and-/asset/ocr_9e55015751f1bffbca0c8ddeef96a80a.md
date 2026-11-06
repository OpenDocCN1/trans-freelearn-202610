# Creating a namespace

Since we're using a single cluster, we will create two namespaces, `staging` and `production`, to isolate the resources dedicated to each environment. In a real scenario, you would instead have two separate clusters, to mitigate the risk of a staging hiccup affecting the production application:

```
$ kubectl create namespace production
namespace "production" created

$ kubectl create namespace staging
namespace "staging" created

$ kubectl get namespaces
NAME STATUS AGE
default Active 31d
kube-public Active 31d
kube-system Active 31d
production Active 45s
staging Active 52s
```

This time, we only used the `kubectl` command-line syntax to create the namespaces, because the only thing we had to configure was the namespace name. Nonetheless, we could have done the same thing by running `kubectl apply -f deploy/elixir-drip-namespace-prod.yml`. As you can see here, the equivalent YAML configuration file simply declares a resource of the Namespace kind with a `production` name:

```
$ cat deploy/elixir-drip-namespace-prod.yml
apiVersion: v1
kind: Namespace
metadata:
    name: production
```

In the next section, we'll create all the application secrets. Notice how we're always passing the `-n production` option to `kubectl`, hence creating every secret in the `production` namespace.