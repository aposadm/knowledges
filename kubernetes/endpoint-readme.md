### Example endpoint.yaml
```bash
apiVersion: v1
kind: Endpoints
metadata:
  name: external-service
subsets:
- addresses:
  - ip: 192.168.1.100
  ports:
  - port: 80
```
### Apply:
``` bash
kubectl apply -f endpoint.yaml
```
