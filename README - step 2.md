- Author Nigel Wardle. Node4

1. Confirm your AZ CLI current working directory is set to 'helm-training' 

```
cd ~/helm-training
```

2. From the AZ CLI, from GitHub, clone the Helm training repository into your AZ CLI. Ask your trainer for PAT.

```
git clone https://PAT@github.com/n4demo/learn-helm-to-deploy-to-AKS.git
```

3. You should see something like this:

```
nigel@Azure:~/helm-training$ git clone https://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@github.com/n4demo/learn-helm-to-deploy-to-AKS.git
Cloning into 'learn-helm-to-deploy-to-AKS'...
remote: Enumerating objects: 249, done.
remote: Counting objects: 100% (249/249), done.
remote: Compressing objects: 100% (186/186), done.
remote: Total 249 (delta 125), reused 143 (delta 61), pack-reused 0
Receiving objects: 100% (249/249), 547.96 KiB | 14.42 MiB/s, done.
Resolving deltas: 100% (125/125), done.
```

### If you have created in the wrong folder you can use: rm learn-helm-to-deploy-to-AKS --force --recursive

4. From AZ CLI change directory into the downloaded cloned repo, then list files.

```
cd ~/helm-training/learn-helm-to-deploy-to-AKS
```

```
ls test-app
```

5. From the AZ CLI Editor, review the files in the 'learn-helm-to-deploy-to-AKS/test-app/templates' directory.

6. To see and test how Helm merges these manifest files within the templates folder with values from the (empty) values file - run: helm template [NAME] [CHART] [flags] and review the trace output. Note this will not deploy anything.

```
helm template MYNAME test-app --namespace=MYNAME --create-namespace
```

7. Now deploy the application: helm install [NAME] [CHART] [flags]

```
helm install MYNAME test-app --namespace=MYNAME --create-namespace
```

```
nigel@Azure:~/helm-training/learn-helm-to-deploy-to-AKS$ helm install nigel test-app --namespace=nigel --create-namespace
NAME: nigel
LAST DEPLOYED: Mon Mar 21 11:01:21 2022
NAMESPACE: nigel
STATUS: deployed
REVISION: 1
TEST SUITE: None
```

```
helm list --all --all-namespaces
```

8. Now we should replace hardcoded values in the manifest files with variable names that match values in the values file. From the editor open the ~/helm-training/learn-helm-to-deploy-to-AKS/test-app/values.yaml file and add the code below:

```
deployment:
 image: "nginx"
 tag: "1.21.6"
```

9. From the editor open:  ~/helm-training/learn-helm-to-deploy-to-AKS/test-app/templates/deploy.yaml. Scroll down to containers and replace the nginx image as below (hint if you make a mistake use CTL Z):

```
- image: {{ .Values.deployment.image }}:{{ .Values.deployment.tag }}
```

10. Upgrade the release 

```
helm upgrade MYNAME test-app --namespace=MYNAME --values ./test-app/values.yaml
```

### You should receive a response similar to the below

```
nigel@Azure:~/helm-training/learn-helm-to-deploy-to-AKS$ helm upgrade nigel test-app --namespace=nigel --values ./test-app/values.yaml
Release "nigel" has been upgraded. Happy Helming!
NAME: nigel
LAST DEPLOYED: Mon Mar 21 11:21:44 2022
NAMESPACE: nigel
STATUS: deployed
REVISION: 2
TEST SUITE: None
```

```
helm list --all --all-namespaces
```

11. Set to an older image from the command prompt and check it has been upgraded:

```
helm upgrade MYNAME test-app --namespace=MYNAME --set deployment.tag=1.8.1
```

```
kubectl describe deploy -n=MYNAME | grep -i image
```

12. To make our chart more generic, let's replace all hardcoded refs to object names with a Helm value expression and value. E.g. for cm.yaml we can edit as below:

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Values.name }}
data:
  name: "test"
```

13.  Rename values.yaml file to dev-values.yaml:

```
mv ~/helm-training/learn-helm-to-deploy-to-AKS/test-app/values.yaml ~/helm-training/learn-helm-to-deploy-to-AKS/test-app/dev-values.yaml
```

13b. Edit dev-values.yaml by as per the code below:

```
deployment:
 image: "node4demo"
 tag: "liverpool"
name: MYNAME-dev
```


14. Now deploy a copy of the app into the Dev namespace with names that reflect that it is being used for dev. Test first using the template option, then installclear:

```
helm template MYNAME-dev-app test-app --namespace=MYNAME-dev --create-namespace --values ~/helm-training/learn-helm-to-deploy-to-AKS/test-app/dev-values.yaml
```

```
helm install MYNAME-dev-app test-app --namespace=MYNAME-dev --create-namespace --values ~/helm-training/learn-helm-to-deploy-to-AKS/test-app/dev-values.yaml
```

```
nigel@Azure:~/helm-training/learn-helm-to-deploy-to-AKS$ helm install nigel-dev-app test-app --namespace=nigel-dev --create-namespace --values ~/helm-training/learn-helm-to-deploy-to-AKS/test-app/dev-values.yaml
NAME: nigel-dev-app
LAST DEPLOYED: Mon Mar 21 11:47:39 2022
NAMESPACE: nigel-dev
STATUS: deployed
REVISION: 1
TEST SUITE: None
```

14b. Check that the objects have been created If the above fails run: kubectl delete ns MYNAME-dev
```
kubectl get all -n=MYNAME-dev
```

15. Finally, create a copy of dev-values.yaml as uat-values.yaml and update the name value accordingly and deploy a UAT version

```
cp ~/helm-training/learn-helm-to-deploy-to-AKS/test-app/dev-values.yaml ~/helm-training/learn-helm-to-deploy-to-AKS/test-app/uat-values.yaml
```

```
deployment:
 image: "node4demo"
 tag: "liverpool"
name: MYNAME-uat
```

```
helm install MYNAME-uat-app test-app --namespace=MYNAME-uat --create-namespace  --values ./test-app/uat-values.yaml
```

```
nigel@Azure:~/helm-training/learn-helm-to-deploy-to-AKS$ helm install nigel-uat-app test-app --namespace=nigel-uat --create-namespace  --values ./test-app/uat-values.yaml
NAME: nigel-uat-app
LAST DEPLOYED: Mon Mar 21 11:55:51 2022
NAMESPACE: nigel-uat
STATUS: deployed
REVISION: 1
TEST SUITE: None
```

```
helm list --all --all-namespaces
```

```
nigel@Azure:~/helm-training/learn-helm-to-deploy-to-AKS$ helm list --all --all-namespaces
NAME            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART           APP VERSION
nigel-dev-app   nigel-dev       1               2022-03-21 11:47:39.342259641 +0000 UTC deployed        test-app-0.1.1  1.10.0     
nigel-uat-app   nigel-uat       1               2022-03-21 11:55:51.958281453 +0000 UTC deployed        test-app-0.1.1  1.10.0    
```

## Congratulations!! You now know how to deploy different versions of an app, with multiple instances across different Kubernetes namespaces. 

## Finally, clear out your objects

```
helm uninstall MYNAME --namespace=MYNAME

helm uninstall MYNAME-dev-app --namespace=MYNAME-dev

helm uninstall MYNAME-uat-app --namespace=MYNAME-uat
```

16. You can also clear out the Kubernetes objects by namespace.

```
k delete ns MYNAME-dev

k delete ns MYNAME-uat
```

17. finally remove your git repo

```
cd ~
rm ~/helm-training -f -r
```
