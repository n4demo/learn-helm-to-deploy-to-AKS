- Author Nigel Wardle. Node4

## Install Prometheus from Helm

https://github.com/prometheus-community/helm-charts/blob/main/charts/kube-prometheus-stack/README.md


```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
```

```
helm repo update
```

```
helm search repo prometheus-community
```


```
helm template prom prometheus-community/kube-prometheus-stack --namespace=prom --create-namespace
```

```
helm install prom prometheus-community/kube-prometheus-stack --namespace=prom --create-namespace
```

```
nigel@Azure:~$ helm install prom prometheus-community/kube-prometheus-stack --namespace=prom --create-namespace
NAME: prom
LAST DEPLOYED: Wed Mar 23 13:17:07 2022
NAMESPACE: prom
STATUS: deployed
REVISION: 1
NOTES:
kube-prometheus-stack has been installed. Check its status by running:
  kubectl --namespace prom get pods -l "release=prom"

Visit https://github.com/prometheus-operator/kube-prometheus for instructions on how to create & configure Alertmanager and Prometheus instances using the Operator.
```


```
k expose deployment deployment/prometheus-grafana --name=prom --type=LoadBalancer --port=3000 --target-port=3000  -n=prom
```

http://xxxx:3000


username: admin
password: prom-operator