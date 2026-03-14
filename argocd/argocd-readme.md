### 1. Install Argo CD¶
```bash
    kubectl create namespace argocd
    kubectl apply -n argocd install.yaml
```
### 2. Apply agocd-ingress.yaml
```bash
    kubectl apply -f argocd-ingress.yaml
```
### 3. Login Using The CLI
``` bash
argocd admin initial-password -n argocd
```
### 4. Accessing Argocd
```bash
    argocd login argocd.apos.com:80 --username admin --password xxxxx --insecure
```