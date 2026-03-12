### Add the Prometheus chart to your system using the following command.
```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
```

### You can list all the charts in the repo using the following command. We are going to use the Prometheus chart.

``` bash
    helm search repo prometheus-community
```
### helm pull

```bash
helm pull longhorn/longhorn --untar
```

## Before you deploy the Prometheus Helm chart, you can view all the YAML manifests by converting the chart to plain YAML files using the following command.
``` bash
helm template prometheus-community prometheus-community/prometheus --output-dir prometheus-manifests
```

## Customize Prometheus Helm Chart Configuration Values

``` bash
helm show values prometheus-community/prometheus > values.yaml
```

# create a namespace
``` bash
kubectl create namespace monitoring
```
