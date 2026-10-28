```
secretName: myapp-tls-secret
rules:
- host: myapp.example.com
  http:
    paths:
    - path: /
      pathType: Prefix
      backend:
        service:
          name: myapp-service
          port:
            number: 80
```

Make sure you have the correct **ingressClassName** for your NGINX Ingress setup and the **myapp-tls-secret** TLS secret if you’re using HTTPS.

Here’s a full YAML for a Traefik IngressRoute with a round-robin load balancing strategy and middleware for basic auth:

```
apiVersion: traefik.containo.us/v1alpha1
kind: IngressRoute
metadata:
  name: traefik-example-ingressroute
spec:
  entryPoints:
  - web
```