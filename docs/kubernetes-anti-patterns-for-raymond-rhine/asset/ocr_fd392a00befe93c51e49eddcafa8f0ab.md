```
routes:
  - match: Host(`myapp.example.com`)
    kind: Rule
    services:
      - name: myapp-service
        port: 80
        strategy: RoundRobin
      middlewares:
        - name: auth-middleware
---
apiVersion: traefik.containo.us/v1alpha1
kind: Middleware
metadata:
  name: auth-middleware
spec:
  basicAuth:
    secret: myapp-basic-auth-secret
```

The middleware for basic authentication refers to a Kubernetes secret called **myapp-basic-auth-secret**, which you need to create beforehand as it contains the encoded credentials.

# Applying the configuration:

Save your chosen configuration to a file – for example, **nginx-ingress.yaml** or **traefik-ingress.yaml**.

Apply the configuration using **kubectl**: