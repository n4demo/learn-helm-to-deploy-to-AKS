- Author Nigel Wardle. Node4

## Install Rancher from Helm

```
helm repo add rancher-latest https://releases.rancher.com/server-charts/latest
```

```
kubectl create namespace cattle-system
```

```
# Install the CustomResourceDefinition resources separately

```
kubectl apply --validate=false -f https://github.com/jetstack/cert-manager/releases/download/v1.0.4/cert-manager.crds.yaml
```

# Create the namespace for cert-manager

```
kubectl create namespace cert-manager
```

# Add the Jetstack Helm repository

```
helm repo add jetstack https://charts.jetstack.io
```

# Update your local Helm chart repository cache

```
helm repo update
```

# Install the cert-manager Helm chart

```
helm install cert-manager jetstack/cert-manager --namespace cert-manager --version v1.0.4
```

```
kubectl get pods --namespace cert-manager
```

```
helm install rancher rancher-latest/rancher --namespace cattle-system --set hostname=rancher.my.org
```

```
kubectl -n cattle-system rollout status deploy/rancher
```