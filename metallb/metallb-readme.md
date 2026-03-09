### 1️⃣ Install MetalLB

### Create metallb-system
``` bash
kubectl create namespace metallb-system
```

### Apply the official manifest:
``` bash
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.14.5/config/manifests/metallb-native.yaml
```

### Check pods:
``` bash
kubectl get pods -n metallb-system
```
### 2️⃣ Create IP Address Pool

### You must assign an IP range from your local network that is not used by DHCP.

Example network:

192.168.1.200 - 192.168.1.210

Create file:

# metallb-ip-pool.yaml
``` bash
apiVersion: metallb.io/v1beta1
kind: IPAddressPool
metadata:
  name: metallb-ip-pool
  namespace: metallb-system
spec:
  addresses:
  - 192.168.1.200-192.168.1.210
```

### Apply:
``` bash
kubectl apply -f metallb-ip-pool.yaml
```

### 3️⃣ Create L2 Advertisement

``` bash

# metallb-l2.yaml
apiVersion: metallb.io/v1beta1
kind: L2Advertisement
metadata:
  name: metallb-l2
  namespace: metallb-system
```
### Apply:
``` bash
kubectl apply -f metallb-l2.yaml
```