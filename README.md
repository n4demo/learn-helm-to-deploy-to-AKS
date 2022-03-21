- Author Nigel Wardle. Node4

# Audience
Developers, Solution Architects  

## Beginners guide to Helm to deploy to AKS - Step 1 of 2

### The purpose of this training course is to understand how Helm can help deploy Kubernetes manifest files as a package.


### Kubectl Cheat sheet https://kubernetes.io/docs/reference/kubectl/cheatsheet/

### Helm docs https://helm.sh/docs/topics/charts

### helpful commands:
#### change directory back to home: *cd ~*
#### list all files: *ls -a*
#### view file: *cat my-file.yaml*
#### Show K8s objects: *kubectl get all --namespace=my-namespace*
#### Remove a directory: *rm helm-training --force --recursive*
#### Run an editor: *vi*

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

helm list
```

## Start

5. Let's now create a new Helm chart called MYNAME and view the files created: helm create [NAME]

```
helm create MYNAME --namespace MYNAME
```

```
ls MYNAME -R
```

### You should see something like this
```
nigel@Azure:~/helm-training$ ls nigel -R
nigel:
charts  Chart.yaml  templates  values.yaml

nigel/charts:

nigel/templates:
deployment.yaml  _helpers.tpl  hpa.yaml  ingress.yaml  NOTES.txt  serviceaccount.yaml  service.yaml  tests

nigel/templates/tests:
test-connection.yaml
```

6. Open the AZ CLI Editor and review the files. Note how typical Kubernetes yaml files exist in the templates directory but have variables instead of hardcoded values.

7. What does nindent 4 mean?

8. now perform a test to see how Helm merges all the manifest files within the templates folder with values from the (empty) values file. Create a new template call MYNAME using the chart MYNAME we created earlier. e.g helm template [NAME] [CHART] [flags] Review the trace output.

```
helm template MYNAME MYNAME --namespace=MYNAME --create-namespace
```

9. Now let's deploy this application to our Kubernetes cluster in the example-app namespace: helm install [NAME] [CHART] [flags]. 

```
clear
```

```
helm install MYNAME MYNAME --namespace=MYNAME --create-namespace
```

10. You should see something like this. Assumes kubectl has access to a cluster

```
nigel@Azure:~/helm-training$ helm install example-app example-app --namespace=example-app --create-namespace
NAME: nigel
LAST DEPLOYED: Mon Mar 21 10:35:00 2022
NAMESPACE: nigel
STATUS: deployed
REVISION: 1
NOTES:
1. Get the application URL by running these commands:
  export POD_NAME=$(kubectl get pods --namespace nigel -l "app.kubernetes.io/name=nigel,app.kubernetes.io/instance=nigel" -o jsonpath="{.items[0].metadata.name}")
  export CONTAINER_PORT=$(kubectl get pod --namespace nigel $POD_NAME -o jsonpath="{.spec.containers[0].ports[0].containerPort}")
  echo "Visit http://127.0.0.1:8080 to use your application"
  kubectl --namespace nigel port-forward $POD_NAME 8080:$CONTAINER_PORT
```

11. Paste the return commands into the AZ CLI


12. List the helm deployments in the given namespace

```
helm list --namespace=MYNAME
```

13. View the objects using kubectl in the given namespace

```
kubectl get all --namespace=MYNAME
```

14. You should now see something like this:

```
nigel@Azure:~/helm-training$ kubectl get all --namespace=nigel
NAME                         READY   STATUS    RESTARTS   AGE
pod/nigel-5c6f9c9dc9-htm5b   1/1     Running   0          80s

NAME            TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)   AGE
service/nigel   ClusterIP   10.0.81.138   <none>        80/TCP    81s

NAME                    READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nigel   1/1     1            1           81s

NAME                               DESIRED   CURRENT   READY   AGE
replicaset.apps/nigel-5c6f9c9dc9   1         1         1       81s
```

15. When ready, delete the Helm Releases

```
helm uninstall MYNAME --namespace=MYNAME
```

```
helm list --namespace=MYNAME
```

16. Now delete all Kubernetes objects hosted in the namespace

```
kubectl delete ns MYNAME
```

17. We may want helm to create our populated template but use gitops (ArgoCD) to deploy. This time generate a file and apply by using Kubectl

```
helm template MYNAME MYNAME  > MYNAME.yaml
```

```
cat MYNAME.yaml
```

18. Does the yaml file specify a target namespace?

```
kubectl create ns MYNAME
```

```
kubectl create -f MYNAME.yaml -n=MYNAME
```

```
kubectl get all -n=MYNAME
```

```
kubectl delete ns MYNAME
```

```
rm ~/helm-training/MYNAME --force --recursive
```

Go to https://github.com/n4demo/learn-helm-to-deploy-to-AKS/blob/master/README%20-%20step%202.md
