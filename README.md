- Author Nigel Wardle. Node4

# Audience
Developers, Solution Architects  

## Beginners guide to Helm to deploy to AKS - Step 1 of 2

### The purpose of this training course is to understand how Helm can help deploy Kubernetes manifest files as a package.


### Kubectl Cheat sheet https://kubernetes.io/docs/reference/kubectl/cheatsheet/

### Helm docs https://helm.sh/docs/topics/charts

### helpful commands:
#### list files: *ls*
#### view file: *cat my-file.yaml*
#### Show K8s objects: *kubectl get all --namespace=my-namespace*

## All commands can be executed by copying to the AZ CLI: >_

 ![The cloud shell icon is highlighted on the menu bar.](media/b4-image35.png "Cloud Shell")



## Prerequisites

1. Request access rights to your Kubernetes cluster (K8s) from your trainer and download your kube-config file.

2. Run the AZ CLI

3. Create a new directory in Azure CLI:

```
mkdir helm-training && cd helm-training
```

4. Check that Helm is installed and working:
```
helm version
```

5. Let's now create a Helm chart and view the files created: helm create NAME

```
helm create example-app
```

### You should see something like this
```
example-app/
├── .helmignore   # Contains patterns to ignore when packaging Helm charts.
├── Chart.yaml    # Information about your chart
├── values.yaml   # The default values for your templates
├── charts/       # Charts that this chart depends on
└── templates/    # The template files
    └── tests/    # The test files
```

```
ls example-app
```

6. Open the AZ CLI Editor and review the files. Note how typical Kubernetes yaml files exist in the templates directory but have variables instead of hardcoded values.

7. What does nindent 4 mean?

8. To see and test how Helm merges the manifest files within the templates folder with  values from the values file by running: helm template [NAME] [CHART] [flags] and review the trace output.

```
helm template my-example-app example-app --namespace=example-app --create-namespace
```

9. Now let's deploy this application to our Kubernetes cluster: helm install [NAME] [CHART] [flags]. 

```
helm install my-example-app example-app --namespace=example-app --create-namespace
```

10. You should see something like this. Assumes kubectl has access to a cluster

```
nigel@Azure:~/helm-training$ helm install example-app example-app --namespace=example-app --create-namespace
NAME: my-example-app
LAST DEPLOYED: Mon Mar 14 16:22:55 2022
NAMESPACE: example-app
STATUS: deployed
REVISION: 1
NOTES:
  Get the application URL by running these commands:
  export POD_NAME=$(kubectl get pods --namespace example-app -l "app.kubernetes.io/name=example-app,app.kubernetes.io/instance=example-app" -o jsonpath="{.items[0].metadata.name}")
  export CONTAINER_PORT=$(kubectl get pod --namespace example-app $POD_NAME -o jsonpath="{.spec.containers[0].ports[0].containerPort}")
  echo "Visit http://127.0.0.1:8080 to use your application"
  kubectl --namespace example-app port-forward $POD_NAME 8080:$CONTAINER_PORT
```

11. Paste the return commands into the AZ CLI


12. List the helm deployments

```
helm list
```

13. View the objects using kubectl

```
kubectl get all --namespace=example-app
```

14. You should now see something like this:

```
nigel@Azure:~/helm-training$ kubectl get all --namespace=example-app
NAME                               READY   STATUS    RESTARTS   AGE
pod/example-app-64db6ff57b-72h4q   1/1     Running   0          73s

NAME                  TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE
service/example-app   ClusterIP   10.0.243.187   <none>        80/TCP    73s

NAME                          READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/example-app   1/1     1            1           73s

NAME                                     DESIRED   CURRENT   READY   AGE
replicaset.apps/example-app-64db6ff57b   1         1         1       73s
```

15. When ready, delete the objects from AKS

```
kubectl delete ns example-app
```

Go to https://github.com/n4demo/learn-helm-to-deploy-to-AKS/blob/master/README%20-%20step%202.md




