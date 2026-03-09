### Kubernetes Ingress (NGINX)
### Basic Ingress YAML
``` yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-ingress
  namespace: default
  annotations:
    kubernetes.io/ingress.class: "nginx"
spec:
  rules:
  - host: myapp.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: my-service
            port:
              number: 80
```
### Apply & verify

```bash

kubectl apply -f ingress.yaml

kubectl get ingress
kubectl describe ingress my-ingress

```
### Multiple paths (one host)
``` yaml
rules:
- host: myapp.example.com
  http:
    paths:
    - path: /api
      pathType: Prefix
      backend:
        service:
          name: api-service
          port:
            number: 8080
    - path: /
      pathType: Prefix
      backend:
        service:
          name: frontend-service
          port:
            number: 3000
```
### Multiple hosts
``` yaml
rules:
- host: app.example.com
  http:
    paths:
    - path: /
      pathType: Prefix
      backend:
        service:
          name: app-service
          port:
            number: 80
- host: api.example.com
  http:
    paths:
    - path: /
      pathType: Prefix
      backend:
        service:
          name: api-service
          port:
            number: 8080
```

### Common NGINX annotations
``` yaml
annotations:
  kubernetes.io/ingress.class: "nginx"
  nginx.ingress.kubernetes.io/rewrite-target: /
  nginx.ingress.kubernetes.io/ssl-redirect: "true"
```