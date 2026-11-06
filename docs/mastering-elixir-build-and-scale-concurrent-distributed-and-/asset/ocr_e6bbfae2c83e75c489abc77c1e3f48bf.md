```
path: /health
port: 4000
initialDelaySeconds: 60
timeoutSeconds: 15
readinessProbe:
  httpGet:
    path: /health
    port: 4000
  initialDelaySeconds: 60
  timeoutSeconds: 15
ports:
  - name: http
    containerPort: 4000
env:
  # ...
```

The first container, elixir-drip-prod, will be based on the image we've just built and pushed to the Docker registry. It defines two probes, a liveness one and a readiness one, and exposes port 4000, before defining all the environment variables needed to spin up the application. As the name suggests, the readiness probe will be used by Kubernetes to detect whether a container is ready to accept requests right after it's deployed, whereas the liveness probe is used to decide whether Kubernetes should restart the container or not.

Here you can find some of the environment variables we're setting inside the container. Some environment variables, such as DB_HOST and DB_PORT, don't contain sensitive information, so we pass their value in clear text. Others, such as GOOGLE_STORAGE_CREDENTIALS and DB_PASS, fetch their values from the secrets we previously created.

```
$ cat deploy/elixir-drip-deployment-prod.yml
apiVersion: extensions/v1beta1
kind: Deployment
# ...
template:
  # ...
  spec:
    # ...
    containers:
      - image: gcr.io/intense-talent-188323/elixir-drip-prod:manual_build
        name: elixir-drip-prod
        env:
          # ...
          - name: DB_HOST
            value: 127.0.0.1
          - name: DB_PORT
            value: "5432"
```