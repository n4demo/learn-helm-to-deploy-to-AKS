- Author Nigel Wardle. Node4

## Install Prometheus from Helm

```
  helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
```

```
  helm repo add stable https://charts.helm.sh/stable
```

```
  helm repo update
```

```
k create ns prom
```

```
 helm install prometheus prometheus-community/kube-prometheus-stack -n=prom
```


```
k expose deployment deployment/prometheus-grafana --name=prom --type=LoadBalancer --port=3000 --target-port=3000  -n=prom
```

http://xxxx:3000


username: admin
password: prom-operator