- Author Nigel Wardle. Node4

# Audience
Developers, Solution Architects  

## Learn helm to deploy to AKS - Step 1 of 3

### The purpose of this training course is to understand how Helm can help deploy Kubernetes manifest files as a package.

https://helm.sh/docs/topics/charts

## Prerequisites

1. Request access rights to your Kubernetes cluster (K8s) from your trainer

2. Run the AZ CLI

3. Create a new directory in Azure CLI:

```
mkdir helm-training && cd helm-training
```
4. Check that Helm is installed and working:
```
helm version
```

5. Let's now create a Helm chart and view the files created:

```
helm create example-app
```
```
ls example-app
```

6. Open the AZ CLI editor and review the files. Note how the typical Kubernetes yaml files exist in the templates directory but have variables instead of hardcoded values.

7. What does nindent 4 mean?

8. To see and test how Helm merges the manifest files within the templates folder with  values from the values file - run: Helm template and review the trace output.
```
helm template example-app example-app --namespace=example-app --create-namespace
```

9. Now let's deploy this application to our Kubernetes cluster. 
```
helm install example-app example-app --namespace=example-app --create-namespace
```

10. You should see something like this. Assumes kubectl has access to a cluster

```
nigel@Azure:~/helm-training$ helm install example-app example-app --namespace=example-app --create-namespace
NAME: example-app
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

11. View the objects using kubectl

```
kubectl get all --namespace=example-app
```

12. You should now see something like this:

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






